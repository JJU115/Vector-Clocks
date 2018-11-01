# Vector-Clocks

An implementation and demonstration of the Vector Clocks algorithm in Golang.

From Wikipedia: A vector clock is an algorithm for generating a partial ordering of events in a distributed system and detecting causality violations. Just as in Lamport timestamps, interprocess messages contain the state of the sending process's logical clock. A vector clock of a system of N processes is an array/vector of N logical clocks, one clock per process; a local "smallest possible values" copy of the global clock-array is kept in each process, with the following rules for clock updates:

<ul>
  <li>Initially all clocks are zero.</li>
<li>Each time a process experiences an internal event, it increments its own logical clock in the vector by one.</li>
<li>Each time a process sends a message, it increments its own logical clock in the vector by one (as in the bullet above) and then sends a copy of its own vector.</li>
<li>Each time a process receives a message, it increments its own logical clock in the vector by one and updates each element in its vector by taking the maximum of the value in its own vector clock and the value in the vector in the received message (for every element).</li>
</ul>
