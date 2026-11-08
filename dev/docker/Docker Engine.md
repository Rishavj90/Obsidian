# Docker Engine (With Analogies)

Docker Engine is the core technology that makes containers work. Let me explain how it actually executes containers and manages images.

## Architecture = A Restaurant Kitchen

Docker Engine is like a restaurant kitchen with different roles:

```
User (You at your computer)
    │
    │ docker run ubuntu bash
    │
    └──→ Docker Client (waiter taking orders)
         │
         │ Sends command via API
         │
         ├──→ Docker Daemon (chef/kitchen manager)
         │    ├─ Manages images
         │    ├─ Creates containers
         │    ├─ Runs containers
         │    ├─ Manages networks
         │    └─ Manages storage
         │
         └──→ Container Runtime (sous chef executing)
              └─ Actually runs the container
```

**The analogy:**

- **You**: Customer ordering food
- **Docker Client**: Waiter who takes your order (CLI commands)
- **Docker Daemon**: Head chef who manages the kitchen
- **Container Runtime**: Sous chefs who execute the actual cooking
- **API**: Communication system between all of them

## The Core Components

### 1. Docker Client = The Waiter

```bash
docker run ubuntu bash
│      │    │
│      │    └─ Command parameter
│      └────── Sub-command
└──────────── Docker Client executable

# Other client commands:
docker build
docker pull
docker push
docker logs
```

**What it does:**

- Sends commands to the daemon via API
- Displays results to you
- Doesn't actually execute anything
- Just a messenger

**The analogy:**

- Waiter takes your order: "I want pasta with garlic"
- Doesn't cook it, just communicates to the kitchen
- Brings results back to your table

### 2. Docker Daemon = The Kitchen Manager

```
dockerd (Docker daemon - always running)
│
├─ Container Manager
│  ├─ Create container
│  ├─ Start container
│  ├─ Stop container
│  └─ Remove container
│
├─ Image Manager
│  ├─ Build images
│  ├─ Pull images
│  ├─ Push images
│  └─ Store images
│
├─ Network Manager
│  ├─ Create networks
│  ├─ Connect containers
│  └─ Manage DNS
│
├─ Storage Manager
│  ├─ Manage layers
│  ├─ Create volumes
│  └─ Handle bind mounts
│
└─ API Server (listens on /var/run/docker.sock)
   └─ Receives commands from client
```

**What it does:**

- Listens for client commands 24/7
- Manages all Docker resources
- Orchestrates the actual execution
- Handles persistence

**The analogy:**

- Head chef manages the kitchen
- Organizes stations, ingredients, staff
- Delegates tasks to sous chefs
- Ensures consistency and quality

### 3. Container Runtime = The Execution Engine

```
runc (containerd-runc)
│
└─ Actually creates and runs the container
   ├─ Sets up namespaces
   ├─ Sets up cgroups
   ├─ Mounts filesystem
   └─ Spawns process
```

**What it does:**

- Low-level container execution
- Talks to Linux kernel
- Creates the isolated environment
- Starts the actual process

**The analogy:**

- Sous chef who actually cooks
- Follows head chef's orders
- Executes the recipe step-by-step
- Produces the actual dish

## How Containers Are Created and Run

When you type `docker run nginx`:

```
Step 1: Client parses command
docker run nginx
├─ Instruction: "run"
├─ Image: "nginx"
└─ Default tag: "latest"

Step 2: Client sends to Daemon via API
{
  "Image": "nginx:latest",
  "Cmd": ["nginx", "-g", "daemon off;"],
  "HostConfig": {...}
}

Step 3: Daemon checks if image exists locally
Does nginx:latest exist? 
├─ YES → Skip to Step 5
└─ NO → Pull from registry (Step 4)

Step 4: Pull image (if needed)
docker pull nginx:latest
├─ Connect to Docker Hub
├─ Download layers
├─ Extract and store locally
└─ Return to Step 5

Step 5: Daemon creates container from image
├─ Generate container ID (UUID)
├─ Create writable layer on top of image
├─ Set up networking
├─ Set up storage mounts
├─ Prepare environment

Step 6: Container Runtime (runc) executes
├─ Create Linux namespaces (isolation)
├─ Create cgroups (resource limits)
├─ Mount filesystem
├─ Execute entrypoint/command
└─ Process runs!

Step 7: Daemon monitors container
├─ Watch for crashes
├─ Handle logs
├─ Restart if policy says so
└─ Track resource usage
```

**Real example with output:**

