This page is a reference for the server guru for the group.

## User Account Administration

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





