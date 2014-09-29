parallel-ssh-cmd
================

A fast parallel SSH with output for simple parsing.

The goal is to make it trustworthy for automatic system administration tasks and output results in a format which is easy to parse.

Features to ease parsing
 - Every written line includes hostname and returncode for command. Search for commands with returncode != 0 to detect faults.
 - For every command executed, at least one line is written to stdout. In case the command outputs nothing:
        1/1  s03n11         0  

Main features:
 - Does unordered parallel execution
 - Set timeout for connection and commands. Retrieve output for failed executions.
 - Set maximum size of thread pool
 - Accepts filename with hosts or list of nodes
 - Use regexp to select set of nodes from list of nodes
 - Supports only SSH with the use of password-less logins
 - Provides continuesly updated status of all current connections
 - No streaming output from executions, but partial output on timeouts
 - Outputs stderr to stderr and stdout to stdout

Outputs:
```html
1/10    s01n11    0    s01n11.genomedk.net

No output from host
  1/10    s01n11    0    

Multiple lines output from host
  1/10    s01n11    0    Updating...
  1/10    s01n11    0    DONE

Timeout of host
  1/10    s01n11    -9  Connection timeout

[index]  [node]  [returncode]  [output]
```

Author: Rune Moellegaard Friborg <runef@birc.au.dk>

