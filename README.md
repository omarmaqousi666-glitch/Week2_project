# Robot Controller (ROS 2)

## Overview
The **robot_controller** package provides three ROS 2 nodes that work together to control and monitor a robot.  
It uses the ROS 2 Python client library (**rclpy**) and standard message types to publish velocity and status information, and to process text-based commands.

The package is intended for simple robot control simulations or real robots using ROS 2.  

---

## Nodes

### 1. `status_publisher`
- **Publishes:** `/robot/status` (`std_msgs/String`)
- **Subscribes:** `/robot/command` (`std_msgs/String`)
- **Purpose:** Periodically publishes the robot's current status (e.g., “Robot is ready”, “Robot started”, or “Robot stopped”).
- **Parameter:**
  - `timer_period`: The interval (in seconds) between publishing messages.
- **Features:**
  - Updates the status based on received commands (`start`, `stop`, `reset`).
  - Logs published status messages for monitoring.

---

### 2. `velocity_publisher`
- **Publishes:** `/cmd_vel` (`geometry_msgs/Twist`)
- **Subscribes:** `/robot/command` (`std_msgs/String`)
- **Purpose:** Publishes linear and angular velocities based on received commands.
- **Parameter:**
  - `timer_period`: The publishing rate for velocity commands.
- **Features:**
  - `start` → linear velocity = 3.0, angular velocity = 0.5  
  - `stop` → linear and angular velocities = 0.0  
  - `reset` → linear and angular velocities = 0.0  
  - Logs all velocity messages.

---

### 3. `command_subscriber`
- **Subscribes:** `/robot/command` (`std_msgs/String`)
- **Purpose:** Receives commands and logs them for debugging.
- **Features:**
  - Converts commands to lowercase before processing.
  - Logs all received messages for monitoring.

---

## Parameters

Node parameters are configured in the file: `config/params.yaml`

```yaml
status_publisher:
  ros__parameters:
    timer_period: 3

velocity_publisher:
  ros__parameters:
    timer_period: 1
```

- **status_publisher.timer_period** → Controls how often the status is published.
- **velocity_publisher.timer_period** → Controls how often velocities are published.

---

## Launch Instructions

### Launch all nodes at once
A launch file is included to start all three nodes automatically:

```bash
ros2 launch robot_controller all_nodes.launch.py
```

### Expected behavior
- **Status Publisher** and **Velocity Publisher** will publish messages at their configured timer rates.
- **Command Subscriber** listens for commands on `/robot/command`.
- You can send commands manually using:

```bash
ros2 topic pub /robot/command std_msgs/String "data: 'start'"
```

---

## Installation & Setup

### Prerequisites
- ROS 2 (Jazzy)
- Python 3
- Colcon build system

### Steps
1. **Navigate to your workspace source folder:**
```bash
cd ~/robot_project_ws/src
```

2. **Clone or copy the package:**
```bash
git clone <your_repository_url>
```

3. **Build the package:**
```bash
cd ~/robot_project_ws
colcon build --packages-select robot_controller
```

4. **Source the workspace:**
```bash
source install/setup.bash
```

5. **Run the launch file:**
```bash
ros2 launch robot_controller all_nodes.launch.py
```

---

## Testing

To run automated tests:

```bash
colcon test --packages-select robot_controller
```

Example tests verify:
- Node parameters are valid.
- Publishers are created correctly.
- Node initialization completes without errors.

---

## Package Structure

```
robot_controller/
├── config/
│   └── params.yaml
├── launch/
│   └── robot_launch.py
├── robot_controller/
│   ├── __init__.py
│   ├── status_publisher.py
│   ├── velocity_publisher.py
│   └── command_subscriber.py
├── test/
│   └── test_status_publisher.py
├── package.xml
├── setup.py
└── README.md
```

---

## Notes
- Each Python node includes detailed inline comments and docstrings explaining its logic.
- YAML and package files include comments describing their purpose.
- The package follows standard **ROS 2 Python conventions** (`ament_python` build type).
- Publishing rates can be adjusted via `config/params.yaml`.

---

## Instructor
**Name:** Dr. Abed Alkarim Al-Banna

---

## Maintainer
**Name:** Omar Emran Al-Maqousi
**ID:** 202310489
