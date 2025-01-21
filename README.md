# Zeek Installation Guide

This guide will walk you through the steps to install Zeek on Ubuntu via the terminal.

## Prerequisites

- Ubuntu OS
- Firefox browser (for downloading the Zeek source code)

## Steps to Install Zeek

### 1. Download Zeek Source Code

1. Open Firefox inside Ubuntu.
2. Navigate to the Zeek website and download the latest Zeek source code tarball (usually `.tar.gz` file).
3. Save the file to your `Downloads` folder.

### 2. Install Required Dependencies

Open a terminal and execute the following commands to install the required dependencies:

```bash
sudo su
Enter password
apt-get update
apt-get upgrade
apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python3-dev swig zlib1g-dev
```

### 3. Extract the Zeek Tarball

Navigate to the `Downloads` directory to locate the Zeek tarball:

```bash
cd ~/Downloads
tar -xzf /path/to/zeek-file.tar.gz
```

### 4. Configure and Compile Zeek

1. Change to the extracted Zeek directory:

   ```bash
   cd zeek
   ```

2. Run the following command to configure Zeek:

   ```bash
   ./configure
   ```

3. Run the following commands to compile and install Zeek:

   ```bash
   make
   sudo make install
   ```

### 5. Set up Zeek Environment

1. Navigate to the Zeek installation directory:

   ```bash
   cd /usr/local/zeek
   ```

2. Verify the existence of the `bin` directory by running:

   ```bash
   ls
   ```

3. Add the Zeek `bin` directory to your `PATH`:

   ```bash
   nano ~/.bashrc
   ```

   At the end of the file, add the following line:

   ```bash
   export PATH=/usr/local/zeek/bin:$PATH
   ```

4. Save and close the file, then apply the changes:

   ```bash
   source ~/.bashrc
   ```

### 6. Configure Zeek's Node Settings

1. Navigate to the `etc` directory:

   ```bash
   cd /usr/local/zeek/etc
   ```

2. Edit the `node.cfg` file:

   ```bash
   nano node.cfg
   ```

3. Check the `localhost` and `interface` settings. You can find your network interfaces by running:

   ```bash
   ip a
   ```

   Choose the correct interface (e.g., `enp0s1`) and paste it into the `interface` section of `node.cfg`.

### 7. Verify Zeek Installation

#### Step 1: Check Zeek Configuration

Run the following command to ensure Zeek is configured correctly:

```bash
zeekctl check
```

You should see the message: `zeek scripts are ok`.

#### Step 2: Deploy Zeek

Run the following command to deploy Zeek:

```bash
zeekctl deploy
```

#### Step 3: Test Zeek's Functionality

To confirm that Zeek is capturing network traffic as expected, follow these steps:

1. Generate some network traffic using tools like `ping` or by visiting a website in your browser:

   ```bash
   ping google.com
   ```

2. Check Zeek logs to ensure traffic is being logged. Navigate to the logs directory:

   ```bash
   cd /usr/local/zeek/logs/current
   ```

3. Verify the presence of log files, such as `conn.log`, `http.log`, or `dns.log`, which indicate Zeek is capturing and analyzing network traffic:

   ```bash
   ls
   ```

4. View the contents of a log file to confirm Zeek is working as expected:

   ```bash
   cat conn.log
   ```

   You should see details of network connections, such as source and destination IPs, ports, and protocols.

#### Step 4: Run a Sample Zeek Script

To further verify that Zeek is functioning, run a simple Zeek script:

1. Create a file called `test.zeek` in any directory:

   ```bash
   nano test.zeek
   ```

2. Add the following lines to the file:

   ```zeek
   event zeek_init() {
       print "Zeek is running successfully!";
   }
   ```

3. Run the script with Zeek:

   ```bash
   zeek test.zeek
   ```

   You should see the message `Zeek is running successfully!` printed in the terminal.

## Troubleshooting

### Permission Issues

If you encounter errors such as:

```
Error: failed to write file: [Errno 13] Permission denied: '/usr/local/zeek/spool/.zeekctl-config.sh.tmp'
Error: failed to read lock file: [Errno 2] No such file or directory: '/usr/local/zeek/spool/lock'
Error: Unable to get lock
```

These errors indicate that Zeek lacks the required permissions to access the necessary files. To resolve this:

1. Ensure your user has the appropriate permissions by running:

   ```bash
   sudo chown -R $USER:$USER /usr/local/zeek
   ```

2. Ensure that the Zeek lock directory exists:

   ```bash
   sudo mkdir -p /usr/local/zeek/spool/lock
   ```

3. If you encounter the following error:

   ```
   Error: failed to read lock file: [Errno 21] Is a directory: '/usr/local/zeek/spool/lock'
   ```

   This indicates that Zeek is treating the lock as a directory. To fix this:

   ```bash
   sudo rm -rf /usr/local/zeek/spool/lock
   sudo touch /usr/local/zeek/spool/lock
   sudo chown $USER:$USER /usr/local/zeek/spool/lock
   ```

### Unable to Stop Zeek

If you encounter the following error after running `zeekctl deploy`:

```
Error: unable to stop zeek: /usr/local/zeek/share/zeekctl/scripts/helpers/stop: line 5: kill: (44192) - Operation not permitted
```

1. Check if Zeek is still running:

   ```bash
   ps aux | grep zeek
   ```

2. If any Zeek processes are listed, kill them using the `kill` command. Replace `ID` with the process ID (PID) from the `ps` command output:

   ```bash
   sudo kill ID
   ```

## Conclusion

You have now successfully installed and verified Zeek on Ubuntu. If you run into any other issues, feel free to consult the Zeek documentation or ask for help.



