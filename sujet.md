# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1. 
In October 2024, Mozilla has revealed that a critical security flaw impacting Firefox has come under active exploitation in the wild, known as [CVE-2024-9680](https://www.mozilla.org/en-US/security/advisories/mfsa2024-51/#CVE-2024-9680) . 

It's an use-after-free bug in the animation component, which is basically a local bug: dangling pointer that lead to undefined behavior. Attackers can exploited this to execute code in your browser if they can figure out how to insert malicious code to that pointer memory address. Mozilla had to immediately release a patch to fix it, but they have had reports of it being exploited in the wild. 

For the users afftected, we don't know that what happened to them as nothing catastrophic appeared, but it is possible that after code execution on their browser, personal data is compromised. For the company, it is certain that this is a hit to their userbase, because the market for browser is competitive right now.

In my opinion, it would be hard to detect this bug even with proper testing as pointer-related bug are always like that. The code still function normaly with dangling pointer, the only way to prevent this is to follow good coding pratice: set pointer to null after freeing it

2.
[MultiSet.Entry::getCount() isn't 0 after removing the last element](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-709?filter=doneissues)

This local bug is about number of entries in a MultiSet. Logically, after removing the last element from the set, its count should be 0, but somehow this is not the case here, when the set have only 1 element, entry.getCount() is 1, removing the last element and entry.getCount() is still 1. The fix is simply to manually set entry count to 0 after remove.

The contributor carefully added a test case that cover exactly the bug. 
```
  @SuppressWarnings("unchecked")
    public void testMultiSetEntrySetUpdatedToZero() {
        if (!isAddSupported()) {
            return;
        }
        final MultiSet<T> multiset = makeObject();
        multiset.add((T) "A");
        multiset.add((T) "A");
        final MultiSet.Entry<T> entry = multiset.entrySet().iterator().next();
        assertEquals(2, entry.getCount());
        multiset.remove((T) "A");
        assertEquals(1, entry.getCount());
        multiset.remove((T) "A");
        assertEquals(0, entry.getCount());
    }
```
3.

### Concrete Experiments Performed:

#### Chaos Monkey: 
Randomly terminates virtual machine instances in production to ensure services can handle instance failures.

#### Chaos Kong:
Simulates failure of an entire Amazon EC2 region to test the system's failover capabilities.

#### Failure Injection Testing (FIT):
Introduces failure scenarios, such as failed requests or injected latencies between services, to validate system resilience and degradation handling.
#### Bookmark Service Experiment: 
Simulates service failures (e.g., bookmark service) and observes their impact on key metrics like Streams Per Second (SPS).

### Requirements for These Experiments:

A distributed system architecture capable of supporting real-world inputs and dynamic behaviors.
Monitoring tools to measure system metrics such as SPS (Streams Per Second) and other steady-state indicators.
Mechanisms to safely inject failures (e.g., FIT tools) without endangering the system.
Risk mitigation strategies, such as testing on a subset of users or simulating rather than directly causing catastrophic events.

### Observed Variables:

Primary Metrics: System-wide health indicators like SPS, which reflect steady-state behavior and user-facing availability.
Fine-Grained Metrics: CPU load, request latency, and database query times, used for internal service monitoring and identifying degraded states.

### Main Results Obtained:

Improved system reliability by identifying weaknesses through controlled failure scenarios.
Encouraged engineering practices that design systems to handle failures gracefully.
Built confidence in the system’s ability to withstand real-world disruptions and maintain availability.

### Are These Experiments Unique to Netflix? No, other organizations like Amazon, Google, Microsoft, and Facebook also implement similar techniques to test resilience in distributed systems, as mentioned in the document.

### Speculations on Implementing Chaos Experiments in Other Organizations:

#### Possible Experiments:

E-commerce platforms could simulate database outages or payment gateway failures to test the fallback mechanisms.
Banking systems might test response times under high transaction volumes or simulate network interruptions.
Healthcare platforms could simulate data unavailability or increased API latency to evaluate system robustness.

#### System Variables to Observe:

Steady-State Metrics: Transaction completion rates, user engagement levels, or API response times.
Service-Level Metrics: Latency, error rates, and resource utilization.
User Experience Indicators: Dropout rates, service accessibility, and time to recovery.

4.
### Advantages of Having a Formal Specification for WebAssembly

Ensures Consistency Across Implementations:
    A formal specification provides a precise, unambiguous definition of WebAssembly's behavior. This ensures all implementations, regardless of platform or browser, behave consistently.

Facilitates Correctness and Safety:
    By rigorously defining the semantics, the specification ensures that the language does not allow undefined behaviors. This is critical for safety in a system where code from untrusted sources executes.

Supports Soundness Proofs:
    The formal specification allows proving important properties, such as memory safety, type safety, and absence of undefined behavior, guaranteeing the integrity of WebAssembly programs.

Enhances Interoperability:
    A clear specification ensures that modules generated by different producers (compilers or tools) can work seamlessly together across implementations.

Simplifies Validation and Compilation:
    The formal definition allows WebAssembly code to be efficiently validated and compiled in a single linear pass, reducing runtime overhead.

Facilitates Tooling and Debugging:
    Developers can create tools (e.g., debuggers or interpreters) that conform to the specification without relying on trial and error, ensuring reliable behavior.

Encourages Clean Design:
    The use of formal methods during design leads to a clean, minimalistic architecture for WebAssembly, avoiding unnecessary complexity.

Serves as a Foundation for Extensions:
    A solid, formally defined base simplifies the process of adding new features or extending WebAssembly in the future, as extensions can build on rigorously defined semantics.

### Should WebAssembly Implementations Still Be Tested?

WebAssembly implementations should still be thoroughly tested. While a formal specification provides a strong foundation, it does not eliminate the need for testing. Because: 

Real-World Complexities:
    Implementations involve numerous practical considerations, like optimizing performance on different hardware, interacting with other web technologies, or dealing with non-standard user inputs. Testing ensures these complexities are handled correctly.

Implementation Bugs:
    Developers might misinterpret parts of the specification or introduce coding errors during implementation. Testing helps uncover such issues.

Integration with Host Environments:
    WebAssembly often interacts with JavaScript and Web APIs. Testing ensures seamless integration and identifies issues arising from interactions between components.

Performance Validation:
    Tests can measure and optimize performance, which is critical for meeting the low-overhead execution goals of WebAssembly.

Security Assurance:
    While formal specifications help mitigate security risks, practical testing (e.g., fuzzing or penetration testing) is essential to identify and fix vulnerabilities.