```bash
$ docker run -d --name webserver -p 8080:80 nginx

# What happens:
# 1. Daemon: "Looking for nginx:latest locally..."
#    ✓ Found locally
# 
# 2. Daemon: "Creating container..."
#    ✓ Container ID: 5a8f9c2b1e3d4f6a
# 
# 3. Daemon: "Setting up networking..."
#    ✓ Assigned IP: 172.17.0.2
#    ✓ Mapped port: 0.0.0.0:8080 → 172.17.0.2:80
# 
# 4. Runtime (runc): "Starting container..."
#    ✓ Process PID: 1 (inside container)
#    ✓ nginx started
# 
# 5. Output: 5a8f9c2b1e3d4f6a

$ docker ps
# Shows: webserver running, port 8080→80
```

## Namespaces = Isolation Magic

Namespaces are Linux kernel features that Docker uses to isolate containers.

### PID Namespace = Process Isolation

```bash
# On Host computer
ps aux
# Shows: Your processes, Docker daemon, other apps
# PID 1 = init system
# PID 42 = Firefox
# PID 3847 = nginx (inside container but visible here)

# Inside Container
ps aux
# Shows: Only container processes
# PID 1 = nginx (looks like it's the main process!)
# PID 3 = bash

# Same process, different view!
# Inside container: Process is PID 1
# Outside container: Process is PID 3847
```

**The analogy:**

- Each container thinks it's the only process running
- Like parallel universes where each has their own Process ID 1
- Prevents containers from seeing each other's processes
- Prevents containers from killing each other

### Network Namespace = Network Isolation

```bash
# Host computer
ip addr show
# Shows: eth0 (real network interface), docker0 bridge
# IP: 192.168.1.100
# Multiple network interfaces visible

# Inside Container
ip addr show
# Shows: eth0 (virtual, unique to this container)
# IP: 172.17.0.2 (from docker bridge)
# Looks like its own machine!

# Same container, multiple network views:
docker exec container1 ip addr
# Shows: 172.17.0.2

docker exec container2 ip addr
# Shows: 172.17.0.3

# Each container thinks it has its own network
```

**The analogy:**

- Each container gets its own fake network interface
- Like having a separate WiFi router for each container
- Containers see different IPs and networks
- Real network isolated from container network

### Mount Namespace = Filesystem Isolation

```bash
# Host filesystem
/
├── bin/
├── etc/
├── home/
│   └── user/
│       └── myapp/
└── var/
    └── lib/
        └── docker/
            ├── images/
            └── containers/

# Container 1 filesystem (mount namespace)
/
├── bin/          ← From image layer
├── etc/
├── app/          ← From COPY command
├── tmp/          ← Writable layer (temp files)
└── var/
    └── log/      ← Volume mount

# Container 2 filesystem (different mount namespace)
/
├── bin/          ← Different view!
├── home/         ← Container's home
└── var/

# Same host, different views!
```

**The analogy:**

- Each container sees a different filesystem root
- Like each container lives in a parallel dimension
- When container modifies files, only its view changes
- Other containers unaffected

### User Namespace = User Isolation

```bash
# Host computer
$ whoami
root

# Inside Container (running as root)
$ whoami
root

# But it's actually:
# Host UID 0 (real root) ← NOT the same as →
# Container UID 0 (fake root, limited powers)

# Advanced mapping (some setups):
# Host UID 100000-165536 ← Mapped to → Container UID 0-65536
# Container can't do real root things
```

**The analogy:**

- Container root user is fake root
- Has limited permissions
- Can't do dangerous things to the host
- Like a security guard wearing a fake uniform

## Cgroups = Resource Limits

```bash
# Without cgroups
docker run myapp
# Container can use: ALL CPU, ALL RAM, ALL disk

# With cgroups
docker run -m 512m --cpus 0.5 myapp
# Container limited to: 512 MB RAM, 0.5 CPUs

# Cgroups prevents resource hogging
┌─────────────────────────┐
│ Host Computer           │
│ 8 CPUs, 16 GB RAM       │
├─────────────────────────┤
│ Container 1             │
│ Limit: 2 CPUs, 4 GB RAM │
│ Current: 1.8 CPUs, 3 GB │
├─────────────────────────┤
│ Container 2             │
│ Limit: 1 CPU, 2 GB RAM  │
│ Current: 0.9 CPU, 1.8 GB│
├─────────────────────────┤
│ Container 3             │
│ Limit: 1 CPU, 2 GB RAM  │
│ Current: 0.3 CPU, 0.4 GB│
├─────────────────────────┤
│ Host processes          │
│ Available: 3.9 CPUs, 5 GB│
└─────────────────────────┘
```

**The analogy:**

