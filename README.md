# raico_base_images

Dockerfiles for RAICo's base images. Each image is built and published to
`ghcr.io/adorno-lab/` only when its own files change, via per-image GitHub
Actions workflows triggered on merge to `main`.

Follows RAICo's [Docker standards](https://github.com/Adorno-Lab/development-guidelines/tree/main/3-docker-standards).

## Layout

Images are organized by project, and each project can hold several
experiments — one directory, one Dockerfile, one workflow per experiment:

```
images/<project_name>/<experiment_name>/Dockerfile       base image for that experiment
.github/workflows/<project_name>__<experiment_name>.yml  its build-and-publish workflow
.github/workflow-templates/                               template to copy for a new one
```

Current projects and experiments:

- `images/robot_laser_cutting/demo1_drawing_task`
- `images/clerice_g1/demo1_manipulation`
- `images/clerice_h1/demo1_whole_body_control`

(`clerice_b1` will be added later, following the same pattern.)

Each Dockerfile above is a placeholder (`FROM ubuntu:24.04`) — replace its
contents with the experiment's actual base image definition. A project can
have any number of experiment subdirectories; each gets its own
Dockerfile + workflow pair.

## Adding or changing a base image

Direct pushes to `main` are blocked — all changes go through a pull
request for review. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full
steps.
