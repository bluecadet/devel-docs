# Persistant SSH connection to Pantheon

Overall we need to create the ssh key on local machine, add it to Pantheon and then make sure the local machines ssh-agent has access to the key.

1. run `ssh-keygen`. (This command works on Linux, MacOS, and Windows 10.)
1. Use defaults unless you absolutely need something different
1. Once the files are created, copy the contents of `~/.ssh/id_rsa.pub` to your clipboard.
    * Linux and Mac users can catthe file to the terminal and copy the output: `cat ~/.ssh/id_rsa.pub`
    * Windows users can achieve the same result with type: `type .ssh\id_rsa.pub`
1. Add SSH key to Pantheon
    * Log in to Pantheon and go to the *Account* page.
    * Click *SSH Keys*
    * Paste the copied public key into the box, and click *Add Key*.
    * Your computer is now set up to securely connect to the Pantheon Git server. You can view a list of available keys on your Pantheon Account page.
1. Add your SSH key to the ssh-agent by running: `ssh-add ~/.ssh/id_rsa`
    * Windows 10 and Cygwin:
    * Open `Manage optional features` from the start menu and make sure you have `Open SSH Client` in the list. If not, you should be able to add it.
    * Open `Services` from the start Menu
    * Scroll down to `OpenSSH Authentication Agent` > right click > properties
    * Change the Startup type from Disabled to any of the other 3 options. I have mine set to `Automatic (Delayed Start)`
    * Open cmd and type where ssh to confirm that the top listed path is in System32. Mine is installed at `C:\Windows\System32\OpenSSH\ssh.exe`. If it's not in the list you may need to close and reopen cmd.
    * In Cygwin, in your bash profile, add:
    ```
    SSHAGENT=/usr/bin/ssh-agent
    SSHAGENTARGS="-s"
    if [ -z "$SSH_AUTH_SOCK" -a -x "$SSHAGENT" ]; then
        eval `$SSHAGENT $SSHAGENTARGS`
        trap "kill $SSH_AGENT_PID" 0
    fi
    ```
    * This will start up ssh-agent for each Cygwin shell you have open. Close your Cygwin shell (if one is open) and open a new one. Now type:
    ```
    ssh-add ~/.ssh/id_rsa
    [enter your password]
    ```

Links:
[Pantheon SSH Keys](https://pantheon.io/docs/ssh-keys)

[How to run ssh-add on windows](https://stackoverflow.com/questions/18683092/how-to-run-ssh-add-on-windows)

[SSH Agent on Cygwin](https://killtheradio.net/how-tos/ssh-agent-on-cygwin/)
