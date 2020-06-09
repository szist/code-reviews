# code-reviews

I've found myself repeating most/parts of this document on many projects, never writing it down. I have been on projects where the requirements for code reviews were very strict, and I've been part of YOLO driven development, too, and it made sense. 😊 

## Questions to answer

I'm hoping these questions could help set up guidelines for code reviews and practices that are agreed upon in any team. For each question I've added my very opinionated answers. Take them with a huge grain of salt. My opinions and my review process change slightly every time I come to a new project as I learn and more and more tools surface to help out with ensuring code quality.

### **What are code reviews?**

Code reviews are not well defined. Every company, even maybe every team will have a different understanding. A good generic definition I found from techopedia:

> A code review is the process of examining written code with the purpose of highlighting mistakes in order to learn from them.

My definition:
A process to ensure code quality, to protect application logic, to spread knowledge, and most importantly learning tool.

### **What is not a code review? What is not part of a code review?**

The answers might be very different. Some people use code reviews to just check for stinkers, other developers just check syntax, yet other developers check everything and it takes them half a day.

My answer:
Anything that can be covered by automation testing and CICD should not be part of the code review:

* syntax check 
* whether the application builds
* formatting (prettier, eslint)
* most functionality check (unit tests, integration tests)

### **What should be part of a code review then?**

What exactly is included depends on the project and should be defined. For example a mobile application would require to check responsiveness; a backend application would have stricter requirements on security. Apart from project specific ones, the following questions should give some pointers.

To give you a concrete example a multi-lang react native application might require the following checks (apart from the generic ones):
* are all string localized
* does the application still work in RTL mode
* is the layout responsive
* are all the icons/pngs high quality and ideally scalable?
* Are the components well-defined, properly separated, easy to reuse, ...?
* ...

### **When do I ask for a code review?**

If you want someone else to take a look at a code change you're working on. It can be as soon as "this doesn't work yet, but I wanted to get your thoughts" and as late as "this change works and I'm about to check it in, please review." The idea is to have code reviewed early and often. The earlier a mistake is corrected, the cheaper it is. More collaboration during the coding process means more mistakes caught earlier, better designs, and an overall more satisfying development experience.

In my experience the **initial design and early discussion are very underestimated** and rarely practiced. Developers usually just take a task, do it, then post a pull request. At the same time developers would complain about review processes taking too long, reviewers requesting changes all the time, reviews taking days/weeks, yet wouldn't do a proper analysis, brainstorm and validate their approach, properly think through code/logic organization, have a quick chat with a colleague even before writing a single line of code. <sup>By the way imho this is what differentiates senior from junior developers most of the time.</sup>

### **Whom do I ask for a code review?**

In general, you should also make your best effort to include anyone who might be affected by your change, or at least representatives from their teams. In general, you want to ask everyone who is necessary to make sure that your change is the best it can be.

