<div align="center">
  <a href="https://github.com/Reloaded-Project/reloaded-templates-rust" target="_blank">
    <img src="assets/reloaded-logo.png" alt="reloaded Logo" width="100"/>
  </a>

  <h1 align="center">Reloaded MkDocs Workflow</h1>

  <p>
    <a href="LICENSE">
      <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License">
    </a>
  </p>
</div>

This is a simple action that can be used to create an MkDocs page and upload it to GitHub Pages.

Complete Example (`.github/workflows/mkdocs.yml`):

```yaml
name: MkDocs Build and Deploy

on:
  workflow_dispatch:
  push:
    branches: [ main ]
    paths:
      - "mkdocs.yml"
      - "docs/**"
  pull_request:
    branches: [ main ]
    paths:
      - "mkdocs.yml"
      - "docs/**"

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    steps:
      - name: Deploy MkDocs
        uses: Reloaded-Project/devops-mkdocs@v1
        with:
          requirements: ./docs/requirements.txt
          publish-to-pages: true
          checkout-current-repo: true # Checks out by default.
```

## Setup

To use this action in your own repository:

1. Create a new workflow file (e.g., `.github/workflows/mkdocs.yml`) in your repository.
2. Copy the example usage code from above into the new workflow file.
3. If you have additional Python dependencies, create a `requirements.txt` file in your `docs` directory and list them there. 

Ensure your MkDocs configuration file is named `mkdocs.yml` and located in the root of your
repository (or update the `CONFIG_FILE` parameter in the workflow).

## Configuration

### Inputs

The action supports the following input parameters:

- `mkdocs-version`: (Optional) The version of MkDocs to use. Defaults to `latest`. This corresponds to the version in the Python package index.
- `requirements`: (Optional) The path to a `requirements.txt` file listing additional Python dependencies. Defaults to `./docs/requirements.txt`.
- `config-file`: (Optional) The path to your MkDocs configuration file. Defaults to `mkdocs.yml` in the repository root.
- `publish-to-pages`: (Optional) Whether to publish the generated site to GitHub Pages. Defaults to `true`.
- `checkout-current-repo`: (Optional) Whether to perform a repository checkout before building the site. Defaults to `true`.
- `output-directory`: (Optional) Location of the built MkDocs site. Defaults to `./site`.
- `pre-build-script`: (Optional) The path to a pre-build script to run before building the site.
- `pre-build-shell`: (Optional) The shell to use for running the pre-build script. Defaults to `bash`.

### Workflow Triggers

By default, the example workflow runs on:

- Pushes to the `main` branch that modify the `mkdocs.yml` file or files in the `docs/` directory. 
- Pull requests targeting the `main` branch that modify the `mkdocs.yml` file or files in the `docs/` directory.
- Manual runs triggered via the `workflow_dispatch` event.

Modify the `on` section of the workflow as needed for your desired trigger conditions.

## Viewing the Generated Docs

After a successful run, the generated static site will be available on the GitHub Pages site for your repository. 

To find the URL:

1. Navigate to your repository on GitHub.
2. Go to the "Settings" tab.
3. Click on "Pages" in the side navigation.
4. The GitHub Pages URL will be listed at the top, in the format `https://<user>.github.io/<repo>/`.

## Why this Exists?

The previous actions for MkDocs that I've used always relied on some sort of virtualization,
e.g. the use of Docker.

Not only was that slow, but sometimes it was not very flexible, for example, the docker
image may not have seen the latest version of MkDocs, if it was not well configured.

This action directly invokes `pip` and `python` on the host machine.

## Contributing

Contributions are welcome! If you encounter any issues or have suggestions for improvements,
please open an issue or submit a pull request in this repository.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.