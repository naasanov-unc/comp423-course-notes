# Setting up a dev container for Rust

* Primary author: [Nicolas Asanov](https://github.com/naasanov-unc)
* Reviewer: [Raghav Arun](https://github.com/RaghavArun)

# Prerequisites
Make sure you have VSCode, Docker, and Git installed

# Initialize the project
Navigate to the folder you want your new project to be in and open it in a command line.
First, make a new directory for the project with the following commands
``` sh
mkdir rust-project
cd rust-project
```
Then, initialize a new Git repository
``` sh
git init
```
Add a `README` file
``` sh
echo "# Beginner Project in Rust" > README.md
git add README.md
git commit -m "Initial commit with README"
```

# Setup a Dev Container
Note: make sure you have the VSCode Dev Containers extension installed.
Create a directory named `.devcontainer` in the root folder of your project.
Create a new file named `devcontainer.json` within this folder
``` json
{
  "name": "Beginner Rust Project",
  "image": "mcr.microsoft.com/devcontainers/rust:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["rust-lang.rust-analyzer"]
    }
  }
}
```
Breakdown of the json file:

* `name`: A descriptive name for the container.
* `image`: The Docker image to use, in this case, the latest version of a Rust environment.
* `customizations`: Adds useful configurations to VSCode, like installing the Rust Analyzer extension.

Finally, reopen the project in the container by pressing `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running `rustc --version` to double check that your dev container is running a recent version of Rust.

# Setup your Rust project
To start setting up your Rust project, run the following command:
`cargo new hello_comp423 --bin --vcs none`
Okay, so theres a lot going on here. Let's walk through each part of the command.

* `cargo`: Cargo is the Rust package manager. If you have ever used `npm` with Javascript or `pip` with Python, it is similar to those. It is a tool that assists you in managing dependencies and builds.
* `new hello_comp423`: This creates a new package named `hello_comp423`.
* `--bin`: [Difference between binary and libary]
* `--vcs none`: This prevents the default behavior of initializing a new Git repository on the creation of a package. This is not needed in this scenario becuase we have already initailzed our repo.

Your directory should now look like this after running the command
```sh
$ tree .
.
├── hello_comp423
│   ├── Cargo.toml
│   └── src
│       └── main.rs
└── README.md

3 directories, 3 files
```

If you take a look at `hello_comp423/src/main.rs`, you'll see that most of our coding has already been taken care for us!
``` rs title='main.rs'
fn main() {
    println!("Hello, world!");
}
```
This should look pretty familiar to you even if you've never used Rust before.
`rs! fn main()` defines a function named main that takes no arguments, and `rs! println!("Hello, world!");` prints the text "Hello, world!" to the console followed by a newline. 
!!! note
  
    If you've never seen Rust before, you might be wondering what the "!" next to `println` is doing there. [Explain diff between macro and function]

