# openrouteservice Workshop Webpage 

## Installation 

1. Create a new python environment. 
2. Install required packages 

    `conda install -c conda-forge jupyter-book ghp-import`


## How to build and deploy

#### 1. Build html files

After making changes to the files, the jupyter book needs to be rebuild. From the repo's root directory, execute 

`jupyter-book build . --all`

#### 2. Deploy to GitHub Pages 

To deploy the current build, execute this from the repo's root directory 

`ghp-import -n -p -f -o _build/html`
