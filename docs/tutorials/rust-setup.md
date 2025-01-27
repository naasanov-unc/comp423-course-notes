# Start a Beginner Project in Rust

## Collaborators
* Primary author: [Nicolas Asanov](https://github.com/naasanov-unc)
* Reviewer: [Raghav Arun](https://github.com/RaghavArun)

## Prerequisites(1) { .annotate }
1. A significant portion of this section is quoted from the [423 MkDocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/#what-you-will-learn)

Make sure you have the following installed

* Git: Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.
* Visual Studio Code (VS Code): Download and install it from [here](https://code.visualstudio.com/).
* Docker: Required to run the dev container. Get Docker [here](https://www.docker.com/products/docker-desktop/).

## Initialize the Project(1) { .annotate }
1. A significant portion of this section is quoted from the [423 MkDocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/#what-you-will-learn)

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

## Setup a Dev Container(1) { .annotate }
1. A significant portion of this section is quoted from the [423 MkDocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/#what-you-will-learn)

First, open the folder you just created in VSCode, using `File > Open Folder`.
Then, create a directory named `.devcontainer` in the root folder of your project and a new file named `devcontainer.json` within this folder.
Paste the following text in it.
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
To start setting up your project, run the following command:
```sh
cargo new hello_comp423 --bin --vcs none
```
Okay, so theres a lot going on here. Let's walk through each part of the command.

* `cargo`: Cargo is the Rust package manager. If you have ever used `npm` with Javascript or `pip` with Python, it is similar to those. It is a tool that assists you in managing dependencies and builds.
* `new hello_comp423`: Creates a new package named `hello_comp423`.
* `--bin`: There are two types of Cargo packages: a binary project and a library project. Binary projects are packages that create an executable application. A file that you can run. Library projects are packages that create a reusable library, which is code that is meant to be shared and included in other products. These are specified with `--bin` and `--lib` respectively. Since we are trying to create a simple project with a simple output, we choose to create a binary project.
* `--vcs none`: This prevents the default behavior of initializing a new Git repository on the creation of a package. We already initialized the Git repo for this project, so we use this flag. 

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

If you take a look at `hello_comp423/src/main.rs`, you'll see that most of the code has already been taken care for us!
``` rs title='main.rs' linenums="1"
fn main() {
    println!("Hello, world!");
}   
```
This should look pretty familiar to you even if you've never used Rust before.
`#!rust fn main()` defines a function named "main" that takes no arguments, and `#!rust println!("Hello, world!");` prints the text "Hello, world!" to the console followed by a newline. 

??? info "What's that exclamation mark doing there?"
  
    If you've never seen Rust before, the `!` next to `println` might seem out of nowhere. Unfortunately, it's not just something you put there when you're especially excited to call a function. It's actually there to signify the usage of a **macro**. Macros are similar to traditional functions, but differ in the sense that they can accept a variable number of arguments and change their behavior based on the arguments passed, somewhat akin to function overloading in Java. You don't need to know the specifics for this tutorial, but be on the lookout for macro usage in the future!

Let's change the string "Hello, world!" to "Hello COMP423". The new main file should look like this.
``` rs title='main.rs' linenums="1"
fn main() {
    println!("Hello COMP423");
}
```

But hold on, how do we run this code? There's no big green button! If you remember from COMP 211, the C language uses GCC in order to compile a C file into an executable, and then run it. Rust does something similar with Cargo. Go ahead and run the following commands.
``` sh linenums="1"
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
There is is!

The `run` command is very similar to `build`, but has one key difference.
It actually just runs the `build` command, and then runs the executable created as a result!

## Conclusion
Great job! You've successfully created a project in Rust! Feel free to use this project to keep learning about the language and its package manager. There is one more thing you have to do before we're finished, and that is to commit your changes to your Git repository! 

Before we make a commit, it's a good idea to add a `.gitignore` file to our project, and put the `target` directory in it. Why do this? The `target` directory contains files that are irrelevant to the actual project, only created as a result of the build process. This directory can grow very large and bog down our repository, so its a good idea to prevent commiting it from the beginning.

Simply create a new file in the root directory named `.gitignore` and paste the following text into it
``` title='.gitignore' linenums="1"
/hello_comp423/target
```

Then go ahead and commit your work.
```sh linenums="1"
cd ..
git add .
git commit -m "Hello COMP423 in Rust"
```