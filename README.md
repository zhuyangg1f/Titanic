# DSAA R Project Template

This repository contains a skeleton for R projects that all staff should strive to follow for production projects. The goal of this is to create a standardized workflow for projects, so that details irrelevant to the project (ie. how do I deploy something) can be abstracted out of our day-to-day workflows. This standardization will also facilitate collaboration, as all project codebases will look similar structurally, allowing for the same development workflows. 

This template may be subject to change in the future, so make sure you stay up to date :)

# Initial set-up

> Code chunks starting with `$` indicate a terminal command; those starting with `>` indicate an R command

1. **[ON GITHUB]** Create a new Github repository, and under "Repository Template", select "LKS-CHART/project-template" (referred to as `project_repo` from here onwards)

2. **[IN RSTUDIO]** Clone your `project_repo` by creating a new RStudio Project from a Version Control source using the repository URL for `project_repo` (eg. git@github.com:LKS-CHART/project_repo.git). 

3. **[IN RSTUDIO]** Initialize an `renv` project library: `> renv::init()`. After initialization, install `devtools`: `renv::install(devtools)`.

4. **[IN RSTUDIO]** In your new `project_repo` RStudio project, create a new R package under `pkg/`: `> usethis::create_package(path = "./pkg", fields = list(Package = "yourPackageName"))`. Replace `yourPackageName` with the name of your choosing.  If you are developing a Shiny golem application, you can use `golem::create_golem` in place of `usethis::create_package`.

    + Note: you may get a warning on this step about creating a package inside a project, just ignore it.

    + Note: a new RStudio project named `yourPackageName.Rproj` will be created after this step in the `pkg/` subfolder. In order to leverage your `renv` project library initialized in Step 6, you should **not** use the `yourPackageName` RStudio project, and instead use the `project_repo` RStudio project.
    
At this point, you should have these things set up:

1. A new `project_repo` Github repository for your new project.

2. `project_repo` should contain the skeleton structure from the `deployment_template` repository.

3. `project_repo` should be cloned to your local development environment.

4. `project_repo/pkg` should contain your `yourPackageName` package source, following the standard R package structure (eg. contains a `DESCRIPTION` file, an `R/` sub-directory).


# `pkg/` - the project package

The `pkg/` sub-folder contains the project package. This means that all project-specific functions, APIs, app modules, etc. should live here. This package contains the guts of your project, and this section provides some tips on how to optimally use this project structure.

## Development workflow

Your day-to-day development will mostly pertain to files within the `pkg/` sub-directory. As much as possible, try to develop code within the `pkg/` sub-directory; you should not have any new function definitions or complicated computations/data processing outside of this sub-directory. This also means that you should try to minimize the lines of code within each deployment under `deployments/`, as code living there will likely not be tested. 

For example, a Shiny golem application deployment `app.R` file may look something like this:

```{r}
# deployments/app/awesome_app.R

# Launch the ShinyApp (Do not remove this comment)
# To deploy, run: rsconnect::deployApp()
# Or use the blue button on top of this file

yourPackageName::run_app(some_runtime_parameter)

```

For a Plumber API, this may look something like this: 

```{r}
# deployments/api/awesome_api.R

library(plumber)

#* @apiTitle A really awesome Plumber API

#* Get model predictions
#* @param input_data 
#* @post /predict
function(input_data) {
    yourPackageName::predict(input_data)
}

```

The functions `run_app` and `predict` will be defined within your project package under `pkg/R/run_app.R` `pkg/R/predict.R`, with associated tests under `pkg/tests`.

