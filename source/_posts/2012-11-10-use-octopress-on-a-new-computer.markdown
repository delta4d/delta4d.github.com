---
layout: post
title: "use octopress on a new computer"
date: 2012-11-10 19:37
comments: true
categories: [octopress,git]
---

when you are bloging with octopress, don't forget the following commands
	git add .
	git commit -m 'ur commit'
	git push origin source

then when you are on another computer, trying to setup a bloging enviroment with octopress, because 'rake deploy' upload the generated files, so it needs a little work, you can go like this:
	$ git clone git@github.com:username/username.github.com.git
	$ cd username.github.com
	username.github.com$ git checkout source
	username.github.com$ mkdir _deploy
	username.github.com$ cd _deploy
	username.github.com/_deploy$ git init
	username.github.com/_deploy$ git remote add origin git@github.com:username/username.github.com.git
	usename.github.com/_deploy$ git pull origin master
	usename.github.com/_deploy$ cd ..

reference:
[dblock.org](http://code.dblock.org/octopress-setting-up-a-blog-and-contributing-to-an-existing-one)
