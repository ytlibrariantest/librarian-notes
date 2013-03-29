Title: Learning librarian p2 - Environment
Date: 2013-03-29

I created the following repositories:

Note taking for this learning:

 - https://github.com/ytlibrariantest/librarian-notes

Modules:

 - https://github.com/ytlibrariantest/shared-motd
 - https://github.com/ytlibrariantest/client3-filemaker
 - https://github.com/ytlibrariantest/shared-filemaker

A vagrant testing environment:

- https://github.com/ytlibrariantest/vagrants

Each client's dev, stage, and production environment:

 - https://github.com/ytlibrariantest/client1prod
 - https://github.com/ytlibrariantest/client1stage
 - https://github.com/ytlibrariantest/client1dev
 - https://github.com/ytlibrariantest/client2prod
 - https://github.com/ytlibrariantest/client2stage
 - https://github.com/ytlibrariantest/client2dev
 - https://github.com/ytlibrariantest/client3prod
 - https://github.com/ytlibrariantest/client3stage
 - https://github.com/ytlibrariantest/client3dev


Lets clone these all out. Ignore my 'ytl.github', I do this as part of a
way of having multiple github accounts and ssh keys. It won't affect your 
pull.

		git clone  git@ytl.github.com:ytlibrariantest/librarian-notes
		git clone  git@ytl.github.com:ytlibrariantest/shared-motd
		git clone  git@ytl.github.com:ytlibrariantest/client3-filemaker
		git clone  git@ytl.github.com:ytlibrariantest/shared-filemaker
		git clone  git@ytl.github.com:ytlibrariantest/client1prod
		git clone  git@ytl.github.com:ytlibrariantest/client1stage
		git clone  git@ytl.github.com:ytlibrariantest/client1dev
		git clone  git@ytl.github.com:ytlibrariantest/client2prod
		git clone  git@ytl.github.com:ytlibrariantest/client2stage
		git clone  git@ytl.github.com:ytlibrariantest/client2dev
		git clone  git@ytl.github.com:ytlibrariantest/client3prod
		git clone  git@ytl.github.com:ytlibrariantest/client3stage
		git clone  git@ytl.github.com:ytlibrariantest/client3dev
		git clone  git@ytl.github.com:ytlibrariantest/vagrants

Now, I want to install librarian-puppet, puppet, and setup my Vagrant directory.

    sudo gem install librarian-puppet
    sudo gem install puppet
    cd vagrants
    librarian-puppet init

The init will create a few directories and add some to .gitignore. It also creates
a sample Puppetfile that does nothing. If we open that file and uncomment the ntp
section, we can get it to install ntp and do some testing.

    jth21@macpro13:~/devel/librarianfun/vagrants$ librarian-puppet install
    jth21@macpro13:~/devel/librarianfun/vagrants$ ls modules/ntp/
		CHANGELOG	Gemfile		Modulefile	Rakefile	manifests	templates
		CONTRIBUTING.md	LICENSE		README.markdown	files		spec		tests  
		jth21@macpro13:~/devel/librarianfun/vagrants$ touch modules/ntp/rogue
		jth21@macpro13:~/devel/librarianfun/vagrants$ ls modules/ntp/
		CHANGELOG	Gemfile		Modulefile	Rakefile	manifests	spec		tests
		CONTRIBUTING.md	LICENSE		README.markdown	files		rogue		templates
		jth21@macpro13:~/devel/librarianfun/vagrants$ librarian-puppet install --verbose
		[Librarian] Ruby Version: 1.8.7
		[Librarian] Ruby Platform: universal-darwin12.0
		[Librarian] Rubygems Version: 1.3.6
		[Librarian] Librarian Version: 0.0.24
		[Librarian] Librarian Adapter: puppet
		[Librarian] Project: /Users/jth21/devel/librarianfun/vagrants
		[Librarian] Specfile: Puppetfile
		[Librarian] Lockfile: Puppetfile.lock
		[Librarian] Git: /usr/bin/git
		[Librarian] Git Version: git version 1.7.12.4 (Apple Git-37)
		[Librarian] Git Environment Variables:
		[Librarian]   (empty)
		[Librarian] Pre-Cached Sources:
		[Librarian]   [:forge, "http://forge.puppetlabs.com", {}]
		[Librarian]   [:git, "git://github.com/puppetlabs/puppetlabs-ntp.git", {}]
		[Librarian] Post-Cached Sources:
		[Librarian]   [:forge, "http://forge.puppetlabs.com", {}]
		[Librarian]   [:git, "git://github.com/puppetlabs/puppetlabs-ntp.git", {}]
		[Librarian] The specfile is unchanged: nothing to do.
		[Librarian] Pre-Cached Sources:
		[Librarian] Post-Cached Sources:
		[Librarian]   [:forge, "http://forge.puppetlabs.com", {}]
		[Librarian]   [:git, "git://github.com/puppetlabs/puppetlabs-ntp.git", {}]
		[Librarian] Installing ntp/0.2.0 <git://github.com/puppetlabs/puppetlabs-ntp.git#master>
		[Librarian] Running `/usr/bin/git reset --hard --quiet` in .tmp/librarian/cache/source/git/66076c171c9e7acc6a0341347cc866b2
		[Librarian]     --- No output
		[Librarian] Running `/usr/bin/git clean -x -d --force --force` in .tmp/librarian/cache/source/git/66076c171c9e7acc6a0341347cc866b2
		[Librarian]     --- No output
		[Librarian] Running `/usr/bin/git rev-parse HEAD --quiet` in .tmp/librarian/cache/source/git/66076c171c9e7acc6a0341347cc866b2
		[Librarian]     --> b1b3132f84d1628800e50a65f6c49bb998872acc
		[Librarian] Deleting modules/ntp
		[Librarian] Copying .tmp/librarian/cache/source/git/66076c171c9e7acc6a0341347cc866b2 to modules/ntp
		jth21@macpro13:~/devel/librarianfun/vagrants$ ls modules/ntp/
		CHANGELOG	Gemfile		Modulefile	Rakefile	manifests	templates
		CONTRIBUTING.md	LICENSE		README.markdown	files		spec		tests    

**That answers one of my first questions. Running the librarian-puppet install (it does the same with update)
will indeed overwrite any local changes I make. So in my workflows, I probably do NOT want to have 'vagrant provision' run librarian-puppet.**






