## Registry

  Components are currently fetched from Github, however in the future we may allow for completely swapping out the registry, as well as multiple registry support.
  Component 1.0.0 uses (https://github.com/component/remotes.js) to normalize remote end points, no need to follow the Github style.
  ~~This means alternative registries __MUST__ comply with the Github-style urls following:~~

  - `GET /:user/:project/master/:file` to fetch files for `*`, otherwise the same as below
  - `GET /:user/:project/:version/:file` to fetch a file's contents (`GET /component/tip/0.0.1/component.json`)
