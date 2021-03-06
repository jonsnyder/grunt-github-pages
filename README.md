# Grunt Github Pages

Publish your project's pages on Github with one command.

A [Grunt][] task that enables you to publish to the `gh-page` branch with one command line.

## Getting Started

```shell
npm install grunt-github-pages --save-dev
```

### Preparing your repository

You need to create a folder and add it to your `.gitignore` file. In that folder you have to clone the same repository again and point to the `gh-page` branch.

Assume the target folder is named `_site/` and our github repository is named `project`:

```shell
$ ls -la
.
..
.git
.gitignore
index.md
```

We add the `_site` folder in `.gitignore` and clone the current repository ( *project* ) to `_site`:

```shell
$ git clone git@github.com:thanpolas/project.git _site

$ cd _site
```

A full copy of the `project` repository now exists in the `_site` folder. We now need to point it to the `gh-pages` branch and we are good to go:

```shell
$ pwd
project/_site/

$ git checkout -b gh-pages
Switched to a new branch 'gh-pages'

$ git push origin gh-pages
```

Your repository is now ready to start using the Github Pages task!

## The `githubPages` Task


Edit your [Gruntfile][] and add the `githubPages` task:

```js
  githubPages: {
    target: {
      options: {
        // The default commit message for the gh-pages branch
        commitMessage: 'push'
      },
      // The folder where your gh-pages repo is
      src: '_site'
    }
  }

  /* ... later on, after grunt.initConfig() call, create an alias: */

// create an alias for the githubPages task
grunt.registerTask('push', ['githubPages:target']);
```

So when you issue `grunt push` on your command line this is what will happen:

1. The Current Working Directory will change to `_site`.
2. The command `git add .` will be performed.
3. The command `git commit -am "push"` will be performed.
4. The command `git push origin gh-pages` will be performed.

### The `src` Option

The `src` option must be a single string that represents a directory. The API is weak at this point but use cases need to be presented before a solution is attempted as there are a few intricacies involved. So do create an issue if you need globbing patterns here.

### The `dest` Option

There are some cases, like building a [Jekyll][] site, that the destination folder `_site` will get overwritten by the generation of static pages, along with the `.git` folder. For these kind of cases a trick would be to maintain one more ignored directory where the `.git` folder is retained.

The second ignored directory should be the `dest` option in your config file:

```js
  githubPages: {
    target: {
      // The folder where your gh-pages repo is
      src: '_site',
      // The second ignored directory with the .git folder
      dest: '_site_git'
    }
  }
```
In this case, when you issue `grunt push` this is what will happen:

1. All the contents of `_site` folder will be deep copied to `_site_git` overriding.
2. The Current Working Directory will change to `_site_git`.
3. The command `git add .` will be performed.
4. The command `git commit -am "auto commit"` will be performed.
5. The command `git push origin gh-pages` will be performed.

### Other Options

#### commitMessage
**Type**: `string` Default: `auto commit`

The default commit message for the `gh-pages` branch.

#### remote
**Type**: `string` Default: `origin`

The name of the *remote* to be used when issuing `git push`.

#### pushBranch
**Type**: `string` Default: `gh-pages`

The name of the *branch* to be used when issuing `git push`.

## Release History
- **v0.0.1**, *Mid Mar 2013*
  - Big Bang

## License
Copyright (c) 2013 Thanasis Polychronakis
Licensed under the [MIT](LICENSE-MIT).

[closure-library]: https://developers.google.com/closure/library/ "Google Closure Library"
[closure-tools]: https://developers.google.com/closure/ "Google Closure Tools"
[grunt]: http://gruntjs.com/
[Getting Started]: https://github.com/gruntjs/grunt/wiki/Getting-started
[package.json]: https://npmjs.org/doc/json.html
[Gruntfile]: https://github.com/gruntjs/grunt/wiki/Sample-Gruntfile "Grunt's Gruntfile.js"
[yeoman]: http://yeoman.io/ "yeoman Modern Workflows for Modern Webapps"
[jekyll]: http://jekyllrb.com/ "transform your text into a monster"
