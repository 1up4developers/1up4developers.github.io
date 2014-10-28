---
author: andrefarina
comments: true
date: 2011-04-25 11:27:29+00:00
layout: post
slug: throw-legacy-part-i-continuous-testing-with-java
title: Throw Legacy - Part I - Continuous testing with Java
wordpress_id: 967
categories:
- web
---

This is my first blog post and I decide to write in English because writing is a good way to learning and I’d like to improve my English, so post a comment if you see some grammar errors. 8)

JRuby has helped me a lot bringing Ruby test “culture” into Java with RSpec.
In this series of blog posts I will show how I’m doing tests with Java legacy code. I will try cover topics including testing dao, service, controller and view layers.
In this part, we’re going to set up our development environment, write a RSpec example and implement with Java through JRuby. We’ll also get continuous testing running with infinity test gem.

The first thing we need is install [rvm (Ruby Version Manager)](https://rvm.beginrescueend.com/), [JRuby](http://www.jruby.org/) and [Ruby Gems](http://rubygems.org/).

    
    bash << ( curl http://rvm.beginrescueend.com/releases/rvm-install-head )


[http://rvm.beginrescueend.com/rvm/install/](http://rvm.beginrescueend.com/rvm/install/) for more info.

Install JRuby:

    
    rvm install jruby


Set rvm to use JRuby:

    
    rvm use jruby


Install RSpec:

    
    gem install rspec


Once you have finish installation let’s start code with a example that expresses a form of payment orders available in Brazil called Boleto.

Create the initial project structure like this:

    
    -boleto
      -spec
      -src
        -com
          -legacy
      -build


Open up **spec/boleto_spec.rb** and make it look like this:

    
    require "date"
    require "java"
    java_import "com.legacy.Boleto"
    
    describe Boleto do
      it "calculates due days" do
        boleto = Boleto.new
        boleto.due_date = java.util.Date.new(Date.parse("2011-01-15").ctime)
        boleto.payment_date = java.util.Date.new(Date.parse("2011-01-25").ctime)
        boleto.due_days.should == 10
      end
    end


Let's run the example and watch it fail.

    
    jruby -J-cp build -S rspec spec/boleto_spec.rb



    
    NameError: cannot load Java class com.legacy.Boleto


Now write just enough code to make it pass on **src/com/legacy/Boleto.java** like this:

    
    package com.legacy;
    import java.util.Date;
    public class Boleto {
      private Date dueDate;
      private Date paymentDate;
    
      public Date getDueDate() { return this.dueDate; }
      public void setDueDate(Date dueDate) { this.dueDate = dueDate; }
      public Date getPaymentDate() { return this.paymentDate; }
      public void setPaymentDate(Date paymentDate) { this.paymentDate = paymentDate; }
      public Integer getDueDays() { return 10; }
    }


Also we need to compile the java class.

    
    javac -d build/ src/com/legacy/Boleto.java


Instead of run the example by manual command again, let’s install a continuous testing gem:

    
    wget --no-check-certificate\
     http://www.github.com/tomas-stefano/infinity_test/tarball/master\
     -O infinity_test.tar.gz
    tar -zxvf infinity_test.tar.gz
    cd tomas-stefano-infinity_test-{version}
    gem build infinity_test.gemspec
    gem install infinity_test-1.0.2.gem


What we have done here is install infinity_test gem that contains a path to pass arguments to specific versions of Ruby.

Let's run the example with continuous testing gem:

    
    infinity_test --rspec --rubies=jruby+"-J-cp build"


Now we can write a test for code on **spec/boleto_spec.rb** and Infinity Test automatic runs the test.
Also we can create a .infinity_test file, like this:

    
    infinity_test do
      use :rubies => %w(jruby),
        :specific_options => {'jruby' => '-J-cp build'},
        :test_framework => :rspec
      notifications :lib_notify   # for linux / growl for mac
    end


And run:

    
    infinity_test


![infinity_test jruby](/images/uploads/2011/04/infinity_test.png)
[https://github.com/tomas-stefano/infinity_test/wiki/Customize-Infinity-Test](https://github.com/tomas-stefano/infinity_test/wiki/Customize-Infinity-Test) for more info.

However, we have to compile the java classes when they change. The solution we came up with is to use editors like [eclipse](http://www.eclipse.org/) or [eclim](http://eclim.org/). This kind of editor auto-recompile Java files whenever they change.

Last notes



	
  * I've posted the code for boleto here [https://github.com/andref5/rspec-java](https://github.com/andref5/rspec-java)

	
  * More info about JRuby

	
    * [https://github.com/jruby/jruby/wiki](https://github.com/jruby/jruby/wiki)

	
    * [http://pragprog.com/titles/jruby/using-jruby](http://pragprog.com/titles/jruby/using-jruby)




	
  * More info about RSpec

	
    * [http://relishapp.com/rspec](http://relishapp.com/rspec)

	
    * [http://kerryb.github.com/iprug-rspec-presentation](http://kerryb.github.com/iprug-rspec-presentation)

	
    * [http://pragprog.com/titles/achbd/the-rspec-book](http://pragprog.com/titles/achbd/the-rspec-book)




	
  * More info about Infinity_test

	
    * [https://github.com/tomas-stefano/infinity_test](https://github.com/tomas-stefano/infinity_test)




	
  * More info about Ruby

	
    * [http://mislav.uniqpath.com/poignant-guide/book/](http://mislav.uniqpath.com/poignant-guide/book/)





