# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1 - **Ghostscript Vulnerability (CVE-2024-29510)**  : this vulnerability exist in how Ghostscript handles EPS (Encapsulated PostScript) 
   ==> considered **global** because it affects a widely used software tool present in diverse systems worldwide
   ==> The bug came to light because Ghostscript failed to properly handle specially crafted EPS files 
   ++ for clients/users :
   ==> Loss of Trust : the users might lose their trust if their data is compromised.
   ==> Data Breaches : the Attacker could steal or access confidential information like medical records etc ..
   ++ for the company:
   ==> Reputation Damage
   ==> Financial Costs
   And For me Yes, testing the right scenario could have likely helped discover the Ghostscript bug earlier because The bug is related to how Ghostscript processes EPS files, a format that can include complex instructions. Testing scenarios that deliberately use malformed or malicious EPS files could reveal vulnerabilities in command execution or memory management.

2 - collection 813 : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-813?filter=doneissues&orderby=cf%5B12314125%5D+ASC%2C+updated+DESC
    ==> The bug was identified in the method CollectionUtils.retainAll(), which is used to retain the elements of one collection that are also present in another collection, with an optional Equator for comparing elements. The issue was that the method did not throw a NullPointerException (NPE) when any of its parameters were null
    ==> This bug can be classified as local, as it only affects the behavior of a specific utility method .
    ==> Solution :  a null check was added for the Equator parameter using Objects.requireNonNull(). This ensures that if the Equator is null, the method immediately throws a NullPointerException
    ==> Yes, the contributors added tests to verify that the retainAll() method now correctly throws an NPE when null is passed as the Equator. The test case provided in the issue demonstrates the scenario where a null Equator is passed and confirms that the NPE is now correctly thrown, making sure that this bug will not reappear in future versions.

3 - concrete experiments performed :
   ==> Chaos Monkey :Randomly terminates virtual machine instances in production to ensure services can withstand single-instance failures.
   ==> Chaos Kong : Simulates the failure of an entire Amazon EC2 region to verify cross-regional failover mechanisms.
   ==> Failure Injection Testing (FIT) : Introduces faults like request failures between services to test graceful degradation.
  ++ the requirements for these experiments :
   ==> Distributed system architecture with production-grade deployment. 
   ==> Metrics to define and monitor steady-state behavior like SPS (stream starts per second).
   ==> Tools to inject failures like  Chaos Monkey, FIT.
  ++ Variables Observed :
  ==> Primary Metric: Steady-state behavior metrics like SPS.
  ==> Fine-Grained Metrics: CPU load, request latency, or service-specific indicators.
  ==> Impact Scope: Observing whether user-facing functionality or system stability is compromised.
  ++ Results Obtained :
  ==> Improved system resilience by identifying and addressing vulnerabilities.
  ==> Enhanced confidence in the system's ability to handle real-world failures.
  ++ Companies like Amazon, Google, Microsoft, and Facebook have implemented similar approaches for resilience testing
  ++ Speculation on Application in Other Organizations 
  ==> E-commerce like Amazon : Simulate failures like a payment system crash. Test how the platform handles such failures without affecting customers' ability to shop.
  ==> Healthcare like Hospitals : Simulate a database failure that stores patient records. Test whether critical systems can still function, such as retrieving emergency patient data.

4 - Advantages of a Formal Specification for WebAssembly
  ==> Clarity and Precision : A formal specification defines WebAssembly's behavior clearly and unambiguously, leaving no room for confusion about how it should work.
  ==> Verification : Developers can use the specification to mathematically prove that their implementation is correct and free from errors like memory leaks or crashes.
  ==> Improved Design : Formalizing WebAssembly from the start leads to cleaner and more reliable system design.
 ++ Should WebAssembly Implementations Still Be Tested : 
  ==> Yes, testing is still important! Even with clear rules (formal specification), mistakes can happen when writing the code for WebAssembly. Testing makes sure that:
       => Everything works properly on different computers and browsers.
       => There are no bugs or unexpected problems when WebAssembly runs.

5 - Why is the Mechanized Specification Useful 
  ==> Clear and Accurate Rules: It defines WebAssembly's behavior in a precise, mathematical way.
  ==> Fixes Mistakes: It found and fixed problems in the original specification, like errors in handling types and exceptions.
  ==> Useful Tools: It helped create tools like a program checker (to catch errors in code) and an interpreter (to run programs).
  ++ Did It Improve the Original Specification?
  ==> Yes! It caught several mistakes and helped improve WebAssembly’s rules to make them more accurate and reliable.
  ++ What Came Out of This Mechanized Specification?
  ==> A type checker that ensures WebAssembly programs are written correctly.
  ==> An interpreter that can run WebAssembly programs safely.
  ==> Tests that compare this specification with real-world WebAssembly engines to check for differences.
  ++ How Was It Verified
  ==> The authors tested their specification by:
      => Proving key properties like "programs keep working correctly as they run."
      => =Running tests and fuzzing (randomized testing) to ensure their tools match real-world behavior.
   ++ Does This Remove the Need for Testing
  ==> No! Testing is still needed because:
         => Real-world implementations can have bugs.
         => The specification might miss practical edge cases.