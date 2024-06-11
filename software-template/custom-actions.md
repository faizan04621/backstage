# Creating a Custom Action in Backstage

Let's understand how to create a simple action that adds a new file and some contents that are passed as input to the function.

## Step 1: Create a TypeScript File

The first step is to create a TypeScript file, let's name it `custom.ts`.

The location of the `custom.ts` file is not specific, but the simplest is to include it alongside your backend package in `packages/backend`.

### `custom.ts`

```typescript
import { createTemplateAction } from '@backstage/plugin-scaffolder-node';
import { writeFile } from 'fs';

export const createNewFileAction = () => {
  return createTemplateAction<{ contents: string; filename: string }>({
    id: 'bottomline:file:create',
    schema: {
      input: {
        required: ['contents', 'filename'],
        type: 'object',
        properties: {
          contents: {
            type: 'string',
            title: 'Contents',
            description: 'The contents of the file',
          },
          filename: {
            type: 'string',
            title: 'Filename',
            description: 'The filename of the file that will be created',
          },
        },
      },
    },
    async handler(ctx) {
      const { signal } = ctx;
      await writeFile(
        `${ctx.workspacePath}/${ctx.input.filename}`,
        ctx.input.contents,
        { signal },
        _ => {},
      );
    },
  });
};
```

### Explanation

- `id: 'bottomline:file:create'` present in the function is used while calling the action in the `template.yaml` file.
- The core logic of the custom action is present in the `handler` function.
- `ctx` is a context object that is used to access input parameters and provide output.