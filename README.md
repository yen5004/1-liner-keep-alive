# 1-liner-keep-alive
1-liner script series

# 1-liner keep-alive

#keep alive ping to keep the terminal open

Keeping a connection alive between two computers, particularly in the context of a penetration tester, often involves ensuring that the connection remains active despite potential network timeouts, inactivity disconnects, or other interruptions. Here are several methods that can be used to keep a connection alive through a command-line interface (CLI):

### **1. Using SSH KeepAlive Options**

If you are using SSH to connect to the remote machine, you can configure SSH to send keep-alive messages:

- **Client-Side Configuration**: You can edit the SSH client configuration file (usually **`~/.ssh/config`**) and add the following options:
    
    ```bash
    Host *
        ServerAliveInterval 60
        ServerAliveCountMax 5
    ```
    
    This configuration will send a keep-alive message every 60 seconds, allowing up to 5 missed responses before terminating the connection.
    
- **Command-Line Option**: You can also specify these options directly on the command line when initiating the SSH connection:
    
    ```bash
    ssh -o ServerAliveInterval=60 -o ServerAliveCountMax=5 user@remote_host
    
    ```
    
- **Server-Side Configuration**: You can configure the SSH server to accept keep-alive messages by adding the following to **`/etc/ssh/sshd_config`**:
    
    ```bash
    ClientAliveInterval 60
    ClientAliveCountMax 5
    ```
    
    This will ensure the server sends keep-alive messages every 60 seconds.
    

### **2. Using a Periodic Ping**

A periodic ping can help keep the network connection active by generating network traffic:

```bash
while true; do ping -c 1 remote_host; sleep 60; done
```

This command sends a single ping to **`remote_host`** every 60 seconds. However, this method is less reliable than SSH keep-alive because it does not interact with the session protocol and might not prevent the SSH connection from timing out.

### **3. Using a Persistent Shell with `tmux` or `screen`**

Using terminal multiplexers like **`tmux`** or **`screen`** can help keep sessions alive and allow you to reattach to them if the connection drops:

- **tmux**:
    
    ```bash
    tmux new -s session_name
    ```
    
    If the connection drops, you can reconnect and reattach to the session:
    
    ```bash
    tmux attach -t session_name
    ```
    
- **screen**:
    
    ```bash
    screen -S session_name
    ```
    
    Reattach to the session with:
    
    ```bash
    screen -r session_name
    ```
    

### **4. Using a Reverse Shell with a Persistent Connection**

For some penetration testing scenarios, a reverse shell with a mechanism to maintain the connection can be used. Tools like **`ncat`**, **`msfvenom`**, or custom scripts can help maintain a persistent reverse shell connection.

### **Summary**

For most scenarios, especially when using SSH, configuring the SSH client and server to send keep-alive messages is the best approach. This method is specifically designed to handle network inactivity and prevent timeouts effectively. Using **`tmux`** or **`screen`** can also be beneficial for maintaining session persistence, allowing easy reconnection if the connection drops. A periodic ping can also be used for simple scenarios, though it is less reliable than the other methods.

*************************

one-liner keep-alive ping script

```bash
while true; do ping -c 1 -W 1 10.10.175.254 && sleep 60 && hwclock -l; done
```
