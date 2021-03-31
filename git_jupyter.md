# Notes for Git & Jupyter Notebooks

* I am using [DVC](https://dvc.org/doc/start/data-and-model-versioning) to store and update the data
    * Note that you can *easily* add GDrive as your main data repo if the Dagshub solution below doesn't work out

* [Dagshub](https://dagshub.com/) is a place that hosts both code and data through `git` and `dvc` respectively
    * I am happy to support them and give them feedback, but they are a small company so I would like some coverage that my notebooks are also stored on github.
    * Adding a **second** report to pull and fetch is described [here](https://jigarius.com/blog/multiple-git-remote-repositories)

* Need to ensure notebooks are **cleaned** before committing. I am using a tool called [nb-clean](https://pypi.org/project/nb-clean/) which when you add a filter `nb-clean add-filter` *should* remove outputs before committing. If this isn't dpne, the files could be massive

* Add [`jupyterlab-git`](https://github.com/jupyterlab/jupyterlab-git) to JupyterLab. However you **need to be careful of version mismatching** - you will probably need to **downgrade** the python package so that it matches the UI version
    * I had to use this command `pip install --pre jupyterlab-git==0.30.0b2`
