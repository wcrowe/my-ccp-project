# README.md for My C++ Project

This is a C++ development environment using VS Code Dev Containers with Docker, Clang 21, CMake, Ninja, and vcpkg for package management. It's reproducible across machines and ideal for collaborative work.

## Prerequisites
- [VS Code](https://code.visualstudio.com/) installed.
- [Docker Engine](https://docs.docker.com/engine/install/) installed and running (use the official method for your OS, e.g., on Ubuntu: follow the steps to add Docker's repo and install `docker-ce`).
- VS Code extension: [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) (install via Extensions view: search "Remote - Containers").
- Add your user to the docker group: `sudo usermod -aG docker $USER` then log out/in.

## Setup
1. Clone the repo: `git clone <repo-url> && cd my-cpp-project`.
2. Open in VS Code: `code .`.
3. When prompted (bottom-right), click "Reopen in Container" (or Ctrl+Shift+P > "Dev Containers: Reopen in Container").

The container will build automatically using `.devcontainer/Dockerfile` and `devcontainer.json`. This installs Clang 21 as default, sets up vcpkg, and configures VS Code extensions/settings for C++ (e.g., clangd for IntelliSense).

## How to Rebuild the Container
Rebuild if you change `.devcontainer/` files (e.g., update Dockerfile or devcontainer.json) or need a fresh start:
1. In VS Code: Ctrl+Shift+P > "Dev Containers: Rebuild Container" (or "Rebuild and Reopen in Container" to reload).
2. Manually via CLI (from project root):
   ```
   docker build -t my-cpp-dev -f .devcontainer/Dockerfile .
   ```
   Then reopen in VS Code to use the new image.

The `postCreateCommand` in devcontainer.json runs CMake automatically on start/rebuild, configuring with Clang 21 and vcpkg toolchain. If it fails (e.g., no CMakeLists.txt), comment it out temporarily.

## Building and Running the Project
Inside the container (VS Code terminal):
```
rm -rf build
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Debug -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_CXX_COMPILER=/usr/bin/clang++-21 -DCMAKE_C_COMPILER=/usr/bin/clang-21 -
ninja -C build  # Build
./build/main    # Run (assuming add_executable(main ...) in CMakeLists.txt)


```

## Customizing
- Add packages: Use `vcpkg install <pkg>` (e.g., `boost`), then rebuild CMake.
- Update Clang: Edit Dockerfile to change version (e.g., replace 21 with 22 when available).
- .gitignore: Already set up to ignore build/, CMakeFiles, etc.

For issues: Check VS Code Output panel > "Dev Containers" log. If Docker errors, verify `docker run hello-world` works.
