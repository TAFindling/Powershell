#ACCESSING THE WINDOWS MACHINE
xfreerdp /u:student /v:10.50.42.187 -dynamic-resolution +glyph-cache +clipboard

#COMMANDS
##DAY 1
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

##DAY 2
$x = get-process
$a = "cat dog"
($x).gettype()                                                      ##returns the variable type of the called variable##
$x -is [array]                                                      ##tests if the variable is of a certain item type, returns true/false##

$x.count                                                            ##returns the number of items in the output##
$array1 = 1,(get-date),"dog",8,7
$array = @()                                                        ##creates an empty array##
$array1[2..4]                                                       ##returns all items in the range of the index positions noted
$array1[$array.length -1]                                           ##returns the number of elements in the array

$jagarray = "joe", "jim, "jan", (1, ('apple', 'pear'), 3), "jay"    ##array within an array
$jagarray[3][0]                                                     ##would call 1 from the array above##

$mylist = @{First = "John"; Last = "Doe"; Mid = "Bon"; Age = 35}    ##Creates a dictionary containing the values listed within
$mylist["Last"]                                                     ##Returns the value of last within the dictionary
$mylist.First                                                       ##returns the value of First whithout
$mylist["First", "Mid", "Last"]                                     ##
$mylist.values                                                      ##returns all of the values contained within the dictionary
$mylist["keys"]
#addi#c

#Script Blocks
$myblock = {get-service | format-table name,status}
& $myblock

#Sorting and Grouping
Get-ChildItem 
#Sort by File Length
gci | Sort-Object -Property length
gci | Sort length
Get-ChildItem | sort -Descending

#Grouping
Get-Service | Group-Object -Property status
Get-ChildItem | group-object {$_.length -lt 1kb}

#Sorting
1, 7, 10, 8, 4, 6 | Sort-Object
"1", "7", "10", "8", "4", "6" | Sort-Object

#get-random
1..10 | Sort-Object -Property {Get-Random}
1..10 | Get-Random

#select-object
Get-Process | Select-Object -First 10
Get-Process | Sort-Object -property starttime | Select-Object -last 10 | ft processname, starttime

#Filtering Results/Where-Object
Get-Service | Where-Object {$_.Status -eq 'running'}

Get-ChildItem *.txt | Where-Object {$_.length -gt 100}
Get-Process | Where-Object {$_.name -notlike 'powershell*'} | sort id -Descending

#Additional Commands
#get-unique
1,2,3,1,2,3,1,2,3,1,2,3 | Sort-Object | Get-Unique

#measure-object
Get-ChildItem | Measure-Object -property length
(Get-ChildItem).count
Get-ChildItem | Measure-Object -Property length -Average -Maximum -Minimum -sum

#compare-object
'this is a string of words, whatever whatever' > comparetest.txt
$before = Get-ChildItem
'40' > comparetest.txt
$after = Get-ChildItem
Compare-Object $before $after -Property length, name

#Create Objects
$MyTruck = New-Object object

#add properties using add-member
Add-Member -MemberType NoteProperty -name Color -value red -InputObject $mytruck
($MyTruck).color

#Add property shorthanded
Add-Member -me NoteProperty -in $mytruck -na make -Value ford

#Same Same w/ Positional Example
Add-Member -InputObject $mytruck NoteProperty model "f-150 raptor"

#Same But Through Pipeline
$MyTruck | Add-Member NoteProperty cab supercrew
$MyTruck | Add-Member -membertype NoteProperty -name cab -value supercrew
$MyTruck.cab

#Adding Methods
Add-Member -MemberType ScriptMethod -InputObject $MyTruck -name drive -value { "going on a road trip" }
$mytruck.drive()

#Creating an Object Using PSCustomObject
$Soldier = [PSCustomObject]@{
    "FirstName"    = "Joe"
    "LastName"     = "Snuffy"
    "MilitaryRank" = "SSG"
    "MOS"          = "17C"
    "Position"     = "Host Analyst"
}
$Soldier

Get-Process | Sort-Object starttime | Select-Object -First 1 -Last 1 | ft processname, starttime

Get-Date | ft DayofWeek

Get-HotFix

Get-HotFix | Sort-Object -Property Description | ft Description, HotFixID, InstalledOn

$ComputerInformation = [PSCustomObject]@{
    "ComputerName" = (Get-WmiObject Win32_ComputerSystem).name
    "OperatingSystem" = (get-ciminstance win32_operatingsystem).Caption
    "Version" = (Get-WmiObject Win32_OperatingSystem).Version
    "Manufacturer" = (get-ciminstance win32_operatingsystem).Manufacturer
    "Disks" = Get-WmiObject win32_logicaldisk

}

