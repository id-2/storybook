---
title: 'Vite'
---

Storybook Vite builder bundles your components and stories with [Vite](https://vitejs.dev/), a fast ESM bundler.

- For applications built with Vite: it allows reusing the existing configuration in Storybook.
- For applications built with Webpack: it provides faster startup and refresh times, with the disadvantage that your component's execution environment differs from your application.

## Setup

If you ran `npx storybook@latest init` to include Storybook in your Vite application, the builder is already installed and configured for you. If you want, you can also opt into it manually.

Run the following command to install the builder.

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/storybook-vite-builder-install.yarn.js.mdx',
    'common/storybook-vite-builder-install.pnpm.js.mdx',
    'common/storybook-vite-builder-install.npm.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

Update your Storybook configuration (in `.storybook/main.js|ts`) to include the builder.

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/storybook-vite-builder-register.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

## Configuration

Out of the box, Storybook's Vite builder includes a set of configuration defaults for the supported frameworks, which are merged alongside your existing configuration file. For an optimal experience when using the Vite builder, we recommend applying any configuration directly inside Vite's configuration file (i.e., [`vite.config.js|ts`](https://vitejs.dev/config/)).

When Storybook loads, it automatically merges the configuration into its own. However, since different projects may have specific requirements, you may need to provide a custom configuration for Storybook. In such cases, you can modify your configuration file (`.storybook/main.js|ts`) and add the `viteFinal` configuration function as follows:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/storybook-vite-builder-aliasing.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

The asynchronous function [`viteFinal`](../api/main-config-vite-final.md) receives a `config` object with the default builder configuration and returns the updated configuration.

### Environment-based configuration

If you need to customize the builder's configuration and apply specific options based on your environment, extend the `viteFinal` function as follows:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/main-config-vite-final-env.js.mdx'
  ]}
/>

<!-- prettier-ignore-end -->

### Override the default configuration

By default, the Vite builder in Storybook searches for the Vite configuration file in the root directory of your Storybook project. However, you can customize it to look for the configuration file in a different location. For example:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/main-config-builder-custom-config.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

<Callout variant="info" icon="💡">

If you do not want Storybook to load the Vite configuration file automatically, you can use the `viteConfigPath` option to point to a non-existent file.

</Callout>

### TypeScript

If you need, you can also configure Storybook's Vite builder using TypeScript. Rename your `.storybook/main.js` to `.storybook/main.ts` and adjust it as follows:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/storybook-vite-builder-ts-configure.ts.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

---

## Troubleshooting

### Working directory not being detected

By default, the Vite builder enables Vite's [`server.fs.strict`](https://vitejs.dev/config/#server-fs-strict) option for increased security, defining the project's `root` to Storybook's configuration directory
If you need to override it, you can use the `viteFinal` function and adjust it.

### ArgTypes are not generated automatically

Currently, [automatic argType inference](../api/argtypes.md#automatic-argtype-inference) is only available for React, Vue3 and Svelte (JSDocs only). With React, the Vite builder defaults to `react-docgen-typescript` if TypeScript is listed as a dependency. If you run into any issues, you can revert to `react-docgen` by updating your Storybook configuration file as follows:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/storybook-vite-builder-react-docgen.js.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

### Interaction tests not working as expected

If you are migrating from a Webpack-based project, such as [CRA](https://create-react-app.dev/), to Vite, and you have enabled Interaction testing with the [`@storybook/addon-interactions`](https://storybook.js.org/addons/@storybook/addon-interactions) addon, you may run into a situation where your tests fail to execute notifying you that the `window` object is not defined. To resolve this issue, you can create a `preview-head.html` file in your Storybook configuration directory and include the following:

<!-- prettier-ignore-start -->

<CodeSnippets
  paths={[
    'common/storybook-vite-builder-jest-mock.html.mdx',
  ]}
/>

<!-- prettier-ignore-end -->

#### Learn more about builders

- Vite builder for bundling with Vite
- [Webpack builder](./webpack.md) for bundling with Webpack
- [Builder API](./builder-api.md) for building a Storybook builder
