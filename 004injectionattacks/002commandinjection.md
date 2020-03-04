# Command injection
Command injection (or OS Command Injection) is a type of injection where the application that constructs a system command using user supplied input does not correctly sanitize this input. 

# Example
Imagine you have the following code running in your back-end PHP server:

```
<?php
//get the value of the ‘filename’ querystring
$file=$_GET['filename’];
//remove the file
system("rm $file");
?>
```

A client accessing this application via the url https://www.example.org/delete.php?filename=test.txt will effectively delete the file `test.txt`. This may be legitimate use case. However, PHP uses a system command for the delete instruction by passing it unsanitized user input. An attacker accessing the url https://www.example.org/delete.php?filename=test.txt;reboot will be able to reboot the server, since the resulting OS command will be `rm test.txt;reboot`.

# Prevent command injection
Defense measures highly depend on the framework (such as PHP) and the Operation System used. In general, the following precautions should be taken:
* Never call OS commands directly (in the example: avoid using `system("<cmd>")`). Instead, use built-in functions that are available by the framework that is used (in the example: use PHP's `unlink` function).
* Sanitize input (in the example: remove `;`)

# Source attribution
Some parts of this page are based on the [OS Command Injection Defense Cheat Sheet](https://owasp.org/www-project-cheat-sheets/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet), which is licensed under [FLOSS](https://owasp.org/about/).

