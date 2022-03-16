# byzantine-generals-in-qiskit
A quantum solution using qutrits or ququatrits and a turn-based game

## Byzantine generals problem

Alice is the commanding general. In her army equipped at the cutting edge of modern technology, the lieutenant generals Bob, Charlieand Dave communicate with Alice and with each other through pairwise authenticated classical and quantum channels.

To order an attack on the enemy that the army surrounds or to initiate a retreat, Alice sends a single-bit order m to each lieutenant.

Traitors, including Alice herself, could be present who will try to confuse the faithful parties. In this seminal [article](https://lamport.azurewebsites.net/pubs/byz.pdf) by Lamport, Shostak and Marshal, it was proved that no classical solution with fewer than 3m + 1 generals (including Alice) can cope with m traitors.

### Quantum solution to the case of four generals with possibly two traitors

Now consider a quantum solution for four generals (including Alice) with two traitors. This solution is inspired by the following [article](https://arxiv.org/abs/quant-ph/0107127#:~:text=Like%20Quantum%20Key%20Distribution%20) which however limits the protocol to three generals and one traitor ("the liar detection problem", which is included in this program). Below, the presentation plan of the protocol is inspired by this article. 

To achieve _detectable broadcast_ the protocol must, after termination, satisfy the following conditions:

1. All faithful generals agree with a common plan of action.


2. If the commanding general is faithful, then all faithful generals agree with the commanding general’s plan

Note that these conditions are always satisfied in the trivial case of none or only one faithful lieutenant.


#### Distribute entanglement

Two successive members of an array of eight suitably entangled qubits are distributed to each party. Each qubit pair emulates a ququatrit belonging to a set of four fully entangled ququatrits. 
The parties measure the qubits they have in the Pauli-Z dimension and get a quatrit (the quaternary equivalent of a bit) for each pair of qubits. A quatrit can take the values 0, 1, 2 or 3.
The distribution is repeated until all parties have a sufficiently long list of quatrits. 

This protocol is somewhat similar the one described in this [article](https://arxiv.org/abs/quant-ph/0210079), which however uses qubits differently entangled to emulate qutrits for the liar's detection problem. The quatrit lists in the present algorithm are random, correlated with the other lists, and each general knows only his/her own list.

#### Check that the entangled states are not corrupted

Methods somewhat similar to those implemented in quantum key distribution can be offered to verify that the states have not been corrupted during the distribution process by an internal traitor or external eavesdropper. Regardless of who created and distributed the states (one of the generals, all in turn, or a trusted outside agent), this check phase comes after the measurement phase. One possibility is that the generals in turn choose an index at random. They ask the values of the corresponding quatrits from the others to verify that all quatrits are different for that index. After a sufficient number of indexes have been tested, the protocol continues if the failure rate is that expected from the noisy quantum system in use. Quatrits used for testing are removed from the original lists.


#### Solve the problem

Step 1: The commanding general sends to each lieutenant a list of indices J corresponding to the quatrits from her own quatrit list which are equal m.

Step 2: Each lieutenant checks the consistency of J, which must not include any index pointing to a quatrit equal to m. A flag is set to the value of $m$ if his/her list is consistent or to -1 if inconsistent.

Step 3: The lieutenants exchange their flag. If they are all the same and not -1, they follow order of the commanding general. 

Step 4: A lieutenants which has flag = -1 doesn't execute any order. The number of generals is then reduced to 3. The ququatrit algorithm also works in this case, but at the cost of more quantum computation than for the qutrit-based algorithm,.

Step 5: Lieutenants who received consistent but different messages $m$ exchange information to uncover a potential traitor. Here the protocol is modified from those published: everything is done so that no lieutenant get an advantage: they start playing turn-based games.  


#### turn-based  game rules for four generals and up to two traitors:


- If all lieutenants say they received the same message, no game takes place. They follow the order of the commanding general.


- Otherwise two games are played, each between two lieutenants whose messages they say they received are different


- In a game, the lieutenants draw whoever starts. They take turns sending an index that must match 2 or 3 in the quadrit list of the receiver, which takes note of the frequency of failures. No index is used twice during this game which ends after a pre-agreed number of rounds.


- A faithful receiver noticing an abnormally high frequency of failure will label the sender 'potential traitor'.


- A faithful recipient noticing a frequency of failure normally expected from a faithful sender will label the sender 'faithful'.


- After completing two games, any faithful lieutenant reports any potential traitor to all lieutenants they know to be faithful.


- If a faithful lieutenant discovers two potential traitors, he concludes that they are indeed traitors and that the commanding general may be faithful. He follows the order sent to him.


- If two faithful lieutenants report the same potential traitor, they conclude that this latter is indeed a traitor and that the commanding general may be faithful. They both follow the order sent to them.


- If two lieutenants assess that each of them is faithful and only one of them denounces a potential traitor, they conclude that this latter and the commanding general are traitors. The two faithful lieutenants agree on a common strategy which seems reasonable to them.


- If three lieutenants assess that each of them is faithful and no potential traitor is detected, they conclude that the commanding general is a traitor. They follow a majority strategy. 