#Conditions/True/False
#Comparison Operators
-eq -ceq -ieq (equals/case sensitive equals/case insensitive equals)
#-li (like) uses wildcards, -eq does not
#-match regex
-replace
-contains
-in / -notin

2 -eq 2
2 -eq 3
1,2,3 -eq 2
"abc" -eq "abc"
"abc" -eq "cba"
"abc", "cba" -eq "abc"
6 -lt 9
7, 8, 6, 9 -lt 8
"powershell" -like "*shell"
"powershell" -eq "*shell"
"powershell", "server" -like "*shell"

#Contains
$fruit = 'apple', 'orange', 'tomato'
$fruit -contains 'tomato'

#Combo
$num = 5
(($num -gt 1) -and ($num -lt 10))

#If Statements
$x = 11
if($x -gt 10){"$x is larger than 10"}
if($x -gt 12){"$x is larger than 10"}

#Else
if($x -gt 10){ write-host "$x is larger than 10"}
else { write-host "$x is not larger than 10"}

#Switch
$val = "Meg"
Switch($val){
    Peter { "That's the father" }
    Lois { "That's the mother" }
    Stewie { "That's the youngest child" }
    Chris { "That's the oldest child" }
    default { "Nobody cares about you $val" }
}

#Loops
#forEach-object Items passed through the pipeline
$nums= 1,2,3,4,5
$nums | ForEach-Object {$_ * 2}

$list = 'a','b','c'
$list | ForEach-Object {$_.Toupper()}

#foreach
#Note pipeline, needs to finish iterating through list to print results
foreach ($item in gci C:\ -Recurse){$item.name}
Get-ChildItem C:\ -Recurse | ForEach-Object{$_.name}

foreach ($num in 1..5){$num * 2}

$teams = "Tigers", "Pistons", "Red Wings", "Lions"
foreach($team in $teams){"Detroit $team"}

#While (executes while the condition is true)
$num = 0
while ($num -lt 3){
    $num
    $num++
}

$var == ""
while($var -ne "Marines"){
    $var = Read-Host "Which branch of the military is the best?"
}

#Do While
$num = 0
do {
    $num
    $num++
}
while($num -lt 3)

#Do Until - goes until condition is met
$num = 0
do {
    $num
    $num++
}
until($num -gt 3)

#for
for($num=1; $num -le 3; $num++){$num}
for($i = 0; $i -le 255; $i++){
    write-host 192.168.0.$i
}

#Read Files
Get-Content .\example.txt
cat .\example.txt

#Write File
Set-Content -path .\example.txt -value "a string" #Overwrites Files
Add-Content -path .\example.txt -value "another string" #Appends to File

