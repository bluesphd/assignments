homework-8.1.md
//Pros and Cons of Testing
//Excerpts from a couple of postings that favor testing of code  … often, in small pieces, and with zero tolerance when the programs that  sustain life
1.  A recommendation to start test thinking is in the design phase, and a call to spend more time teaching test design in computer programming classes ( seems like good advice)
http missing
  “Reducing the pain of testing requires several things of which having good code is just one. It’s one important piece of course but not the whole piece. Having good, solid testing data with known expectations of results is a piece. 
  So is testing small pieces of code so that fewer things can go wrong. The fewer the places for things to go badly the easier it is to find and repair them. By coincidence (not) breaking ones code into smaller, 
  more manageable and testable pieces almost invariably results in better code. Sections of code that are too long and that try to do too many things in one module are the huge run-on sentences of programming. 
  In natural language writing run-on sentences are a “bad thing” and make understanding difficult for the reader. Modules of code with too many lines make testing and understanding difficult. They also make modification, expansion, reuse, 
  maintenance and all sorts of other things difficult if not impossible.” ……. Comment:” There's a huge amount of data that shows that in industrial software development, putting off validation and verification -- including testing -- 
  until all the code is written can cause a project to take 5 or ten times as long to complete than if it's done at every step, starting with requirements elicitation.”
2. A call for the testing, with zero tolerance of error, of code residing in systems that sustain life (can’t argue with this)
https://communities.coverity.com/blogs/development-testing-blog/2013/03/06/fatal-errors-dive-computers-required-clean-code
  “There are a number of commercial solutions available that can help software developers find and fix problems with their code. We are not in the coding dark ages and consequently, I think the release of code that can kill is as dire as 
  the release of cars or planes that have physical defects that can result in a catastrophic accident. The dive computer is a great example of a series of software controlled, mathematical algorithms, which conveys information 
  that is critical to the survival of the end user. That software should be 100% rock solid. It needs to be tested and it needs to be tested to the point of total assurance.”

//Excerpts from a postings that point out that unit testing is not always necessary or effective 
1. A proposal that the benefit of unit testing is correlated with the non-obviousness of the code under test, and with the number of dependencies (concrete or interface) that a code unit has (basically an argument against 
overtesting obvious and trivial code with little need for integration testing, which drives up the cost of programming)
http://blog.stevensanderson.com/2009/11/04/selective-unit-testing-costs-and-benefits/
  On Unit Testing
  “… if your code is not obvious at a single glance … then additional design and verification assistance … through unit testing…is needed”  “… if your code is basically obvious … then additional design and verification …  
  through unit testing  yields extremely minimal benefit, if any.” 
On Itegration Testing
  “Trivial code with many dependencies is not a candidate for unit testing … it’s expensive to do and yields little practical benefit.”  “Trival code with few dependencies … it doesn’t matter whether you unit test it or not.”
2. Excerpts form a WIKIPEDIA article on Test-Driven Development that list some shortcomings of the approach (which boil done to the idea that over testing consumes time to write and maintain the tests at substantial costs)
http://en.wikipedia.org/wiki/Test-driven_development
  •	Unit tests created in a test-driven development environment are typically created by the developer who is writing the code being tested. The tests may therefore share the same blind spots with the code
  •	A high number of passing unit tests may bring a false sense of security, resulting in fewer additional software testing activities, such as integration testing and compliance testing.

//Journal Article
http://dl.acm.org/citation.cfm?id=1380664
  Those that favor testing often and in small units fit well in the camp that espouses test-driven development (TDD).  The amount of time taken to finish a project using TDD is greater for a project that uses this approach than one that does not; 
  clearly there are costs associated with TDD.  Here is an abstract of a research paper from the Journal of Empirical Software Engineering that finds the extra costs may well be worth it. 

    Test-driven development (TDD) is a software development practice that has been used sporadically for decades. With this practice, a software engineer cycles minute-by-minute between writing failing unit tests and writing implementation 
    code to pass those tests. Test-driven development has recently re-emerged as a critical enabling practice of agile software development methodologies. However, little empirical evidence supports or refutes the utility of this practice in 
    an industrial context. Case studies were conducted with three development teams at Microsoft and one at IBM that have adopted TDD. The results of the case studies indicate that the pre-release defect density of the four products decreased 
    between 40% and 90% relative to similar projects that did not use the TDD practice. Subjectively, the teams experienced a 15---35% increase in initial development time after adopting TDD.

//Links to Testing Frameworks for PHP

http://www.phptherightway.com/
  PHPUnit is the de-facto testing framework for writing unit tests for PHP applications, but there are several alternatives:
  SimpleTest
  Enhance PHP
  PUnit
  Atoum
  Many of the same tools that can be used for unit testing can be used for integration testing as many of the same principles are used.

http://net.tutsplus.com/tutorials/php/testing-your-php-codebase-with-enhancephp/
  Tutorial on Testing your PHP Codebase with EnhancePHP
    Writing Tests
    Running Tests
    Getting the Tests to Pass
    Writing More Tests

http://www.yiiframework.com/doc/guide/1.1/en/test.overview
  Developers can write some code to automate a testing process so that each time when we need to test something, we just need to call up the code and let the computer to perform testing for us. 
  This is known as automated testing, which is the main topic of this chapter.  The testing support provided by Yii includes unit testing and functional testing.

http://cakephp.org/
  CakePHP is an open source web application framework. It follows the Model-View-Controller (MVC) approach and is written in PHP, modeled after the concepts of Ruby on Rails, and distributed under the MIT License.  It uses well-known software 
  engineering concepts and software design patterns, as Convention over configuration, Model-View-Controller, ActiveRecord, Association Data Mapping, and Front Controller.
  No complicated XML or YAML files. Just setup your database and you're ready to bake.  The things you need are built-in.	 Translations, database access, caching, validation, authentication, and much more are all built into one of the original 
  PHP MVC frameworks.

  http://www.simpletest.org/
  The SimpleTest PHP unit tester is available for download.  It is a PHP unit test and web test framework. The JWebUnit style functionality is more complete now. It has support for SSL, forms, frames, proxies and basic authentication. 
  The idea is that common but fiddly PHP tasks, such as logging into a site, can be tested easily.
