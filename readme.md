Specs for Component.
If you're new to Component,
please read this [blog post](http://tjholowaychuk.com/post/27984551477/components).

Each module of component's specs is contained within its own folder.
Each folder will have the following files:

  - `specifications.md` - motivations, goals, and specifications
  - `implementation.md` - implementation notes
  - `faq.md` - frequently asked questions

Modules are defined with suffixes.
Without a suffix,
a module is assumed to be a command line tool.
For example, `component.json/` is the specifications for `component.json`,
and `component-build` is the specifications for the `command build` command.

## Contributing

Open an issue or a pull request with any suggestions you may have.

Do natural line breaks (periods, commas, etc.) in markdown so diffs look better.

## License

  MIT