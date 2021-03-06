# A Behat+Mink Demo

## Mink

Mink is a browser emulators abstraction layer.

It defines a basic API through which you can talk with specific browser emulator libraries.

Mink drivers define a bridge between Mink and those libraries.

Read [this article](http://www.knplabs.com/en/blog/one-mink-to-rule-them-all) to know more about Mink.

**This repository will allow you to easily try Mink and Behat to test… wikipedia.org!**

## Installation

### Requirements:

You need a valid PHPUnit 3.5 installation:

``` bash
pear channel-discover pear.phpunit.de
pear channel-discover components.ez.no
pear channel-discover pear.symfony-project.com

pear install phpunit/PHPUnit
```

Behat doesn't care what you use to validate your steps. But Mink uses PHPUnit assertions internally!

## Usage 

Clone this repo:

``` bash
git clone https://github.com/knplabs/mink-demo
```

Launch Behat: the two first scenarios should use Goutte.
The third one checks that the JS autocomplete field works on wikipedia: it uses Sahi!
but lets ignore it for a quick start with `--tags` filter:

``` bash
php behat-2.0.1.phar --tags ~@javascript
```

You should see an output like:

``` gherkin
Feature: Search
  In order to see a word definition
  As a website user
  I need to be able to search for a word

  Scenario: Searching for a page that does exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Behavior Driven Development"
    And I press "searchButton"
    Then I should see "agile software development"

  Scenario: Searching for a page that does NOT exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Glory Driven Development"
    And I press "searchButton"
    Then I should see "Search results"

3 scenarios (3 passed)
12 steps (12 passed)
0m8.517s
```

### Sahi

If you want to test `@javascript` part of feature, you'll need to install Sahi.
Sahi gives you ability to run `@javascript` tagged scenarios in real browser.

1. Download and run the Sahi jar from the [http://sahi.co.in/w/](Sahi website)
2. Run sahi proxy before your test suites (you can start this proxy during system startup):

    ``` bash
    cd $YOUR_PATH_TO_SAHI/bin
    ./sahi.sh
    ```

Now if you run:

``` bash
php behat-2.0.1.phar
```

you should see an output like:

``` gherkin
Feature: Search
  In order to see a word definition
  As a website user
  I need to be able to search for a word

  Scenario: Searching for a page that does exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Behavior Driven Development"
    And I press "searchButton"
    Then I should see "agile software development"

  Scenario: Searching for a page that does NOT exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Glory Driven Development"
    And I press "searchButton"
    Then I should see "Search results"

  @javascript
  Scenario: Searching for a page with autocompletion
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Behavior Driv"
    And I wait for the suggestion box to appear
    Then I should see "Behavior Driven Development"

3 scenarios (3 passed)
12 steps (12 passed)
0m8.517s
```

