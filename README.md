# nev
 
```

<?php
 
require_once('/functions.php');
require_once('/Amazon/SQS/Queue.php');
 
ini_set('display_errors',0);
print "Parent : ". getmypid() . "\n";
 
global $pids;
$pids = Array();
 
// Daemonize
$pid = pcntl_fork();
if ($pid) {
    // we are the parent or an could not fork
    exit();
}

function handler ($signo) {
    global $pids, $pidFileWritten;
    if ($signo == SIGTERM || $signo == SIGHUP || $signo == SIGINT) {
        // If we are being restarted or killed, quit all children
 
        // Send the same signal to the children which we recieved
        foreach ($pids as $p) { 
            posix_kill ($p,$signo); 
        }
 
        // let the processes exit
        foreach ($pids as $p) { 
            pcntl_waitpid($p,$status); 
        }
        exit();
    } else if ($signo == SIGUSR1) {
        print "I currently have " . count($pids) . " children\n";
    }
}
```