- Cgroups are like budget limits
- Each container gets an allowance
- Can't go over budget (kernel enforces)
- Prevents one greedy container from starving others

## Images and Layers = Stacked Blueprints

### How Images are Stored

```bash
# Build command
docker build -t myapp:1.0 .

# Creates image with layers:
Layer 1: ubuntu:20.04 base     (hash: abc123...)
Layer 2: apt-get install curl  (hash: def456...)
Layer 3: COPY app.js .         (hash: ghi789...)
Layer 4: RUN npm install       (hash: jkl012...)

# Image hash = combination of all layers
# Image ID: sha256:mno345...
```

### Where Layers Live

```
/var/lib/docker/overlay2/
│
├── abc123abc123abc123.../    # Layer 1 (base)
│   ├── diff/                 # Changes in this layer
│   │   ├── bin/
│   │   ├── etc/
│   │   └── lib/
│   └── link                  # Short ID reference
│
├── def456def456def456.../    # Layer 2 (curl)
│   ├── diff/
│   │   └── usr/local/bin/curl
│   └── link
│
├── ghi789ghi789ghi789.../    # Layer 3 (app.js)
│   ├── diff/
│   │   └── app/
│   │       └── app.js
│   └── link
│
├── jkl012jkl012jkl012.../    # Layer 4 (npm install)
│   ├── diff/
│   │   └── node_modules/
│   └── link
│
└── xyz789xyz789xyz789.../    # Container layer (writable)
    ├── merged/               # Final view
    ├── work/                 # Temp space
    └── upper/                # Writable changes
```

**The analogy:**

- Each layer is like a chapter in a book
- Layers stored separately on disk
- When container runs, chapters stacked
- Reader (container) sees complete book

### Copy-on-Write in Action

```bash
# Image has file: /etc/nginx/nginx.conf

# Container 1 starts
# Reads /etc/nginx/nginx.conf from Layer 3
# File in memory, not copied

# Container 1 modifies file
# Docker intercepts: "Wait, you're modifying a read-only layer"
# Docker creates copy in Container 1's writable layer
# Modification written to writable layer only
# Original file in Layer 3 unchanged

# Container 2 starts (same image)
# Reads /etc/nginx/nginx.conf from Layer 3
# Sees original, unmodified version
# Doesn't see Container 1's changes

# Result:
# Image on disk: 1 copy of file (500 MB)
# Container 1: Modified version in writable layer
# Container 2: Original version from Layer 3
# Memory/storage saved!
```

**The analogy:**

- It's like a library lending books
- Original book in library (Layer)
- Reader 1 borrows book, writes notes on copy
- Reader 2 borrows same book, sees original
- Library keeps originals pristine

## Container Lifecycle = Birth, Life, Death

```
CREATE
  │
  ├─ Generate container ID
  ├─ Create filesystem
  ├─ Set up networks
  └─ Prepare storage
      │
      ▼
START
  │
  ├─ Set up namespaces
  ├─ Set up cgroups
  ├─ Mount filesystem
  └─ Execute entrypoint/command
      │
      ▼
RUNNING
  │
  ├─ Container process executing
  ├─ Logs being captured
  ├─ Resource usage tracked
  └─ Monitoring for crashes
      │
      ├─→ (Crash/Exit)
      │
      ▼
PAUSED (optional)
  │
  └─ Container frozen, can be resumed
      │
      ▼
STOPPED
  │
  ├─ Process terminated
  ├─ Namespaces cleaned up
  ├─ Cgroups removed
  ├─ But container still exists (can restart)
  └─ Writable layer preserved
      │
      ├─→ RESTART (goes back to START)
      │
      ▼
REMOVED
  │
  ├─ Container deleted
  ├─ Writable layer deleted
  ├─ Storage cleaned up
  └─ Gone forever (unless committed to image)
```

**Commands for each state:**

```bash
# CREATE (implicit in docker run)
docker create ubuntu bash
# Container created but not started

# START
docker start <container-id>
# Existing container starts

# RUNNING
docker run ubuntu bash
# Create + Start in one command

# PAUSE
docker pause <container-id>
# Freeze container (CPU paused)

# UNPAUSE
docker unpause <container-id>
# Resume container

# STOP
docker stop <container-id>
# Graceful shutdown (SIGTERM)

# KILL
docker kill <container-id>
# Immediate termination (SIGKILL)

# REMOVE
docker rm <container-id>
# Delete container permanently
```

## The Actual Execution Process (Deep Dive)

### When you run: `docker run -d --name webserver -p 8080:80 nginx`

