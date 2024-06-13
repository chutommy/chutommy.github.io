---
title: "Analysis of Bernstein-Vazirani quantum protocol"
description: "Project BVVIZ - Simulation and visualization of the Bernstein-Vazirani quantum protocol"
date: "2023-04-29T08:14:29+02:00"
weight: 99

hideMeta: false
showPostNavLinks: false
showReadingTime: false

showToc: false
tocOpen: false
disableHLJS: false
disableAnchoredHeadings: true

math: true
#{{< math.inline >}}\(  \){{</ math.inline >}}

cover:
  image: "/projects/bvviz/cover2.png"
---

Project BVVIZ is a tool that provides a user-friendly playground for running noisy quantum simulations and visualizing the Bernstein-Vazirani quantum algorithm.

{{< figure src="/projects/bvviz/img1.png" align="center" width=600 caption="source: bernstein-vazirani.streamlit.app" >}}

The Bernstein-Vazirani protocol is a quantum algorithm introduced in 1992, by computer scientists Ethan Bernstein and Umesh Vazirani. It was shown that there can be advantages in using a quantum computer as a computational tool for more complex problems than the [Deutsch-Jozsa problem](https://en.wikipedia.org/wiki/Deutsch%E2%80%93Jozsa_algorithm).

It has a potential use in classical cryptography and quantum key distribution and communication. [Shor's protocol](https://en.wikipedia.org/wiki/Shor%27s_algorithm), one of the most popular quantum algorithms - capable of a prime factorization in
{{< math.inline >}}\( \mathcal{O}( \log{} n ) \){{</ math.inline >}},
uses many properties of quantum circuits leveraged in this protocol.

## The problem

The Bernstein-Vazirani problem is a [promise problem](https://en.wikipedia.org/wiki/Promise_problem). It involves a black-box function
{{< math.inline >}}\( f_s \colon \{0, 1\}^n \rightarrow \{0, 1\} \){{</ math.inline >}},
which takes a bit string as input and returns either 0 or 1. The function guarantees to return the dot product between _x_ and a secret string {{< math.inline >}}\( s \in \{ 0, 1 \}^n \){{</ math.inline >}} modulo 2:

$$ f_s(\{ x_0, x_1, ..., x_{(n-1)} \}) = x \cdot s $$
$$ x \cdot s = x_0 s_0 \oplus x_1 s_1 \oplus ... \oplus x_{n-1} s_{n-1} $$

The objective is to determine the secret string _s_ of the function {{< math.inline >}}\( f_s \){{</ math.inline >}}.

## Solution

The project BVVIZ implements both classical and quantum solutions for the Bernstein-Vazirani algorithm. These solutions are compared in terms of the number of queries made to the oracle function {{< math.inline >}}\( f_s \){{</ math.inline >}}, computational time, accuracy, and overall complexity.

{{< figure src="/projects/bvviz/oracle.png" align="center" title="The reversible circuit of the Bernstein-Vazirani oracle" caption="image source: Qiskit" >}}

### Classical algorithm

The implementation of the classical algorithm finds the secret string by evaluating the function {{< math.inline >}}\( f_s \){{</ math.inline >}} _n_-times with the input values {{< math.inline >}}\( x = 2^i = 1 \ll i \){{</ math.inline >}} for all {{< math.inline >}}\( i \in \{0, 1, ..., n-1\} \){{</ math.inline >}}. By querying the _i-th_ bit, the _i-th_ bit of the secret string is determined.

The project BVVIZ implementation of the classical algorithm is also the most efficient since no more than one bit of information can be revealed by a single query to the oracle. The complexity of the classical approach is {{< math.inline >}}\( \mathcal{O}( n ) \){{</ math.inline >}}.

### Quantum algorithm

On the other hand, project BVVIZ's implementation of the quantum algorithm obtains the hidden secret string with certainty in a single query to the oracle function {{< math.inline >}}\( f_s(x) \){{</ math.inline >}}, resulting in a complexity of {{< math.inline >}}\( \mathcal{O}( 1 ) \){{</ math.inline >}}.

This is achieved by introducing input qubits {{< math.inline >}}\( |0 \rangle_n \){{</ math.inline >}} into an equal linear superposition of the {{< math.inline >}}\( 2^n \){{</ math.inline >}} basis states by applying _n_ Hadamard gates. An additional auxiliary qubit, carrying the output bit of information, is set to {{< math.inline >}}\( |-\rangle \){{</ math.inline >}} using a single Hadamard and Pauli-X gate.

The state ofter applying the quantum oracle function {{< math.inline >}}\( f_s \){{</ math.inline >}} is

$$ H^{\otimes n} |0\rangle \otimes HX|0\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^{n-1}} |x\rangle \otimes |-\rangle . $$

The oracle function {{< math.inline >}}\( f_s \){{</ math.inline >}} flips each qubit {{< math.inline >}}\( q_i \){{</ math.inline >}} that satisfies {{< math.inline >}}\( f_s(\text{bin}(2^i)) = 1 \){{</ math.inline >}}, which changes the sign of these states. This is also known as [phase kickback](https://eduardsmetanin.github.io/PhaseKickback.pdf).

Since all quantum gates are their own inverses, _n_ Hadamard gates are applied to obtain the final result:

$$ \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^{n-1}} (-1)^{s \cdot}  \xrightarrow{H^{\otimes n}} |s\rangle $$

## Implementation

{{< figure src="/projects/bvviz/circuit.png" align="center" caption="An implementation of the Bernstein-Vazirani protocol for a secret string 101" width=550 >}}

The underlying problems of quantum technology are often misunderstood. This tool guides users to understand both benefits and constraints of quantum computing.


### Quantum simulation

BVVIZ allows the freedom to customize the simulator device, including the backend system, number of shots, secret string, or their own noise and transpiler model. This enables full control over the quantum hardware that is being simulated to run the Bernstein-Vazirani experiment.

{{< figure src="/projects/bvviz/layout.png" align="center" caption="The quantum register mapping of a preconfigured Brooklyn system with sabre layout and stochastic routing" width=500 >}}

### Accuracy assessment

As a result of the quantum simulation, metrics, plots, and charts are generated to help users understand the impact of the noisy quantum devices. It encourages to take a closer look at the experimental settings, analyze the results, think about the statistics, and come up with independent conclusions and their own way of understanding limitations of quantum computing.

{{< figure src="/projects/bvviz/counts.png" align="center" caption="A count chart of an experiment with secret string 11001 and 1000 shots" width=540 >}}

If the provided statistics are inadequate or lacking, users are free to download the quantum circuit ([OpenQASM 2.0](https://en.wikipedia.org/wiki/OpenQASM)) and the measurements.

### Purpose

Project BVVIZ helps those who are new to quantum computation not only to learn about the Bernstein-Vazirani algorithm, but also to gain a deeper understanding of the capabilities of quantum technology, which is currently very much limited by the unreliable hardware.

{{< figure src="/projects/bvviz/errors.png" align="center" caption="The metrics of an experiment performed on a very noisy device" width=600 >}}

## Conclusion

{{< figure src="/projects/bvviz/img2.png" align="center" width=600 caption="source: bernstein-vazirani.streamlit.app" >}}

The simulation and visualization of the Bernstein-Vazirani protocol works properly and as expected. The application does a decent job of delivering the results.

The project was designed and developed with modularity in mind, allowing for easy expansion. Integrating new metrics, statistics, and plots can be done straightforwardly. With minor adjustments, it could also simulate and visualize other quantum protocols like Simon's, Deutsch-Jozsa's, and Grover's algorithms.

{{< youtube LX07C1eFSJc >}}

### Try it yourself

Feel free to experiment with BVVIZ first hand. The source code is available at **[github.com/chutommy/bvviz](https://github.com/chutommy/bvviz/)**.