RMarkdown documents may contain some code additional code, but try to incorporate any complicated calculations or methods as individual functions within your package, and then calling those functions inside your RMarkdown file. For pipelines written in RMarkdown, you can use the `drake` package (see example [here](https://github.com/LKS-CHART/drakeExample)).

## Contributing to the project

- Develop new features and bug fixes in a feature branch (ie. not `staging`, or
`master`).
- Include the relevant ClickUp ID for the feature you are working on in each
commit message. ClickUp can pickup on these IDs to manually update task
progress, and this helps us associate each commit to a task.
- Please include new test cases when contributing new code.
- When your feature is ready for review, create a PR into the `staging` branch
and ask a collaborator to review it. Please complete the PR template when
opening a new PR.
- When your PR passes review, you may merge it into `staging`, triggering an
automatic deployment to the staging environment.
- Confirm the changes are functional on the staging environment and complete any
required UI Testing.
- When ready to deploy to production, create a PR from `staging` into `master`.
Assign a project lead to approve the request.
- Once the PR passes review, and you have notified any relevant stakeholders of
the new deployment, merge the changes into `master`. The changes will
automatically deploy to the production environment.
- Tag the new release using semantic versioning on GitHub and delete your
feature branch (locally and on GitHub). More on this in [this section](#maintaining-your-project-package)

## OK, I have a package. Now what?

Once you have your project repository set up and your project package is created, you will need to submit a request to have your project package served on RStudio Package Manager (RSPM). Going through this process will allow your package to be installable from our development (eg. laptops) and production environments (eg. Veneto). This will then allow you to install and use your package as a dependency in your deployments (eg. allowing you to deploy a Shiny application that calls `yourPackageName::run_app()`). 

To have your package served on RSPM:

1. Ensure that your R package can be installed using `remotes::install_github("LKS-CHART/project_repo", subdir = "pkg")`

2. Ensure that any dependencies that your package has is also available on RSPM. All CRAN packages should be available, but this may not be the case for Github packages. If a Github dependency is not available, you will also need to submit a request for that package to be added to RSPM.

3. [Complete the Click-up request form](https://forms.clickup.com/f/27kem-7665/U3047DXL6TGY5CDVEI) and send Colin & David an email/Slack DM. 

4. Colin/David will add your package to RSPM, and all **tagged releases** of your package will be automatically synced to RSPM.

## Maintaining your project package

After your package has been added to RSPM, any updates to your package will need to be **versioned** and **tagged**. This is crucial, as RSPM will only sync your package if the it detects a new Github tag. This has several implications:

1. As you develop your project, try to plan releases with your Project Manager and project team. You should work together to determine what are the important features to work on, and group these into releases. 

2. Once you have identified a set of tasks to complete for your next release, work on these tasks in separate feature branches, and merge these back into `staging` or a `release-version` branch when the task is completed. 

3. Make sure to increment your package DESCRIPTION version number ([how to choose a version number?](https://r-pkgs.org/release.html#release-version))

4. Once you have completed all tasks for a release and incremented your version number, you can merge the `release-version` branch back to the `master` branch, and [create a tagged release](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/managing-releases-in-a-repository). **Note: your tag version name must match your DESCRIPTION version number (eg. if your version number is 1.0.0, your tag should be named 1.0.0)**

5. All tagged releases will be synced and installable from RSPM using `install.packages("yourPackageName", repos = "http://172.27.21.214:4242/prod/latest")`


## Exceptions

For the most part, you can follow the same development workflow as if you were developing the package normally, with a few exceptions.

Since your package is no longer in the root of your working directory, for various package development tools you will now need to specify that the package is under `pkg/`. For example:

* `devtools::load_all()` will need to be replaced with `devtools::load_all(path = "./pkg")`
* `devtools::test()` will need to be replaced with `devtools::test(pkg = "./pkg")`
* `usethis::use_package()` will unfortunately not work, since it has a rigid expectation that the package `DESCRIPTION` file be in the root folder. You will need to manually add package dependencies by editing the `DESCRIPTION` file directly. See [here](https://r-pkgs.org/description.html#dependencies) for more info on handling package dependencies.

Unfortunately, this also means that you won't be able to use RStudio's built-in keyboard shortcuts for these two commands, but [here is a workaround](https://github.com/gadenbuie/shrtcts) to create your own custom keyboard shortcuts.

# Deployments

## Deployments file structure

All deployments should be placed under the `deployments/` sub-folder. Each deployment should be placed in its own folder, similar to what is illustrated below:

```
project_repo/
├── README.md
├── .gitignore
│   ├── deployments/
|       ├── api/
|           ├── cool_api/
|           ├── awesome_api/
|       ├── app/
|           ├── fancy_app/
|           ├── slick_app/
|       ├── rmarkdown/
|           ├── slickity_report/
|           ├── sleek_report/
│   └── pkg/
|       ├── ...
└── ...
      
```

Within each deployment's folder, it may look something like this:

```
...
│   ├── deployments/
|       ├── api/
|           ├── cool_api/
|               ├── plumber.R
|               ├── manifest.json
|       ├── app/
|           ├── fancy_app/
|               ├── app.R
|               ├── manifest.json
|       ├── rmarkdown/
|           ├── slickity_report/
|               ├── slickity_report.Rmd
|               ├── manifest.json
...
```

If you are not using Git-backed deployment, you will not have an associated `manifest.json` for each deployment. Simply use push button deployment from RStudio, and it will create the necessary deployment configurations in a `rsconnect/` sub-folder. 

## Deployment workflow

All projects should have at least two branches: 1) a `master` or `main` branch, and 2) a `staging` branch. The `master`/`main` branch should always reflect what is deployed in production; the `staging` branch should contain new features that are not ready to be pushed to production. You may have additional branches (eg. day-to-day feature branches like `f/add-this-feature`) where you will develop new features, which may then be merged back to `staging` after code review.

Following this branch scheme, you can set up your production and staging deployments from their respective branches. Your deployments should follow this URL structure:

`staging` branch --> RStudio Connect `.../staging/<deployment_name>`

`master`/`main`  branch  --> RStudio Connect `.../<deployment_name>`

## Git-backed deployment vs. Push-button deployment

RStudio Connect supports Git-backed deployment (please see [documentation in Clickup](https://app.clickup.com/2346452/v/dc/27kem-5880/27kem-2821)) and push-button deployment from RStudio. This project structure naturally supports Git-backed deployment, but for applications that are not frequently updated, you may wish to use push-button deployment. In general, for larger projects where multiple team members are developing code, Git-backed deployment is preferred as the `manifest.json` fully specifies the deployment, and is editable by any team member. 
