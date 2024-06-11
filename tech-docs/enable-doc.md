# Enable Documentation in Backstage

## Step 1: Create `mkdocs.yml`

Create a `mkdocs.yml` file in the root of your repository with the following content:

```yaml
site_name: 'example-docs'

nav:
  - Home: index.md

plugins:
  - techdocs-core
```

## Step 2: Update `catalog-info.yaml`

Update your component's entity description by adding the following lines to its `catalog-info.yaml`:

```yaml
metadata:
  annotations:
    backstage.io/techdocs-ref: dir:.
```

## Step 3: Create Documentation Folder

Create a `/docs` folder in the root of your repository with at least an `index.md` file in it. If you add more Markdown files, update the `nav` in the `mkdocs.yml` file to get proper navigation for your documentation.

### Example `docs/index.md`

```markdown
# example docs

This is a basic example of documentation.
```