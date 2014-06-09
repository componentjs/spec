# builder

  These are the specifications for building components.
  Component(1) uses component/builder2.js, here you can find the [documentation](https://github.com/component/builder2.js/tree/master/docs).

## Order

  There are two types of components: dependencies and locals. The builder will flatten the dependency tree into a list of components. The order of components in the build must follow the following rules:

  - All dependencies must be included before locals.
  - A component must be included after all its dependencies and locals.
  - Any duplicate components must be ordered by their semantic version, ascending.
