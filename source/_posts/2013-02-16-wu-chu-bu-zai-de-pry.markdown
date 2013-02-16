---
layout: post
title: "无处不在的pry"
date: 2013-02-16 23:45
comments: true
categories: 
---



http://lucapette.com/pry/pry-everywhere/

# Pry Everywhere

about a year ago in pry
I have to confess that I’m generally skeptical about alternatives for tools that work so fine like IRB. And I really like IRB. So why Pry? And why everywhere? Because Pry features blew my mind.
When I wrote about IRB customization, I was doing what I always do when I like a tool: customize it in order to feel the tools more familiar with my way of thinking. So, when I run into Pry, some of its features blew my mind because they were exactly what I really would like to have in IRB.
For example, sometimes you would like to explore a class or an object quickly. The way you can do it with Pry feels just natural. It just seems the right way too do it:

	1.9.2 (main):0 > cd Array
	1.9.2 (Array):1 > ls -m
	[:[], :allocate, :new, :superclass, :toy, :try_convert, :yaml_tag]
	1.9.2 (Array):1 > show-
	show-command  show-doc      show-input    show-method   show-source
	1.9.2 (Array):1 > show-method toy

	From: /home/lucapette/.pryrc @ line 15:
	Number of lines: 3

	def self.toy(n=10, &block)
	  block_given? ? Array.new(n,&block) : Array.new(n) {|i| i+1}
	end

Toy is a little method I have in my .pryrc (and previously in my .irbrc) that I use when I want to play with arrays. Pry comes with wonderful commands like cd that operates both with classes and instance objects or like ls that you can use to list all the class methods (-m option) or instance methods (-M option). And a very long list of other terrific features as editor integration, shell integration or gist integration. But I don’t need to persuade you to use Pry because I’m sure you will use it after taking a look at these wonderful resources.

So the title of this post is “Pry everywhere” and now I’m going to show you quickly what I’ve done to migrate to Pry. There are a lot of solutions about this topic but all of them involve something I don’t like especially about rails integration. My requirements were fairly simple:

- I don’t want to lose the customizations I’ve done with IRB
- The same for rails console
- I don’t want to add any gem (although this is very nicely done) to my Gemfile in rails projects.

After a bit of researching, I came up with the following solution:

My current .irbrc:

	# https://github.com/carlhuda/bundler/issues/183#issuecomment-1149953
	if defined?(::Bundler)
	  global_gemset = ENV['GEM_PATH'].split(':').grep(/ruby.*@global/).first
	  if global_gemset
	    all_global_gem_paths = Dir.glob("#{global_gemset}/gems/*")
	    all_global_gem_paths.each do |p|
	      gem_path = "#{p}/lib"
	      $LOAD_PATH << gem_path
	    end
	  end
	end
	# Use Pry everywhere
	require "rubygems"
	require 'pry'
	Pry.start
	exit

Pratically, everytime I start IRB I will start a Pry session. It feels like a dirty solutions and I have to confess I don’t know if it has any issues. For now, it’s working just fine with my requirements. The bundler code is necessary to require pry and other gems from rvm global gemset in a rails console without declaring them in the Gemfile. Then, in the .pryrc I have:

	# vim FTW
	Pry.config.editor = "gvim --nofork"

	# My pry is polite
	Pry.hooks = { :after_session => proc { puts "bye-bye" } }

	# Prompt with ruby version
	Pry.prompt = [proc { |obj, nest_level| "#{RUBY_VERSION} (#{obj}):#{nest_level} > " }, proc { |obj, nest_level| "#{RUBY_VERSION} (#{obj}):#{nest_level} * " }]

	%w{map_by_method hirb}.each { |gem| require gem }

	# Toys methods
	# Stealed from https://gist.github.com/807492
	class Array
	  def self.toy(n=10, &block)
	    block_given? ? Array.new(n,&block) : Array.new(n) {|i| i+1}
	  end
	end

	class Hash
	  def self.toy(n=10)
	    Hash[Array.toy(n).zip(Array.toy(n){|c| (96+(c+1)).chr})]
	  end
	end

	# loading rails configuration if it is running as a rails console
	load File.dirname(__FILE__) + '/.railsrc' if defined?(Rails) && Rails.env

If you compare this file with my previous .irbrc you’ll notice that this one is shorter. This means that Pry is doing a piece of the job I would like to have by default, like colors and history commands. My .railsrc is very similar to the previous one but it has a different that could interest you if you are an hirb user:

	# https://github.com/cldwalker/hirb/issues/46#issuecomment-1870823
	Pry.config.print = proc do |output, value|
	  Hirb::View.view_or_page_output(value) || Pry::DEFAULT_PRINT.call(output, value)
	end

	Hirb.enable

In this way, Hirb is working flawlessly. And the combination of Rails and Pry is just fantastic. Give it a try. I have been able to migrate to Pry with a fair effort, hoping this kind of configuration can help you too!