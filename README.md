<div align="center">
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/github_badge.png" alt="GitHub Badge" width="100">
  <h1><br>🛠️ Development Environment Setup Script 🚀<br></h1>
  <p>Your Fast Lane to a Ready-to-Code Linux System!</p>
</div>

---

<div align="center">
  <img src="https://img.shields.io/badge/Debian-D70A53?style=for-the-badge&logo=debian&logoColor=white" alt="Debian Badge">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu Badge">
  <img src="https://img.shields.io/badge/Shell-FFD700?style=for-the-badge&logo=gnu-bash&logoColor=black" alt="Bash Badge">
</div>

---

This script is your trusty sidekick for quickly setting up a basic development environment on a fresh **Debian** or **Ubuntu**-based Linux system. Say goodbye to tedious manual installations and hello to a ready-to-code machine in just a few steps! Choose the tools you need for **Python**, **Java**, and **Node.js** development, along with essential utilities and command-line editors.

## ✨ Features at a Glance

* **Effortless Setup:** Automates the installation of key development tools.
* **Language Choice:** Select the environment for **Python**, **Java**, or **Node.js**.
* **Essential Utilities:** Installs `git`, `curl`, and `wget` for common development tasks.
* **Code Editor Options:** Choose between the powerful `vim` and the user-friendly `nano`.
* **Environment Management:** Offers optional installation of `virtualenv` and `virtualenvwrapper` for Python.
* **Build Tool Support:** Includes options to install Maven and Gradle for Java projects.
* **Node.js Management:** Leverages `nvm` for easy Node.js installation and version management, with optional Yarn.
* **Clear Guidance:** Provides informative messages and prompts throughout the process.

## ⚙️ Prerequisites

Before you embark on this setup journey, ensure you have:

* A pristine installation of **Debian** or **Ubuntu** Linux.
* `sudo` superpowers (the script will politely ask for your password when needed).
* A stable internet connection to fetch those essential packages.

## 🚀 Getting Started

Follow these simple steps to launch your development environment:

