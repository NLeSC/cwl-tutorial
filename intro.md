# Basic Common Workflow Language Tutorial

This is a basic Common Workflow Language (CWL) tutorial meant to be given in a [software carpentry](https://software-carpentry.org/) style lesson (hands on live coding).

The content of this tutorial is based on [A Gentle Introduction to the Common Workflow Language](http://www.commonwl.org/v1.0/UserGuide.html), more fitting for self-learning.

## Lesson Objectives

- What is cwl
- How to install cwl
- how to write a basic workflow

## Does CWLTool Work?

First things first, we will check if cwltool is properly installed.

Command: Running the cwltool and asking it to print its version.
```
(cwl) niels@ESLT0051:~$ cwltool --version
```

Output: something like
```
/home/niels/.virtualenvs/cwl/bin/cwltool 1.0.20161007181528
```

If this fails see https://github.com/common-workflow-language/cwltool for installation instructions, usually as simple as

```
pip install cwltool
```

## Simplest example

Write the following into a file:

[hostname.cwl](hostname.cwl)
```yaml
cwlVersion: v1.0
class: CommandLineTool
baseCommand: hostname
inputs: []
outputs: []
```

and run this simple workflow with the cwl-tool command:

```
cwltool hostname.cwl
```

## With Arguments

- How do we get arguments to a command line tool?

[echo.cwl](echo.cwl)
```yaml
cwlVersion: v1.0
class: CommandLineTool
baseCommand: echo
arguments:
    - Hello world!
inputs: []
outputs: []
```

## With inputs (Strings)

- How do we get non-hardcoded input?
- Define input, and how it gets to the command line tool

[echo-with-input.cwl](echo-with-input.cwl)
```yaml
class: CommandLineTool
baseCommand: echo
inputs:
    message: 
      type: string
      inputBinding:
        position: 1
outputs: []
```

run:
```
cwltool echo-with-input.cwl
```

ERROR

oeps. We also need to _specify_ the values of the input

[message.yml](message.yml)
```yaml
message: Hello world!
```

run:
```
cwltool echo-with-input.cwl message.yml
```

## Input File



## Output File

## parameter reference

## two-step workflow

## With (two) Docker Containers

