---
layout: post
title: Problem solver for GitHub Workflow
comments: true
image_url: ''
tags: ['setup', 'se']
---

I wanted to create a GitHub workflow that did web API query and return the results as text files in the repo. Here are several problems I've solved during the development.

<!--more-->

## Passing keys and tokens via secrets to the web API

Several tokens and secrets are necessary to query the web API. I stored that as GitHub secrets and access them in the workflow file via:

{% raw %}
```yaml
jobs:
  job-name:
    environment: query_env
    runs-on: ubuntu-latest
    name: ...
    steps:
      ...
      - name: Query API
        id: query_api
        env:
          oauthtoken: ${{ secrets.OAUTH_TOKEN }}
          oauthtokensecret: ${{ secrets.OAUTH_TOKEN_SECRET }}
        run: python run_script.py $oauthtoken $oauthtokensecret
        ...
```
{% endraw %}

After running `run_script.py`, there will be several `.txt` files produced in the directory `data_dir/` inside the repository which I want to push to the GitHub repository. I tried committing and pushing the files with `actions/checkout@v4` but it does not work:

{% raw %}
```yaml
      ...
      - name: add files to git # Below is a version that does not work
        uses: actions/checkout@v4
        with:
           token: ${{ secrets.REPO_TOKEN }}
      - name: do the actual push
        run: |
          git add data_dir/*.txt
          git commit -m "add files"
          git push

```
{% endraw %}

Running this, I receive an error: `nothing to commit, working tree clean. Error: Process completed with exit code 1.` .

The version that works eventually looks like this:

{% raw %}
```yaml
      - name: Commit files
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
           token: ${{ secrets.REPO_TOKEN }}
```
{% endraw %}

Note that it would commit all files produced to the repository, including some unwanted cached files. Therefore, I included a step before this to clean up the files:

{% raw %}
```yaml
      - name: Remove temp files
        run: |
          [ -d "package_dir/__pycache__" ] && rm -r package_dir/__pycache__
```
{% endraw %}
