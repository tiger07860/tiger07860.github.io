# tiger07860.github.io

Personal website and blog for Tahir Hussain (UBC Master of Data Science), built with [Quarto](https://quarto.org). It has three computational posts using the Palmer Penguins data:

- `posts/penguins-r`: an analysis in R
- `posts/penguins-python`: an analysis in Python
- `posts/r-and-python`: a bonus post that uses R and Python in one document

The Python environment is pinned with [uv](https://docs.astral.sh/uv/) and the R environment with [renv](https://rstudio.github.io/renv/).

Live site: https://tiger07860.github.io

## 1. Install first

Versions I used:

| Tool | Version | Check with |
|------|---------|------------|
| Quarto | 1.10.18 | `quarto --version` |
| uv | 0.12.5 | `uv --version` |
| R | 4.6.1 | `R --version` |
| Python | 3.14 (installed by uv, pinned in `.python-version`) | `uv run python --version` |

`renv` installs itself the first time R starts in this project. You need an internet connection for the install steps below.

## 2. Build the site

Run every command from the **top level of the repository** (the folder that contains `_quarto.yml`).

**Shell:** clone the repository and create the Python environment from `uv.lock`.

```bash
git clone https://github.com/tiger07860/tiger07860.github.io.git
cd tiger07860.github.io
uv sync
```

**Shell:** start R in this folder. This reads `.Rprofile` and turns renv on.

```bash
R
```

**Inside R:** install the R packages from `renv.lock`, then quit.

```r
renv::restore()
q()
```

If R asks whether to install renv or restore packages, answer `y`.

**Shell:** render the site. `uv run` makes Quarto use the project's `.venv`.

```bash
uv run quarto render
```

## 3. Where the site is and how to open it

The built site is written to the `docs/` folder. Open `docs/index.html` in a web browser, or run:

```bash
uv run quarto preview
```

## 4. Data

All posts use the Palmer Penguins data (Horst, Hill and Gorman, 2020; CC0 licence), from https://allisonhorst.github.io/palmerpenguins/.

- R post: the `palmerpenguins` R package.
- Python post: the `palmerpenguins` Python package.
- Bonus post: the `palmerpenguins` R package.

The data ships inside these packages, so rendering does **not** need the network to fetch data. The network is only needed to install packages in `uv sync` and `renv::restore()`.

## 5. Bonus post: R and Python together

The post `posts/r-and-python/index.qmd` runs R and Python in the same document using the [reticulate](https://rstudio.github.io/reticulate/) package. R computes a summary (`species_summary`), Python reads it with `r.species_summary`, and R reads the Python results back with `py$python_summary` and `py$heaviest`.

- `reticulate` is recorded in `renv.lock`.
- The post points reticulate at this project's Python with `use_virtualenv("../../.venv", required = TRUE)`. That path is relative to the post's folder, so run `uv sync` first to create `.venv`.
- No extra commands are needed beyond section 2. `uv run quarto render` builds it along with the other posts.
- Once the site is built, its page is `docs/posts/r-and-python/index.html`, and on the live site it is at https://tiger07860.github.io/posts/r-and-python/.
