## Component Server Implementation Notes

  Philosophically, each server can be considered an [npmd](https://github.com/dominictarr/npmd) instance.

## GET /components

  Right now, every client recursively resolves the dependencies of all the components. This is annoying because:

- It requires an excessive amount of HTTP requests
- It's slow
- There's no caching involved
- Single point of failure (GitHub)

  Since `server`s have a list of components and their releases in the registry, it should be able to resolve all a build's dependencies without a single HTTP request (unless you're using branches or commits).

  When the `server` is on the local host, this is effectively the same thing, but with caching.

## GET /:user/:project/:reference

  Contents is mirrored by GitHub's API.

  Idea behind multiple URLs is to allow mirroring. A `server` could tell the client to download a file directly from GitHub raw or another `server` instead of itself.

## /build and /components

  Maybe. Something like how [c8](https://github.com/gingkoapp/c8) works right now, but with `GET /components`' API.

## Transformations

  I expect the server to be able to do transformations, but I don't think it belongs in the specification. For example, we can do the following transformation to avoid concatenating every file to a build:

```
GET  /component/emitter/1.1.1/index.js?cjs
```

```js
modules['component/emitter@1.1.1'] = function (module, exports, require) {
  // ...
}
```

  Then instead of an install and build process, component can have a tool that outputs script tags appropriately that you just include in your template:

```html
<script src="/components/require.js"></script>
<script src="http://component.io/component/emitter/1.1.1/index.js?cjs"></script>
<script src="http://component.io/component/domify/1.1.1/index.js?cjs"></script>
<script src="/app/boot/index.js"></script>
```

  Now there's no build process. This basically eliminates the need for AMD, and due to Component's builder, there's no path resolution users have to worry about. No `../../../client/index.js` crap.

  We can also do `UMD` wraps, so people just write CommonJS modules, and we'll wrap it in that silly UMD wrap for them. This will encourage people to write CJS instead of AMD or any weird or custom build systems people use.

  The installation process only resolves dependencies which could be done with a single HTTP request to a `server`. If you have a server locally, we're talking about `< 50ms` installation times if everything is cached.

  Since each file is downloaded from a CDN, it's much faster than doing `bower install` or `npm install`.

  However, you should still be able to download the components locally, but you should only have to do it when you're making a production build or debugging your dependencies.

## component.io

  `http://component.io` will be a CDN in front of a `server`. We can run it behind a single free Heroku instance and put a CloudFlare in front of it.

  The cool part is that we've effectively replaced [cdnjs](https://github.com/cdnjs/cdnjs) and every other cdn like it, such as [bootstrapcdn](http://www.bootstrapcdn.com). Updates to `component.io` will happen automatically unlike cdnjs. Look at all those issues and PRs it has.

  It's also very useable. With a URL like `http://component.io/jquery/jquery/2.0.3/jquery.js`, people know exactly where the repository is and what file version it is. Non-component people will benefit.

  I don't think `component.io` should bother with branch or commit releases - we can require people to use their own local server to support that. In fact, we should always encourage people to use their own local server. We just have to make sure it's easy.

## Deployment

  Servers are ephemeral since they are "backed" by a registry. Thus, we can just cache everything locally in the file system. If the server dies or crashes, just spin up a new instance and it'll redownload everything.

  Servers won't need a database, making deployment easy. We can store `registry.json` in memory since it should be small. If we do need a database, we can use an embedded one like leveldb.

## If we do this, Component will win.

  Component is a full-stack solution, but it is modular so you can use many different parts of it.

- You might want to skip the 100s of script tags and just go for a build process (like we do currently).
- Maybe you just want to use component's CDN on a really simple static site.
- Maybe you only want to use it as a build process and deployment tool for your open source library

  Reasons I think Component will win:

- People will use the CDN, which is free marketing and free for us
- People will be inclined to include a `component.json` in their repos even if they don't use component.
    - They can push their code to a CDN just by tagging a new release, making it easier for people to use their code.
    - They can write CJS while supporting AMD and globals as well
    - They don't need to make any releases or include it in their git repo
- People would use it just for dependency and file management and automatically creating script and style tags. This is basically what Bower currently does, but we'll do it better
- Great usability due to GitHub integration and namespaces
- No lengthy install process unlike every other package manager
- It would eliminate the need for AMD. Just write CJS and Component will handle the rest. AMD people concat and minify their files anyways, so there won't be any compelling reason to use AMD anymore.
- Relatively easy migration to ES6 (I think, not sure, but it's an important goal)
