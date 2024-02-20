
# Setting Up a Quilibrium Ceremony Client Node

To set up a Quilibrium Ceremony Client node and start acquiring $QUIL, follow these simplified steps. This guide includes session management with `tmux` and comprehensive process management instructions, now ordered to minimize the need for restarts.

## 1. Provision a VPS

Choose a VPS provider and set up a server running **Ubuntu 22.04**.

## 2. Access Your Server

Use SSH to connect to your VPS.

## 3. Install Prerequisite Software

Execute the following commands to install necessary software:

```bash
sudo apt -q update
sudo apt-get install jq tmux -y  # Includes tmux installation
git --version  # Ensure git version is 2.34.1 or later
```

## 4. Install Golang

Download and install Golang with these commands:

```bash
wget https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
sudo tar -xvf go1.20.14.linux-amd64.tar.gz -C /usr/local
echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$GOPATH/bin:$GOROOT/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
go version  # Verify installation with go version go1.20.14 linux/amd64
```

## 5. Clone the Ceremony Client Repo

Clone the repository and navigate to the node directory:

```bash
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
cd ceremonyclient/node
```

## 6. Configure gRPC and Firewall Before Starting the Node

- Ensure the `.config/config.yml` file's `listenGrpcMultiaddr` is set to `/ip4/127.0.0.1/tcp/8337`.
- Configure the firewall to allow necessary traffic:

  ```bash
  sudo ufw allow 22 && sudo ufw allow 8336 && sudo ufw allow 8337 && sudo ufw allow 8338 && sudo ufw allow 8317 && sudo ufw allow 8316 && sudo ufw enable
  sudo ufw status  # Confirm firewall settings
  ```

## 7. Use tmux to Manage Terminal Sessions

Initiate a tmux session for multitasking:

```bash
tmux new -s quilnode
```

## 8. Start the Node in Development Mode

Start the node within the tmux session:

```bash
GOEXPERIMENT=arenas go run ./...
```

Detach from the tmux session with `Ctrl+b` then `d` after confirming the node starts successfully.

## Managing the Node Process

- To find the PID of all running Go processes:

  ```bash
  ps -aux | grep go
  ```

- To stop the node if necessary:

  ```bash
  kill PID
  ```

Replace `PID` with the actual process ID.

## Additional Steps

Proceed with any additional setup steps as required for your node's operation, such as obtaining a node peer ID or checking your $QUIL balance, without the need to restart the node.