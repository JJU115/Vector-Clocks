# Vector-Clocks

An implementation and demonstration of the Vector Clocks algorithm in Golang.

<b>From Wikipedia:</b> A vector clock is an algorithm for generating a partial ordering of events in a distributed system and detecting causality violations. Just as in Lamport timestamps, interprocess messages contain the state of the sending process's logical clock. A vector clock of a system of N processes is an array/vector of N logical clocks, one clock per process; a local "smallest possible values" copy of the global clock-array is kept in each process, with the following rules for clock updates:

<ul>
  <li>Initially all clocks are zero.</li>
<li>Each time a process experiences an internal event, it increments its own logical clock in the vector by one.</li>
<li>Each time a process sends a message, it increments its own logical clock in the vector by one (as in the bullet above) and then sends a copy of its own vector.</li>
<li>Each time a process receives a message, it increments its own logical clock in the vector by one and updates each element in its vector by taking the maximum of the value in its own vector clock and the value in the vector in the received message (for every element).</li>
</ul>

<br>

Processes are represented by goroutines, the number of these for VectorClocks.go to simulate must be specified when running the .exe: VectorClocks <num_processes>

<br>

Each process has a co-executing goroutine: "clockHolder" which handles all aspects of updating its process's vector timestamp, receiving timestamps from other processes, and sending timestamps to other processes' clockHolders. A global array of channels allows communication between clockHolders and processes. Note that timestamps are zero-based arrays. Index 0 = clock of process 0 and so on.

<br>

There are two results files obtained from running VectorClocks.go, 5PResults.txt and 10PResults.txt, with 5 and 10 simulated processes respectively.

<br>

For the purposes of testing, both test files were run with the restriction that only 1 process is 'active', that is capable of experiencing events, at any given time. All non-running processes that haven't finished are still able to receive and process messages from running processes. The correctness of the Vector Clocks can be verified by examining the output and finding that for any process, any of its event's Vector timestamps are not less than any event's Vector timestamp for all previous processes. For example: If process 8 finishes before process 5 begins then it should be found that for any pair of process 8/process 5 events the timestamp of the process 5 event will have at least one value greater than the corresponding spot in the process 8 timestamp meaning the process 5 event can not definitively be said to come before the process 8 event.
