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
cwlVersion: v1.0
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

- How to get a input file. So, we switch to cat as an example.

[cat.cwl](cat.cwl)
```yaml
cwlVersion: v1.0
class: CommandLineTool
baseCommand: cat
inputs:
    message_file: 
      type: File
      inputBinding:
        position: 1
outputs: []
```

[inputs-file.yml]
```yaml
message_file:
       class: File
       path: message.txt
```

[message.txt](message.txt)
```
Hello DTL!
```

command:
```
cwltool cat.cwl inputs-file.yml
```

## Output File

[wc.cwl](wc.cwl)
```yaml
cwlVersion: v1.0
class: CommandLineTool
baseCommand: wc
stdout: output.txt
inputs:
    text_file: 
      type: File
      inputBinding:
        position: 1
outputs:
    output:
        type: stdout
```




run:
```
cwltool wc.cwl inputs-long-file.ym
```


[inputs-long-file.yml](inputs-long-file.yml)
```yaml
text_file:
       class: File
       path: some_text.txt
```

[some_text.txt](some_text.txt)
```
put text here
```


## two-step workflow

ls.yml
```
cwlVersion: v1.0
class: CommandLineTool
baseCommand: ls
stdout: output.txt
arguments:
    - /home
inputs: []
outputs:
    output:
        type: stdout
```

workflow.yml
```
cwlVersion: v1.0
class: Workflow
inputs: []

outputs:
  files_in_home:
    type: File
    outputSource: count/output

steps:
  list:
    run: ls.cwl
    in: []
    out: [output]

  count:
    run: wc.cwl
    in:
      text_file: list/output
    out: [output]
```


## parameter reference


## With (two) Docker Containers

