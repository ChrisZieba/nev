# nev
 
```
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
