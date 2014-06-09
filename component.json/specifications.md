# component.json

  Components __MUST__ provide a "component.json" file to describe the component's functionality and contents.
  Component developers __MUST__ explicitly state the relevant file(s) via `scripts`, `styles` and others.
  This makes it easy for consumers to work with packages and reduces I/O.
  Another added benefit of this is that component installation tools may skip tarballs and fetch these files directly if desired, which can drastically improve installation times.
  Below is an example of a tooltip component:

```js
{
  "name": "tip",
  "repository": "component/tip",
  "description": "Tip component",
  "version": "0.0.1",
  "keywords": ["tooltip", "tip", "ui"],
  "dependencies": {
    "component/emitter": "*",
    "component/jquery": "*"
  },
  "templates" : ["template.html"],
  "scripts": ["index.js"],
  "styles": ["tip.css"],
  "license": "MIT"
}
```

## .private

  A boolean specifying whether the component is __private__, defaulting to `false`. A component is considered either __private__ or __public__. A __private__ component has different restrictions than __public__ components.

## .name

  A public component __MUST__ have a "name". This is what will be passed to `require()`.

  A `local` component is referenced by its container folder's name. A `local` component should either have no name or a name matching the containing folder's.
  
  The name __MUST__ only consist of lowercase letters, numbers, dashes, and underscores. It must match the regular expression `/^[0-9a-z-_]+$/`.

## .repository

  The __public__ component __MUST__ have a "repository" property,
  this is registry end-point consisting of `<username>/<project>`,
  for example "visionmedia/page.js" or "component/dialog".

## .description

  The component __SHOULD__ have a "description" property.
  This helps people find and understand your component.

## .version

  The __public__ component __MUST__ include a version,
  allowing other scripts to depend on specific releases of the component.
  
  __local__ components do not need a version.

## .keywords

  Keywords are used when searching for a component.
  A __public__ component __SHOULD__ list a few keywords.

## .main

  It is recommended that you use "index.js" for the main component file,
  however if you use another filename,
  you __MUST__ define a "main" field for that.
  A component __MUST__ have only one "main" file specified,
  and it __MUST__ still be listed in the "scripts" array.

## .scripts

  The `scripts` field explicitly specifies the scripts for this component. For __public__ components, these __must__ be regular JavaScript files. For __private__ components, these __should__ be regular Javascript files.

## .styles

  The `styles` field explicitly specifies the stylesheets for this component. For __public__ components, these __must__ be regular CSS files. For __private__ components, these __should__ be regular CSS files.

## .json

  The `json` field explicitly specifies the `JSON` files for this component. Each file __must__ be valid `JSON`.

## .images

  The `images` field __MUST__ be supported and fetched upon installation,
  this allows component build tools to rewrite stylesheet `url()`s in order to accomodate various file serving techniques.

## .fonts

  The `fonts` field __MUST__ be supported and fetched upon installation,
  this allows component build tools to rewrite stylesheet `url()`s in order to accomodate various file serving techniques.

## .files

  In the future we will classify more file types,
  however for those which are not treated uniquely such as fonts may be placed in a `files` array to aid build and installation tools.

## .dependencies

  Runtime dependencies. For example:

```json
{
  "dependencies": {
    "component/emitter": "*",
    "visionmedia/page.js": "1.3.0"
  }
}
```

  Each dependency must be defined in the following form:

```
"<user>/<repo>": "* | <git reference> | <semver range>"
```

A `*` wildcard dependency will try the following:

  1. Use a version already included in the build
  2. Use the highest semantic version tag
  3. Use the the main branch, usually `master`

Each reference can be:

- a `git` reference including:
  
    - `<tag>`
    - `<branch>` such as `master` or `gh-pages`
    - `<commit>` sha

The __public__ component __should__ have semantic versioned dependencies.

## .locals

  Local dependencies are already located on disk,
  these are not installed,
  but _are_ however included in the builds,
  thus no versions need to be defined.
  Local components should be located in a directory specified within `.paths`.

```json
{
  "locals": ["my-emitter"]
}
```

## .remotes

  The __public__ component __must not__ contain any `remotes`.

  The "remotes" property __MAY__ be supported to support additional component servers,
  where github is implied.
  When supported installation attempts __MUST__ be made in the order defined by the array.

```json
{
  "remotes": [
    "http://localhost:3000",
    "http://user:pass@private"
  ]
}
```

  The remote "https://raw.github.com" is implied and automatically added to the _end_ of the `remotes` array at all times.
  To increase performance if most of your components originate from GitHub it is recommended to add it as the _first_ remote so that it is hit before the others:

```json
{
  "remotes": [
    "https://raw.github.com",
    "http://localhost:3000",
    "http://user:pass@private"
  ]
}
```

## .paths

  The __public__ component __must not__ contain any `paths`.

  The root component __SHOULD__ be able to utilize an array of lookup paths,
  allowing users to separate local and remote components:

```json
{
  "paths": ["my-components", "path/to/my-other-components"]
}
```

## .templates

  The templates array __MUST__ provide the contents of each file as a require-able module.
  For example the following must provide the HTML string via `require('user.html')`.
  If you're using `component-build` the files will be converted automatically using the [string plugin](https://github.com/component/builder2.js/blob/master/docs/scripts.md#string) of builder2.

```json
{
  "templates": ["user.html"]
}
```

## .development

  `component.json` properties valid only during development. For example:

```json
{
  "development": {
    "dependencies": {
      "component/assert": "*",
      "visionmedia/mocha": "*"
    },
    "scripts": [
      "test.js"
    ],
    "styles": [
      "test.css"
    ]
  }
}
```

## .license

  The license string such as "MIT" may be used for search output and other reporting,
  developers __SHOULD__ specify this field

## Glob Support

  Properties __may__ use globs and globstars to list files __only when the order of files is insignificant__.

## Custom properties

  Custom properties may of course be used to facilitate custom build steps and is, in fact, encouraged.
  For example if you're a fan of CoffeeScript and you wish to skip manual compilation for custom app-specific components,
  you could simply add a property named `coffee: ["foo.coffee"]` and handle the translation in the build step.
  Custom properties __SHOULD__ be namespaced to prevent future collisions.
  