#DAY 3
$text1 = "one Terabyte is $(1TB / 1GB) Gigabytes"
####BACKTICKS
"Here is `$cat `"quotes`"" #backticks make special characters work
`n #newline
`t #tab character

####HERE STRING
$text = @"
Here is some text with "quotes".
1TB equals $(1TB / 1GB) GB.
Variables are resolved
"$PWD" is your current path
"@

####


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
Create an array containing a range with a random starting and stopping point. The starting point will be a random number from -10 through 0. The ending point will be a random number from 1 through 20.
  $a= get-random -minimum -10 -maximum 0
  $b= get-random -minimum 1 -maximum 20
  $c= $a..$b
Create a variable that contains the contents of the array in reverse
  echo [array]::Reverse($c)

Create two empty hash tables with the following names:

employee1

employee2

Using the following table of key-value pairs, apply each key-value to the empty hash tables.

Table 1. keys and values
First	Last	ID	Job
Mary

Hopper

001

Software Developer

John

Williams

002

Web Developer

The keys on the first row of the table while the values are on the first column of the table

Now add a new key called Username which holds a contraction of the employee’s first initial then last name then ID. (i.e. mhopper001).

Mary got promoted to "Software Lead" so the job key for Mary needs to be changed to "Software Lead"

Create a new hash table called "employee3" that contains the following values with the respective keys.

Table 2. employee3
First	Last	ID	Job
Alex

Moran

003

Software Developer

Add a new key called "Status" that holds the values:

Table 3. Status
Mary	John	Alex
Management

Intermediate

Entry Level

Make sure you use both dot notation and square brackets to manipulate your hash tables.

$employee1 = [ordered]@{}
$employee2 = [ordered]@{}

$emplyee1.First = "Mary"
$employee1["Last"] = "Hopper"
$employee1.ID = "001"
$employee1["Job"] = "Software Developer"
$employee1.Username = "mhopper001"
$employee1.Job = "Software Lead"
$employee1.Status = "Management"

$emplyee2.First = "John"
$employee2["Last"] = "Williams"
$employee2.ID = "002"
$employee2["Job"] = "Web Developer"
$employee2.Username = "jwilliams002"
$employee2.Status = "Intermediate"

$emplyee3.First = "Alex"
$employee3["Last"] = "Moran"
$employee3.ID = "003"
$employee3["Job"] = "Software Developer"
$employee3.Username = "amoran003"
$employee3.Status = "Entry Level"

Display the start time of the earliest and latest running processes
  Get-Process | Sort-Object starttime | Select-Object -First 1 -Last 1 | ft processname, starttime
Identify a cmdlet that returns the current date and time then using this cmdlet and Select-object, display only the current day of the week
  Get-Date | Select-Object DayofWeek or (get-date).DayOfWeek
Identify a cmdlet that displays a list of installed hotfixes.
  get-hotfix
Extend the expression to sort the list by install date, and display only the install date and hotfix ID.
  Get-HotFix | Sort-Object -Property InstalledOn | ft InstalledOn, HotFixID
Extend the expression further, but this time sort by description, include description, hotfix ID, and install Date.
  Get-HotFix | Sort-Object -Property Description | ft Description, HotFixID, InstalledOn

Create a custom object that contains information about the host system using the following:

Win32_ComputerSystem

Win32_BIOS

Win32_OperatingSystem

Win32_LogicalDisk

Use the cmdlet Get-WmiObject to obtain the needed information

Store the objects results into variables to be used in the custom object

The custom object will only contain properties not methods

The final output of this exercise should look like the following:

Endstate
ComputerName    : DESKTOP-5KJDVS2
OperatingSystem : Microsoft Windows 10 Pro
Version         : 10.0.17134
Manufacturer    : Dell Inc.
Disks           : {\\DESKTOP-                          5KJDVS2\root\cimv2:Win32_LogicalDisk.DeviceID="C:",
                  \\DESKTOP-5KJDVS2\root\cimv2:Win32_LogicalDisk.DeviceID="D:"}

$ComputerInformation = [PSCustomObject]@{
    "ComputerName" = (Get-WmiObject Win32_ComputerSystem).name
    "OperatingSystem" = (get-ciminstance win32_operatingsystem).Caption
    "Version" = (Get-WmiObject Win32_OperatingSystem).Version
    "Manufacturer" = (Get-WmiObject win32_BIOS).Manufacturer
    "Disks" = Get-WmiObject win32_logicaldisk
}         

Output:
  ComputerName    : WIN-OPS
  OperatingSystem : Microsoft Windows 10 Pro
  Version         : 10.0.19045
  Manufacturer    : Microsoft Corporation
  Disks           : {\\WIN-OPS\root\cimv2:Win32_LogicalDisk.DeviceID="C:", \\WIN-OPS\root\cimv2:Win32_LogicalDisk.DeviceID="D:"}

Find and extract the model number from the provided lines of text. If there isn’t a model number then display to the user that a model number wasn’t found

Check both lines for model numbers and report individually the line and model number if found.

Use the following variables for your script
$line1 = "Do you have model number: MT5437 for john.doe@sharklasers.com?"
$line2 = "What model number for john.doe@sharklasers.com?"
Exercise Criteria
Must use at least ONE comparison operator

Must use at least ONE If Condition OR Switch Statement

Reminder of options available
Switch Statement
If/IfElse/Else Condition
match, contains, in, -eq, -le, etc…​
Comparison combinations

$line1 = "Do you have model number: MT5437 for john.doe@sharklasers.com?"
$line2 = "What model number for john.doe@sharklasers"
$val = "MT5437"
if($line1.contains($val)){
    write-host "$line1"
    Write-Host "Model Number found: $val"
}
else{
    Write-Host "Model Number Not Found"
    }
if($line2.Contains($val)){
    write-host "$line2"
    Write-Host "Model Number found: $val"
}
else{
    Write-Host "Model Number Not Found"
    }

Part 1

Use an array to iterate and open the following

Notepad

MS Edge

MSpaint

Query the processes

Kill the processes from PowerShell

  

Part 2

Use an array to iterate and open the following

Notepad

MS Edge

MSpaint

Query the processes

Save the ProcessIDs to a text file called procs.txt

Iterate through the ProcessIDs in the text file and kill them

  $procs = @('notepad','msedge','mspaint')
  foreach( $process in $procs){}

Part 3

Use an array to iterate and open the following

Notepad

MS Edge

MSpaint

Query the processes and display only the following information in order by process ID

Process ID

Process Name

The time the process started

The amount of time the process has spent on the processor

The amount of memory assigned to the process

  
