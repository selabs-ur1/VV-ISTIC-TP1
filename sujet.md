# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

---
---


##### Shcherbakova Kateryna 
##### Tkachenko Oleksii
### Question 1
#### Therac-25 Software Bug
##### Overview
The Therac-25 software bug stands as a notable case in the history of software engineering, emphasizing the critical importance of thorough testing and validation in software development. The Therac-25 was a radiation therapy machine produced by Atomic Energy of Canada Limited (AECL) in 1982 for cancer treatment. It was discovered that there was a software error in the machine that caused at least 3 deaths and 3 injuries. The bug occurred when the machine was being operated in a specific sequence too quickly, causing it to deliver radiation doses significantly higher than intended. 

##### Bug Description
At least four critical bugs were discovered in the Therac-25 software, all of which could lead to radiation overdoses:

1. **Shared Variable Issue**: A shared variable used for both input analysis and turntable position tracking caused a race condition. Quick data input via the terminal could leave the turntable in the wrong position.
2. **Bending Magnets Delay**: The system took approximately 8 seconds for the bending magnets to set in place. If an operator changed the beam type and power within that time and then moved the cursor to the final position, the system failed to detect these changes.
3. **Division Error**: Division by the variable controlling beam power could lead to a zero-division error, resulting in an increase in power up to the maximum possible value.
4. **Boolean Variable Assignment**: Setting a one-byte Boolean variable to "true" was performed using the command "x = x + 1". Pressing the "Set" button could result in the system failing to recognize the message about an incorrect turntable position once in every 256 attempts.

##### Fixes Implemented
To rectify the issues, several fixes were implemented in the Therac-25 system:
1. **Treatment Interruption**: All interruptions related to the dosimetry system would halt the treatment process instead of suspending it, requiring operators to reenter all parameters.
2. **Single-Pulse Shutdowns**: Both software and independent hardware single-pulse shutdown mechanisms were added to improve safety measures.
3. **Error Messages**: Cryptic malfunction messages were replaced with meaningful messages, and dose-rate messages were displayed on the monitor for better understanding and control.
4. **Turntable Monitoring**: A potentiometer was added to monitor the turntable location, ensuring accurate positioning.
5. **Deadman Switch**: A motion-enable footswitch (deadman switch) was added so that the turntable and other machine parts could move only while the operator held the switch closed, enhancing safety measures.
6. **Interlocking Mechanism**: In X-ray mode, an interlocking mechanism with the 270-degree bending magnet was added to ensure the target and beam flattener were properly positioned before operation.

##### Local or Global Impact
The bug was a **global** issue affecting all Therac-25 machines in operation. 

##### Repercussions
For **clients** and consumers, the repercussions were catastrophic. Patients undergoing treatment suffered severe radiation burns and long-term health issues due to the excessive doses delivered by the machine. Families of the affected patients faced immense emotional distress and financial burdens due to the medical complications arising from the treatment.

For the **company** behind the faulty program, the repercussions were severe in terms of legal battles, tarnished reputation, and financial liabilities. Lawsuits were filed against the company, resulting in significant compensation payouts to victims and their families. The incident severely damaged the company's credibility and trust within the medical community and among the general public.

##### Testing Scenarios
In hindsight, thorough testing of various scenarios could have potentially helped discover the fault in the Therac-25 software. Specifically, rigorous testing of the machine's response to different user inputs and operational sequences might have revealed the flaw that caused the radiation overdose. Additionally, employing more robust software validation techniques and incorporating fail-safe mechanisms could have prevented such catastrophic outcomes.

##### Conclusion
The Therac-25 incident serves as a stark reminder of the importance of meticulous testing, stringent quality control measures, and comprehensive validation protocols in the development of safety-critical software systems, especially those used in healthcare.

--- 

### Question 2 

#### Issue: Flat3MapTest.tesetEntrySet() is Flaky. Issue Number: COLLECTIONS-768

##### Local or Global
This issue is localized as it pertains to a specific testEntrySet() function within the context of utilizing putAndRemove() in tests.

##### Problem
The problem stems from the test function testEntrySet() relying on the helper function putAndRemove(), which incorrectly assumes that map iterators execute in insertion order, while they can iterate in any order. The test currently asserts that the last iterated element (and subsequently removed) is the last inserted element. Consequently, the test passes only if the last iterated element happens to be the last inserted element; otherwise, it fails.

##### Resolution
The resolution involved replacing the code segment that depended on the iteration order with a more reliable alternative. Instead of directly validating the sequence of insertion and removal, alternative validation methods were implemented, independent of the iteration order. The updated code verifies the values associated with the removed element to ensure they accurately reflect the correct map values after the element is removed. This approach makes testing more robust by removing the dependency on the iteration order.

##### Code Modification
The commented-out code segment was replaced with a more reliable solution to resolve the error:

```java
final Map.Entry<K, V> mapEntry1 = it.next();
final Map.Entry<K, V> mapEntry2 = it.next();
final Map.Entry<K, V> mapEntry3 = it.next();
V removedValue = mapEntry3.getValue();
it.remove();
// assertEquals(2, map.size());
// assertEquals("one", map.get("A"));
// assertEquals("two", map.get("B"));
// assertEquals(null, map.get("C"));
assertNotNull(removedValue);
assertEquals(removedValue.equals("one") ? null : "one", map.get("A"));
assertEquals(removedValue.equals("two") ? null : "two", map.get("B"));
assertEquals(removedValue.equals("three") ? null : "three", map.get("C"));
} 
```

