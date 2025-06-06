# Keras.io documentation generator

This repository hosts the code used to generate the [keras.io](https://keras.io) website.

## Generating a local copy of the website

```
pip install -r requirements.txt
# Update Keras version to 3
pip install keras==3.0.2
cd scripts
python autogen.py make
python autogen.py serve
```

If you have Docker (you don't need the gpu version of Docker), you can run instead:

```
docker build -t keras-io . && docker run --rm -p 8000:8000 keras-io
```

It will take a while the first time because it's going to pull the
image and the dependencies, but on the next times it'll be much faster.

Another way of testing using Docker is via our Makefile:

```
make container-test
```

This command will build a Docker image with a documentation server and run it.


## Call for examples

Are you interested in submitting new examples for publication on keras.io?
We welcome your contributions!
Please read the information below about adding new code examples.

We are currently interested in [the following examples](https://github.com/keras-team/keras-io/blob/master/call_for_contributions.md).


## Fixing something in an existing code example

### Fixing typos

If your fix is very simple, please send out a PR simultaneously updating
the `.py`, the `.md`, and the `.ipynb` files for the example.

## More extensive fixes

For larger fixes, please send a PR that only includes the `.py` file,
so we only update the other two files once the code has been reviewed
and approved.


## Adding a new code example

Keras code examples are implemented as **tutobooks**.

A tutobook is a script available simultaneously as a notebook,
as a Python file, and as a nicely-rendered webpage.

Its source-of-truth (for manual edition and version control) is
its Python script form, but you can also create one by starting
from a notebook and converting it with the command `nb2py`.

Text cells are stored in markdown-formatted comment blocks.
the first line (starting with `"""`) may optionally contain a special
annotation, one of:

- `shell`: execute this block while prefixing each line with `!`.
- `invisible`: do not render this block.

The script form should start with a header with the following fields:

```
Title: (title)
Author: (could be `Authors`: as well, and may contain markdown links)
Date created: (date in yyyy/mm/dd format)
Last modified: (date in yyyy/mm/dd format)
Description: (one-line text description)
Accelerator: (could be GPU, TPU, or None)
```

To see examples of tutobooks, you can check out any `.py` file in `examples/` or `guides/`.


### Creating a new example starting from a `ipynb` file

1. Save the `ipynb` file to local disk.
2. Convert the file to a tutobook by running:
(assuming you are in the `scripts/` directory)


```
python tutobooks.py nb2py path_to_your_nb.ipynb ../examples/vision/script_name.py
```

This will create the file `examples/vision/script_name.py`.

3. Open it, fill in the headers, and generally edit it so that it looks nice.

NOTE THAT THE CONVERSION SCRIPT MAY MAKE MISTAKES IN ITS ATTEMPTS
TO SHORTEN LINES. MAKE SURE TO PROOFREAD THE GENERATED .py IN FULL.
Or alternatively, make sure to keep your lines reasonably-sized (<90 char)
to start with, so that the script won't have to shorten them.

4. Run `python autogen.py add_example vision/script_name`. This will generate an ipynb and markdown
rendering of your example, creating files in `examples/vision/ipynb`,
`examples/vision/md`, and `examples/vision/img`. Do not modify any of these files by hand; only the
original Python script should ever be edited manually.
5. Submit a PR adding `examples/vision/script_name.py` (only the `.py`, not the generated files). Get a review and approval.
6. Once the PR is approved, add to the PR the files created by the `add_example` command. Then we will merge the PR.


### Creating a new example starting from a Python script

1. Format the script with `black`: `black script_name.py`
2. Add tutobook header
3. Put the script in the relevant subfolder of `examples/` (e.g. `examples/vision/script_name`)
4. Run `python autogen.py add_example vision/script_name`. This will generate an ipynb and markdown
rendering of your example, creating files in `examples/vision/ipynb`,
`examples/vision/md`, and `examples/vision/img`. Do not modify any of these files by hand; only the
original Python script should ever be edited manually.
5. Submit a PR adding `examples/vision/script_name.py` (only the `.py`, not the generated files). Get a review and approval.
6. Once the PR is approved, add to the PR the files created by the `add_example` command. Then we will merge the PR.


### Previewing a new example

You can locally preview what the example looks like by running:

```
cd scripts
python autogen.py add_example vision/script_name
```

(Assuming the tutobook file is `examples/vision/script_name.py`.)

NOTE THAT THIS COMMAND WILL ERROR OUT IF ANY CELLS TAKES TOO LONG
TO EXECUTE. In that case, make your code lighter/faster.
Remember that examples are meant to demonstrate workflows, not
train state-of-the-art models. They should
stay very lightweight.

Then serving the website:

```
python autogen.py make
python autogen.py serve
```

And navigating to `0.0.0.0:8000/examples`.


## Read-only autogenerated files

The contents of the following folders should **not** be modified by hand:

- `site/*`
- `sources/*`
- `templates/examples/*`
- `templates/guides/*`
- `examples/*/md/*`, `examples/*/ipynb/*`, `examples/*/img/*`
- `guides/md/*`, `guides/ipynb/*`, `guides/img/*`


## Modifiable files

These are the only files that should be edited by hand:

- `templates/*.md`, with the exception of `templates/examples/*` and `templates/guides/*`
- `examples/*/*.py`
- `guides/*.py`
- `theme/*`
- `scripts/*.py`
#   M L - N L P  
 