# Contributing a base image

Before you start, read RAICo's Docker standards:
https://github.com/Adorno-Lab/development-guidelines/tree/main/3-docker-standards

In short: images are published to `ghcr.io/adorno-lab/`, tagged with both
`latest` and a `DD_MM_YYYY_HH_MM_SS` timestamp.

Images are organized **by project, then by experiment** — a project
(`robot_laser_cutting`, `clerice_b1`, `clerice_g1`, `clerice_h1`, ...) can
hold several experiments, each with its own base image:

```
images/<project_name>/<experiment_name>/Dockerfile
```

For example: `images/clerice_h1/demo1_whole_body_control`.

## Adding a new base image

1. **Fork this repository.**
   You cannot create branches or push directly to this repository. You will
   need to fork this repository before contributing to it.

2. **Create the experiment directory.**
   ```
   images/<project_name>/<experiment_name>/Dockerfile
   ```
   Add any other files the build needs (scripts, configs) alongside it.
   Use the project's existing name if it already has other experiments, or
   a new project name if this is its first. The two directory names become
   both the GHCR image path and the workflow's path filter.

3. **Add its workflow.**
   Copy `.github/workflow-templates/build-and-publish.yml` to
   `.github/workflows/<project_name>__<experiment_name>.yml` and replace
   every `<your_project_name>` and `<your_experiment_name>` placeholder
   with your directory names. Any of the existing
   `images/<project_name>/<experiment_name>` +
   `.github/workflows/<project_name>__<experiment_name>.yml` pairs (e.g.
   `clerice_h1/demo1_whole_body_control`) is a working reference.

4. **Open a pull request.**
   - You cannot push directly to `main`; a maintainer must review and
     approve.
   - Opening the PR runs your image's workflow in *build-only* mode
     (`push: false`) as a required check, so you'll see build failures
     before review.
   - Because the workflow's `paths:` filter is scoped to
     `images/<your_project_name>/<your_experiment_name>/**`, only your
     image builds — a PR that only touches one experiment never rebuilds
     any other experiment, in this project or any other.

5. **After merge.**
   The same workflow re-runs on the merge commit to `main`, this time
   building *and* pushing to
   `ghcr.io/adorno-lab/<your_project_name>/<your_experiment_name>` with the
   `latest` and timestamp tags.

## Updating an existing base image

Edit files under `images/<project_name>/<experiment_name>/` and open a PR
as above — no workflow changes needed, since it's already wired to that
path.
