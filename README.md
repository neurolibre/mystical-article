# mystical-article

This repository provides a NeuroLibre-compatible article template using MyST (Markedly Structured Text) for creating interactive scientific preprints.

https://events.neurolibre.org/mystical-article

## Learn the ropes of MyST through GitHub Actions

This repository includes a GitHub Actions workflow that builds the MyST-formatted preprint and publishes it to GitHub Pages. 

> [!TIP]
> To enable this, you need to enable GitHub Pages in the repository settings (select GitHub Actions as the `Build and deployment` source).

Once you push your changes to the `main` branch, the GitHub Actions workflow will build the MyST-formatted preprint and publish it to your GitHub Pages.

> [!WARNING]
> The actions file (`.github/workflows/deploy.yml`) attempts to **execute** the executable content in this preprint. However, this doesn't always work smoothly in the GitHub Actions environment, as it requires starting a Jupyter Server and connecting to it. In other words, it may not generate the interactive figures as intended. Nevertheless, any changes you make to the **narrative** content will be reflected in the preprint.

The ultimate guide for authoring narrative and executable content in MyST documents is the [MyST Guide](https://mystmd.org/guide). 

## The real deal: NeuroLibre workflow

Below is a decision tree guiding you through the process of preparing your living preprint before submitting it to the [NeuroLibre](https://neurolibre.org) for publication.

![Decision tree](https://github.com/neurolibre/brand/blob/main/png/myst_flow.png?raw=true)


> [!NOTE]
> This template includes executable content (simple Python code to generate interactive figures) and needs some data to be used as input. Let's take a look at how they are managed.

### The `binder` folder

* `runtime.txt` declares `python-3.10` as the Python version.
* `requirements.txt` declares the Python packages that will be installed in your reproducible runtime environment.
* `data_requirement.json` declares a download source (a simple csv file), which will be downloaded to a folder named `neurolibre-demo-dataset` (project-name). 
    * In your runtime environment the data will be mounted to `data/neurolibre-demo-dataset`, relative to the base if your repository.

> [!TIP]
> Feel free to make small modifications to runtime dependencies and change your code to work with the [provided data](https://raw.githubusercontent.com/plotly/datasets/master/hobbs-pearson-trials.csv). However, if you need to work with a new data, [let us know](https://github.com/neurolibre/info/issues/new?assignees=agahkarakuzu&labels=DOWNLOAD&projects=&template=data_cache.md&title=)

The complete list of REES configuration files for different programming languages can be found [here](https://mybinder.readthedocs.io/en/latest/using/config_files.html).

### NeuroLibre preview Binder X RoboNeuro preview service

We provide a [public Binder instance](https://binder-preview.conp.cloud/) to help you create reproducible, interactive computing environments. Upon successfully building a Binder, a Docker image is pushed to our private registry, tagged with the `commit hash` of your repository.

NeuroLibre can use that Docker image over and over again to build new versions of your living preprint. So hold onto that `commit hash` and keep pushing commits to work on your narrative and executable content! Unless you need to install new dependencies or modify your input data, you don't need to build a new Binder.

![](https://github.com/neurolibre/brand/blob/main/png/let_it_go.png?raw=true)

RoboNeuro will be glad to build the `latest` version of your preprint (from the `main` branch) at each request!

Once you are happy with the current shape and form of your living print, submit it to NeuroLibre! Our team will start a technical screening to ensure that your preprint is ready for publication with the help of RoboNeuro on a GitHub issue.

