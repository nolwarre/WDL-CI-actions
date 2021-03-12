[![HelloWDLBadge][hello-WDL-badge]][hello]
[![HelloCWLBadge][hello-CWL-badge]][hello]

[hello-WDL-badge]: https://github.com/nolwarre/WDL-CI-actions/actions/workflows/wdl.yml/badge.svg
[hello]: https://github.com/nolwarre/WDL-CI-actions/actions?query=workflow%3AHelloWorldWDL

[hello-CWL-badge]: https://github.com/nolwarre/WDL-CI-actions/actions/workflows/cwl.yml/badge.svg
[hello]: https://github.com/nolwarre/WDL-CI-actions/actions?query=workflow%3AHelloWorldCWL
# Creating Testing Badges for Workflows
## Badge Creation
Badges are a common markdown tool used in README and are used to display the status of a given service. This is often used to show that a repository is up to date and working as expected.

They follow the general form of `[![BadgeName](badge.svg)](status)` \
This is what a test badge looks like [![Awesome Badges](https://img.shields.io/badge/badges-awesome-green.svg)](https://github.com/Naereen/badges) 

The BadgeName can be anything that describes what the badge is used for.

The badge.svg is the path which contains the image for the badge, when using github actions this points to the .github/workflows/badge.svg

The status is the important part that contains the api call for the github action which will provide the status to present on the badge.

## Setting up the GitHub Action

GitHub actions are made from yml files that are located in the .github/workflows of the repository that the action is resposible for.


