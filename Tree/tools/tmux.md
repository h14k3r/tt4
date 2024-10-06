### **TMUX for Penetration Testers: Comprehensive Guide**

TMUX (Terminal Multiplexer) is an essential tool for penetration testers, as it allows you to manage multiple terminal sessions within a single window. This is invaluable during long engagements, especially when you need to run multiple tools concurrently, automate workflows, or maintain persistent sessions over SSH connections. Let’s walk through everything you need to become a TMUX expert—from installation to advanced customization—specifically tailored to your role as a penetration tester.

### **1. Installation of TMUX**

#### **macOS (via Parallels)**

For macOS, you can easily install TMUX using Homebrew:

1. **Install Homebrew** (if you haven’t already):
    
    bash
    
    Copy code
    
    `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
    
2. **Install TMUX**:
    
    bash
    
    Copy code
    
    `brew install tmux`
    

#### **Kali Linux**

Since TMUX is often pre-installed on Kali Linux, you can verify this by running:

bash

Copy code

`tmux -V`

If it’s not installed, use `apt`:

bash

Copy code

`sudo apt update sudo apt install tmux`

#### **Other Linux Distributions**

For Debian/Ubuntu-based systems:

bash

Copy code

`sudo apt update sudo apt install tmux`

For Red Hat/CentOS/Fedora-based systems:

bash

Copy code

`sudo yum install tmux`

### **2. Basic Usage**

#### **Starting a TMUX Session**

bash

Copy code

`tmux`

This command opens a new TMUX session.

To name your session (helpful in pentesting engagements):

bash

Copy code

`tmux new -s pentest-session`

#### **Managing Windows and Panes**

- **Windows**: Think of these as separate terminal tabs.
    
    - Create a new window: `Ctrl+b c`
    - Switch between windows: `Ctrl+b n` (next) or `Ctrl+b p` (previous)
    - List windows: `Ctrl+b w`
- **Panes**: These divide a window into multiple sections.
    
    - Split pane horizontally: `Ctrl+b %`
    - Split pane vertically: `Ctrl+b "`
    - Navigate between panes: `Ctrl+b arrow-key`

#### **Practical Use Case: Session Persistence**

During long pentesting engagements, TMUX sessions can help maintain persistence even if your SSH connection drops.

- Detach a session: `Ctrl+b d`
- Reattach a session:
    
    bash
    
    Copy code
    
    `tmux attach -t pentest-session`
    

#### **Common Commands**

- **Rename session**: `Ctrl+b $`
- **Kill a session**: `tmux kill-session -t session_name`

### **3. Advanced Usage**

#### **Scripts and Automation in TMUX**

You can use TMUX scripts to open multiple windows and run specific commands for your penetration testing tools. For example, here’s a simple script that creates two windows with tools like `nmap` and `metasploit` running:

bash

Copy code

`#!/bin/bash tmux new-session -d -s pentest tmux rename-window -t pentest:0 'Nmap' tmux send-keys -t pentest:0 'nmap -sP 192.168.1.0/24' C-m  tmux new-window -t pentest:1 -n 'Metasploit' tmux send-keys -t pentest:1 'msfconsole' C-m  tmux attach -t pentest`

Save this as a `.sh` file and execute it to automatically launch your session setup.

#### **Key Bindings**

- **Customize key bindings** in the TMUX configuration file (`~/.tmux.conf`) to make navigation and commands more efficient. For example, bind the `r` key to reload the TMUX config:
    
    bash
    
    Copy code
    
    `bind r source-file ~/.tmux.conf \; display-message "~/.tmux.conf reloaded"`
    

#### **Session Sharing**

TMUX allows you to share a session between multiple users, which is incredibly useful during team-based penetration testing engagements.

- Start a shared session:
    
    bash
    
    Copy code
    
    `tmux new-session -s shared-session`
    
- Others can join the session:
    
    bash
    
    Copy code
    
    `tmux attach-session -t shared-session`
    

### **4. Customization**

#### **Status Bar Customization**

You can customize the TMUX status bar to display useful information during assessments, such as system stats or current time. Here’s a sample `~/.tmux.conf` to change the status bar:

bash

Copy code

`set-option -g status-bg black set-option -g status-fg green set -g status-right "#(date +'%Y-%m-%d %H:%M')"`

This will display the current date and time on the right side of the status bar. You can add other details like CPU usage or the current session name.

#### **Color Schemes**

You can change the color scheme of your TMUX panes and status bar. In `~/.tmux.conf`:

bash

Copy code

`set -g status-bg colour235 set -g status-fg colour136 set -g pane-border-fg colour235 set -g pane-active-border-fg colour51`

#### **Plugins with TPM**

The TMUX Plugin Manager (TPM) allows you to easily manage TMUX plugins. This is useful for pentesters who want to extend TMUX's functionality. Install TPM by adding the following to `~/.tmux.conf`:

bash

Copy code

`# List of plugins set -g @plugin 'tmux-plugins/tpm' set -g @plugin 'tmux-plugins/tmux-sensible'  # Initialize TPM (keep this at the bottom of the config file) run '~/.tmux/plugins/tpm/tpm'`

Then, install TPM:

bash

Copy code

`git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

### **5. Scripting and Automation**

#### **Automating Workflow**

Automate starting multiple pentesting tools in different windows and panes. For example, you can use a script to start `burpsuite`, `gobuster`, and `msfconsole` in different windows:

bash

Copy code

`tmux new-session -d -s pentest tmux send-keys 'burpsuite' C-m tmux split-window -h tmux send-keys 'gobuster dir -u http://target -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt' C-m  tmux new-window -n 'msfconsole' tmux send-keys 'msfconsole' C-m  tmux attach`

This setup will help automate repetitive tasks during penetration testing.

#### **Logging Output**

TMUX allows you to log everything that happens in a pane to a file. Here’s how you can automatically log output for each tool:

bash

Copy code

`tmux pipe-pane -o 'cat >> ~/pentest_logs/pane.log'`

### **6. Efficiency Tips**

- **Fast Pane Switching**: Use `Ctrl+b o` to quickly switch to the next pane, speeding up your workflow.
- **Resize Panes**: Resize panes easily with `Ctrl+b` followed by arrow keys.
- **Copy Mode**: Scroll through your terminal output in copy mode: `Ctrl+b [` and use arrow keys to navigate.

#### **Common TMUX Mistakes**

- **Detached Sessions**: Forgetting about detached sessions can lead to resource usage over time. Regularly check for lingering sessions:
    
    bash
    
    Copy code
    
    `tmux ls`
    
- **Over-customization**: While customization is great, avoid overly complex key bindings that might slow you down.
    

### **Conclusion**

Mastering TMUX will significantly enhance your efficiency as a penetration tester. From managing multiple terminal sessions, automating workflows, to customizing your environment, TMUX allows you to handle complex engagements with ease. Keep practicing with scripts, key bindings, and custom setups tailored to your pentesting needs. This guide provides the foundation—over time, you’ll discover even more advanced uses that will further improve your workflow.