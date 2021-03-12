[![HelloWDLBadge][hello-WDL-badge]][hello]
[![HelloCWLBadge][hello-CWL-badge]][hello]

[hello-WDL-badge]: https://github.com/nolwarre/WDL-CI-actions/actions/workflows/wdl.yml/badge.svg
[hello]: https://github.com/nolwarre/WDL-CI-actions/actions?query=workflow%3AHelloWorldWDL

[hello-CWL-badge]: https://github.com/nolwarre/WDL-CI-actions/actions/workflows/cwl.yml/badge.svg
[hello]: https://github.com/nolwarre/WDL-CI-actions/actions?query=workflow%3AHelloWorldCWL
# Creating Testing Badges for Workflows
#### Table Of Contents
- [Badge Creation](#badge-creation)  
- [GitHub Actions](#setting-up-the-github-action)  
    - [GitHub Actions for WDL](#wdl-specific-actions)
    - [GitHub Actions for CWL](#cwl-specific-actions)

## Badge Creation
Badges are a common markdown tool used in README and are used to display the status of a given service. This is often used to show that a repository is up to date and working as expected.

They follow the general form of `[![BadgeName](badge.svg)](status)` \
This is what a test badge looks like [![Awesome Badges](https://img.shields.io/badge/badges-awesome-green.svg)](https://github.com/Naereen/badges) 

The BadgeName can be anything that describes what the badge is used for.

The badge.svg is the path which contains the image for the badge, when using github actions this points to the .github/workflows/badge.svg

The status is the important part that contains the api call for the github action which will provide the status to present on the badge.

## Setting up the GitHub Action

GitHub actions are made from YAML files that are located in the .github/workflows of the repository that the action is responsible for. It is a set of functions that can execute when pushed that will execute to verify the commit is valid. To learn more about the syntax, read the GitHub documentation on GitHub Actions.

### WDL specific actions
The example GitHub Actions for WDL are located in `.github/Workflows/wdl.yml`. To set up GitHub Actions for WDL, use Cromwell and Womtoll in order to validate the workflow and run the workflow on test data.

Both Cromwell and Womtool use Java in order to execute. GitHub has actions that are located on the marketplace to setup features, one of which is Java.
By calling the GitHub actions in the steps and specifying a java version to be setup the action will include Java. This is done with the syntax.
```   
    - name: Setup Java
      uses: actions/setup-java@v1
      with:
        java-version: '8'
```
Once Java has been setup in the action, it is time to install Cromwell and Womtool with the `run` command.
The `run` command is a shell that can be used to run commands.
```
    - name: Install Cromwell and Womtool
      run: |
          wget https://github.com/broadinstitute/cromwell/releases/download/58/cromwell-58.jar
          wget https://github.com/broadinstitute/cromwell/releases/download/58/womtool-58.jar
```
This installs both Cromwell and Womtool which can be executed with Java for the wdl. In order to verify that both Cromwell and Womtool were installed correctly, execute both and call `--version`
```
   - name: Verify Cromwell and Womtool
      run: |
          java -jar cromwell-58.jar --version
          java -jar womtool-58.jar --version
```
Womtool can be used to verify that the syntax of the WDL is correct.
```
    - name: Validate wdl with Womtool
      run: java -jar womtool-58.jar validate wdl/HelloWorld.wdl
```
Cromwell is used to execute the WDL workflow with examples inputs, use small datasets so that the actions don't take a long time to verify the commit.
```
    - name: Run wdl with Cromwell
      run: java -jar cromwell-58.jar run wdl/HelloWorld.wdl -i wdl/Hello.json
```

### CWL specific actions
The example GitHub Actions for CWL are located in `.github/Workflows/cwl.yml`. CWL can be executed using cwltool with test inputs. cwltool can be installed with apt-get since it is an ubuntu environment.

Python can be installed and executed with the prewritten action by GitHub.
```
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v1
      with:
        python-version: ${{ matrix.python-version }}
```
Using the `run` command, cwltool is installed with apt-get
```
    - name: Install dependencies
      run: sudo apt-get install cwltool
```
Verify that cwltool was installed correctly by finding the verison.
```
    - name: Verify cwltool
      run: cwltool --version
```
Finally, execute the cwl workflow using cwltool and the test inputs.
```
    - name: Test with cwltool
      run: cwltool cwl/hello.cwl cwl/hello.json
```
