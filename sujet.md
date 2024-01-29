# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Question 1
The choosen article is [Data from August Breach of Amazon Partner Juspay Dumped Online](https://threatpost.com/data-from-august-breach-of-amazon-partner-juspay-dumped-online/162740/). 
A secutity researcher found a huge data breach from an third party software which deals about online payement handling. Indeed, informations such as name, phone number, bank name were exposed. 


The bug seems like local because threat actors used an old, unrecycled Amazon Web Services (AWS) access key to gain unauthorized access to the server. Which mean the bug went in first place from an Amazon issue. 

The bug sum up by unhotorized use of data which triggered an alerte from Juspay due to an overuse of internal ressources.

For the companie, this bug forced them to enhence their API and revalidate some key. Futhermore they lost a lot of customer confident. For the customers, their info has been lost in world wide web and the info came to them a long time after the issue has been discovered. The society minimzed the leak which was actually a big loss of data.

This bug showed us the importance of alerte in a project bug this also showed that a review of old functionnaliie is very important for old project that had updates.


### Question 2
The choosen issue is [StackOverflowError in SetUniqueList.add() when it receives itself](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-701?filter=doneissues).
It's a local bug that happen in a class which implements List but without duplicates elements (such as a Set).
The bug was detected by a unit test and it's a StackOverflowError happening in SetUniqueList.add() when it receives itself such as following :
```java
test() {        
   SetUniqueList l = new SetUniqueList(new LinkedList<Object>()) ;        
   l.add((Object) l) ;    
}
```
SetUniqueList has a Set attribute, the method is recursive but it is called before adding the object to the set. To correct this problem, the object is first added to the set before recursion is called.
```java
@Override
public void add(final int index, final E object) {

    // adds element if it is not contained already
    if (set.contains(object) == false) {
-       super.add(index, object);
        set.add(object);
+       super.add(index, object);
    }
}
```
Futhermore new tests have been added to ensure that the bug is detected if it reappears in the future.


Source : https://threatpost.com/data-from-august-breach-of-amazon-partner-juspay-dumped-online/162740/

### Question 3

Netflix is one of the biggest streamming application in the world. They are well known for their fast responding server system and how accurate are their recommendations. But to achieve this kind of services with the less crash possible, they had to process a specific kind of development; Chaos Engineering.

Chaos Engineering is the discipline of experimenting on a distributed system in order to build confidence in its
capability to withstand turbulent conditions in production.
To do so, netflix used a service named Chaos Monkey which
randomly selects virtual machine instances that host their production services and terminate them. It's purpose was to force engineers to develop services that handle errors of inidivual instances. In order to have a controll on what's happening, they use it only on normal working hours.
They extended this approach by deliberately injecting faults into the production system to improve resilience. This includes exercises like "Chaos Kong," simulating the failure of an entire Amazon region, and Failure Injection Testing where they intentionally cause requests between Netflix services to fail. Notice that they don't really shutdown a region, instead they redirect the data flow such as it should happends if there was a real shutdown.

In order to track the result of theses experiences, the careffuly watch the SPS (strem per second)  which is a good indicator of a potential issues on services. Other variables may include finer-grained metrics like CPU load or time to complete a database query, but these metrics are not used for designing experiments, rather for checking the proper functioning of the system.
The requirement for theses experiences was that experiments should be based on hypotheses about the stable state behavior of the system.

In result, this approach helped Netflix to build a confidence in their system and the ability to continue to well process the services even under real world issues such as bad users connections which occur bad written requests.

But Netflix isn't the only companie to do so. Indeed Amazon or Facebook use the same aproach. Think

The experiments could be carried in other organnisation by using more accurate mertics for their system and prevent other issues like huge amount of connections and steaming at the same time.

### Question 4


Webassembly aims to be a safe and low level web langage developped by major developpers in the web.
This kind of language should respect some specificities such as: 
-safetfy: users data should be protected,
-being fast: in order to give a fast response, the code has to be a low level one
-portable: it's important that the code has to be plateform independant
-compact : in order to reduce the load times

To do so they went on a formal specification to develop their language.
This kind of specification has advantages such as the clarity of the description: what should do the language. This provide an easier understanding on the project and lead to an easier developpement. This can also optimise the performance as all aspect of the language are well respected. This specification also allow to provide a stroger security. Indeed every functionnality of the language are well defined and so, their behaviour can't be maliciously used. Issues can also being anticipated by a good specification which helps to avoid ambiguities in documentation.

But it's not because a project is well specified that it can dodge test.
In fact, erros such as implementation or interpretation can still happend. Even if a good specification gives a strong fundation, some unexpected scenario can occur and gives some error wich were not found before.
This is why WebAssembly should be well tested despite a formal specification.

### Question 5


As we saw before, WebAssembly is a new low-level language currently being implemented in all major web browsers. It's specification follow formal rules wich help to develop a strong project. However, good specification can't get rid of testing but the author of this paper made a project about mechanising and verifying WebASsembly specification in order to complete its strenght. 
According to him, this enhencement provide more help: The original paper formalisation defines 46 reduction rules for the language where as their mechanisation defines 65. They typespecialise some arithmetic and bitwise operations in order to provide a cleaner interface for code extraction and implement host function behaviours that the paper formalisation did not support. Another point is tha the specification’s reduction semantics define an execution/evaluation context structure of a single hole surrounded by nested labels. He also point out that their model enjoys a significant advantage in preserving eyeball closeness in that all WebAssembly reduction and typing rules in the specification already include a definition in formal notation. They claim that their work represent the first mechanised formalisation ofthe complete WebAssembly core language, as well as the first full proof, mechanised or otherwise, of the soundness of the WebAssembly type system.

The autors point out that their work in proving WebAssembly’s type soundness properties identified several important issues with the official specification which they could not have discovered without embarking on such a “deep" proof of a language property, and might not have been so immediately found.

Despite the mechanised specification, the autor highlighted the creation of thype-checker executable with Isabelle wich has been prooved as correct.
They went throught a fully mechanised proof ment by induction.
Even with a mechanised specification, test can't be avoided in order to catch errors linked with real world program execution such as y discovering semantic bugs in commercial WebAssembly engines.



