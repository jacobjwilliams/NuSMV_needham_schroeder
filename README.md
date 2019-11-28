# avcs_coursework

Coursework for Analysis and Verification of Concurrent Systems module at UWE  
UFCFYN-15-M  
  
## Runtime  
The communication between A, B, and S works autonomously. One could say this
model is designed to appear as if the user is controlling the Intruder looking
upon the seemily autonomous authentication process being carried out by the
machines.  
There are several states at every transition, each of these represents a
different combination of actions for the intruder:  
1. Delay = TRUE  
	Capture = TRUE  
	Replay = TRUE   
2. Delay = FALSE  
	Capture = TRUE  
	Replay = TRUE  
3. Delay = TRUE  
	Capture = FALSE  
	Replay = FALSE  
4. Delay = TRUE  
	Capture = TRUE  
	Replay = FALSE  
5. Delay = FALSE  
	Capture = TRUE  
	Replay = FALSE  
6. Delay = TRUE  
	Capture = FALSE  
	Replay = FALSE  
7. Delay = FALSE  
	Capture = FALSE  
	Replay = FALSE  
  
## Outline  
Design, analyze, and verify authentication protocol using NuSMV model checker.  
Five step communication protocol with symmetric key, usually known as the 
Needham-Schroeder protocol used to secure and insecure network.  

The protocol involves the principles:  
* A (initiator)  
* B (responder)  
* S (auth server)  

## Operation  
1. Machine A sends message to server S, intending to communicate with Machine B,
where Na is a nonce generated by Machine A.  
A -> S : A, B, Na  
2. Server S sends message back to the machine A containing Kab, and one
encrypted copy of Kab which will be sent to Machine B by the Machine A. Ka and
Kb are the keys of A and B shared with S, respectively. Kab is a secret session
key for A and B provided by S.  
S -> A : {Na, B, Kab, {Kab, A} Kb} Ka  
3. Machine A forwards {Kab, A}Kb, encrypted copy of Kab from step 2, to the
machine B. With the key it has, Machine B can decrypt it. This is because the
message has been encrypted by S with the symmetric key Kb of the Machine B.  
A -> B : {Kab, A}Kb  
4. Machine B replies to the Machine A by sending a nonce value encrypted by Kab,
confirming that Machine B has the secret session key provided by the server S.  
B -> A : {Nb}Kab  
5. Machine A then performs a simple operation on Machine B's nonce and returns 
it to the Machine B just to verify that Machine A has the key.  
A -> B : {Nb-1}Kab  
  
Model as a concurrent system in NuSMV the protocol described above as the
interaction of processes, A, B, S, I. Multiple sessions may overlap,
asynchronously. However, to ensure finiteness of the model state space, consider
a maximum number of n sessions, and verify the satisfaction properties above
under such a limitation.  
  
## Mark Distribution  
1. Design and draw a state transition diagram of the system considering four
processes mentioned above. Note that this should be a high-level transition
system just showing the interaction of the processes and not the NuSMV 
transition (which will be very complex, unable to draw.)   
(10 MARKS)  
2. In your NuSMV model all the processes should work concurrently, and in an
asynchronous manner.   
(30 MARKS)  
3. Identify and express four authentication and secrecy properties using either
CTL or LTL.  
(4 * 5 = 20 MARKS)  
4.Verify all the properties above.  
(4 * 5 = 20 MARKS)  
5. Q/A session during the demo.  
(20 MARKS)  
  
## Deliverables  
Submit a portfolio via Blackboard on or before December 5, 2019 1400h.  
Submit a zip file with NuSMV code (.smv) along with a PDF file containing the
system design and state transition diagram, and the verification results showing
the execution logs. The logs should clearly show verified properties as "true"
or "false". In the case of "false" it must show a counter example.