---
numbering:
  heading_2: false
  figure:
    template: Fig. %s
---

## Articles with code vs articles from code

One of the main advantages of articles written in MyST Markdown is the fact that you can bundle several types of outputs (such as figures, tables, equations, etc.) from your Jupyter Notebooks in a single document. This is made possible by the use of `directives`, which are special commands that instruct MyST Markdown to include the content of a notebook, a file, or a chunk of text in your document or cite references. You can use DOIs, [](https://doi.org/10.31219/osf.io/h89js) or a local bibliography file (`paper.bib`) for citations @Boudreau2023.


:::{figure} static/banner.jpg

A funny take on the difference between articles with code and articles from code.
:::

Let's see how directives work with a simple example by rendering a video from an external source:

:::{iframe} https://cdn.curvenote.com/0191bd75-1494-72f5-b48a-a0aaad296e4c/public/links-8237f1bb937ea247c2875ad14e560512.mp4
:label: figvid
:width: 100%

Video reused from [mystmd.org](https://mystmd.org) (CC-BY-4.0, [source](https://mystmd.org/guide)).
:::

Yet, the main purpose of this article is to not to showcase all the [authoring tools](https://mystmd.org/guide/typography) available in MyST Markdown, but rather to provide a simple template to get you started with your own article to publish on NeuroLibre.


:::{seealso}
You can refer to the [MyST Guide](https://mystmd.org/guide/typography) to see all the cool stuff you can do with MyST Markdown, such as creating a `mermaid` diagram like this:

```{mermaid}
flowchart LR
  A[Jupyter Notebook] --> C
  B[MyST Markdown] --> C
  C(mystmd) --> D{AST}
  D <--> E[LaTeX]
  E --> F[PDF]
  D --> G[Word]
  D --> H[React]
  D --> I[HTML]
  D <--> J[JATS]
```

Or you can see how hover-over links work for [wikipedia sources](https://en.wikipedia.org/wiki/Wikipedia#:~:text=Wikipedia%20is%20a%20free%20content,and%20the%20wiki%20software%20MediaWiki.) and cross references figures (e.g., [Fig. %sf](#fig1), [Figure %sf](#fig2), [Video %sf](#figvid)).
:::

Typically, when publishing an article following the traditional route, you would write your article in a word processor where you need to deal with the generation of figures, tables etc. elsewhere, and then bring them together in the final document manually. This eventually leads to a cluttered set of files, code, dependencies, and even data that are hard to manage in the long run. If you've been publishing articles for a while, you probably know what we are talking about:

> Where is the endnote reference folder I used for this article?

> What is the name of the script I used to generate the second figure? This script has the title `fig_2_working.py` and is in the  `karakuzu_et_al_2016_mrm` folder, but it does not seem to be the one that generated the figure...

> I cannot create the same runtime environment that I used for this analysis in my current project because `python 3.8` is not available in the current distribution of Anaconda... It is so tricky to get this running on my new computer...

MyST Markdown offers a powerful solution to this by allowing you to create an article ✨from code✨, linking all the pieces of your executable and narrative content together in the body of this one document: your canvas.

:::{figure} https://cdn.curvenote.com/0191bd75-1494-72f5-b48a-a0aaad296e4c/public/reuse-jupyter-output-2e6bfa91772dbb6bbc022dc6aee80d2b.webp
:label: fig0

An article with two figures created in Jupyter Notebooks. Each figure can be labeled directly in the notebook and reused in any other page directly.

Figure reused from [mystmd.org](https://mystmd.org) (CC-BY-4.0, [source](https://mystmd.org/guide/reuse-jupyter-outputs#reuse-jupyter-outputs)).
:::



For example, the following figure is the output of the `content/fig_1.ipynb` notebook:

:::{figure} #fig1cell
:label: fig1

An example of a figure generated from a Jupyter Notebook that lives in the `content` folder of this repository. Check `content/figure_1.ipynb` to see how this figure was generated and where the label `#fig1cell` is defined.
:::

Here is another figure generated from another notebook:

:::{figure} #fig2cell
:label: fig2

An example of a figure generated from a Jupyter Notebook that lives in the `content` folder of this repository.  Check `content/figure_2.md` to see how this figure was generated and where the label `#fig2cell` is defined.
:::

Both interactive, cool right! All your assets are centralized in this one document, which ideally lives in a GitHub repository. You may forget what you did, but your commit history will be there to remind you.

## NeuroLibre and MyST Markdown

NeuroLibre is a reproducible preprint publisher that makes it a seamless experience to publish preprints written in MyST Markdown, and commits to the long term preservation of the content, runtime, data, and the functionality of the code.

:::{seealso}
You can refer to [this blogpost](https://conp.ca/about-neurolibre/#:~:text=NeuroLibre%20is%20a%20platform%20for,articles%2C%20tutorials%2C%20and%20reports) for more information about NeuroLibre.
:::

### Data

Unless your preprint does not include any output that requires computational resources, you typically need to provide a set of inputs to supplement the generation of the assets in your article. The type and size of the data can vary greatly from one article to another, from a `50KB` excel spreadsheet to a `2GB` neuroimaging dataset of brain images.

The only requirement is that the data must be publicly available to be accessed by NeuroLibre. To specify your data dependencies, you can provide a `binder/data_requirement.json` file in the root of your repository, with the following structure:

```json
{
  { "src": "https://drive.google.com/uc?id=1hOohYO0ifFD-sKN_KPPXOgdQpNQaThiJ",
  "dst": "../data",
  "projectName": "neurolibre-demo-dataset"
  }
}
```

:::{note}
The above configuration specifies that the data will be downloaded from Google Drive and placed in and saved in a `data/neurolibre-demo-dataset` at the root of your repository. The `dst` field indicates `../data` as the root of the repository is one directory up from the `binder` directory where the `data_requirement.json` file is located.

Even when the `dst` field is specified differently, NeuroLibre will always mount the data in the `data` folder at the root of your repository. We recommend you to follow the same convention while working locally and to remember to `.gitignore` the `data` folder!
:::

:::{seealso}
You can refer to [this documentation](https://docs.neurolibre.org/en/latest/STRUCTURE.html#the-binder-folder-data) for more information about the `binder/data_requirement.json` file and the available options to specify your data dependencies.
:::

#### How can I get NeuroLibre to cache my data dependencies? 

You can use [this issue template](https://github.com/neurolibre/info/issues/new?assignees=agahkarakuzu&labels=DOWNLOAD&projects=&template=data_cache.md&title=) to request the caching of your data dependencies.

### Code 

NeuroLibre follows the [reproducible runtime environment specification (REES)](https://repo2docker.readthedocs.io/en/latest/specification.html) to define a runtime environment for your preprint. Any programming language supported by Project Jupyter (e.g. python, R, julia, etc.) can be used to create your executable content, where you place necessary [BinderHub configuration files](https://mybinder.readthedocs.io/en/latest/using/config_files.html#config-files) in the `binder` folder.

#### How much computational resources are available and for how long my notebooks can run to generate the outputs?

For each preprint, we guarantee a minimum of 1 CPU core and 4 GB of RAM (8GB maximum), and a maximum of 1 hours of runtime to execute all the notebooks in the `content` folder.

> Do you really want someone to run your code for 1 hour? Probably not.

Even though long-running code cells may be of interest to interactive tutorials, a reader who is interested in reproducing your Figure would be less likely to wait for more than a few minutes for the outputs to be generated. This is why we encourage you to create notebooks that can be run in the "attention span" of a reader.

### Preview your preprint

#### Locally

It is always a good practice to be able to build your MyST article locally before publishing it to NeuroLibre. If you install MyST as described [here](https://mystmd.org/guide/installing), in a virtual environment that has all your code dependencies installed, you can build your myst article:

```bash
cd path/to/your/repo
myst build --execute --html
```

:::{note}
NeuroLibre also supports Jupyter Book for publishing preprints. You can refer to [this documentation](https://jupyterbook.org/en/stable/intro.html) to find out more about it. However, as of late 2024, MyST is our recommended route for writing preprints.
:::

#### Roboneuro preview service

If you have a data dependency and have NeuroLibre cached it for you, you can start using the Roboneuro preview service to build your preprint: https://robo.neurolibre.org

#### Technical screening

Once you submit your preprint to NeuroLibre, our team will perform a technical screening to ensure that your preprint can be built successfully. This is to make sure that your preprint is in line with our guidelines and to avoid any issues that may arise during the build process.

You can visit our technical screening repository [neurolibre/neurolibre-reviews](https://github.com/neurolibre/neurolibre-reviews/issues) to see examples of this process.

### After your preprint is published

We archive all the reproducibility assets of your preprint on Zenodo and link those objects to your reproducible preprint that is assigned a DOI and indexed by [Crossref](https://www.crossref.org/) (also by Google Scholar, Researchgate, and other platforms that use Crossref metadata).

Your preprint can be found, cited, and more importantly, reproduced by any interested reader, and that includes you (probably a few years after you published your preprint)!

### Is the idea of wanting to publish a dashboard with your preprint too crazy?

Absolutely not! We encourage you to publish your dashboard alongside your preprint to showcase your results in the best way possible.

:::: {admonition} An interactive dashboard developed with R Shiny
:class: tip
:::{iframe} https://shinybrain.db.neurolibre.org
:width: 100%
:label: intdashboard
:align: center

MRShiny Brain interactive dashboard at [https://shinybrain.db.neurolibre.org](https://shinybrain.db.neurolibre.org)
:::
::::

:::: {admonition} An award-winning dashboard developed using Plotly Dash
:class: tip
:::{iframe} https://rrsg2020.db.neurolibre.org/
:width: 100%
:label: intdashboard2
:align: center

ISMRM RRSG 2020 interactive dashboard at [https://rrsg2020.db.neurolibre.org/](https://rrsg2020.db.neurolibre.org/)
:::
::::

These dashboards [](#intdashboard) and [](#intdashboard2) are embedded in their respective NeuroLibre preprints! If you are interested in publishing your own dashboard with NeuroLibre, please open an issue using [this template](https://github.com/neurolibre/info/issues/new?assignees=agahkarakuzu&labels=dashboard&projects=&template=new_dashboard.md&title=%5BNEW+DASHBOARD%5D).

If you have any questions or need further assistance, please reach out to us at `info@neurolibre.org`.
