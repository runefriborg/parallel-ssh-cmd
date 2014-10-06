parallel-ssh-cmd
================

A fast parallel SSH with output for simple parsing.

The goal is to make it trustworthy for automatic system administration tasks and output results in a format which is easy to parse.

Features to ease parsing
 - Every written line includes hostname and returncode for command. Search for commands with returncode != 0 to detect faults.
 - For every command executed, at least one line is written to stdout. In case the command outputs nothing:
        1/1  s03n11         0  

<h4>Main features</h4>
 - Does unordered parallel execution
 - Set timeout for connection and commands. Retrieve output for failed executions.
 - Set maximum size of thread pool
 - Accepts filename with hosts or list of nodes
 - Use regexp to select set of nodes from list of nodes
 - Supports only SSH with the use of password-less logins
 - Provides continuesly updated status of all current connections
 - No streaming output from executions, but partial output on timeouts
 - Outputs stderr to stderr and stdout to stdout

<h4>Examples</h4>

Never wait more than 10 seconds for the command to finish on a node.

```html
> parallel-ssh-cmd -c 10 -t 10 -f /com/etc/computenodes.genomedk /com/sbin/slurm-sanity-check
   1/152  s01n12         1  
   2/152  s01n11         1  
   3/152  s01n14         1  
   [snip]
 150/152  s09n01         0  
 151/152  s09n02         0  
 152/152  s03n68         0  
```

```html
>  parallel-ssh-cmd -n s01n11,s01n12,s01n13 hostname
     1/3  s01n13         0  s01n13.genomedk.net
     2/3  s01n12         0  s01n12.genomedk.net
     3/3  s01n11         0  s01n11.genomedk.net
```

```html
> parallel-ssh-cmd -n santaclaus hostname
     1/1  santaclaus   255  
     1/1  santaclaus   255  ssh: Could not resolve hostname santaclaus: Name or service not known

> parallel-ssh-cmd -n santaclaus hostname 2> /dev/null
     1/1  santaclaus   255  
> parallel-ssh-cmd -n santaclaus hostname > /dev/null
     1/1  santaclaus   255  ssh: Could not resolve hostname santaclaus: Name or service not known
```

Multiple lines output from host
```html
> parallel-ssh-cmd -n s01n11,s01n12 ps   
     1/2  s01n12         0  PID TTY          TIME CMD
     1/2  s01n12         0  18635 ?        00:00:00 sshd
     1/2  s01n12         0  18636 ?        00:00:00 bash
     1/2  s01n12         0  18662 ?        00:00:00 ps
     2/2  s01n11         0  PID TTY          TIME CMD
     2/2  s01n11         0   6176 ?        00:00:00 sshd
     2/2  s01n11         0   6177 ?        00:00:00 bash
     2/2  s01n11         0   6203 ?        00:00:00 ps
```

Execute one command at a time in order
```html
> parallel-ssh-cmd -p 1 -n s01n11,s01n12,s01n13 hostname
     1/3  s01n11         0  s01n11.genomedk.net
     2/3  s01n12         0  s01n12.genomedk.net
     3/3  s01n13         0  s01n13.genomedk.net
```

Output format:
```html
[index]  [node]  [returncode]  [output]
```

<h4>Help</h4>
```html
> parallel-ssh-cmd -h
parallel-ssh-cmd version 1.01 by Rune M. Friborg (updated 2014-06-12)
Usage:
  parallel-ssh-cmd <parameters> <command>

A fast parallel SSH with output for simple parsing

Run a command on N nodes in parallel

Parameters
  -c, --connect-timeout  Set timeout on SSH connection phase (default 10 seconds)
  -t, --command-timeout  Set timeout for command phase
  -p, --max-threads      Maximum amount of parallel threads
  -f, --nodefile         File with list of nodes (one node per line)
  -n, --nodelist         Comma separated list of nodes (supplements -)
  -r, --regexp           Filter nodelist using Python-style regular expression
  -s, --status           Output status message of all connections (use for interactive sessions)

  -h, --help      This help message
```

Author: Rune Moellegaard Friborg <runef@birc.au.dk>

