Title: Learning librarian-puppet p1
Date: 2013-03-27

I've been diving deep into puppet at work recently and we're struggling with maintaining our collection of puppet environments, modules, vagrant repositories, and backend repositories. In learning to simplify all this, I've been looking heavily at the following setup:

1. Put each customer's environment data (manifest files) in its own repository.
2. Put each module in its own repository.
3. Glue an environment to the modules using [librarian-puppet](http://librarian-puppet.com/).

As a quick into, librarian-puppet uses a configuration file called "Puppetfile". This file contains a list of all modules and the git repository to pull them from. Running librarian-puppet in the same working directory as Puppetfile creates a directory "modules/" and downloads each of the modules referenced into that directory, overwriting anything within the directory. If one of those modules is updated in the upstream repository, librarian can update the local copy. librarian-puppet is run by hand, but could be called from a cron job or continuous integration server such as [Jenkins](http://jenkins-ci.org/)


This is all well and good, but despite the growing popularity of librarian-puppet, I still am a long way from figuring how it would work with multiple contributors and our multi-customer, multi-environment approach. Also, I want to be able to develop and test modules using vagrant. 

My theoretical workflow for improving and testing modules would go like this:

1. Pull down the vagrants repo (a repo containing various vagrant files).
2. Go into the directory containing the particular vagrant instance I want to work with.
3. Do a "vagrant up", which will use librarian's Puppetfile to pull down necessary modules.
4. Go into the modules directory, make changes, test with "vagrant provision"
5. Check that module's changes into upstream. 
6. Vagrant instances would pull and commit to the 'master' branch of each module repository. 
7. Each module repository would additionally have a 'dev', 'stage', and 'production' branch.
8. As we move a module through its paces, we would merge it into dev, then stage, and finally production.
9. Once checked into a branch, we could use librarian to update a customer's environment and verify the modules functionality.
10. By the time the module reaches stage and ultimately production, we should be fully confident in that module's usability.
11. If necessary, additional branches could be created for specific circumstances.

In preparing for that workflow, a few questions come to mind.

1. When exactly does librarian-puppet overwrites the "modules/" directory?
2. If I am making changes in a specific module while under Vagrant, and 'vagrant provision' is set to run librarian-puppet, could I clobber my own changes while testing? 
3. How can I avoid this but still have the most recent versions of the module in my testing environment?
4. The documents reference Puppetfile.lock is important in tracking module version. What is the importance of this file in reference to the base Puppetfile?

In order to fully answer these questions, I am going to setup several repositories on github and a local vagrant instance. I will have a testing environment: "vagrants", 9 client environments: "client[1-3]dev, client[1-3]stage, client[1-3]production", and 3 module directories: "shared-filemaker", "client3-filemaker", and "shared-motd". So as to not clutter up my personal github (which is already getting cluttered), I will probably make a fresh account just for this. The repository names do not entirely matter, except for my own conveinence. librarian-puppet will save the module with the name defined in Puppetfile. For instance, both "shared-filemaker" and "client3-filemaker" could be saved as the module name "filemaker", and as long as they are each in their own environment, there will be no namespace conflict.




