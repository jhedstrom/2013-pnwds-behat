# Quality Assurance using Behat and the Drupal Extension

This session will cover Behavior Driven Development (BDD) using
[Behat](http://behat.org) and the
[Drupal Extension](http://drupal.org/project/drupalextension) to test
Drupal sites. We will start with the basics behind BDD, cover what is
possible now using the Drupal Extension, and finish with a roadmap for
the future of the extension, as well as efforts to move much of the
core testing framework to Behat in Drupal 9.

## Behavioral Driven Development

With BDD, you write human-readable stories that describe the behavior
of your application. These stories can then be auto-tested against
your application.

## Testing Drupal sites, what is currently possible

The Drupal Extension extends Behat to provide **step-definitions** for
common tasks that are specific to Drupal. Examples of this include:

 * Creating nodes
 * Creating users
 * Logging into a site as a user with a given role or permission
 * Creating taxonomy terms

An example

    Scenario: Target links within table rows
    Given I am logged in as a user with the "administrator" role
    When I am at "admin/structure/types"
    And I click "manage fields" in the "Article" row
    Then I should be on "admin/structure/types/manage/article/fields"
    And I should see text matching "Add new field"

## Drupal 9

The current code to test if node form **buttons** are working are not
can be seen
[here](http://drupalcode.org/project/drupal.git/blob/refs/heads/8.x:/core/modules/node/lib/Drupal/node/Tests/NodeFormButtonsTest.php#l41). It
is long, complex, and understood only by the people that wrote the
test...barely.

The following is a start at converting the above test to Behat:

    Given I am logged in as a user with the "administer nodes, bypass node access" permissions
    When I visit "node/add/article"
    And fill in "Title" with "A sample node title"
    And press "Save and publish"
    Then I should see the heading "A sample node title"
