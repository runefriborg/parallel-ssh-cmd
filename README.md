parallel-ssh-cmd
================

A fast parallel SSH with output for simple parsing

This can be trusted for automatic system administration tasks and has
an output which is easy to parse.

Main features:
 - Does unordered parallel execution
 - Set timeout for connection and commands. Retrieve output for failed executions.
 - Set maximum size of thread pool
 - Accepts filename with hosts or list of nodes
 - [todo] Use regexp to select set of nodes from list of nodes
 - Supports only SSH with the use of password-less logins
 - [todo] Ctrl-C behaviour similar to pdsh
 - No streaming output from executions, but partial output on timeouts
 - Outputs stderr to stderr and stdout to stdout

Outputs:

Hostname output from host
  1/10    s01n11    0    s01n11.genomedk.net

No output from host
  1/10    s01n11    0    

Multiple lines output from host
  1/10    s01n11    0    Updating...
  1/10    s01n11    0    DONE

Timeout of host
  1/10    s01n11    -9  Connection timeout

[index]  [node]  [returncode]  [output]

Author: Rune Moellegaard Friborg <runef@birc.au.dk>

