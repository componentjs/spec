# Component Server Specifications

  The server is the middleman between clients, the component sources (such as GitHub), and the registry. The server could be on the local host, a server on the network, a proxy, or an official site. The server:

  - Caches components locally
  - Resolves build dependencies
  - Is cheap and ephemeral

## GET /server

  Return information about this server:

  - `registries` [required] - registries' `registry.json` which the server is capable of handling. Each server __must__ implement at least one registry. Registries __must__ be listed in descending priority.
  - `hostname` [optional] - the hostname of this server clients __should__ use.

## GET /components

  Resolves the dependencies of a list of components.
  The client __must__ send a list of projects to build.

  The server __must__ respond with all components in the build in the correct order. Each component __must__ include all data corresponding to a `GET /:user/:project/:reference` request.

  The server __must__ include all dependencies in the response.

## GET /components/:project.tar

  The server __must__ redirect to a valid location to download the component's latest semantically versioned release __or__ serve the component directly.

## GET /:user/:project/:reference

  The server __must__ support the component's releases as a `reference` and any semantic version ranges.
  The `server` __should__ support branches and commits.

  If a version if satisfied, it must return that project's `component.json`.

  The response should add the following properties:

  - `contents[]` - an array of all files in the component. Each `content` must have the following properties:
    - `mode`
    - `type` - `blob` or `dir`
    - `sha` - `sha1` sum
    - `path` - path
    - `size` - byte size of the file
    - `urls[]` - an array URLs where the file can be downloaded. When downloading the file, the client __must__ try each `url` in the order specified.
  - `tarballs[]` - an array of tarballs where the project can be downloaded. Each tarball must have the following properties:
    - `sha` - `sha`
    - `size` - `size`
    - `url` - URL of the tarball. When downloading the tarball, the client __must__ try each `url` in the order specified.

  If `registry.hostname` is not defined, each `url` __must__ be an absolute or protocol-relative URL.

## GET /:user/:project/:reference.tar

  Downloads the project in a tar ball. The server __must not__ include any files not explicitly listed in `component.json`.

## GET /:user/:project/:reference/*

  Download each file of a project directly.

  The `server` __must__ allow downloading files from valid releases. The `server` __should__ allow downloading files from semver, branch, or commit releases.
