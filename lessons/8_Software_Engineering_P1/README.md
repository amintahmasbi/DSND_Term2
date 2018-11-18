Standard practices of software engineering and how they apply in data science.

- Writing clean and modular code
- Writing efficient code
- Code refactoring
- Adding meaningful documentation
- Using version control

### Clean and Modular code

- **PRODUCTION CODE:** software running on production servers to handle live users and data of the intended audience. Note this is different from *production quality code*, which describes code that meets expectations in reliability, efficiency, etc., for production. Ideally, all code in production meets these expectations, but this is not always the case.
- **CLEAN:** readable, simple, and concise. A characteristic of production quality code that is crucial for collaboration and maintainability in software development.
  - Meaningful Names:
    - **Be descriptive and imply type** - E.g. for booleans, you can prefix with `is_` or `has_` to make it clear it is a condition. You can also use part of speech to imply types, like verbs for functions and nouns for variables.
    - **Be consistent but clearly differentiate** - E.g. `age_list` and `age` is easier to differentiate than `ages` and `age`.
    - **Avoid abbreviations and especially single letters** - (Exception: counters and common math variables) 
    - **Long names != descriptive names** - You should be descriptive, but only with relevant information. E.g. good functions names describe what they do well without including details about implementation or highly specific uses.
  - Nice Whitespace:
    - Organize your code with consistent indentation - the standard is to use 4 spaces for each indent. You can make this a default in your text editor.
    - Separate sections with blank lines to keep your code well organized and readable.
    - Try to limit your lines to around 79 characters, which is the guideline given in the PEP 8 style guide. In many good text editors, there is a setting to display a subtle line that indicates where the 79 character limit is.
