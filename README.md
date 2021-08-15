### How I created this website: Process

# Website making using Hugo, Github pages, and Github actions.
Here are the instructions step by step

## Prerequisite

- Create a Github account
- Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Hugo](https://gohugo.io/getting-started/installing/)

## 
- Create a new Github repository named `{anything}`
- Add a README and make an initial commit
- Clone the repository on your computer

```
$ cd Downloads
$ git clone {the link that we copied directly from github repository}
```

- Initialize a new Hugo site in the same directory

```
$ hugo new site {the name of the cloned repository} --force
```

- Now, we can add the theme in two ways.
  - **First way: Add a [theme](https://themes.gohugo.io/) using git submodule**
    ```
    $ git submodule add https://github.com/ojroques/hugo-researcher.git themes/researcher
    ```
  - **Second way: copying mannually**
    - Download the theme mannually
    - Extract the zip file
    - Copy the folder into `/themes`
    
- Copy all contents from `themes/researcher/exampleSite` to the root folder and edit as desired
- Run the following command and check the website locally at http://localhost:1313/{optional-path-defined-in-base-url}

```
$ hugo server
```

- Create a `.github/workflows/gh-pages.yaml` file in the root directory and copy the following contents there:

```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          force_orphan: true  # Publish branch with only the latest commit
```

- Make a commit and push to Github
```
$ git add . 
$ git commit -m "{whatever you want}"
$ git push origin main
```

- If remote repository has different commits than local repository, I have to pull the updates from remote before push. (Remote repository = Github repository, local repository = local folder in the computer)
```
$ git pull origin main
```

- Go to Github and check the status of the workflow on the `Actions` tab. It will publish the generated website on the `gh-pages` branch

- Go to `Settings > Pages > Source` and select the `gh-pages` branch

- Your website should be published at `{username}.github.io/{path}`

