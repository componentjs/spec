# Component Registry Specifications

  The purpose of a registry is to catalog a set of components. Every registry __must__ have a set of criteria for components to be added to the registry. For example, Component's default registry will be for web components, and every component must either have `scripts`, `styles`, or any other web-related file consumable by a browser.

  Registries only store components' metadata, specifically their `component.json`. It notifies clients and agents how to download the component, but it does not serve the actual components.

## GET /components

Returns all components in the registry.

## GET /registry

  Returns `registry.json`, the details of the registry.

- `.name` [required] - the name of the registry. The registry must match the following regular expression: `/^[a-z-]{3,}$/`.
- `.hostname` [optional] - its hostname without any protocols or slashes, i.e. `component.io`

## GET /tail

  Return an [EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource) stream. Every time a component releases a new version, the registry __must__ send that `component.json` to all listening clients.

## GET /:user/:project

  Returns the project's latest release's `component.json`.

  The following properties are added:

  - `releases[]` - a list of valid releases. Each `release` has the following properties:
    - `release` - the name of the release.
    - `version` - the version of the release, if any.

## GET /:user/:project/:release

  Returns a release's `component.json`.

## PUT /:user/:project

  Add a project to the registry.

  The registry __must__ validate the component's `component.json` and reject the component if it does not comply.

  The registry __must__ reject the component if it does not have any semantically versioned releases.

  The registry __may__ add additional requirements on the component.

  The registry __should__ normalize the component as applicable.

## PATCH /:user/:project

  Update a project's metadata from its source. The registry then __must__ updates the project's list of releases, if any.

  The registry __must__ validate every new release.

  If updated, the registry __must__ push the latest release's `component.json` to all clients listing on `/tail`.

## DELETE /:user/:project

  Deletes a project from the registry.
  Aside from authorization, this request __should__ be valid only if any of the following criteria are met:

  - The project no longer exists at the source
  - The project is not a valid component in the registry
