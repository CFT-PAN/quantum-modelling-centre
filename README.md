# Introduction

This website is generated using the static site generator (SSG) Jekyll. 
Unfortunetly Jekyll has limited support for the Windows operating system, and requires a rather involved set-up of Ruby as well as some mangement of Ruby packages called "Gems". 
Knowledge of the Ruby programming langauge itself is not required, however a solution that makes use of a more familiar programming langauge to physics researchers (such as Python, or Julia) may be better for long term maintainablity.
On the plus side, Jekyll is the "default" SSG for Github pages and is thus well intregrated, and support is easy to find online owning to its widespread use.

## Publishing

This website is automatically published and hosted via Github pages provided the repository settings allow for this. 
The website can also be tested locally by first cloning the repository, and then running
```bash
bundle exec jekyll serve
```
in the root directory. 
This will create a directory `_site` in the root directory which should not be tracked by `git` and is therefore included in the `.gitignore` file.
One can then view the website in the browser at the address `http://localhost:4000`.

## Editing

Pages can be either written in markdown (recommended) or HTML.
To contribute to this website, you should first fork the repository, then

  1. `git pull` to sync your fork with this repository (perhaps others have contributed since your last contribution),
  2. make your changes,
  3. test the website *locally*,
  4. commit your changes,
  5. open a pull request to the main repository which will be reveiwed and eventually merged,

You should never contribute directly to the `main` branch of this repository (there are measures in place to prevent this) as this is the branch that automatically gets published.
