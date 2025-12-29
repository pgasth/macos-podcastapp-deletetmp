# Cleaning up the temporary files of the podcast app 

Sometimes Apple's Podcast app does not automatically delete temporary files (~/Library/Containers/com.apple.podcasts/Data/tmp/StreamedMedia) that it needs to maintain continuity functionality. Over time, this leads to significant memory consumption, which is displayed in the system settings under “System Data”. With this solution, you don't have to sacrifice the convenience of Continuity or storage space. 

## Solution

A shell script deletes files in the above directory that are older than 24 hours. The script can either be executed manually or automated using “launchd.” 

## Setup

### 1. Download and customize the script
Since the “launchd” system process uses a pristine shell without system variables, the path specified in the script must be customized by inserting the home directory. The script should then be saved in a location of your choice.

### 2. Authorize the Bash interpreter
The lauchd process also does not inherit any permissions. In order for the script to be executed, the Bash interpreter must be equipped with the necessary “Full Disk Access.” This is done via the system settings: Privacy & Security -> Full Disk Access -> “+” -> select the Bash binary in the “/bin” directory. 

### 3. Configure Launch Demon using Launch Control
To ensure that the script is executed automatically on a regular basis, launchd must be configured using launchctl.

3.1 To do this, download the .plist file and customize it. It is important to include the path to the script and, if necessary, the changed script name. The start interval can also be modified. Further configuration options can be found in the launchctl documentation.
3.2 Save the .plist file in the path “~/Library/LaunchAgents”
3.2. Start the daemon with the command: launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.your.label.plist
