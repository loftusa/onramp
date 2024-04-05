## Why posting on arXiv is a special moment

An interesting new paper will get a spike in attention and word-of-mouth when it is first made available.  Readers who are excited by its findings will bookmark it to read, they will skim the materials, and they will form a first impression.  And then they might decide to pass it on to other people who could be excited and pass it on further.

After a few days, the surprising results in the paper become somewhat-known, and the process slows down dramatically. Even when new readers discover and share it, their friends will have seen it before, and the work will not be naturally promoted in the same way.  Eventually the findings of the paper will become part of the generally-known common wisdom, and people might not even remember where they learned the ideas that you have created in your paper. Readers might encounter the paper and wonder why somebody wrote about something so obvious - even though the paper might have been the origin of the idea in the first place.

Each paper only has one chance to be a new paper, so it is worthwhile to maximize the paper's chance at getting the attention, credit, and citations that it deserves. In our fast-moving field, usually the birth of a paper into public consciousness is the moment when the paper is posted on arXiv for the first time.

Here are the steps to follow at that moment.

## Ten Steps to Follow When arXiv'ing a paper

Before you arXiv a paper:
1. Check the **authors**. Verify the order of the authors, and verify explicitly with all the authors that they are OK going "on the record" with the arXiv'ed paper as it is. Make sure everybody's name is spelled correctly and that contact information is accurate.
2. Add **acknowledgements** to the paper. Be sure to acknowledge and thank funding sources. Think about non-authors who have contributed ideas or efforts or resources and thank them.
3. Prepare a **source code release** that allows the methods in the paper to be reproduced by a researcher who might want to build on top of it. The code should be clean and made available on github.
4. Prepare a **website** that explains visually, and in web-friendly non-formal language, what readers can learn from the paper. You can copy our website template; it should include links to all the other resources, visual explanations of methods and results, and citations and bibliographic information. It should include analytics scripts to allow traffic to be measured.
5. Prepare a **data release** where any data (or trained model weights etc.) needed to reproduce your paper are cleanly formatted and made available e.g., as a zip to download.  This can be posted on a directory of the website.
6. Prepare a **twitter thread** with 5-10 twitter posts that link to previous conversations about background research, and that explain the paper's motivation and main findings.  (Typefully can be used to manage the post.)
7. Consider preparing **tutorial slides** for a short presentation about the paper's methods and findings (e.g., a pptx file).  Such slides will be used by people who are thinking of teaching your paper in a reading group or in a class.  Once you have slides, consider recording a **short video** of an author presenting the slides.
8. Consider creating an **online demo** of your work.  This could be as simple as a colab notebook that demonstrates how to apply the method, or it could be a more-involved custom web demo.
9. Make all the resources **link** to all the others.  I.e., The paper should link to the website and github, and the website and github should link to each other and the paper. The twitter thread and slides and demos and videos should link to all of them.
10. Finally, **post it** on arXiv (follow [these guidelines](Preparing-files-for-arXiv)) simultaneously to making the website live. The arXiv TLDR should include a link to the website. Wait a day for arXiv to make it public, link everything to their final URLs, and then post the twitter thread.

By the way, at the end of peer-review when you are preparing a camera-ready version of the paper, it is a moment to go through the 10 steps again and update all your materials to their finished peer-reviewed form. After peer-review is also a moment to consider preparing new materials, such as stories designed for press outreach, aimed at the nontechnical lay audience. Presenting at a conference, especially if you win an award, is another moment for people to form a first impression, and another moment to go refresh the ten steps.

## The principles behind the guidelines

What makes a paper worth passing on between researchers?  There are several levels of commitment that a reader can develop with a piece of research. They can be:

1. A **reader**: Because it teaches us something new that we want to know.
2. A **citer**: Because it validates (or challenges) the reader's own work and beliefs in the community discussion.
3. A **teacher**: Because it explains something well that we would like to teach to others.
4. A **researcher**: Because it forms the foundation for further work we want to pursue.

An academic paper is typically too dense to absorb quickly, so all the supporting material around a paper will help people assess whether the research meets their needs.  Ideally, when a person sees the paper for the first time, we give them enough information to know how high on the "ladder of commitment" they will want to go with the paper.  Should they just skim it for their own personal knowledge? Should they share it with their collaborators? Should they learn it well enough to re-teach its ideas?  Should they dig into it well enough to apply the methods and build their own work on it?

Unfortunately, most papers that are published have no supporting materials, so even if there are many readers who in principle should re-teach and build upon its findings, they cannot.  So most people will take a quick look at a paper and decide "there's not enough here for me to invest time in," and they will forget about it after a quick skim.

The **webpage** is our chance to quickly demonstrate that the paper is worth investing time in.  It should immediately answer question number 1: **"Is this paper about something new that you want to know?"** and it should be a hub that people will expect to use to get to all the other resources to answer questions 2, 3, and 4.  In other words, it should link to other papers to situate it in the community discussion; it should provide teaching materials for people who might want to re-teach the paper; and it should provide code and data for people who may want to reproduce, apply, and extend the research.

The **twitter thread** is focused on the question number 2: **"Does this paper validate or challenge my own beliefs in the academic discussion?"**  It is important that the twitter thread is not just another summary webpage - it should link to the webpage, but then it should take advantage of the social medium to quote-tweet ongoing discussions in the community that are relevant to the paper, in addition to explaining, illustrating, and linking the main results.

**Slides, videos, graphics, and demos** are about question number 3: **"Can I re-teach this paper?"**  If you publish slides and demos, then the answer is "yes." If you don't, then the answer will be, "it'll take some work."  When you made your paper, you made figures. Slides that gather together your figures in a very explanatory way make it much more likely that your paper will end up in classrooms, seminars, and reading groups this year.

The **code and data release**, and in particular any working **colab-notebook demos** answer question number 4: **"Can I apply the methods in this paper in my own work?"**  It is sadly difficult to reproduce most published work, and most research is therefore never applied. You should make it very easy to reproduce your work, and furthermore you should *let people know* that are doing this by making it obvious on the first day of release that the code, data, and models are available and easy to run, modify, and apply.  Having people adopt your methods and code is the highest form of recognition of the significance of your work.  And work that is truly reproducible in practice in makes a much stronger contribution to the field than simply a published paper.