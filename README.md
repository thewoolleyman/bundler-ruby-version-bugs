# bundler-ruby-version-bugs

Repo to illustrate bugs in new ruby version support for bundler >= 1.12

(notes from Slack)

Regarding the mismatched RUBY VERSION in Gemfile.lock - I looked into
the code and I think it's a bug in the recent PR
for using version operators in
Gemfile: https://github.com/bundler/bundler/pull/4064 .
Specifically, if you bundle WITH a ruby version in your Gemfile,
then subsequently remove it from Gemfile,
it never gets removed from Gemfile.lock.

Also there's already an unanswered StackOverflow about the issue:
http://stackoverflow.com/questions/36316399/can-i-stop-bundler-from-adding-ruby-version-to-gemfile-lock .

See relevant code here:
https://github.com/bundler/bundler/blame/master/lib/bundler/definition.rb#L333  .

You have to have some other non-ruby-version change to trigger
the Gemfile.lock rewrite.

...and there's (separate?) bugs where 
1. a change (or deletion) in Gemfile ruby version doesn't trigger
   a Gemfile.lock rewrite if there are no other changes, and 
2. bundle check passes even if ruby version in Gemfile.lock doesn't
   match what's in Gemfile (not sure if this is check command's job
   but something should blow up or prevent this).
