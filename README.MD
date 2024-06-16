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

## Example Usage

`.github/workflows/mkdocs.yml`:

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
          publish-to-pages: ${{ github.event_name == 'push' }}
          checkout-current-repo: true
```

## Setup

To use this action in your own repository:

1. Create a new workflow file (e.g., `.github/workflows/mkdocs.yml`) in your repository.
2. Copy the example usage code from above into the new workflow file.
3. If you have additional Python dependencies, create a `requirements.txt` file in your `docs` 
   directory and list them there.

Ensure your MkDocs configuration file is named `mkdocs.yml` and located in the root of your
repository (or update the `config-file` parameter in the workflow).

## Configuration

### Inputs

| Input                     | Required | Default                   | Description                                    |
| ------------------------- | -------- | ------------------------- | ---------------------------------------------- |
| `mkdocs-version`          | No       | `latest`                  | MkDocs version to use                          |
| `requirements`            | No       | `./docs/requirements.txt` | Path to the requirements.txt file              |
| `config-file`             | No       | `mkdocs.yml`              | Path to the mkdocs.yml file                    |
| `publish-to-pages`        | No       | `true`                    | Whether to publish to GitHub Pages             |
| `checkout-current-repo`   | No       | `true`                    | Whether to perform repository checkout         |
| `output-directory`        | No       | `./site`                  | Custom output directory for the MkDocs build   |
| `pre-build-command`       | No       |                           | Path to the pre-build command                  |
| `pre-build-command-shell` | No       | `bash`                    | Shell to use for running the pre-build command |

### Workflow Triggers

By default, the example workflow runs on:

- Pushes to the `main` branch that modify the `mkdocs.yml` file or files in the `docs/` directory.
- Pull requests targeting the `main` branch that modify the `mkdocs.yml` file or files in the 
  `docs/` directory.
- Manual runs triggered via the `workflow_dispatch` event.

Modify the `on` section of the workflow as needed for your desired trigger conditions.

## Examples

Find more examples in [the tests](./.github/workflows/test-mkdocs-workflow.yml).

### Custom MkDocs Version

```yaml
- name: Deploy MkDocs
  uses: Reloaded-Project/devops-mkdocs@v1
  with:
    mkdocs-version: '9.5.24'
```

### Custom Pre-build Command (Bash)

```yaml
- name: Deploy MkDocs
  uses: Reloaded-Project/devops-mkdocs@v1
  with:
    pre-build-command: touch pre-build-bash-executed.txt
    pre-build-command-shell: bash
```

### Custom Output Directory

```yaml
- name: Deploy MkDocs
  uses: Reloaded-Project/devops-mkdocs@v1
  with:
    output-directory: custom_site_dir
```

## Viewing the Generated Docs

After a successful run, the generated static site will be available on the GitHub Pages site for
your repository.

To find the URL:

1. Navigate to your repository on GitHub.
2. Go to the "Settings" tab.
3. Click on "Pages" in the side navigation.
4. The GitHub Pages URL will be listed at the top, in the format 
   `https://<user>.github.io/<repo>/`.

## Why this Exists?

Previous actions for MkDocs that I've used relied on virtualization, such as Docker. This was slow
and sometimes inflexible, for example, the Docker image may not have the latest version of MkDocs
if not configured properly.

This action directly invokes `pip` and `python` on the host machine for improved performance and
flexibility.

## Contributing

Contributions are welcome! If you encounter any issues or have suggestions for improvements,
please open an issue or submit a pull request in this repository.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.