1.  **Save the Script:**
    * Open a plain text editor on your Linux system (`nano`, `vim`, `gedit`, `mousepad`, etc.).
    * Copy and paste the entire script code (provided below) into the editor.
    * **Save the file as `setup_dev_tools.sh`**. The `.sh` extension is crucial for indicating that it's a shell script. Ensure you save it as plain text without any rich formatting.

    ```bash
    #!/bin/bash

    #------------------------------------------------------------------------------
    # Script to automatically install essential development tools for selected languages
    # on a fresh Debian/Ubuntu-based Linux installation.
    #------------------------------------------------------------------------------

    # --- Step 1: Check for sudo privileges ---
    if [[ $EUID -ne 0 ]]; then
      echo "This script requires sudo privileges. Please run it with sudo."
      exit 1
    fi

    # --- Step 2: Update Package Lists ---
    echo "Step 1: Updating package lists..."
    apt update && apt upgrade -y
    echo "Package lists updated and system upgraded."
    echo "--------------------------------------------------"

    # --- Function to install Python development tools ---
    install_python() {
      echo "Installing Python development tools..."
      apt install python3 python3-pip -y
      if [ $? -eq 0 ]; then
        echo "Python 3 and pip installed successfully."
        command -v pip3 >/dev/null 2>&1 || { echo "pip3 not found!"; }
        pip3 --version
        read -p "Do you want to install virtualenv and virtualenvwrapper? (y/N): " install_venv
        if [[ "$install_venv" == "y" || "$install_venv" == "Y" ]]; then
          echo "Installing virtualenv and virtualenvwrapper..."
          pip3 install virtualenv virtualenvwrapper
          if [ $? -eq 0 ]; then
            echo "virtualenv and virtualenvwrapper installed successfully."
            echo "Remember to add the following lines to your shell's configuration file (~/.bashrc or ~/.zshrc):"
            echo "export WORKON_HOME=~/.virtualenvs"
            echo "export PROJECT_HOME=\$WORKON_HOME"
            echo "source /usr/local/bin/virtualenvwrapper.sh"
            echo "Then run 'source ~/.bashrc' or 'source ~/.zshrc' to activate them."
          else
            echo "Error installing virtualenv or virtualenvwrapper."
          fi
        fi
      else
        echo "Error installing Python 3 or pip."
      fi
      echo "--------------------------------------------------"
    }

    # --- Function to install Java development tools ---
    install_java() {
      echo "Installing Java development tools..."
      apt install openjdk-17-jdk -y # You can change the JDK version if needed
      if [ $? -eq 0 ]; then
        echo "OpenJDK 17 installed successfully."
        java -version
        javac -version
        read -p "Do you want to install Maven? (y/N): " install_maven
        if [[ "$install_maven" == "y" || "$install_maven" == "Y" ]]; then
          echo "Installing Maven..."
          apt install maven -y
          if [ $? -eq 0 ]; then
            echo "Maven installed successfully."
            mvn -v
          else
            echo "Error installing Maven."
          fi
        fi
        read -p "Do you want to install Gradle? (y/N): " install_gradle
        if [[ "$install_gradle" == "y" || "$install_gradle" == "Y" ]]; then
          echo "Installing Gradle..."
          apt install gradle -y
          if [ $? -eq 0 ]; then
            echo "Gradle installed successfully."
            gradle --version
          else
            echo "Error installing Gradle."
          fi
        fi
      else
        echo "Error installing OpenJDK 17."
      fi
      echo "--------------------------------------------------"
    }

    # --- Function to install Node.js development tools ---
    install_nodejs() {
      echo "Installing Node.js development tools..."
      # Install Node.js using nvm (recommended)
      curl -o- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh) | bash
      export NVM_DIR="$([ -s "$HOME/.nvm/nvm.sh" ] && echo "$HOME/.nvm" || printf "nvm not found - install it first")"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
      [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash completion

      # Install LTS version of Node.js
      nvm install --lts
      nvm use --lts

      if [ $? -eq 0 ]; then
        echo "Node.js (LTS) and npm installed successfully."
        node -v
        npm -v
        read -p "Do you want to install Yarn? (y/N): " install_yarn
        if [[ "$install_yarn" == "y" || "$install_yarn" == "Y" ]]; then
          echo "Installing Yarn..."
          npm install -g yarn
          if [ $? -eq 0 ]; then
            echo "Yarn installed successfully."
            yarn --version
          else
            echo "Error installing Yarn."
          fi
        fi
      else
        echo "Error installing Node.js."
      fi
      echo "--------------------------------------------------"
    }

    # --- Function to install common development tools ---
    install_common_tools() {
      echo "Installing common development tools (git, curl, wget)..."
      apt install git curl wget -y
      if [ $? -eq 0 ]; then
        echo "Git, curl, and wget installed successfully."
      else
        echo "Error installing common development tools."
      fi
      echo "--------------------------------------------------"
    }

    # --- Function to install a command-line code editor ---
    install_editor() {
      read -p "Do you want to install a command-line code editor? (vim/nano/N): " install_editor_choice
      if [[ "$install_editor_choice" == "vim" ]]; then
        echo "Installing vim..."
        apt install vim -y
        if [ $? -eq 0 ]; then
          echo "vim installed successfully."
        else
          echo "Error installing vim."
        fi
      elif [[ "$install_editor_choice" == "nano" ]]; then
        echo "Installing nano..."
        apt install nano -y
        if [ $? -eq 0 ]; then
          echo "nano installed successfully."
        else
          echo "Error installing nano."
        fi
      fi
      echo "--------------------------------------------------"
    }

    # --- Main Menu ---
    while true; do
      echo "Choose the development tools to install:"
      echo "1. Python"
      echo "2. Java"
      echo "3. Node.js"
      echo "4. Install common development tools (git, curl, wget)"
      echo "5. Install a command-line code editor"
      echo "6. Exit"
      read -p "Enter your choice (1-6): " choice

      case "$choice" in
        1)
          install_python
          ;;
        2)
          install_java
          ;;
        3)
          install_nodejs
          ;;
        4)
          install_common_tools
          ;;
        5)
          install_editor
          ;;
        6)
          echo "Exiting script."
          break
          ;;
        *)
          echo "Invalid choice. Please enter a number between 1 and 6."
          ;;
      esac
    done

    echo "Development environment setup complete!"
    ```

2.  **Grant Execution Permissions:**
    Open your terminal, navigate to the directory where you saved the script, and make it executable:
    ```bash
    chmod +x setup_dev_tools.sh
    ```

3.  **Execute with Elevated Privileges:**
    Run the script using `sudo`:
    ```bash
    sudo ./setup_dev_tools.sh
    ```
    Enter your password when prompted.

4.  **Follow the Interactive Menu:**
    The script will present a menu of options. Enter the number corresponding to the tools you want to install and press Enter.

## 🛠️ Available Tool Options

* **1. Python:** Installs Python 3 and `pip`. Optionally installs `virtualenv` and `virtualenvwrapper`.
* **2. Java:** Installs OpenJDK 17. Optionally installs Maven and Gradle.
* **3. Node.js:** Installs Node.js (LTS) and `npm` using `nvm`. Optionally installs Yarn.
* **4. Install common development tools:** Installs `git`, `curl`, and `wget`.
* **5. Install a command-line code editor:** Choose between `vim` and `nano`.
* **6. Exit:** Closes the script.

## 📌 Important Notes

* **Saving the Script:** Ensure you save the script as a plain text file with the `.sh` extension (e.g., `setup_dev_tools.sh`).
* **Execution:** You need to make the script executable using `chmod +x setup_dev_tools.sh` before you can run it.
* **`sudo`:** The script requires `sudo` privileges to install software.
* **Python Virtual Environments:** Configure your shell after installing `virtualenvwrapper` as instructed by the script.
* **Node.js and `nvm`:** You might need to restart your terminal session after the script finishes for `nvm` to be properly initialized.
* **Customization:** Feel free to modify the script to include other tools or specific versions.

---

<div align="center">
  <p>Happy coding! 🚀</p>
</div>
