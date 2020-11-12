# GitHub action for exporting Figma components

[![npm version](https://img.shields.io/npm/v/@primer/figma-action.svg)](https://www.npmjs.org/package/@primer/figma-action)

A [GitHub action](https://github.com/features/actions) that will export [Figma](https://figma.com/) components from design files to your repository.

## Usage

**Example workflow**

```workflow
name: Export SVG from Figma
on: [push]
jobs:
  all:
    name: Figma Export
    runs-on: ubuntu-latest
    steps:
      - uses: primer/figma-action@v1.0.0-alpha.3
        with:
          args: "format=svg outputDir=./build/"
        env:
          FIGMA_FILE_URL: "https://www.figma.com/file/ZFjrph2HUwFK8Q3uEtJIu1PT/yourfilename"
          FIGMA_TOKEN: ${{ secrets.FIGMA_TOKEN }}
```

### Variables

**Secrets**

`FIGMA_TOKEN` **(required)**

This token is used to access the [Figma API](https://www.figma.com/developers/docs#access-tokens). It's required to generate one so this action will work. It's recommended to set the token in a [secret token](https://help.github.com/articles/virtual-environments-for-github-actions#creating-and-using-secrets-encrypted-variables) in your repository.

**env variables**

`FIGMA_FILE_URL` **(required)**

This is the file url that you would like to export from. The action will search the file for [components](https://help.figma.com/article/66-components) and export them with your configurations.

**Workflow args (optional)**

* `format` – The export format for exporting from Figma. Options are `svg`, `jpg`, `png`. Default is `jpg`
* `outputDir` – Where you would like the exported files to live. Default is `./build/`
* `scale` – When choosing an image format `jpg` or `png` this is the export scale between `0.01` and `4` that will allow you to scale the image. Default is `1`.

### Output

The output of this action lives in `./build/` by default, but can be configured. In addition to the files exported you will see a `data.json` file exported. This contains information about the exported components mapped by component `id`.

The directory will look like this:

```
./outputDir/
  ├── format/
  |     └── componentName.format
  └── data.json
```

The data.json file will look like this:

```js
{
  "0:639": {
    "name": "plus", // component name
    "id": "0:639", // component figma id
    "key": "89696b0b52493acc8692546ac829bd4e334c63a2", // component global figma id
    "file": "FP7lqd1V00LUaT5zvdklkkZr", // figma file key
    "description": "keywords: add, new, more", // figma component description
    "width": 12, // width of the component frame
    "height": 16, // height of the component frame
    "image": "https://s3-us-west-2.amazonaws.com/figma-alpha-api/img/1/6d/1234" // aws URL for the exported file
  }
}
```

## License

[MIT](./LICENSE) &copy; [GitHub](https://github.com/)