```
Step 1: Docker Client parses
┌──────────────────────────────┐
│ docker run -d --name ... nginx│
├──────────────────────────────┤
│ Parsed:                      │
│ - Detached: yes              │
│ - Name: webserver            │
│ - Port: 8080→80              │
│ - Image: nginx:latest        │
└──────────────────────────────┘

Step 2: Send to Daemon API
POST /containers/create HTTP/1.1
{
  "Image": "nginx:latest",
  "HostConfig": {
    "PortBindings": {
      "80/tcp": [{"HostPort": "8080"}]
    }
  },
  "Name": "webserver"
}

Step 3: Daemon processes request
├─ Image check: nginx:latest exists? YES
├─ Container ID: Generate UUID (5a8f9c2b1e3d4f6a)
├─ Writable layer: Create at /var/lib/docker/overlay2/xyz789...
├─ Network: Assign IP 172.17.0.2 on docker0 bridge
├─ Port forwarding: Set up 0.0.0.0:8080 → 172.17.0.2:80
└─ Storage: Prepare volume mounts if any

Step 4: runc creates container process
┌─────────────────────────────────────┐
│ runc create 5a8f9c2b1e3d4f6a        │
├─────────────────────────────────────┤
│ Linux syscalls:                     │
│ - clone() with CLONE_NEWPID        │
│   → Create new PID namespace       │
│ - clone() with CLONE_NEWNET        │
│   → Create new network namespace   │
│ - clone() with CLONE_NEWMNT        │
│   → Create new mount namespace     │
│ - clone() with CLONE_NEWUTS        │
│   → Create new UTS namespace       │
│ - clone() with CLONE_NEWIPC        │
│   → Create new IPC namespace       │
│ - clone() with CLONE_NEWUSER       │
│   → Create new user namespace      │
└─────────────────────────────────────┘

Step 5: Set up cgroups
┌──────────────────────────────────┐
│ cgroup configuration               │
├──────────────────────────────────┤
│ /sys/fs/cgroup/memory/docker/... │
│ - memory.limit_in_bytes: unlimited│
│ - cpuset.cpus: all               │
│ - pids.max: unlimited            │
│ - blkio limits: none             │
└──────────────────────────────────┘

Step 6: Mount filesystem
┌──────────────────────────────────────┐
│ Overlay2 filesystem setup             │
├──────────────────────────────────────┤
│ Lower layers:                        │
│ - ubuntu base layer (read-only)      │
│ - nginx layer (read-only)            │
│                                      │
│ Upper layer (writable):              │
│ - Container changes here             │
│                                      │
│ Mount merged view:                   │
│ - /merged → Complete filesystem      │
│ - Container sees this as root /      │
└──────────────────────────────────────┘

Step 7: Execute entrypoint
┌────────────────────────────────┐
│ runc exec                      │
├────────────────────────────────┤
│ Entrypoint: /usr/sbin/nginx    │
│ Args: [-g "daemon off;"]       │
│ Env: PATH=/usr/local/sbin:...  │
│ Working dir: /                 │
│ User: root                     │
│                                │
│ execve("/usr/sbin/nginx", ...) │
│ PID 1 in container = nginx     │
└────────────────────────────────┘

Step 8: Daemon monitors
┌─────────────────────────────────┐
│ Docker Daemon                   │
├─────────────────────────────────┤
│ - Watch process PID 3847 (host) │
│ - Capture logs (stdout/stderr)  │
│ - Track resource usage          │
│ - Monitor for crashes           │
│ - Handle restart if policy      │
│ - Respond to api requests       │
│   (docker logs, docker exec)    │
└─────────────────────────────────┘

Step 9: Container running
┌──────────────────────────────┐
│ Inside Container             │
├──────────────────────────────┤
│ Sees:                        │
│ - PID namespace: nginx=PID 1 │
│ - Network: 172.17.0.2 (eth0) │
│ - Filesystem: / = merged     │
│ - Can't access host:         │
│   - Other processes          │
│   - Host network interfaces  │
│   - Host filesystem          │
│ - Listening on port 80       │
└──────────────────────────────┘

Step 10: Connection through port mapping
┌──────────────────────────────────────┐
│ User connects to localhost:8080      │
├──────────────────────────────────────┤
│ 0.0.0.0:8080 → Docker port mapping   │
│ ↓                                    │
│ 172.17.0.2:80 → Container IP:port   │
│ ↓                                    │
│ nginx (inside container) responds    │
│ ↓                                    │
│ Response sent back through same path │
└──────────────────────────────────────┘
```

## Logging and Inspection

