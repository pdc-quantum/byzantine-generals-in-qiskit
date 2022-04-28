# Byzantine Generals in Qiskit
Quantum solutions using qutrits or ququatrits and a turn-based game

## The Byzantine generals problem

The Byzantine army is now equipped at the cutting edge of modern technology. The lieutenant generals communicate with the commander general and with each other through pairwise authenticated classical and quantum channels.

To order an attack on the enemy that the army surrounds or to initiate a retreat, the commander general sends a single-bit order m to each lieutenant.

Traitors could be present who will try to confuse the faithful parties. In this seminal [article](https://lamport.azurewebsites.net/pubs/byz.pdf) by Lamport, Shostak and Marshal, it was proved that no classical solution with fewer than 3m + 1 generals can cope with m traitors.



### Quantum solution using four qubits to the case of three generals with possibly one traitor 

A [quantum solution](https://arxiv.org/pdf/quant-ph/0107127v1.pdf) to this problem was proposed, based on qutrits

A derived solution using five qubits is described by Adan Cabello in this [article](https://arxiv.org/pdf/quant-ph/0210079.pdf) and was [experimentally demonstrated using four-photon entanglement](https://arxiv.org/pdf/0710.0290v2.pdf)

The [first notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/byzantine-agreement.ipynb) in this repository presents a protocol using this latter solution. The traitor detection is achieved by a turn-based game between the lieutenants.

### Quantum solution using five qubit to the case of three generals with possibly two traitors
The [first notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/byzantine-agreement.ipynb) includes also a solution using 5 qubits for the case of the four generals, largely inspired by the Cabello protocol for three generals.

### Solutions using pair of qubits each mapped to a qutrit or a ququatrit:

These solutions are presented in the [second notebook](https://github.com/pdc-quantum/byzantine-generals-in-qiskit/blob/main/qu-n-it-byzantine-agreement.ipynb).

#### *Protocol in the case of 4 generals and eight qubits*

#### Distribute entanglement

Two successive members of an array of eight suitably entangled qubits are distributed to each party. Each qubit pair emulates a ququatrit belonging to a set of four fully entangled ququatrits. 
The parties measure the qubits they have in the Pauli-Z dimension and get a quatrit (the quaternary equivalent of a bit) for each pair of qubits. A quatrit can take the values 0, 1, 2 or 3.
The distribution is repeated until all parties have a sufficiently long list of quatrits. 

The quatrit lists in the present algorithm are random, correlated with the other lists, and each general knows only his/her own list.

#### Check that the entangled states are not corrupted

Methods somewhat similar to those implemented in quantum key distribution can be offered to verify that the states have not been corrupted during the distribution process by an internal traitor or external eavesdropper. Regardless of who created and distributed the states (one of the generals, all in turn, or a trusted outside agent), this check phase comes after the measurement phase. One possibility is that the generals in turn choose an index at random. They ask the values of the corresponding quatrits from the others to verify that all quatrits are different for that index. After a sufficient number of indexes have been tested, the protocol continues if the failure rate is that expected from the noisy quantum system in use. Quatrits used for testing are removed from the original lists.


#### Solve the problem

Step 1: The commanding general sends to each lieutenant a list of indices J corresponding to the quatrits from her own quatrit list which are equal m.

Step 2: Each lieutenant checks the consistency of J, which must not include any index pointing to a quatrit equal to m. A flag is set to the value of m if his/her list is consistent or to -1 if inconsistent.

Step 3: The lieutenants exchange their flag. If they are all the same and not -1, they follow order of the commanding general. 

Step 4: A lieutenants which has flag = -1 doesn't execute any order. The number of generals is then reduced to 3. The ququatrit algorithm also works in this case, but at the cost of more quantum computation than for the qutrit-based algorithm,.

Step 5: Lieutenants who received consistent but different messages m exchange information to uncover a potential traitor. Here the protocol is modified from those published: everything is done so that no lieutenant get an advantage: they start playing turn-based games.  


### Turn-based  game rules for four generals and up to two traitors:

#### *valid for both notebooks*


- If all lieutenants say they received the same message, no game takes place. They follow the order of the commanding general.


- Otherwise two games are played, each between two lieutenants whose messages they say they received are different. Faithful lieutnants who declared to each other that they received the same message trust each other. They refrain from playing against each other. 


- In a game, the lieutenants draw whoever starts. They take turns sending an index that must match the correct expectation in the list of the recipient, which takes note of the frequency of failure. No index is used twice during this game which ends after a pre-agreed number of rounds.


- A faithful recipient noticing an abnormally high frequency of failure will label the sender 'doubtful'. Otherwise the recipient will label the sender 'trustable'.


- After completing two games, any faithful lieutenant reports any doubtful lieutenant to all lieutenants they trust.


- If a faithful lieutenant discovers two doubtful lieutenants, he concludes that they are indeed traitors and that the commanding general may be faithful. He follows the order sent to him.


- If two faithful lieutenants trusting each other report the same doubtful lieutenant, they conclude that this latter is indeed a traitor and that the commanding general may be faithful. They both follow the order sent to them.


- If one out of two faithful lieutenants trusting each other denounces the third lieutenant as doubtful, the faithful lieutenants conclude that this latter and the commanding general are traitors. The two faithful lieutenants agree on a common strategy which seems reasonable to them.


- If the three lieutenants are trusting each other at the end, they conclude that the commanding general is a traitor. They follow a majority strategy. 
