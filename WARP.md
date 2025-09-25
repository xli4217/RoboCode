# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Repository Context

This is a robotics and automation code repository integrated with **Task Master AI** for intelligent project management and development workflows. The repo supports multi-language development with emphasis on robotics frameworks.

## Task Master AI Integration

This project uses Task Master AI for intelligent task management. Key points:

- **Core task management**: Use `task-master` CLI or MCP tools for project planning
- **AI-powered workflows**: Task parsing from PRDs, complexity analysis, and intelligent task expansion
- **Claude integration**: Full MCP server integration with custom slash commands
- **Multi-model support**: Supports Claude, OpenAI, Perplexity, and other AI models

### Essential Task Master Commands

```bash
# Daily workflow
task-master next                    # Get next available task
task-master show <id>              # View detailed task information
task-master set-status --id=<id> --status=done  # Mark task complete

# Task management
task-master add-task --prompt="description" --research
task-master expand --id=<id> --research --force
task-master update-subtask --id=<id> --prompt="notes"

# Analysis & planning
task-master analyze-complexity --research
task-master expand --all --research
```

## Supported Technologies & Build Commands

### Python (Primary robotics language)
```bash
# Environment setup
python -m venv .venv
source .venv/bin/activate
pip install -U pip

# If requirements exist
if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
if [ -f pyproject.toml ]; then pip install -e .; fi

# Development
pytest -q                         # Run tests
ruff check . && ruff format .    # Lint and format
python -m <package>               # Run main module
```

### ROS 2 (When applicable)
```bash
# Environment setup
source /opt/ros/$ROS_DISTRO/setup.bash
rosdep install --from-paths src --ignore-src -r -y

# Build and test
colcon build --symlink-install
colcon test && colcon test-result --all

# Run nodes
ros2 run <package> <node>
ros2 launch <package> <launch_file>
```

### C/C++ (For performance-critical robotics code)
```bash
# Build setup (CMake/Ninja)
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build build -j

# Test and format
ctest --test-dir build -j
clang-format -i $(git ls-files '*.hpp' '*.h' '*.cpp' '*.c')
```

### Node.js (For web interfaces and tooling)
```bash
# Environment setup
if command -v nvm >/dev/null; then nvm use || nvm install; fi
npm ci  # or pnpm i

# Development
npm run dev                       # Development server
npm run build                     # Production build
npm test                         # Run tests
```

### Firmware/Microcontrollers (PlatformIO)
```bash
pio run                          # Build firmware
pio run -t upload                # Flash to device
pio run -t monitor              # Serial monitor
```

## Simulation & Testing

### Gazebo Classic/Ignition
```bash
# Launch simulation
gz sim -v4 <world_file.sdf>
gazebo <world_file.world>
```

### Webots
```bash
webots simulation/<world>.wbt
```

### Hardware-in-the-loop testing
```bash
# Sync code to robot
rsync -avz --exclude 'build' --exclude 'venv' . user@robot:/opt/robocode

# Deploy services (when scripts exist)
./scripts/deploy/deploy-to-robot.sh production
```

## Container-Based Development

```bash
# Development container
docker build -f docker/Dockerfile -t robocode:dev .
docker compose up --build

# Production deployment
docker compose -f docker-compose.prod.yml up -d
```

## Project Structure Recommendations

As code is added, organize following this structure:

```
├── apps/                    # Runnable applications and nodes
├── libs/                    # Shared libraries and utilities
├── robots/                  # Robot-specific configurations
├── firmware/                # Microcontroller projects (PlatformIO)
├── simulation/              # Gazebo/Webots worlds and models
├── perception/              # Computer vision and sensor processing
├── control/                 # Motion control algorithms
├── planning/                # Path planning and navigation
├── scripts/                 # DevOps and deployment scripts
├── tools/                   # Development utilities
├── config/                  # Configuration files (YAML/JSON)
├── docker/                  # Container definitions
├── docs/                    # Documentation and diagrams
├── tests/                   # Integration and end-to-end tests
└── .taskmaster/            # Task Master AI project management
```

## Development Workflow with Task Master

### Starting a new development session
```bash
# Get next task and start working
task-master next
task-master show <id>
task-master set-status --id=<id> --status=in-progress

# Log implementation progress
task-master update-subtask --id=<id> --prompt="implementation details..."

# Complete task
task-master set-status --id=<id> --status=done
```

### Working with PRDs
```bash
# Parse new requirements document
task-master parse-prd .taskmaster/docs/new-requirements.txt --append

# Analyze and break down complex tasks
task-master analyze-complexity --research
task-master expand --all --research
```

### Multi-terminal development
Use git worktrees for parallel development:
```bash
git worktree add ../robocode-control feature/new-controller
git worktree add ../robocode-perception feature/vision-upgrade
```

## Common Robotics Dependencies

### System-level (macOS with Homebrew)
```bash
brew install cmake ninja opencv eigen boost
brew install gazebo ros2  # If using ROS 2
```

### Python robotics packages (typical)
```bash
pip install numpy scipy matplotlib
pip install opencv-python pillow
pip install rclpy geometry_msgs sensor_msgs  # ROS 2
pip install robotics-toolbox-python spatialmath-python
```

## Hardware Integration

### Serial/USB devices
```bash
# Check connected devices
ls /dev/tty.*                    # macOS
ls /dev/ttyUSB* /dev/ttyACM*     # Linux

# Set permissions (Linux)
sudo usermod -a -G dialout $USER
```

### Camera access
```bash
# Test camera access
python -c "import cv2; cap = cv2.VideoCapture(0); print('Camera OK' if cap.read()[0] else 'Camera failed')"
```

## Key Configuration Files

- `.taskmaster/config.json` - AI model configuration
- `.env` - API keys for Task Master AI
- `.mcp.json` - MCP server configuration for Claude integration
- `CLAUDE.md` - References Task Master integration rules
- `pyproject.toml` / `requirements.txt` - Python dependencies
- `package.xml` / `CMakeLists.txt` - ROS 2 package configuration
- `docker-compose.yml` - Container orchestration

## WARP-Specific Tips

- Use `edit <file>` to open files in the WARP editor
- Leverage Task Master's MCP integration for intelligent task management
- Run all commands from repository root unless specified otherwise
- Use `task-master models --setup` to configure AI assistants
- Prefer `task-master next` to `task-master list` for focused development
- Log implementation details with `task-master update-subtask` as you code

## Maintenance

- Keep Task Master tasks updated as implementation progresses
- Update this WARP.md as new robotics frameworks are added
- Sync .env.example when new API integrations are added  
- Use `task-master analyze-complexity` before major feature additions
- Consider `task-master parse-prd --append` for requirements changes

This repository is designed for AI-assisted robotics development with intelligent project management through Task Master AI integration.