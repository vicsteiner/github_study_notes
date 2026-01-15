# github_study_notes

A repo for keeping notes for my study of github actions based on the course: https://learning.oreilly.com/course/github-actions-masterclass/9781837025411/.

## Basics about yaml

Basic they are key pair dictionaries.

YAML supports various data types, including scalars (strings, numbers, booleans), sequences (lists), and mappings (key-value pairs).

It is widely used in configuration files, infrastructure automation (Infrastructure as Code (IaC)), API specifications, data serialization and data exchange, especially in tools like Kubernetes, Docker, and Ansible.

In our case it is the format used to defined the workflows containing the github actions to be executed. 

Additionally, YAML is a superset of JSON, meaning valid JSON files can be parsed as YAML. YAML files typically have a .yaml or .yml extension.

Specification can be find at https://yaml.org/spec/history/2003-09-01.html in https://yaml.org/ and a simple introduction can be read at https://www.datacamp.com/blog/what-is-yaml.

A simple example:

```yaml
string: string
number: 10
bool: true
array:
  - red
  - blue
  - green
# a comment
dict:
  another_dict: string
array_of_dicts:
  - a: 1
    b: 2
    labels:
      - da
      - "de"
      - 'di "di"'
      - "do's"
  - c: 3
    d: 4

literal: |
  This is a
  multi-line string.

folded: >
  This is another 
  multi-line string.

string_implicit: Hello, YAML!  # No quotes needed unless necessary
string_double_quoted: "Supports escape sequences like \n and \t"
string_single_quoted: 'Raw text, no escape sequences'

integer: 42  # Whole numbers
float: 3.14  # Numbers with decimals

boolean_true: true
boolean_false: false

null_value: null  # Null value
null_tilde: ~  # Another way to represent null

explicit_string: !!str 123  # Forces 123 to be a string
explicit_integer: !!int "42"  # Forces "42" to be an integer
explicit_float: !!float "3.14"  # Forces "3.14" to be a float

# Anchors and aliases are especially useful in large configuration files where repeating values manually would be inefficient. 
defaults: &default_settings
  retries: 3
  timeout: 30

server1:
  host: example.com
  retries: *default_settings  # Reuses the retries value from defaults

server2:
  <<: *default_settings  # Merges all key-value pairs from default_settings
  host: example.com  # This key is added to the merged data
```

## GitHub Actions (a CI/CD solution provided by GitHub) building blocks

- Workflows
  - Jobs
    - Steps

Workflows:

- Defined at repository level and define which triggers actually tart the workflow
- Composed of one or more jobs


Jobs:

  - Defined at workflow level
  - Define in which execution environment they are run
  - Composed by one or more steps
  - Run in parallel by default (in independent virtual machines; no shared state)

Steps:

  - Defined at job level
  - Define the actual script or GitHub Action (a peace of code that we can run as part of a job that we take from somewhere else like another repository or from GitHub Atctions marketplace for instance) that will be executed
  - Run sequentially by default

## First exercise - create a siple workflow

DONE! Check commits.

## Workflow events

Events are triggers and there are multiple wayt to trigger a worklow

- Repository level: push, issues, pull_request, pull_request_review, fork ... and many others
- Manual triggers: triggered via UI, API, from another workflw
- Schedule; crnjob

Docs are at https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows

## Workflow runners

- GitHub-hosted runners
  - managed service
  - a VM is scoped to a job (each job receives a clean VM); steps share the VM but jobs don't
- Self-hosted runners
  - full controll
  - run in your own infrastructure
  - managed by yourself
  - jobs can share the same VM or use machines with state
  - do not use those in public repos

Each job in a workflow has a set up steps where one can check the Runner Image log which contains links to thew included software in the image as well as the image release itself.

## Using pre-built actions

Actions built by GitHUb r other vendors like AWS for instance. 

- Prevents code duplication (encapsulaiton) and reduces the chances of mistake for instances in tasks like setting up node js in a VM.
- Can be configured via the with key-value pair enabling flexibility and reusability
- Can be combined with other steps; can be used to encapsulate tasks beore running other commands
- We can create our own actions for public or private use; not restricted only to the actions available at the marketplace (https://github.com/marketplace?type=actions)

## Using Event Filters and Activity Types

Event filters  allow us **to speciy under which conditions** a specific event triggers our workflow.

Under a push event fr instaqnce we can specify filters like branches, branch_ignore, tags, tags_ignore, paths, paths_ignore ... (https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#onpushbranchestagsbranches-ignoretags-ignore)

Activity types specify which types fo certain triggers execute our workflow. For instance a pull_request event has several steps in its life cycle as. opened, synchronize, closed, assigned, labeled, edited etc. An activity type allow us to define in which step of the event the actions is to be triggered (https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#pull_request)

## Workflow contexts

Workflow contexts (sources of data for different dimensions of our workflow) access information about runs, variables, jobs, repository and more.

Contexts are for instance github, env, inputs (passed via the kyword with to an action, reusable wrkflow, manually triggered workflow) and vars (contains custom coniguration variables set at the organization, repository and environment levels). Ohter cntexts exist like secrets, matrix, needs ...

https://docs.github.com/en/actions/reference/workflows-and-actions/contexts

## Expressions

Used to reference information from multiple sources within the workflow
${{ <expression> }} syntax
Can by any combination of literal values, context values, functions
Support use of functions and logic operators

## Variables

In a single workflow they can be defined at levels

- workflow
    - job
        - step

There is a precedence order applied. The variable defined in a inner level overwrittes uotside level variables with same name.

For variables to be shared across multiple workflows

- organization
    - repository
        - environment

Precedence applies as previously mentioned

## Functions

Out-of-the-box functions to model complex behaviour

- general purpose functions
  - set of utility functions to interact with data from multiple contexts to model complex behaviour like advanced control of the workflow job execution
- status check functions
  - a set of functions that allow using the status of the workflow, previous jobs or steps to define wether a certain job or step should be executed

https://docs.github.com/en/actions/reference/workflows-and-actions/expressions?versionId=free-pro-team%40latest&productId=actions#functions

## References

- https://docs.github.com/en
- https://docs.github.com/en/actions
- https://cli.github.com/manual/

