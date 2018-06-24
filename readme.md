# Wedding Seat Problem

## Running in Docker

Pull the Docker image (first time only)
```bash
docker pull scrussell24/deap
```

Run the Docker container (from the project directory)
```bash
docker run -p 8888:8888 -v `pwd`:/deap scrussell24/deap
```

## Python Notebooks in Source Control

To prevent conflict galore, follow these steps to have git apply a filter when a Python Notebook is staged (without altering your local copy). Do note that if you unstage the notebook, the removal of execution counts and output data will be mixed in with the staged changes.

Open the notebook's metadata (Edit > Edit Notebook Metadata) and add the following to the top level of the json and then save the notebook:

```
{
    "git": {
        "suppress_outputs": true
    },
    ...
}
```

Add the following to `.git/config`:

```
[filter "clean_ipynb"]
    clean = jq --indent 1 --monochrome-output '. + if .metadata.git.suppress_outputs then { cells: [.cells[] | . + if .cell_type == \"code\" then { outputs: [], execution_count: null } else {} end ] } else {} end'
    smudge = cat
```

Create the file `.git/info/attributes` and add the following content:

```
*.ipynb  filter=clean_ipynb
```

To have the current notebook state tracked by source control (execution counts and outputs), just remove the metadata or set `suppress_outputs` to false.

Source: https://gist.github.com/pbugnion/ea2797393033b54674af
