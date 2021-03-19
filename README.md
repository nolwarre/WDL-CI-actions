[![HelloWDLBadge][hello-WDL-badge]][hello]
[![HelloCWLBadge][hello-CWL-badge]][hello]

[hello-WDL-badge]: https://github.com/nolwarre/WDL-CI-actions/actions/workflows/wdl.yml/badge.svg
[hello]: https://github.com/nolwarre/WDL-CI-actions/actions?query=workflow%3AHelloWorldWDL

[hello-CWL-badge]: https://github.com/nolwarre/WDL-CI-actions/actions/workflows/cwl.yml/badge.svg
[hello]: https://github.com/nolwarre/WDL-CI-actions/actions?query=workflow%3AHelloWorldCWL
# Creating Testing Badges for Workflows
<details>
<summary>Table of content</summary>

#### Table Of Contents
- [Badge Creation](#badge-creation)  
- [GitHub Actions](#setting-up-the-github-action)
    - [GitHub Actions for WDL](#wdl-specific-actions)
    - [GitHub Actions for CWL](#cwl-specific-actions)
</details>

## Badge Creation
Badges are a common markdown tool used in README and are used to display the status of a given service. This is often used to show that a repository is up to date and working as expected. Badges can also be used to display any information you want to format.

They follow the general form of `[![BadgeName](badge.svg)](link)` \
This is what a test badge looks like [![Awesome Badges](https://img.shields.io/badge/badges-awesome-green.svg)](https://github.com/Naereen/badges)

The BadgeName can be anything that describes what the badge is used for.

The badge.svg is the path which contains the source for the badge, when using GitHub actions this points to the `.github/workflows/name/badge.svg`. This creates a badge for a specific action name and will display the status of the action.

The link allows people to click on the badge to find what it is referring to. For GitHub actions this points to `https://github.com/user/repo/actions?query=workflow%3ANameofAction`

## Setting up the GitHub Action

GitHub Actions are automation of tasks that will occur when an event occurs. In this case the GitHub Action will execute a GitHub workflow. To learn more about GitHub Actions, [read the GitHub documentation](https://docs.github.com/en/actions/quickstart). \
GitHub workflows are made from YAML files that are located in the .github/workflows of the repository that the action is responsible for. It is a set of functions that can execute when specified that can verify the integrity of the code. To learn more about the syntax, read the [GitHub documentation on GitHub Actions](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions).

Each workflow has keywords that are used in the yaml to specify properties of the workflow. Some of the most important ones are
[`name`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#name),
[`on`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#on), and
[`jobs`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobs). Click on keywords to get taken to the relevant documentation.
- [`name`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#name): Included in the top of the workflow and is used to indicate the name of the workflow in order to reference.
- [`on`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#on): Used to specify the events that will trigger the workflow to run. The most common events that are used to trigger the workflow are [`push`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#push),
[`pull_request`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#pull_request),
[`workflow_run`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_run),
and [`workflow_dispatch`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_dispatch)
which is used allows you to manually run the workflow. With each of these events, the event can be customized to include specific branches or times as defined [here](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).
- [`jobs`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobs): This is what defines what will happen when the workflow is ran. The values that are expected will be the actual job id. The job id is a custom string that is unique for that specific job. All values of the job id will be the workflow. Values for the jobs keywords are important since it defines what the workflow will do. The common jobs keywords are [`name`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idname),
[`runs-on`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on),
and [`steps`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps).
 - [`runs-on`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on):
 This is the type of device that the machine will run jobs on. For a list of jobs [look here](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on).
 - [`steps`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps):
 The crucial part of the workflow since these are the sequence of tasks that will be used to run commands. There is a seperate section for more information on [`steps`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps) [here](#github-jobs-steps)

Example:
 ```
 jobs:
   job_id:
     name: Test Job
     runs-on: ubuntu-latest
     steps:
       - name: Hello!
         run: echo Hello!

 ```
#### GitHub Jobs Steps
[`steps`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps) are a sequence of tasks that will be run for an action. Each step is separated by a `-` and can contain different keywords depending on how deep you want to step to be. For a particular step in steps, it can contain a [`name`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsname)
as well as a [`run`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsrun)
command. With just these two keywords, GitHub actions can be created that will be very powerful. The [`run`](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsrun) command will use an operating system's shell to run command-line instruction that is specified in the command. This can be used to install dependencies as well as execute the validation of your commit.

### WDL specific actions
The example GitHub Actions for WDL are located in `.github/Workflows/wdl.yml`. To set up GitHub Actions for WDL, use Cromwell and Womtoll in order to validate the workflow and run the workflow on test data.

Both Cromwell and Womtool use Java in order to execute. GitHub has actions that are located on the marketplace to setup features, one of which is Java.
By calling the GitHub actions in the steps and specifying a java version to be setup, the action will include Java. This is done with the syntax.
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
