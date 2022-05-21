# Byzantine Generals in Qiskit

## The Byzantine generals problem

The Byzantine army is now equipped at the cutting edge of modern technology. The lieutenant generals communicate with the commander general and with each other through pairwise authenticated classical and quantum channels.

To order an attack on the enemy that the army surrounds or to initiate a retreat, the commander general sends a single-bit order m to each lieutenant.

Traitors could be present who will try to confuse the faithful parties. In this seminal [article](https://lamport.azurewebsites.net/pubs/byz.pdf) by Lamport, Shostak and Marshal, it was proved that no classical solution with fewer than 3m + 1 generals can cope with m traitors.



### Quantum solution using four qubits to the case of three generals with possibly one traitor 

A [quantum solution](https://arxiv.org/pdf/quant-ph/0107127v1.pdf) to this problem was proposed, based on qutrits

A derived solution using five qubits is described by Adan Cabello in this [article](https://arxiv.org/pdf/quant-ph/0210079.pdf) and was [experimentally demonstrated using four-photon entanglement](https://arxiv.org/pdf/0710.0290v2.pdf)

The [first notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/byzantine-agreement.ipynb) in this repository presents a protocol using this latter solution. The traitor detection is achieved by a turn-based game between the lieutenants.

### Quantum solution using five qubit to the case of four generals with possibly two traitors
The [first notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/byzantine-agreement.ipynb) includes also a solution using 5 qubits for the case of the four generals, largely inspired by the Cabello protocol for three generals.

A detailed description of the agreement process can be found in the [second notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/game-scenarios-byzantine-agreement.ipynb).

A hardware demo for this solution is presented in the [third notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/hardware-demo-four-generals.ipynb).

### Solutions using pair of qubits each mapped to a qutrit or a ququatrit:

These solutions are presented in the [last notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/qu-n-it-byzantine-agreement.ipynb).
However, the noisy system solution for four generals is not possible at this time, because of the depth of the circuit.
WIP: code in this second notebook not yet including use of hypergeometric distributions or distances for demasking traitors.