- **MODULAR:** logically broken up into functions and modules. Also an important characteristic of production quality code that makes your code more organized, efficient, and reusable.
  - **DRY (Don't Repeat Yourself)**:  Modularization allows you to reuse parts of your code. Generalize and consolidate repeated code in functions or loops.
  - **Abstract out logic to improve readability:** Abstracting out code into a function not only makes it less repetitive, but also improves readability with descriptive function names. 
  - **Minimize the number of entities (functions, classes, modules, etc.):** There are trade-offs to having function calls instead of inline logic. If you have broken up your code into an unnecessary amount of functions and modules, you'll have to jump around everywhere if you want to view the implementation details for something that may be too small to be worth it. 
  - **Functions should do one thing:** If a function is doing multiple things, it becomes more difficult to generalize and reuse. Generally, if there's an "and" in your function name, consider refactoring.
  - **Arbitrary variable names can be more effective in certain functions:** Arbitrary variable names in general functions can actually make the code more readable.
  - **Try to use fewer than three arguments per function:** Try to use no more than three arguments when possible. This is not a hard rule and there are times it is more appropriate to use many parameters. 
- **MODULE:** a file. Modules allow code to be reused by encapsulating them into files that can be imported into other files.

### Code Refactoring:

- **REFACTORING:** restructuring your code to improve its internal structure, without changing its external functionality. This gives you a chance to clean and modularize your program after you've got it working.

### Writing Efficient Code:

- **EFFICIENT:** Knowing how to write code that runs efficiently is another essential skill in software development. Optimizing code to be more efficient can mean making it:
  - Execute faster
  - Take up less space in memory/storage

### Adding meaningful documentation

- **DOCUMENTATION:** additional text or illustrated information that comes with or is embedded in the code of software.
- Helpful for clarifying complex parts of code, making your code easier to navigate, and quickly conveying how and why different components of your program are used.
- Several types of documentation can be added at different levels of your program:
  - **In-line Comments** - line level
    - In-line comments are text following hash symbols throughout your code. They are used to explain parts of your code, and really help future contributors understand your work.
    - One way comments are used is to document the major steps of complex code to help readers follow. Then, you may not have to understand the code to follow what it does. However, others would argue that this is using comments to justify bad code, and that if code requires comments to follow, it is a sign refactoring is needed.
    - Comments are valuable for explaining where code cannot. For example, the history behind why a certain method was implemented a specific way. Sometimes an unconventional or seemingly arbitrary approach may be applied because of some obscure external variable causing side effects. These things are difficult to explain with code.
  - **Docstrings** - module and function level. Docstring, or documentation strings, are valuable pieces of documentation that explain the functionality of any function or module in your code. Ideally, each of your functions should always have a docstring. Docstrings are surrounded by triple quotes. The first line of the docstring is a brief explanation of the function's purpose.
    - One line docstring
    - Multi line docstring: The next element of a docstring is an explanation of the function's arguments. Here you list the arguments, state their purpose, and state what types the arguments should be. Finally it is common to provide some description of the output of the function. Every piece of the docstring is optional; however, doc strings are a part of good coding practice.
  - **Project Documentation** - project level
    - A great first step in project documentation is your README file. It will often be the first interaction most users will have with your project.

### Using Version Control:

[git branching strategy](http://nvie.com/posts/a-successful-git-branching-model/):

- The main branches:  the central repo (`origin`) holds two main branches with an infinite lifetime:
  - master: We consider `origin/master` to be the main branch where the source code of `HEAD` always reflects a *production-ready* state.
  - develop: We consider `origin/develop` to be the main branch where the source code of `HEAD` always reflects a state with the latest delivered development changes for the next release.

Therefore, each time when changes are merged back into `master`, this is a new production release *by definition*. We tend to be very strict at this, so that theoretically, we could use a Git hook script to automatically build and roll-out our software to our production servers everytime there was a commit on `master`.

- Supporting branches:  These branches always have a limited life time, since they will be removed eventually.

  - Feature branches

    - May branch off from: `develop`

    - Must merge back into: `develop`

    - Branch naming convention:

      anything except `master`, `develop`, `release-*`, or `hotfix-*`

  Feature branches typically exist in developer repos only, not in `origin`.

  - Release branches

    - May branch off from: `develop`

      Must merge back into: `develop` and `master`

      Branch naming convention: `release-*`

  The key moment to branch off a new release branch from `develop` is when develop (almost) reflects the desired state of the new release. At least all features that are targeted for the release-to-be-built must be merged in to `develop` at this point in time. All features targeted at future releases may not—they must wait until after the release branch is branched off.

  - Hotfix branches

    - May branch off from: `master`

      Must merge back into: `master` and `develop`

      Branch naming convention: `hotfix-*`

  The essence is that work of team members (on the `develop` branch) can continue, while another person is preparing a quick production fix. 

  **when a release branch currently exists, the hotfix changes need to be merged into that release branch, instead of develop**

NOTE: `git merge --no-ff`. The `--no-ff` flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature. 

### Data versioning:

One good practice is to treat data as immutable, meaning don’t ever naively edit or overwrite your raw data, and keep only one version of the raw data.

 If the data results from some heavy-processing and time-consuming scripts. Be sure to mark well the generating scripts and its version.

Existing solutions:

- **Data Version Control (DVC).** [DVC](https://dataversioncontrol.com/) 
- **Pachyderm**. Another option is [Pachyderm](https://github.com/pachyderm/pachyderm) (building on Docker)
- **Cookiecutter Data Science.** Yet a third option is [Cookiecutter Data Science](https://drivendata.github.io/cookiecutter-data-science/) 

### Model versioning:

Models should be treated as immutable as well.

- Specify a name, version, and the training scripts of the model. Having a uniform naming convention helps: e.g., (datetime)-(model name)-(model version)- (training script id)
- Make sure file names are unique (to avoid accidental overwriting)
- Have a global json file to store model names and score mapping (e.g., key being the model name and value being a 3-element tuple, storing training/validation/test set scores)
- Have a script to serve & rotate models according to a policy (e.g., test set score), and clean up legacy models (those at the bottom x% according to the policy or haven’t been served for x months/years)
- **JSONify (hyper)parameters**. No matter how tempting it is to put parameters in your scripts, don’t do it. Instead put them into a global JSON file. 
- **Use virtual environment.** it’s always a good idea to test & integrate virtual environment into your versioning system, e.g., using [Data Science Toolbox](https://github.com/DataScienceToolbox/data-science-toolbox/), [Virtualenv](http://python-guide-pt-br.readthedocs.io/en/latest/dev/virtualenvs/), [Anaconda](https://www.continuum.io/downloads), or even [Docker container](https://www.docker.com/) to create a unified environment for versioning data science.

### Notebook versioning:

**Removing notebook output.** This can either be done manually by clearing all output cells, or programmatically with library such as [`nbstripout`](https://github.com/kynan/nbstripout).

**Refactor the good parts.**  instead of copying/pasting the code block into another notebook, we might want to create a `utils.py` file, refactor the script into a function, and import the function in notebooks next time we use it.

**Naming convention.** Common naming pattern is (date-author-topic.ipynb), so we can search by:

- date (`ls 2017-01-*.ipynb`)
- author (`ls 2017*-johndoe-*.ipynb`)
- topic (`ls *-*-feature_creation.ipynb`)

