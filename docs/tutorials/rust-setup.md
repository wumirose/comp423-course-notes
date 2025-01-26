# Setting up a Dev Container for Rust 

* Primary author: [Olawumi Olasunkanmi](https://github.com/wumirose)

## **Introduction**

Welcome! In this tutorial, you'll learn how to create a static website to teach Rust programming using **Material for MkDocs**. By the end, you'll have a polished, functional website deployed on GitHub Pages. This website will feature organized lessons, examples, and practical exercises designed to guide learners through the fundamentals of Rust.

### Why Rust?

[Rust](https://www.rust-lang.org/learn) is a cutting-edge systems programming language that prioritizes performance and safety. Its applications range from web assembly and game development to server-side programming. By learning Rust, you’ll gain skills to write software that is both efficient and reliable, equipping you for modern programming challenges.

### Why This Tutorial?

This tutorial is designed to help you:

- Master Rust in a structured, beginner-friendly way.
- Create and share valuable teaching resources for yourself or others.
- Gain hands-on experience with modern development practices like CI/CD and containerized development. 

--- 

## **Prerequisites**

Before we dive in, make sure you have:

- **A GitHub account**: If you don’t have one yet, sign up at [GitHub](https://github.com/).

- **Git installed**: Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.

- **Visual Studio Code (VS Code)**: Download and install it from [here](https://code.visualstudio.com/).

- **Docker installed**: Required to run the dev container. [Get Docker here](https://www.docker.com/).

- **Command-line basics**: A quick online course should provide the necessary information. A suggested text is attached [here](https://www.learnenough.com/command-line-tutorial/basics?srsltid=AfmBOoo-cNiimvpZ0gjStoCaKkLb28Z3x73CFC5OQDVN1AvisxNV6J48).


## **Part 1. Rust Project Setup: Creating the Repository**

### Step 1. Create a Local Directory and Initialize Git

A. Open your terminal or command prompt.

B. Create a new directory for your project.
```bash 
mkdir rust-setup-notes
cd rust-setup-notes
```
C. Initialize a Git repository in the new directory.
```bash
git init
```
D. Create a `README.md` file that describes the project:
```bash 
echo "# Rust Setup Notes" > README.md  
git add README.md  
git commit -m "Initial commit with README"  
```

### Step 2. Create a Remote Repository on GitHub
1. Log in to your GitHub account and navigate to the [Create a New Repository](https://github.com/new) page.

2. Fill in the details as follows:
    - **Repository Name**: `rust-setup-notes`
    - **Description**: "Notes and resources for setting up and working with Rust, containerized development and basic Rust programming"
    - **Visibility**: Public

3. Do **not** initialize the repository with a `README`, `.gitignore`, or `license`

4. Click **Create Repository**.

### Step 3. Link Your Local Repository to GitHub

1. Add the GitHub repository as a remote:
```bash
git remote add origin https://github.com/<your-username>/rust-setup-notes.git
```
Replace `your-username` with your GitHub username.

2. Check your default branch name with the subcommand `git branch`. If it's not `main`, rename it to main with the following command: `git branch -M main`. Old versions of `git` choose the name `master` for the primary branch, but these days `main` is the standard primary branch name.

3. Push your local commits to the GitHub repository:
```bash
git push --set-upstream origin main
```

> `git push --set-upstream origin main`: This command pushes the main branch to the remote repository origin. The `--set-upstream` flag sets up the main branch to track the remote branch, meaning future pushes and pulls can be done without specifying the branch name and just writing `git push origin` when working on your local `main` branch. This long flag has a corresponding `-u` short flag.

4. Back in your web browser, refresh your GitHub repository to see that the same commit you made locally has now been pushed to remote. You can use git log locally to see the commit ID and message which should match the ID of the most recent commit on GitHub. This is the result of pushing your changes to your remote repository.

## **Part 2. Setting Up the Development Environment**

### What is a Devlopment (Dev) Container?

A dev container provides a consistent and isolated development environment across different machines. It’s a preconfigured setup using Docker, ensuring all necessary tools, libraries, and dependencies are included for your project. This eliminates setup inconsistencies and reduces errors caused by "it works on my machine" issues. Dev containers also streamline onboarding by allowing new team members to start coding quickly with minimal setup.

### How are software project dependencies managed?

Managing dependencies is crucial for ensuring compatibility in software projects. In Rust, dependencies are listed in the `cargo.toml` file, which helps manage external libraries. For this project, our primary dependency is `mkdocs-material`, available through Cargo.

The `devcontainer.json` ensures a consistent development environment by automatically setting up dependencies from `cargo.toml` when the dev container is created. This automates the environment setup process, allowing all collaborators to quickly start coding in the same setup without manual configuration.

Lets establish your static website development environment:

### Step 1. Add Development Container Configuration

1. In VS Code, open the `rust-setup-notes` directory. You can do this via: File > Open Folder.
2. Install the Dev Containers extension for VS Code.
3. Create a `.devcontainer` directory in the root of your project with the following file inside of this "hidden" configuration directory: `.devcontainer/devcontainer.json`. 

The `devcontainer.json` file defines the configuration for your development environment. Here, we're specifying the following:
    - **name**: A descriptive name for your dev container.
    - **image**: The Docker image to use, in this case, the latest version of a Rust environment. [Microsoft maintains a collection of base images for many programming language environments](https://hub.docker.com/r/microsoft/vscode-devcontainers), but you can also create your own!
    - **customizations**: Adds useful configurations to VS Code, like installing the Rust extension. When you search for VSCode extensions on the marketplace, you will find the string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.
    - **postCreateCommand**: A command to run after the container is created. The command will install Rust dependencies listed in `cargo.toml`.

```json
{
  "name": "Rust Dev Container",
  "image": "mcr.microsoft.com/devcontainers/rust:latest",
  "customizations": {
    "vscode": {
      "extensions": ["rust-lang.rust-analyzer"]
    }
  },
  "postCreateCommand": "rustc --version"
}
```

### Step 2. Reopen the Project in a VSCode Dev Container

Reopen the project in the container by pressing `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running `rustc --version` to see your dev container is running a recent version of Rust without much effort!

## **Part 3. Create a new Rust Project**

### Step 1. Run the `cargo new` command

With the dev container ready, create a new Rust project by running the following in your terminal:
```rust
cargo new rust_project --vcs none
cd rust_project
```
- `cargo new`: Creates a new binary Rust project.
- `--vcs none`: Prevents the creation of an additional Git repository since you already initialized one earlier.

### Step 2. Open the `src/main.rs` file and modify it

Inside your new rust_project directory, Open the `src/main.rs` file and modify it to print "Hello COMP423":
```rust
fn main() {
    println!("Hello COMP423");
}
```

## **Part 4. Building and Running the Rust Project**

### Step 1. Build the Project

Next, use the cargo build command to compile the project.
```bash
cargo build
```
The `cargo build` command compiles the project and generates a binary file in the target directory. *I did not take COMP211

### Step 2. Run the Project 

```bash
cargo run
```

Key Differences:

- `cargo build`: Only compiles the project. The binary is saved in the target/debug directory, and you must manually run it using its path.
- `cargo run`: Compiles and directly runs the binary, making it a more convenient option during development.

For example:

```bash
# Build only
cargo build
./target/debug/rust_project

# Running directly
cargo run
Both commands will output:
```

```
Hello COMP423
```

## **Conclusion**

Congratulations! You have successfully set up a development container for Rust, created a new project, and compiled and executed your first Rust program. By following this tutorial, you’ve learned:

- How to configure and use a Dev Container for Rust development, ensuring a consistent and portable development environment.
- How to initialize and manage a Git repository for your project.
- The basics of Rust project setup using cargo, including creating, building, and running a project.
- The differences between cargo build and cargo run, and how they compare to compiling workflows you’ve encountered in other languages like C.
