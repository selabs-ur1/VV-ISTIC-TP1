# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link <https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues> points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?

5. Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: <https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf>. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. The *Vmin Shift Instability* (too-high voltage issue) causes Intel's newest chips to be permanently damaged overtime.

    The *Vmin Shift Instability* issue is local to 13th and 14th Gen “Raptor Lake” Intel's chips.
    Appearing in February 2024, gamers playing games with *DirectX12* started to experience frequent crashes

    this bug was confusing due to the wrong error message affirming that the GPU (Nvidia GeForce Graphics drivers) was in fault: *VRAM errors*.

    > Users are also receiving misleading error messages about running out of video driver memory, despite having sufficient memory.

    But NVidia denied in April 2024 and game developers, [from Alderon games](https://alderongames.com/intel-crashes), Fortnite, Warframe (blame on Raptor Lake), started to monitor what causes could lead to this problem.
    It causes 100% failure rate in certain contexts, for all end customers (game users, game servers and developers).

    The first fix was to underclock these CPUs at the lower voltage, resulting in an estimated 9% performance loss.

    from [Intel's public blog post](https://community.intel.com/t5/Blogs/Tech-Innovation/Client/Intel-Core-13th-and-14th-Gen-Desktop-Instability-Root-Cause/post/1633239):

    > **Vmin Shift Instability Root Cause**
    >
    > Intel® has localized the *Vmin Shift Instability* issue to a clock tree circuit within the IA core which is particularly vulnerable to reliability aging under elevated voltage and temperature. Intel has observed these conditions can lead to a duty cycle shift of the clocks and observed system instability.

    This bug caused Intel to recall and refund all faulty chips.

    This is not a software bug, but with sufficient system testing (stress test) it could have been discovered.
    Unfortunately the problem is appearing after months of usage, so in practice we can't test this much without having any idea of what we're testing.

2. [CollectionUtilsTest.getFromMap() is flaky](https://issues.apache.org/jira/browse/COLLECTIONS-775) bug

    This minor bug is local as it's the `CollectionUtils.get(expected, check)` that causes the problem.

    The `CollectionUtilsTest.getFromMap()` is failing due to `CollectionUtils.get()` which uses a new Java Iterator each time it is called. It's possible that `get(0) != get(0)` and `get(0) = get(1)` in two subsequent calls to the "expected" HashMap.

    The solution is a [simple fix](https://github.com/apache/commons-collections/pull/200):

    > instead of adding `CollectionUtils.get(expected, 0)` and `CollectionUtils.get(expected, 1)` to a new HashMap (`found`) and compare the two maps, assert the 0 and 1 entries are either `zeroKey=zero` or `oneKey=one`.

    The contributors fixed the test and added a new one for `ORDERED` maps like `LinkedHashMap`.
    As it was a fix on only method test, an anti-regression test is not necessary.

3. [Chaos Engineering](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf)

    > Chaos Engineering is the discipline of experimenting on a distributed system in order to build confidence in its capability to withstand turbulent conditions in production.

    To evaluate their system, Netflix uses some key scenarios:

    - terminate virtual machine instances
    - inject latency into requests between services
    - fail requests between services
    - fail an internal service
    - make an entire Amazon region unavailable
  
    However, the chaos aspect is essential, the events must be unpredictable.
    This is why, they launch this kind of 'tests' in production.
    They ensure that the system is powerful enough by running experiments continuously.

    Some concrete experiments consist to overloaded/stress server.

    Some other solutions to unwanted events, reside in the preservation of the main feature (provide a video player) over small QoL, controlled by other micro-services (bookmark service (`where were you in the film?`), catalogue, etc.).

    There are some requirements for these experiments.
    - First start by defining 'steady state', a case-control to represent the normal behavior.
    - Claim that this steady state will continue in both the control and experimental group.
    - Introduce the bad events
    - Analyze the difference in steady state between the control and the experimental group. If found any disprove the claim.

    The **Variables** that Netflix uses is mainly SPS, (stream) starts per seconds.
    If the SPS drops, the failures introduced are effective.

    **Netflix's results**
    > At the conclusion of an experiment, we either have more confidence in the system’s ability to maintain behavior in the presence of our variables, or a weakness has been uncovered and suggests a path for improvement.

    Netflix is not the only company performing these experiments.
    > We know from informal discussions with other Internet scale organizations that they are applying similar approaches.

    The Chaos Engineering may be essential to ensure that a distributed system is operational.
    For example, Google (I'm sure they already use it) is distributed across the globe. Their metric could be the request per seconds (RPS). They could introduce latency with backup and data center synchronization.
    As the system must be always up.
    The real case of backing up data/transferring load is already happening in production.
    They already have sustained methods to apply their update, their new data center installation, etc.
