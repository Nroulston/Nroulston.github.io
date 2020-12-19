---
layout: post
title:      "So you want to learn BDD with RSpec"
date:       2020-12-19 19:00:05 +0000
permalink:  so_you_want_to_learn_bdd_with_rspec
---


If you have been listening you probably have heard the acronyms TDD(Test Driven Development) and BDD(Behaviour Driven Development) thrown around. For the longest time I thought TDD was all about writing tests that drove how you code, and BDD was asserting some kind of behavior as a goal and then writing all of the neccesary code to achieve that goal. **Boy was I wrong** BDD is an evolution of TDD. The docs explain it more succinctly than I. 

> 
> RSpec is a Behaviour-Driven Development tool for Ruby programmers. BDD is an approach
> to software development that combines Test-Driven Development, Domain Driven Design,
> and Acceptance Test-Driven Planning. RSpec helps you do the TDD part of that equation,
> focusing on the documentation and design aspects of TDD.
> 
*https://relishapp.com/rspec/*

What the docs don't tell you is that BDD is the methodology of taking a high level concept and using testing to drive your development from what is it supposed to do, to how it does it at a detailed level. 


*Disclaimer all the code on this blog is derived from my journey through Effective testing with RSpec 3 I highly reccomend it.*
## Quick primer on RSpec syntax
```
RSpec.describe 'An ideal sandwich' do  ## This is called an example group it defines what you are testing
 
 it 'is delicious' do ##creates an example/instance/individual test
    sandwich =Sandwich.new('delcicious', [])

    taste = sandwich.taste
    expect(taste).to eq('delicious')  ##asserts/verifies expected outcome
 
  end
end
```
Every test you write will have this same basic layout. From this structure you drive your development via testing. 

## Behavior Driven Development with RSpec

What is your codes behavior? By writing expressive tests you can easily document what your code is supposed to do. Once you a test is written that succinctly expresses exactly the expected behavior you can code a succint code base to pass that test. RSpec is designed to help you express with the right words specificaly what you mean in your tests. 

One of the biggests conceptual differences between strict TDD and BDD is that you work from an outside-in approach. The first thing you write is acceptance tests, and you are then driven by testing down to unit tests. 

A huge takeaway from all of this is learning how to think about your problem, and breaking it down into bitsized behaviours. 

TBH I am just delving into the world of thinking via the BDD paradigm. You can check out my progression through Effective testing with RSpec 3 at my [repo](https://github.com/Nroulston/learn_rspec)









