# Component Registry Implementation Notes

  The registry is only meant to replace the [List of Components](https://github.com/component/component/wiki/Components) in its current form.

## Validation

  To make components higher quality, we want to validate components before they are added to the registry. We also want to normalize `component.json`s, such as expanding globs.

## Component Releases

  The registry requires components to have releases, specifically git tags. We do not want components that have no releases or releases that aren't semantic versions.

  Git branches and commits must be handled by a server, not the registry.

## Distributions

  I expect the event source to be something like:

```js
event: 'data'
data: {"name":"emitter","version":"1.1.1"}
```

  Idea is that we want the registry to be cheap (a single free Heroku instance). We can then have agents mirror the registry, and people listen to the mirrors, not the registry directly, creating a network of chains. It's not like you need to know about all updates immediately.

## Adds and Updates

  There are a couple of ways to send updates. A `component registry update` command would be nice to ping the registry to update. This is probably the easiest.

  We can then add GitHub oAuth authentication that hooks into your repos and sends an update command to the registry every time a tag is pushed.