---

### Question 3
#### Experiments in Netflix Chaos Engineering Paper (2016)
##### Experiments performed:
1. **Chaos Monkey**: Terminating random virtual machine instances during normal working hours.
2. **Chaos Kong**: Simulating the failure of an entire Amazon EC2 region.
3. **Failure Injection Testing (FIT)**: Intentionally causing requests between Netflix services to fail.

##### Requirements for these experiments:
1. Real-world simulations: Replicating hardware failures, network issues, or service outages.
2. Monitoring systems: Continuous observation of metrics such as starts per second (SPS) and user experience.
3. Risk mitigation strategies: Strategies to limit the scope of experiments to a subset of users if necessary.

##### Observed Variables:
1. SPS (Starts Per Second): Primary indicator of system health.
2. Fine-grained metrics: Request latency, CPU load, service-level indicators for specific services.
3. Impact on user experience: User ability to stream content or sign up for the service.

##### Main Results Obtained:
1. Successful service degradation: Services designed to handle instance failures without significant impact on SPS.
2. Graceful failover in different regions: Minimal impact on SPS when failing over from one region to another.
3. Emphasis on system availability: Focus on metrics like SPS to gauge overall health, ensuring users can access content.

##### Other Companies & Speculation on Experimentation
Netflix isn't the only company: Similar techniques are adopted by Amazon, Google, Microsoft, and Facebook, as mentioned in the paper. Other tech giants and Internet-scale organizations likely employ comparable methodologies for resilience testing.

##### Speculation on Experimentation in Other Organizations:
###### Types of Experiments:
1. Hardware and Software Failures: Simulating server crashes, network latency, or database malfunctions.
2. Traffic Surges: Injecting sudden spikes in requests or testing response under heavy loads.
3. Runtime Configuration Changes: Modifying configuration parameters to observe system behavior.

###### Observed System Variables:
1. Key Metrics: Vary based on the organization's primary services (e.g., e-commerce sites might track completed purchases per second).
2. User Experience: Emphasizing indicators like user access to services or successful transactions.

As Chaos Engineering evolves, it will likely involve case studies, improved adoption strategies, standardized tooling, and advanced event injection models to ensure system resilience in diverse organizational setups.

---

### Question 4 

##### Overview
The article "Bringing the Web up to Speed with WebAssembly" discusses the importance of WebAssembly as low-level code on a web platform and the benefits of having a formal specification for it. 

##### Advantages
The main benefits of having a formal specification for WebAssembly include:
1. **Security**: WebAssembly provides secure code execution, which is important on the web where code comes from untrusted sources. This protects user data and system state.
2. **Speed**: One of the goals of WebAssembly is to achieve high performance. It allows you to use the full performance of the machine by maintaining optimised machine code.
3. **Portability**: The code written for WebAssembly should be independent of language, hardware platform and operating system. This allows applications to run on different types of browsers and devices with the same behaviour.
4. **Compactness**: WebAssembly's binary format provides a more compact code representation than JavaScript, which reduces load times and improves application responsiveness.

##### Necessities of testing WebAssembly implementations
Having a formal specification does not mean that WebAssembly implementations do not need to be tested. Testing remains critical to be sure that the specification is implemented correctly. Even if you have a formal specification, the implementation may contain bugs or inconsistencies that can lead to code execution problems. Thus, testing remains a key step in development, even if you have a formal specification.

---

### Question 5 

##### Overview
The paper "Mechanising and Verifying the WebAssembly
Specification" presents a mechanised WebAssembly specification focused on improving the original formal language specification. The authors developed an executable typer and interpreter, and identified and eliminated bugs, thus enriching the verification process of this language specification.

##### Advantages
The author of the "Mechanising and Verifying the WebAssembly
Specification" paper highlights several advantages of the mechanized specification:
1. **Error Identification and Correction**: The mechanized specification process revealed errors in the original formal specification of WebAssembly. Identified errors ranged from issues causing unsoundness in the type system to problems with exception propagation and return operations.
2. **Enhanced Formalization**: By meticulously mechanizing the specification, the authors ensured a higher level of precision and clarity in the language's formal definition. This process involved proving soundness properties, establishing auxiliary lemmas, and correcting issues that could compromise the language's correctness.
3. **Artifact Derivation**: The mechanized specification led to the creation of additional artifacts such as an executable type checker and an executable interpreter. These artifacts, derived from the mechanized specification, provided tools for further validation and testing.

##### Verification of the specification
The author verified the specification through proofs and theorems, such as induction and establishing auxiliary lemmas, to demonstrate the correctness of the mechanized specification. This rigorous mathematical verification helped ensure the accuracy of the formal definitions and properties.

##### Need for testing
While the mechanized specification and its artifacts significantly enhance the language's formal definition and correctness, they do not eliminate the need for testing entirely. Testing complements formal verification by validating the real-world implementation and uncovering issues not addressed in the formal model or its artifacts. 
The author's executable interpreter successfully passed various core language conformance tests. Differential testing against major WebAssembly engines also helped validate the interpreter's behavior and potentially discover bugs. This validation, alongside the formal verification, contributes to a more robust and reliable implementation of WebAssembly.

---
---



