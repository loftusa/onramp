This page is a reference for the server guru for the group.

## User Account Administration

### Setting up access to LDAP servers

Both the baulab cluster at Khoury ("baulab.us") and David's cluster at baukit.org each have a separate LDAP domain served by a little openldap server.

LDAP server address on baukit.org: "names" (192.168.0.2)
LDAP server address on baulab.us: "baunames" (10.200.205.143)

You do not authenticate ssh into these machines using LDAP - naturally, because we want to make sure you can still access them if LDAP is down. The administrator have a local account set up on each of these machines (e.g., I set up "localdavidbau") so that it's possible to directly log in and figure out how to fix LDAP if ldap itself becomes broken.

For managing user accounts on each server we run a small apache server running [phpLDAPadmin](https://github.com/leenooks/phpLDAPadmin) which provides a very rudimentary UI to LDAP.  This apache server does not serve its user interface on the public internet.  To access it, you will need to ssh from your laptop with a port-forwarding tunnel to the appropriate ports.  I have the following in my .ssh config for doing this:

```
Host baunames
    ProxyJump login.khoury.northeastern.edu
    User localdavidbau
    LocalForward 8877 localhost:8877

Host names
    ProxyJump baukit.org
    User localdavidbau
    LocalForward 8876 localhost:8876

Host login.khoury.northeastern.edu
    User davidbau

Host baukit.org
    User davidbau
```

### Adding a user

Remind users that they will have several different login IDs.
   * The Northeastern login ID that is for their northeastern email and Windows passwords on Northeastern computers.
   * The Khoury login id for `ssh` access to `login.khoury.northeastern.edu`.
   * The baulab login ID for baulab student machines at 177.
   * The baukit login ID for David's personal cluster at his house (baukit.org), usually the same name as the baulab one.
   * And of course CAIS cluster etc are all different.

Khoury login ids can be obtained here: https://my.khoury.northeastern.edu/account/apply - David will need to approve after the form is filled out.

For baulab and baukit accounts, you will add them youselves with these steps.

To add a user "lorem", do this:
  1. `ssh baunames` (or "names")
  2. browse to `http://localhost:8877` (or `http://localhost:8876`)
  3. Log in as the administrator `cn=admin,dc=baulab,dc=us` (or `cn=admin,dc=thevisible,dc=net`). Admins will know the password.
  4. Use the left nav to visit the user record of the previous last user we added.
  5. "Copy or move the entry", and copy it to a new entry named `uid=lorem,ou=People,dc=thevisible,dc=net`
  6. In the new record do NOT change the gid (it should still be 1025, research).
  7. But DO update the user's real name
  8. And DO update the home directory, which should be `/share/u/lorem`.
  9. And DO set the user's initial password, which you can create and tell the user.
  10. And DO update the uid for the user, which should be incremented by one.
  11. Remember to click "Create Object" to save the user record.

Then: Manually add the same user with the same password on the other cluster, so both clusters have the same list of users.

### Changing a password for a user

Just like adding a user - but modify the user record and use the phpLDAPadmin interface to set a new password.  Usually a good idea to set passwords at both baulab and baukit at the same time.

### How the remaining steps of login work

See [this medium post](https://medium.com/@fengliplatform/understanding-nss-and-pam-using-a-ssh-example-80512eb0f39e) that explains the services we use for login on client machines in our little clusters.

On ubuntu, user identification is mediated by the "GNU Name Service Switch" (nsswitch) which is responsible for figuring resolving who the username resolves during login.  The `/etc/nsswitch.conf` configuration file has a line `passwd: files systemd sss` which means that usernames and passwords are drawn from (1) first the standard linux files such as `/etc/passwd`, which will have local accounts like `localdavidbau`; (2) then the systemd service which enforces `root` should exist even if not listed in `/etc/passwd`; then (3) the system sercurity services (sss) daemon which knows how to use an LDAP server, and which will resolve ordinary network accounts like `davidbau`.

In our cluster, The "System Security Services Daemon" SSSD knows how to talk to LDAP; it is configured at `/etc/sssd/sssd.conf`.  That file tells SSSD how to reach the LDAP server, which is used to get the cluster-wide numeric userid for a username.  It can also check or change the password.

When it comes time to actually collect a password, linux consults the Pluggable Authentication System (PAM) which is configured in files within `/etc/pam.d`.  Specifically, we also set up PAM to consult with SSS for both checking and changing the password, which again, knows how to talk to LDAP.

SSSD needs to use a trusted encrypted connection to transmit passwords.  We have a self-signed certificate that we use to trust the encryption used by the LDAP server; that trusted certificate is listed in `/etc/ssl/certs/ca-certificates.crt`.

Once the user is authenticated (their password is OK), they're given their userid and they can log in.  The first thing done at login is to change to their home directory (by convention we have it as the `/share/u/[username]` directory), but on first login that directory does not yet exist.  

We also set up `/etc/ldap/ldap.conf` that also describes how to read the LDAP server (I don't think this is used by the login process). This file allows the `ldapsearch` utility to run on all client machines, e.g., `ldapsearch -x` lists all the public information about users within the LDAP system on the cluster.

All these configuration files are installed by the ansible script `ldapclient.yml`.

## Topics Needed

- Ansible scripts and workstation setup.
    - Where and how to run ansible.  /etc/ansible and the playbooks and the ansible script repo.

    
- NFS setup, both client and server
    - How autofs is set up on the clients
    - The synology NFS servers and how to administer them remotely.