This was a point of contention in every project I've been in. With a lack of clear code owners, team members changing from month-to-month, team structure changing it's very challenging to find the right person(s). In some projects whole teams were notified (read spammed), on other projects the owners had to hunt down anyone willing to do a review for them (ain't nobody got time for that). 

My opinion is that the owners should identify and request who would be best to review their code, with the reviewers having the opportunity to pass on the responsibility. But the latter happens without the author's involvement.

### **What should a pull request look like?**

It depends on the phase when the PR is created (preliminary review or final review). Against preliminary PRs I have very little expectations. For (semi-)final reviews, though, my list is longer:

* useful PR description with relevant links (issue tracker, designs, API documentations)
* description contains all non-trivial decisions made during development (anything agreed upon with business, designers)
* ideally sensible commits, otherwise expecting squash&merge (the whole rebase vs merge battle)
* the PR is self-contained (not a partial result) and correctly split up (not too large, unrelated things are separated out)

### **How do I do a code review?**

Everyone has a different process. My review process usually looks like roughly this:

1. Check out the task (if tracked in an issue tracker for example), read through the acceptance criteria.
1. Give it some time to understand the requirements, imagine what would need to happen to do it with the limited knowledge I have at this point. (this can take from few seconds to max few minutes, otherwise I would just ask the author to have a quick chat)
1. Read the PR description to see if there were any decisions made during development, any additional details that I should consider and might not have been clear to me.
1. Briefly check out what it does for myself (running the application, starting up the service, poke around), nothing serious at this point.
1. Review the code and **!!understand!!** the changes that are done, why they are done. (might be a second call/chat with author, if something non-trivial is happening)
1. With the knowledge of how it's implemented, try to break the application. Try methods, edge-cases not covered by tests.
1. Approve/Comment/Request changes

Some would argue that the second to last one should be QA work. At some companies there is even a dedicated QA role for this, who's sole task is to add automation and ensure that the code is safe. However, I think all developers would be better off learning to get their hands dirty, to break things, see the edge cases, and learn how to detect stinky parts in the future.

### **In general what do I look for when doing a review?**

A generic list of things to check:

<dl>
<dt>Correctness</dt>
<dd>Does the code change have the desired effect?</dd>

<dt>Risk</dt>
<dd>Does the change or is there a possibility that the change might break something elsewhere? Can it be prevented?</dd>

<dt>Code quality</dt>
<dd>This does not include things like "should there be a space after that comma". That should be covered by formatter, linter. What should be included are things not covered by tooling. Some of them might be even covered by some linters: from "that method has too many parameters" (see Code Quality below) to "I don't think that variable's name is clear" to "I think this would be simpler if you used the visitor pattern". When you look at this code again in a month, is it readable enough that you'll understand why the change was made without searching through old emails/slack messages/issue tracker tasks/github issues?</dd>

<dt>Testability</dt>
<dd>Is there an automated test that covers this change? If not, does it make sense to write one?</dd>

<dt>Security</dt>
<dd>This part is often ignored, but should always be at the back of every developer's mind. Could this change make users more vulnerable to hacking? Is there a way to ensure better security?</dd>

<dt>Performance</dt>
<dd>Will this change cause a performance problem under some situations? Does it cause problems with database load? Memory use? Network traffic? Browser load? Application lag</dd>

<dt>Other reviewers</dt>
<dd>Is there someone else, or some other team, that you think should be aware of this change? If so, make sure that the reviewee invites them to join the review, or add them directly (github allows you).</dd>
</dl>

### **What am I responsible for when I'm an: author?**

This is strictly opinionated, and might radically differ in different projects/teams.
For me it's always the author who is primarily responsible for anything happening in the PR!

* Assign reviewers who would best fit to review the code
* Primarily it's the author who has to make sure the reviewers do it in a timely fashion (ping them if necessary). This is partly on the reviewers as well, see below.
* incorporate changes
* keep the PR up to date and merge after the process is done

You should answer all the reviewer's questions to their satisfaction. If the reviewer asks for a change, you must either make it or explain to him or her why it's not necessary. Remember, you asked the reviewer for the review. Make sure you listen to what he or she has to say. By agreeing to the change, the reviewer is becoming responsible for it, too.

That said, reviews are not the place to have lengthy discussions as that's detrimental to productivity. For me the same rule applies here as with remote work communication: the moment it becomes longer than a paragraph and nobody else would get anything out of it => take it offline, give it a call, discuss it briefly, write a summary!

### **What am I responsible for when I'm a: reviewer?**

The responsibility directly affects the process of a reviewer (described above). I don't think responsibilities change much between projects. Once you are assigned as a reviewer, you become responsible for the code. 

You must understand the code change well enough to make an informed decision about whether it should be released. You must make sure that the author answers all your questions satisfactorily and either makes any changes you think are necessary or convinces you that they're really not. That includes changes needed for all aspects of the code review (see above). Not just correctness. If a code change goes out that causes an injection, it's your fault, as well as the fault of the developer making the change. If a code change goes out that makes automated testing more difficult, it's your fault, too.  If a code change goes out that makes the application sluggish, it's your fault, too. Remember, the author asked for your code review. He or she wants your opinion and your best judgment. Make sure you give it to him or her.


### **When should I approve a review?**

The most important question after all is this. The easy answer is that whenever all of the above is satisfied. In a nutshell: I understand the code, there are no stinkers, the code quality is excellent, no security issues, there is proper test coverage, all edge cases are accounted for.

In reality it's not that simple, is it? 😊😊😊

We are pressed by deadlines, we are blocking other people, there are a ton of small things we could do to improve code quality, we could add tests, but we are just adding some temporary code.

In general, my approach is to approve, whenever only small concerns were raised and we agreed to do some changes in followups. I have to trust the author to follow through with these. I'm expecting the author to quickly address small concerns before merging in and make sure followups are tracked (either new PR is created or in an issue tracker). My approach requires building trust with the team members, know their strengths and weaknesses, which is the exact opposite how I'm supposed to view pull requests: don't trust anything. It's a compromise for me to ensure velocity, productivity and agility.

## Code Quality

I think this deserves a separate section. There are many facets to code quality and it's a very subjective matter most of the time. In general what I found are good indicators of code quality:

<dl>
<dt>Readability aka the WTF factor</dt>
<dd>Good code is simple to read, easy to understand, doesn't repeat itself (unbind your copy&paste shortcuts! /jk). This ranges from using well-known design patterns and conventions to using shorthand expressions to reduce the amount of code to read. It includes good naming practices, well-defined and easy to grasp logic separation, single responsibility principle, avoiding leaky abstractions, proper encapsulation, etc. There are whole books written on this topic, so I would just stick to "how many wtf's pop up in your head during a code review" unless you know better.</dd>

![wtf code](https://i2.wp.com/commadot.com/wp-content/uploads/2009/02/wtf.png?w=550)

<dt>Flexibility</dt>
<dd>How easy it is to change the code if new requirements come in (they will come!) or business logic changes (it will!)? It can range from very flexible (smells of over-engineering) to very rigid (you better rewrite the whole thing if something changes). I haven't found an exact way to measure this. For me it's usually comes down to following good programming principles, KISS and whatever makes sense at any given time.</dd>

<dt>Maintainability</dt>
<dd>This is related to flexibility. The difference is that flexibility covers business logic changes. By maintainability here I mean if the code will work even after a few months. Is it using any experimental syntax, unstable libraries? How pragmatic should I be writing this code? Can I use the new shiny state managers for react like ReactN or am I better of just going the boring-battle-tested-not-without-any-issues redux? Will the maintainers curse me after I handed over my code?</dd>

<dt>Reusability</dt>
<dd>How much of the code I'm writing could be reused later? Am I copy&pasting this code the second time now? Does it make sense to make my code re-usable at this stage? Can my code be reused between platforms? backend frontend? A very good example for this is I think graphql typings where the backend systems and frontend would have common type definitions. Another example might just be a list of input fields I'm registering in a form - probably it would make sense to have the input definitions in a data structure and just loop through that, right?</dd>

<dt>Correctness</dt>
<dd>Does the code have the desired effect in all scenarios? Does it have a defined result for every possible input? This is usually easy to see and most of the time should be covered by tests. Which brings us to:</dd>

<dt>Testability</dt>
<dd>This is kind of a separate topic, but in my experience it's very tightly coupled with code quality. Is the code easy to test automatically? Do I have to mock half the world just to get my tests running? Can the code be split up to help with that? In my experience when a code is easily testable, it usually follows most good programming practices and pragmatic approaches. It forces me to split up the code to bite-sized chunks: easy to read, easy to reason about, easy to test. Unfortunately this is not always achievable (think unstable network connection, animations, transitions, distributed computations...)</dd>


For an even more opinionated idea about good code check out (The No Spaghetti Manifesto)[https://www.vacuumlabs.com/spaghetti]



