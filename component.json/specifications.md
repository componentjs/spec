# component.json

  Components __MUST__ provide a "component.json" file to describe the component's functionality and contents.
  Component developers __MUST__ explicitly state the relevant file(s) via `scripts`, `styles` and others.
  This makes it easy for consumers to work with packages and reduces I/O.
  Another added benefit of this is that component installation tools may skip tarballs and fetch these files directly if desired, which can drastically improve installation times.
  Below is an example of a tooltip component:

```js
{
  "name": "tip",
  "repo": "component/tip",
  "description": "Tip component",
  "version": "0.0.1",
  "keywords": ["tooltip", "tip", "ui"],
  "dependencies": {
    "component/emitter": "*",
    "component/jquery": "*"
  },
  "scripts": ["index.js", "template.js"],
  "styles": ["tip.css"],
  "license": "MIT"
}
```

## .private

  A boolean specifying whether the component is __private__, defaulting to `false`. A component is either __private__ or __public__.

  A __private__ component cannot be published to a public registry and has different restrictions than __public__ components.

## .name

  The component __MUST__ have a "name", this is what will be passed to `require()`.

## .repo

  The __public__ component __MUST__ have a "repo" property,
  this is registry end-point consisting of `<username>/<project>`,
  for example "visionmedia/page.js" or "component/dialog".

## .description

  The component __SHOULD__ have a "description" property,
  this helps people find and understand your component.

## .version

  The __public__ component __MUST__ include a version,
  unless used as a local component,
  allowing other scripts to depend on specific releases of the component.

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

  `styles` __must not__ use any globs or globstars.

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
  2. Use the latest tag release
  3. Use the the main branch

  Each reference can be:

  - a `git` reference including:
    - `<tag>`
    - `<branch>` such as `master` or `gh-pages`
    - `<commit>` sha

## .development

  Development dependencies. For example:

```json
{
  "development": {
    "component/assert": "*",
    "visionmedia/mocha": "*"
  }
}
```

## .locals

  The __public__ component __must not__ contain any `locals`.

  Local dependencies are already located on disk,
  these are not installed,
  but _are_ however included in the builds,
  thus no versions need to be defined.
  Local components should be located in a directory specified within `.paths`.

```json
{
  "local": ["my-emitter"]
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

```json
{
  "templates": ["user.html"]
}
```

## .demo

  Optional full url for a live demonstration of a component.

## .license

  The license string such as "MIT" may be used for search output and other reporting,
  developers __SHOULD__ specify this field

## Custom properties

  Custom properties may of course be used to facilitate custom build steps.
  For example if you're a fan of CoffeeScript and you wish to skip manual compilation for custom app-specific components,
  you could simply add a property named `coffee: ["foo.coffee"]` and handle the translation in the build step.
  Custom properties __SHOULD__ be namespaced to prevent future collisions.

## Templates

  Templates __SHOULD__ be compiled down to regular JavaScript and added to the `scripts` array before publishing a release to Github.
  For example: Jade, Hogan and many other template engines allow templates to be compiled to a self-contained executable function; they do not require the entire library itself.
  When possible it is strongly suggested that you use raw HTML and intrinsic DOM manipulation for public components so that contributors feel comfortable.
  This is often easily possible, as components are focused,
  for example a tooltip or dialog has very little markup.
  Your private application may utilize custom properties to auto-compile templates as part of the build,
  public components must use pre-built templates.

## Glob Support

  Properties __may__ use globs and globstars to list files unless noted otherwise.