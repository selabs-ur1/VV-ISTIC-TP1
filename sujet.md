# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Question 1

[Article](https://www.theregister.com/2023/11/27/openzfs_2_2_0_data_corruption/): A data-destroying bug has been discovered following the release of OpenZFS 2.2.0. It appears to be caused by a new feature (Block cloning) exacerbating a previously unknown underlying bug. This [bug](https://github.com/openzfs/zfs/issues/15526#issuecomment-1825181463) apparently existed as far back as 2013, but it did not seem to manifest itself in production. Still, in conjunction with Block Cloning, whole blocks get overwritten by zeroes.

[Bronek Kozicki](https://github.com/Bronek) explains the conditions necessary for this bug to manifest in a [GitHub Issue comment](https://github.com/openzfs/zfs/issues/15526#issuecomment-1826412289):

> You need to understand the mechanism that causes corruption. It might have been there for decade and only caused issues in a very specific scenarios, which do not normally happen. Unless you can match your backup mechanism to the conditions described below, you are very unlikely to have been affected by it.

> 1. a file is being written to (typically it would be asynchronously – meaning the write is not completed at the time when writing process "thinks" it is)

> 2. at the same time when ZFS is still writing the data, the modified part of file is being read from. The same time means "hit a very specific time", measured in microseconds (that's millionth of a second), wide window. Admittedly, as a non-developer for ZFS project, I do not know if using HDD as opposed to SSD would extend that time frame.

> 3. if it is being read at this very specific moment, the reader will see zeros where the data being written is actually something else

> 4. if the reader then stores the incorrectly read zeroes somewhere else, that's where the data is being corrupted

Worse still, OpenZFS' own volume integrity checking tooling does not report anything wrong whenever data gets corrupted.

We consider this bug to be local, as it hinges on the existence of a previously unknown issue deep in the code base. The immediate consequences for the developers was embarrassment over the perceived loss in confidence from their users. Distributions shipping OpenZFS 2.2.0 will have to update to OpenZFS 2.2.1 as soon as possible. As for the users, it seems like very few actually lost data in this whole ordeal. A fix for the underlying bug is in the works and should be available for OpenZFS 2.2.2.

I do not known whether or not this bug could have been discovered through what would have been considered *reasonable* testing, as it seems to be quite mystical. However, new features such as block cloning should always be tested in scenarios as close to production as possible before shipping. File systems are critical systems in any circumstances and should absolutely be subjected to the most severe testing routines before updates.
