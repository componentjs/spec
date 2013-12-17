# component install

  Tools such as the JavaScript [component(1)](https://github.com/component/component) implementation allow consumers to install, search, and build components.
  These tools may vary from community to community, however they __SHOULD__:

  - allow installation via `<user>/<project>` for example "component/tip"
  - default installation to the `./components` directory
  - allow building of the component(s) for development
  - allow searching components

  The installation tool __MUST__ install the component to a directory using the `<user>` / `<project>` combo, with the `/` replaced by a hypen `-`.
  For example `$ component install visionmedia/page` would install the files to `./components/visionmedia-page`.
  The following snippet further illustrates this for a "popover" component:

```
$ ls -1 components
component-emitter
component-jquery
component-tip
```