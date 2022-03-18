# Students

**Alexis Leloup**

**Simon Lecordier**

# Practical Session #1: Introduction

### 1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

The Steam client for Linux could accidentally delete all the user's files in every directory on the computer. 

A shell script in the application is the cause:

```shell script
STEAMROOT="$(cd "${0%/*}" && echo $PWD)"

# Scary!
rm -rf "$STEAMROOT/"*
```

This bug happened to users who had moved the default Steam installation directory.

The first line tries to find the directory containing the script, but if the directory was moved while the script was running. 
```$0``` is the full path name of the running script; the given expression removes the last component of the path, to get the directory.
So if i ```echo $PWD``` is an empty field, or if there is an error, having no error clause, ```STEAMROOT``` can be empty.

In the delete command, if ```$STEAMROOT``` is empty then ```rm -rf /*``` deletes all directories in the OS root.

The major repercussion of this bug is that users running the application as root can see all the files on their PC deleted. 
This can lead to the deletion of important files especially in the case of Linux PCs for professional use.

From our point of view, it is unthinkable not to put an error clause on such a critical command, as well as not to perform tests on it. It would only be necessary to test the case where the variable is empty.

##
### 2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

[Issue](https://issues.apache.org/jira/browse/COLLECTIONS-673?jql=project%20%3D%20COLLECTIONS%20AND%20statusCategory%20%3D%20Done%20ORDER%20BY%20cf%5B12310200%5D%20ASC)

The bug appears during the call to the method **ListUtils.partition()** on a large list, a potential overflow must appear.

The bug was found on  2nd February 2018 and solved on June 2018, 12th. 

We think that the bug is local because it was observed with a really huge list whose size was greater than Integer.MAX_VALUE.

To solve it, **size()** method have been updated from

```java
public int size() {
    return (list.size() + size - 1) / size;
}
```

to

```java
@Override
public int size() {
    return (int)Math.ceil((double)list.size() / (double)size);
}
```

And they add non regression test :
```java
List<List<Integer>> partitionMax = ListUtils.partition(strings, Integer.MAX_VALUE);
assertEquals(1, partitionMax.size());
assertEquals(strings.size(), partitionMax.get(0).size());
assertEquals(strings, partitionMax.get(0));
```



##
### 3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

Netflix uses chaos engineering to ensure that their system always works properly in any host environment.

Roughly speaking, this consists of testing a system by injecting variables to disrupt the system.

For example, they will inject latency, and will deliberately make requests fail, exchanges between services or even an entire service.

Concretely, the principle is to simulate two environments, a control one which is stable and an experimental one which is stable at the beginning, then we inject real world variables (server failure, hard disk failure, network failure...) and try to maintain the experimental system in a state equivalent to the control system.

Testing the reliability of Netflix's global system does not stop at streaming only, but at all the different services they have set up on their platform.

These experiments show that the system will manage to maintain its state if it encounters complications related to unforeseen events

If a weakness is encountered it can be corrected to maintain software quality.

Many companies use this system such as Twilio, Netflix, LinkedIn, Facebook, Google, Microsoft, Amazon


##
### 4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

WebAssembly is a type of code that can be executed in a modern web browser.

It is a low-level, assembler-like language that achieves performance close to native applications while running on the Web.

WebAssembly is designed to work in conjunction with JavaScript.

The formal implementation has several advantages:

- It's more secure because managed languages enhance memory security, preventing programs from compromising user data or system state.
- It's faster because low level code is interpreted faster by the machine
- It's more portable, low level code allows to be used more easily on any type of architecture
- it's more compact, data used on networks must be low, the binary format is more compact than JavaScript

In our opinion, the code should still be tested because a formality error can be written during development and would not be found without testing.

##
### 5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

#### According to the author of this second paper, what are the main advantages of the mechanized specification?

The automated specification allows among other things to increase the reliability of the code and to increase the performance, especially for web animations.

#### Did it help improving the original formal specification of the language?

Yes, this fixed several deficiencies in the official WebAssembly specification, discovered through their proof and model work.

#### What other artifacts were derived from this mechanized specification?

#### How did the author verify the specification?

They created a mechanised proof for the type soundness properties stated in the working group’s paper.

They also carried out fuzzing experiments. 

Fuzzing consists in sending, in a massive and automated way, data to a program in order to identify vulnerabilities.

#### Does this new specification removes the need for testing?

No, but this specification increases the reliability of the system enormously.