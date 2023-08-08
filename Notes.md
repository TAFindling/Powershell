#ACCESSING THE WINDOWS MACHINE
xfreerdp /u:student /v:10.50.42.187 -dynamic-resolution +glyph-cache +clipboard

#COMMANDS
  get-command                                                       ##lists all commands##
  get-verb                                                          ##lists all verbs##
  get-command -verb get                                             ##lists all commands with verb get##
  get-command -noun process                                         ##lists all commands with noun process##
  tasklist                                                          ##runs an outside program to get the task list##
  get-eventlog -logname 'windows powershell' -Verbose              
  get-childitem -path C:\users\student\desktop                      ##returns all objects within the given filepath##
  get-help                                                          ##displays the powershell help file##
  update-help                                                       ##updates the help file##
  get-help get-printer                                              ##shows the help page for the get-printer command##
  get-help get-printer -full                                        ##shows the full help file for the given command##
    get-help get-printer -examples                                  ##shows examples of command##
    get-help *ip*                                                   ##shows every command that can be run with IP in it##
    get-help about_pipelines                                        ##shows everything about pipelines##
    " " -showwindow                                                 ##opens the about page in a new window##
    " " -online                                                     ##goes to the help page on the windows website##
  get-alias                                                         ##lists aliases##
  get-alias -definition get-childitem                               ##lists all aliases of get-childitem
  set-alias                                                         ##creates a new alias##
    set-alias edit notepad.exe                                      ##creates an alias called edit that opens notepad.exe##
  remove-item alias:edit                                            ##removes the alias edit##
  (get-process).processname                                         ##displays the process name for each object found in get-process##
  (get-process notepad).kill()                                      ##gets the process id of notepad then kills the task
  get-process | get-member                                          ## ##
  read-host                                                         ##prompts the user for input##












#EXAMPLES
##DAY 1
Ensure that you have the latest copy of help by updating your help system.
  update-help
Which cmdlets deal with the viewing/manipulating of processes?
  get-help *process*, get-command -noun process
Display a list of services installed on your local computer.
  get-service
What cmdlets are used to write or output objects or text to the screen?
  write-host, write-output
What cmdlets can be used to create, modify, list, and delete variables?
  set-variable, get-variable, remove-variable, new-variable
What cmdlet can be used, other than Get-Help, to find and list other cmdlets?
  get-command
Find the cmdlet that is used to prompt the user for input.
  read-host
Display a list of running processes.
  get-process
Display a list of all running processes that start with the letter "s".
  get-process -name s*
Find the cmdlet and its purpose for the following aliases:
  get-alias
gal

dir

echo

?

%

ft

Display a list of Windows Firewall Rules.
  get-netfirewallrule
Create a new alias called "gh" for the cmdlet "Get-Help"
  new-alias -name "gh" get-help
Create a variable called "var1" that holds a random number between 25-50.
  $var1 = get-random -minimum 25 -maximum 50
Create a variable called "var2" that holds a random number between 1-10.
  $var2 = get-random -minimum 1 -maximum 10
Create a variable called "sum" that holds the sum of var1 and var2.
  $sum = $var1 + $var2
Create a variable called "sub" that holds the difference of var1 and var2.
  $sub = $var1 - $var2
Create a variable called "prod" that holds the product of var1 and var2.
  $prod = $var1 * $var2
Create a variable called "quo" that holds the quotient of var1 and var2.
  $quo = $var1 / $var2
Replace the variables in text with their values in the following format:

"var1" + "var2" = "sum"
write-host -nonewline $var1 + $var2 = $sum
Replace the variables in text with their values in the following format:

"var1" - "var2" = "sub"
write-host -nonewline $var1 - $var2 = $sub
Replace the variables in text with their values in the following format:

"var1" * "var2" = "prod"
write-host -nonewline $var1 * $var2 = $prod
Replace the variables in text with their values in the following format:

"var1" / "var2" = "quo"
write-host -nonewline $var1 / $var2 = $quo

##DAY 2
