# Custom Template Creation in Backstage

Templates are stored in the Software Catalog under a kind Template.

## Steps to Create a Custom Template

### 1. Define a `template.yaml` File in `packages → backend`

A simple `template.yaml` definition might look something like this:

```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
# some metadata about the template itself
metadata:
  name: v1beta3-demo
  title: Test Action template
  description: scaffolder v1beta3 template demo
spec:
  owner: backstage/techdocs-core
  type: service

  # these are the steps which are rendered in the frontend with the form input
  parameters:
    - title: Fill in some steps
      required:
        - name
        - repoUrl
      properties:
        name:
          title: Name
          type: string
          description: Unique name of the component
        repoUrl:
          title: Repository Location
          type: string

  # here's the steps that are executed in series in the scaffolder backend
  steps:
    - id: fetch-base
      name: Fetch Base
      action: fetch:template
      input:
        url: ./template
        values:
          name: ${{ parameters.name }}

    - id: publish
      name: Publish
      action: publish:github
      input:
        allowedHosts: ['github.com']
        description: This is ${{ parameters.name }}
        repoUrl: ${{ parameters.repoUrl }}
```

### 2. Add the `template.yaml` File to the Catalog through Static Location Configuration in the `app-config.yaml` File

```yaml
catalog:
  locations:
    - type: url
      target: https://github.com/backstage/software-templates/blob/main/scaffolder-templates/react-ssr-template/template.yaml
      rules:
        - allow: [Template]
    - type: file
      target: template.yaml   # custom template
      rules:
        - allow: [Template]
```

## Frontend Part

To add or remove input fields, edit inside `spec → parameters`.

Let's consider we want to add two input fields, a name, and a repo URL. You need to add the following code to the `template.yaml` file:

```yaml
spec:
  owner: bottomline@gmail.com
  type: website
  parameters:
    - title: Required Details
      required:
        - providerName
        - repoUrl
      properties:
        providerName:
          type: string
          title: name of the provider
          description: Name of the provider will be added in catalog-info.yaml file
        repoUrl:
          type: string
          title: repoUrl
          description: The GitHub repo URL to your catalog-info.yaml file. Example <https://github.com/blob/master/catalog-info.yaml>
```

## Backend Part

To implement different actions, edit inside `spec → steps`.

The scaffolder has several built-in actions for fetching content, registering in the catalog, and creating and publishing a git repository. A list of all registered actions can be found under `/create/actions`. For local development, you should be able to reach them at [http://localhost:3000/create/actions](http://localhost:3000/create/actions).

If we want to extend the functionality of the Scaffolder, we can do so by writing custom actions that can be used alongside our built-in actions. To learn more about custom actions refer to [Writing Custom Actions](https://backstage.io/docs/features/software-templates/writing-custom-actions).

---

Please refer to the following page for a detailed explanation and input examples.