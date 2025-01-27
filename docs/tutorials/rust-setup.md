# Start a Beginner Project in Rust

## Collaborators
* Primary author: [Nicolas Asanov](https://github.com/naasanov-unc)
* Reviewer: [Raghav Arun](https://github.com/RaghavArun)

## Prerequisites
Make sure you have the following installed

* Git: Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.
* Visual Studio Code (VS Code): Download and install it from [here](https://code.visualstudio.com/).
* Docker: Required to run the dev container. Get Docker [here](https://www.docker.com/products/docker-desktop/).

## Initialize the Project
Navigate to the folder you want your new project to be in, open it in a command line, and follow these steps.

1) Make a new directory for the project with the following commands
``` sh
mkdir rust-project
cd rust-project
```
2) Initialize an empty Git repository
``` sh
git init
```
3) Add a `README` file
``` sh
echo "## Beginner Project in Rust" > README.md
git add README.md
git commit -m "Initial commit with README"
```

## Setup a Dev Container
First, open the folder you just created in VSCode, using `File > Open Folder`.
Then, create a directory named `.devcontainer` in the root folder of your project and a new file named `devcontainer.json` within this folder
``` json title='devcontainer.json'
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
Here's what every part of this file is for:

* `name`: A descriptive name for the container.
* `image`: The Docker image to use, in this case, the latest version of a Rust environment.
* `customizations`: Adds useful configurations to VSCode, like installing the Rust Analyzer extension.

Before you can open the dev container, you need the "Dev Containers" VSCode extension installed. If it's not installed already, navigate to the "Extensions" tab in the sidebar, search "Dev Containers", and install the option offered by Microsoft.

Now you can open the project in the container by pressing `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running `rustc --version` to double check that your dev container is running a recent version of Rust.

## Setup your Rust project
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

Let's change the string "Hello, world!" to "Hello COMP423", so the new main file should look like this
``` rs title='main.rs'
fn main() {
    println!("Hello COMP423");
}
```

But hold on, how do we run this code? There's no big green button! If you remember from COMP 211, the C language uses the GCC in order to compile and run their files. Rust does something similar with Cargo. Go ahead and run the following commands
``` sh
cd hello_comp423
cargo build
```
Now your file structure should look something like this
```sh
$ tree .
.
├── Cargo.lock
├── Cargo.toml
├── src
│   └── main.rs
└── target
    ├── CACHEDIR.TAG
    └── debug
        ├── build
        ├── deps
        │   └── ...
        ├── examples
        ├── hello_comp423
        ├── hello_comp423.d
        └── incremental
            └── ...

10 directories, 18 files
```
!!! note
    If you don't see a change in your file structure after running `cargo build`, click the "Refresh Explorer" button at the top of the VSCode file explorer.

If you look closely, there is a file named `hello_comp423` with no file extension (just like with `gcc`) in the debug directory.
Now you can simply run `./target/debug/hello_comp423` in the command line, and there you go!
```sh
$ ./target/debug/hello_comp423 
Hello COMP423
```

But typing out the entire file path every time you want to run your file is kind of a pain. Luckily, Cargo has an easier way to deal with this, with `cargo run`. Try running it in your command line.
```
$ cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.30s
     Running `target/debug/hello_comp423`
Hello COMP423
```
The `run` command is very similar to `build`, but has one key difference.
It actually just runs the `build` command, and then runs the executable created as a result!

## Conclusion
Great job! You've successfully created a project in Rust! Feel free to use this project to keep learning about the language and its package manager. There is one more thing you have to do before we're finished, and that is to commit our changes to you Git repository! Before we make a commit, it's a good idea to add a `.gitignore` file to our project, and put the `target` directory in it. Why do this? The `target` directory contains files that are irrelavant to the actual project, only created as a result of the build process. This directory can grow very large and bog down our repository, so its a good idea to ignore it from the beginning.

Simply create a new file in the root directory named `.gitignore` and paste the following text into it
``` title='.gitignore'
/hello_comp423/target
```

Then go ahead and commit your work.
```sh
cd ..
git add .
git commit -m "Hello COMP423 in Rust"
```