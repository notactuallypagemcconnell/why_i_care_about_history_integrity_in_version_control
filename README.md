# On Git (or similar) Version Control History Integrity

Git is super useful even if we dont use it effectively.

At the end of the day, it provides us with a means to see snapshots at points of history we choose.
These snapshots can have notes in them.
Even if the notes are as mysterious as the mess left in the kitchen by a drunk hungry person at 2am, you can piece through the picture of the situation and assess what happened.


## On Commit Messaging
A big part of history, it turns out, is writing it down.
No matter what history will happen, "time keeps on slippin' into the future", as our friend Steve Miller said.
Its really easy to forget the impact of writing down the events that happened.

Let's take a look at a really old Rails commit.
I went way far back because I figured before it was such a MASSIVE project that ran significant portions of the entire internet, that there might be a few meh commit messages slipping in.

[Here](https://github.com/rails/rails/commit/9ad6e1a62818032cb58ae58f093fb528fe521307) is an example of a commit that isn't super useful and doesn't provide much context.

> Various problems with in-memory sqlite dbs

This doesn't tell us much.
And it isn't a crisis.
DHH has kept rails going just fine along with many others even though this commit doesn't have a great message.

However, [a more recent commit](https://github.com/rails/rails/commit/ad0bde58d4e5dd3686fdbdc21c7e4b6d71e371e4) is a great example of an awesome commit.


> Don't translate non-database exceptions.

> The AbstractAdapter will translate all StandardErrors generated during the course of a query into ActiveRecord::StatementInvalids. Unfortunately, it'll also mangle non-database-related errors generated in ActiveSupport::Notification callbacks after the query has successfully completed. This should prevent it from translating errors from ActiveSupport::Notifications.

This tells us just about anything we could want to know from the changes provided.
It takes time to write up your thoughts like this.
But it provides _extremely_ valuable context to future users.

It also has another benefit: when you write things down, you realize bad ideas a lot faster.
I have had times where I've got my work done and I go to write my final 'good' commit message with an amend before opening a PR, and once I get to writing it up I realize a flaw in my approach and make further changes.
This really means I should write things down when I start and save some time, but it also shows the value of writing in general.
Writing down a bad idea is _hard_ for a rational person if it means "this is how it shall be".


## On History Integrity by Passing Tests
One thing about the two prior commits: though one lacked a great message with context, it still is not that bad in my book because the tests passed.
Having a commit without tests passing might not sound like a big deal when youre first learning Git.
"So what? I have a snapshot where I know my test was failing in that way and its true I was stuck that way."
This is valid.
When working locally, _*not every commit needs to have passing tests*_, sometimes you need to be able to just say "I NEED TO GET TO THIS SPECIFIC POINT OF STATE NO MATTER WHAT".
Groovy, do that.

But, once it comes time for a PR: no matter how many commits in the PR, be it 1 or many, *every single one should have green tests*.

Why?

Let's imagine a situation.
You have found a bug in prod.
You have written a test for it.

In fact, let's use a real example I encountered at work recently.
On [Learn.co](http://learn.co) if you are one of our online students, you get a feature called expert chat.
It links you up with someone who will help you get unstuck on whatever lesson you're working on.
Pretty neat!

There was a bug reported recently.
I got a ticket opened up that said a message is showing in the dialogue but when they go to reply it says, "No active room!".
It had loaded the message state from the top message, but since they didn't click the room that message was in the chatter couldn't reply to the student.

I used [react-testing-library](https://github.com/kentcdodds/react-testing-library) to write a test that replicated this bug.
This would be enough to fix it, but what if we wanted to understand even more.
What if we found the exact commit that introduced the bug so we could maybe paint a bigger picture of how it came to be.

We can do that with [git bisect](https://git-scm.com/docs/git-bisect).
It will take two revisions (commits) and then proveed to do a binary search and using our test as passing or failing, find the commit which introduced the bug.

Thats pretty friggin neat right?

## On Spelunking and Discovery
There is another great aspect to having good history: archaeology for new folks.

When you join a new team, understanding some history of the codebase can be really really useful.
I often find myself writing a quick shell script to find the commits throughout a projects history that have the biggest code diffs, and the biggest commit msg line counts.
If you find the biggest diffs and the longest commits and read them from oldest to newest, you learn a lot.
I find this super valuable when working on new projects.

## Conclusion
History is important, and when we write it down we should do so with some rigid standards on passing tests, messaging, and an emphasis on the _why_ of things, because it will make everyones lives easier.

Thats pretty friggin neat right?