```bash
# Daemon captures logs
docker logs webserver
# Reads from:
# - Container's stdout
# - Container's stderr
# - Sends to log driver
# - Stored in:
#   /var/lib/docker/containers/<id>/<id>-json.log

# Execute command in running container
docker exec webserver curl localhost
# Daemon:
# 1. Opens existing container's namespace
# 2. Spawns new process in container
# 3. Executes command
# 4. Returns output

# Inspect container
docker inspect webserver
# Daemon returns full container configuration:
# - Image ID
# - Container ID
# - IP address
# - Port mappings
# - Environment variables
# - Mounts
# - Current state
# - Process ID (on host)
```

## Resource Monitoring

```bash
# Docker tracks resource usage
docker stats webserver
# Reads from cgroup stats files:
# /sys/fs/cgroup/memory/docker/.../memory.usage_in_bytes
# /sys/fs/cgroup/cpuacct/docker/.../cpuacct.usage
# /proc/<pid>/io → I/O statistics

# Output shows:
# CONTAINER  MEM USAGE / LIMIT  MEM %  CPU %  NET I/O
# webserver  45.2 MB / unlimited 0.23%  0.05%  2.1 kB / 1.5 kB
```

## Container Communication

```bash
# Container 1 wants to reach Container 2
# (on same bridge network)

Container 1
  ├─ Wants to reach: container2:80
  ├─ Container2 not in routing table
  ├─ Asks Docker DNS (127.0.0.11:53)
  ├─ DNS returns: 172.17.0.3 (container2's IP)
  └─ Connects to 172.17.0.3:80
        ↓
     Bridge network (docker0)
        ↓
Container 2
  ├─ Receives connection on 172.17.0.3:80
  ├─ Application responds
  └─ Response flows back

# Daemon's role:
# - Manages DNS resolution
# - Configures network bridges
# - Sets up iptables rules for routing
# - Maintains network connectivity
```

## Image Building Process

```bash
# docker build -t myapp:1.0 .

Step 1: Parse Dockerfile
FROM ubuntu:20.04
RUN apt-get update
COPY app.js /app/
RUN npm install
CMD ["node", "app.js"]

Step 2: For each instruction
┌──────────────────────────────────────┐
│ FROM ubuntu:20.04                    │
├──────────────────────────────────────┤
│ - Pull ubuntu image if not exists    │
│ - Set as base layer (abc123...)      │
└──────────────────────────────────────┘

┌──────────────────────────────────────┐
│ RUN apt-get update                   │
├──────────────────────────────────────┤
│ - Create temporary container         │
│ - Mount previous layer + writable    │
│ - Execute "apt-get update"           │
│ - Capture changes in writable layer  │
│ - Commit layer (def456...)           │
│ - Remove temporary container         │
└──────────────────────────────────────┘

┌──────────────────────────────────────┐
│ COPY app.js /app/                    │
├──────────────────────────────────────┤
│ - Create temporary container         │
│ - Copy app.js into writable layer    │
│ - Commit layer (ghi789...)           │
│ - Remove temporary container         │
└──────────────────────────────────────┘

Step 3: Final image
Image ID: sha256:mno345...
Layers: abc123, def456, ghi789
Config: { Cmd: ["node", "app.js"] }

Stored in: /var/lib/docker/images/
```

## Key Takeaways

|Component|Role|Analogy|
|---|---|---|
|**Client**|Send commands|Waiter|
|**Daemon**|Orchestrate|Head Chef|
|**Runtime**|Execute|Sous Chef|
|**Namespaces**|Isolation|Parallel universes|
|**Cgroups**|Limits|Budget constraints|
|**Layers**|Image parts|Book chapters|
|**CoW**|Efficiency|Shared copies|
|**Network**|Container comms|Internal phone system|
|**Storage Driver**|Filesystem|Filing system|

## Quick Summary: What Happens When You Run a Container

```bash
docker run -d -p 8080:80 --name web nginx

1. Client → Daemon: "Please run nginx"
   
2. Daemon checks: "Do I have nginx image?" 
   
3. Daemon creates: Container ID, writable layer, network config
   
4. Runtime (runc) sets up: Namespaces, cgroups, filesystem
   
5. Runtime executes: nginx process (PID 1 in container)
   
6. Daemon monitors: Logs, crashes, resource usage
   
7. Network configured: Port 8080 on host → 80 in container
   
8. User connects: localhost:8080 → nginx inside container
   
Container is now running, isolated, and managed by Docker Engine!
```

Docker Engine is truly fascinating—it orchestrates Linux kernel features to create the illusion of complete virtual machines while maintaining lightweight efficiency!