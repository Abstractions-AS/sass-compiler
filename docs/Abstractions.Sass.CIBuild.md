## Abstractions Sass Compiler

Compile Sass and SCSS to CSS in Visual Studio and on your build server, without Node.js. Start a web project with Bulma, Bootstrap or Pico CSS in one click, compile on save while you work, and compile on build everywhere else, with the same settings.

- **Start in one click.** **Sass Compiler** › **Initialize with** sets up an ASP.NET Core or Blazor project with an empty Sass file, or with Bulma, Bootstrap or Pico CSS: it downloads the Sass sources of the framework with LibMan, writes a stylesheet that shows how to set the framework's variables, and compiles it. Bulma comes with styles for the validation of ASP.NET Core, and a Blazor project gets the error and loading styles of Blazor.
- **Icon fonts in one click.** **Add icon font** adds Font Awesome, Material Symbols or Bootstrap Icons: it downloads the font, imports it in your Sass, and creates the build target the font needs, which copies its fonts to `wwwroot/fonts` on every build and publish, so the icons just work, locally and on the build server.
- **Built on LibMan, no magic files.** Frameworks and icon fonts come with [LibMan](https://learn.microsoft.com/aspnet/core/client-side/libman/), Visual Studio's own library manager: each is a plain entry in `libman.json`, its files are in `Styles/lib` where you can see them, and **Manage Client-Side Libraries** updates or restores them as any other library. Everything the extension adds is an ordinary file in your project that you can read and change: the Sass, `sassconfig.json`, `libman.json` and a short target in the project file.
- **A clean start.** **Cleanup VS template...** moves the Bootstrap, jQuery and site CSS and JavaScript that the ASP.NET Core templates put in `wwwroot` to the Recycle Bin.
- **Compile on save.** Saving a Sass file compiles it, and saving a partial compiles the files that import it. Errors go to the Error List, with their file and line.
- **Compile on build, anywhere.** One click adds the NuGet package `Abstractions.Sass.CIBuild`, which compiles when the project builds, with `dotnet build` or the MSBuild of Visual Studio, also on Windows and Linux build servers, and fails the build on a Sass error.
- **Two compilers.** LibSass, the default, or Dart Sass for modern Sass with `@use` and the built-in modules, chosen for each file.
- **No Node.js, no npm.** What the compiler needs, its native libraries included, comes with the extension and the NuGet package.

Originally based on and heavily influenced by the awesome [WebCompiler by Mads Kristensen](https://github.com/madskristensen/WebCompiler).

### What's new in the latest version

- **The documentation and the changelog of each release are on [GitHub](https://github.com/Abstractions-AS/sass-compiler)**, with a release for each version. Visual Studio's extension manager links to it, under More info and Release notes.
- **Icon fonts and Bulma 1.0 show in compressed CSS.** Compressed CSS with a character outside ASCII was written with two byte order marks, so a browser dropped its first rule, such as the `@font-face` of an icon font. Delete such a CSS file, or save `sassconfig.json`, and build, to write it again.
- **New files are named as `sassconfig.json` names them.** On Windows, a new CSS file, its source map and its folders were named in lower case, which a case-sensitive file system (Linux, Docker) does not find by its name. Rename one that is in lower case once (in git, `git mv -f`).
- **Initialize with** sets up an ASP.NET Core or Blazor project in one click: an empty Sass file for LibSass or Dart Sass, or Bulma 0.9.4 or 1.0.4, Bootstrap 5.3.9 or Pico CSS 2.1.1, downloaded with LibMan, with a stylesheet that shows how to set the framework's variables. Bulma comes with styles for the validation of ASP.NET Core, and a Blazor project gets the error and loading styles of Blazor.
- **Add icon font** adds Font Awesome, Material Symbols or Bootstrap Icons, and the build target that copies their fonts to `wwwroot/fonts` (see Icon fonts below).
- **Cleanup VS template...** moves what the ASP.NET Core templates put in `wwwroot` to the Recycle Bin, for a clean start.
- **LibSass reports `@use` and `@forward` as an error**, on the line of the rule, where it wrote them to the CSS without an error and the CSS was broken. A build that passed with such a file now fails: set `"runtime": "DartSass"` for it.
- **The Sass Compiler menu of a project has moved** below Manage NuGet Packages and Manage Client-Side Libraries, where it is easier to find.

### Choosing the runtime
The compiler can use [DartSassHost](https://github.com/Taritsyn/DartSassHost) in addition to [LibSassHost](https://github.com/Taritsyn/LibSassHost), which is the default.
Use [DartSassHost](https://github.com/Taritsyn/DartSassHost) in order to make use of newer SASS/SCSS features that are not supported by the now deprecated `libsass`.

To switch to `dart-sass` set the `runtime` parameter, either per file in `sassconfig.json`:

```json
[
  {
    "outputFile": "wwwroot/site.css",
    "inputFile": "Styles/site.scss",
    "runtime": "LibSass"
  },
  {
    "outputFile": "wwwroot/someotherfile.css",
    "inputFile": "Styles/someotherfile.scss",
    "runtime": "DartSass"
  }
]
```

 or globally in `sassconfig.json.defaults`:

```json
{
  "sourceMap": false,
  "loadPaths": [],
  "style": "Expanded",
  "precision": 10,
  "lineFeed": "CrLf",
  "includeInProject": true,
  "runtime": "DartSass"
}
```

### Key differences

 - Works with Visual Studio 2022 and 2026
 - Only works with Sass/Scss files
 - No dependency on node; [LibSassHost](https://github.com/Taritsyn/LibSassHost), or [DartSassHost](https://github.com/Taritsyn/DartSassHost) is used instead
 - CI builds works on Linux build hosts as well as Windows
 - Configuration files are simplified to reflect sass-only compilation
 - Errors point to the correct source file even when they occurred in an import


### Visual Studio extension

A Visual Studio extension that compiles Sass/Scss files. Works with Visual Studio 2022 and 2026, and supports CI/CD with cross-platform build support.

Get the extension on the Visual Studio Marketplace: [Abstractions Sass Compiler](https://marketplace.visualstudio.com/items?itemName=abstractionsas.AbstractionsSassVisualStudioExtension), or in Visual Studio under Extensions › Manage Extensions.


### Features

- Compiles Sass (`.sass`) and SCSS (`.scss`) to CSS, with LibSass or Dart Sass, chosen for each file
- **Initialize with** sets up a web project with a template: Empty (LibSass or Dart Sass), Bulma 0.9.4 or 1.0.4, Bootstrap 5.3.9, or Pico CSS 2.1.1
- **Cleanup VS template...** removes what the ASP.NET Core templates put in `wwwroot`
- **Add icon font** adds Font Awesome, Material Symbols or Bootstrap Icons, with a target that copies their fonts to `wwwroot/fonts`
- Saving a Sass file compiles it, and saving a partial compiles the files that import it
- Compiles on build with the NuGet package, also on build servers
- Options for each file, or for all of them: output style, source maps, load paths, line feeds and relative urls
- Error List integration
- Shows a watermark when opening a generated file
- Compiles every file in the solution with one command (`Shift+Alt+Y`)
- An agent skill that tells AI coding agents how the compiler works


### Getting started

**Sass Compiler** › **Cleanup VS template...** first removes what the ASP.NET Core project templates put in `wwwroot`, so that the project starts with only its own Sass: after asking, it moves `wwwroot/lib` (Bootstrap, jQuery and jQuery Validation, with their licenses) and every `.css`, `.js` and `.map` file in the rest of `wwwroot` (`css/site.css`, `js/site.js`, or `app.css` in Blazor) to the Recycle Bin, and removes the folders that are left empty. Images and `favicon.ico` are kept, and the layout is not changed: change its links to the files that were removed. It is shown for web projects that have `wwwroot` and no `sassconfig.json`.

To start a web project that has no `sassconfig.json` yet, right-click the project and choose **Sass Compiler** › **Initialize with** › **Empty template (libsass)**, or **Empty template (dart-sass)** to compile with DartSass. It writes `Styles/style.scss` with an empty rule (`body { }`), and a `sassconfig.json` that compiles it to `wwwroot/style.css` (with `"runtime": "DartSass"` for the second), with `sassconfig.json.defaults` next to it, compiles it, opens the Sass file, and turns on compile on build (see below), which adds the NuGet package `Abstractions.Sass.CIBuild` to the project. A `Styles/style.scss` that exists already is left as it is. The menu is shown for ASP.NET Core projects, Razor class libraries and Blazor projects, and only while the project has no `sassconfig.json`.

**Sass Compiler** › **Initialize with** › **Bulma 0.9.4 (recommended)** starts the project with [Bulma](https://bulma.io) instead. It downloads the Sass sources of Bulma 0.9.4 from jsDelivr into `Styles/lib/bulma`, and adds them to `libman.json` (which is created if the project has none), so that Manage Client-Side Libraries can restore or update them. `libman.json` is written as version 3.0, the version of LibMan in Visual Studio 2022 and 2026; one of a lower version, or without a version, is set to 3.0. It writes `Styles/theme.scss`, where you set Bulma's variables (it has examples), and `Styles/style.scss`, which imports the theme and Bulma and is compiled to `wwwroot/style.css`. Bulma 0.9 compiles with LibSass, the default runtime. If `libman.json` has Bulma already, in any version, it is left as it is and nothing is downloaded. Commit `Styles/lib/bulma` with the project: the build compiles the Sass, and a build server that checks out the project needs the Bulma files.

**Bulma 1.0.4 (Dart Sass)** does the same with Bulma 1.0.4, which is written with `@use` and can only be compiled with DartSass, so its `sassconfig.json` sets `"runtime": "DartSass"`. It writes only `Styles/style.scss`, which uses Bulma; to set Bulma's variables, configure it with `@use "lib/bulma/sass" as bulma with (...)`, as the example in the file shows. 0.9.4 is recommended, as it compiles with LibSass, the default, and its CSS is about a third of the size. Both Bulma templates also write `Styles/validation.scss`, which `style.scss` imports after Bulma: it styles the validation of ASP.NET Core with Bulma (a red or green border on fields that are invalid or valid, a red `*` after the label of a required field, the messages as help text, and the list of errors as a danger notification). For 1.0 it uses the custom properties of Bulma (`var(--bulma-danger)`), so it follows its themes and the dark mode.

**Sass Compiler** › **Initialize with** › **Bootstrap 5.3.9** does the same with the Sass sources of [Bootstrap](https://getbootstrap.com) in `Styles/lib/bootstrap`, with a `Styles/theme.scss` for its variables, and compiles with LibSass. `wwwroot/style.css` has all of Bootstrap: if the layout links the CSS of Bootstrap (as the ASP.NET Core templates do, from `wwwroot/lib/bootstrap`), link `~/style.css` instead. Its JavaScript, `bootstrap.bundle.min.js`, goes in `wwwroot/js/lib` (out of `wwwroot/lib`, which Cleanup VS template removes), and is in `libman.json` too, with `fileMappings`; the layouts of the ASP.NET Core templates load it from `~/lib/bootstrap/dist/js/`, so change the script to `~/js/lib/bootstrap.bundle.min.js`. A file that is there already is left as it is.

**Sass Compiler** › **Initialize with** › **Pico CSS 2.1.1 (Dart Sass)** sets up the project with [Pico CSS](https://picocss.com), which styles the HTML elements with few classes: its Sass sources go in `Styles/lib/pico` (and in `libman.json`), and `Styles/style.scss` uses it and is compiled to `wwwroot/style.css` with DartSass, as Pico is written with `@use`. Its settings, such as `$theme-color` and `$enable-classes` (to style the elements only), are set with `@use "lib/pico/scss" as pico with (...)`, as the example in the file shows.

In a Blazor project (one with `.razor` files: a Blazor Web App, Blazor WebAssembly or a Razor class library with components), every template also writes `Styles/blazor.scss`, with the default error and loading styles of the Blazor templates (`#blazor-error-ui`, `.blazor-error-boundary`, `.loading-progress`), and imports it in `Styles/style.scss` after the framework, so that it comes after the framework's styles, as `app.css` does in the Blazor templates. Remove those styles from `wwwroot/app.css`, or let Cleanup VS template remove `app.css`, and link `style.css` in `App.razor` instead.

Right-click a `.scss` or `.sass` file in Solution Explorer and choose **Sass Compiler** › **Compile file** (`Shift+Alt+Q`). The file is added to `sassconfig.json` in the root of the project, which is created the first time, with `sassconfig.json.defaults` next to it, and it is compiled. For a file that is compiled already, the command is **Re-compile file**, and **Remove from compilation...** takes the file out of `sassconfig.json` again.

Right-click `sassconfig.json` or the project for the commands that work on all of its files, in the same **Sass Compiler** menu:

- **Re-compile all files** compiles every file in `sassconfig.json`.
- **Delete all output files...** deletes the CSS files, after asking.
- **Enable compile on build** compiles them when the project builds (see below).

### Icon fonts

Right-click the project or `sassconfig.json` and choose **Sass Compiler** › **Add icon font** › **Font Awesome**, **Material Symbols** or **Bootstrap Icons** (shown when `sassconfig.json` has a file to compile). The font is added to the first file in `sassconfig.json`, in these steps, each only when it is not done already:

- Its Sass and its fonts are downloaded from jsDelivr into `Styles/lib` and added to `libman.json`: Font Awesome 6.7.2 for a file that compiles with LibSass, and 7.3.1 for DartSass (Font Awesome 7 is written with `@use`); Material Symbols 0.49.0, outlined only (each style is a font of several megabytes); Bootstrap Icons 1.13.2.
- The project file gets the target `CopyWebFonts`, which copies the fonts to `wwwroot/fonts` before the build (so that they are in the output of `dotnet publish` too), and a `WebFonts` item for the font, in an `ItemGroup` labelled `WebFonts`. The rest of the project file is not changed, and it is saved first if it has unsaved changes.
- `Styles/font-awesome.scss`, `Styles/material-symbols.scss` or `Styles/bootstrap-icons.scss` sets the font path to `/fonts` and imports the font, and the main Sass file imports it: with `@import` at the end for LibSass, or with `@use` after its other `@use` for DartSass.

The fonts are copied when the project builds. Commit `Styles/lib`: the `WebFonts` items find the fonts when the project is loaded, so a build server needs them in the checkout. The paths are absolute (`/fonts`): change them in the font's Sass file if the site runs under a path, such as `https://server/app/`. Font Awesome is chosen for the runtime when it is added: a project that moves from DartSass to LibSass later needs Font Awesome 6.

### Compile on save

Any time a `.scss` or `.sass` file or one of the configuration files (`sassconfig.json` or `sassconfig.json.defaults`) is saved, the compiler runs automatically to produce the compiled output file.

### Compile on build / CI support

You can compile as part of the build, also on a build server: right-click `sassconfig.json` or the project and choose **Sass Compiler** › **Enable compile on build**. This adds the NuGet package `Abstractions.Sass.CIBuild` to the project, and **Disable compile on build** removes it again. The status bar shows the progress, and the **Abstractions Sass Compiler** pane of the Output window says what was done.

A `PackageReference` will be added to the project, with `PrivateAssets="all"`, so that a project that is itself packed as a NuGet package does not pass it on. A reference that was added before the package set this does not have it: add `PrivateAssets="all"` to it yourself. The NuGet package contains an MSBuild task that runs the same compilers on the `sassconfig.json` file in the root of the project.
The task runs before every build and cleans the output files on `Clean`. To skip it, for example in one configuration, set the property `RunSassCompiler` to `False`:

```xml
<PropertyGroup>
  <RunSassCompiler>False</RunSassCompiler>
</PropertyGroup>
```

The package runs in the 64-bit MSBuild: `dotnet build`, the build in Visual Studio, and `MSBuild\Current\Bin\amd64\MSBuild.exe`. It does not run in the 32-bit `MSBuild\Current\Bin\MSBuild.exe`, as it only has 64-bit native libraries.

### Agent skill

The package brings a skill for AI coding agents, such as Claude Code and GitHub Copilot, that tells them how the compiler works: `sassconfig.json` and its options, that LibSass, the default, has no `@use` or `sass:math`, how urls are written, and how the build reports an error.

When the repository already uses an agent, the build copies the skill into it: into `skills/abstractions-sass-compiler` in the nearest `.claude` or `.agents` folder (both, when both are there), looking in the folder of the project and then in the folders above it, up to the root of the repository. Outside a repository, only the folder of the project is looked in. A repository without such a folder gets nothing, and the folder of the user (`~/.claude`) is never written to. The skill is only written when it changed, so it changes in the repository when an update of the package changes it. Commit it, or ignore it, as you do with the other skills of the repository, and do not edit it: the next update overwrites it.

To turn it off:

```xml
<PropertyGroup>
  <SassAgentSkills>False</SassAgentSkills>
</PropertyGroup>
```

To choose the folders yourself, set `HerreruudAgentSkills` to `claude` (`.claude/skills`), `agents` (`.agents/skills`) or `all`, in the folder of the project or the folder that `HerreruudAgentSkillsRoot` names. The Herreruud.Framework packages read the same properties, so one setting copies the skills of all of them.

### Compile all

**Build** › **Re-compile all files in solution** (`Shift+Alt+Y`), also on the context menu of the solution, compiles the files of every `sassconfig.json` in the solution.

### Error list

When a file does not compile, the Error List shows the error with its file, line and column, also when it is in an imported file, with **Abstractions Sass Compiler** as its source. Double-click it to go to it.
### Source maps

Source maps are supported when enabled either in `sassconfig.json` or `sassconfig.json.defaults`.

### Relative urls

By default, a `url()` is written to the CSS as it is in the Sass, as other Sass compilers do. A relative url is then relative to the CSS file, so write it from where the CSS ends up, or use a path that starts with `/`:

```scss
// Styles/site.scss, compiled to wwwroot/css/site.css
.logo { background: url(../images/logo.png); }   // wwwroot/images/logo.png
.hero { background: url(/images/hero.jpg); }      // from the root of the site
```

Set `rewriteUrls` to `true`, per file in `sassconfig.json` or for every file in `sassconfig.json.defaults`, to write relative urls from the Sass file instead. The compiler then rewrites them to be relative to the CSS file:

```json
[
  {
    "inputFile": "wwwroot/styles/site.scss",
    "outputFile": "wwwroot/css/site.css",
    "rewriteUrls": true
  }
]
```

With that, `url(images/logo.png)` in `wwwroot/styles/site.scss` becomes `url(../styles/images/logo.png)` in the CSS. The query and fragment of a url are kept, and urls that start with `/` or `#`, or have a scheme (`data:`, `https:`), are never changed. The compiled CSS does not say which file a url came from, so a url in an imported partial is read as relative to the main Sass file, not to the partial.

Libraries such as Font Awesome and Bootstrap Icons write their urls relative to the CSS, so leave `rewriteUrls` off for them.
### sassconfig.json

The extension adds a `sassconfig.json` file at the root of the
project which is used to configure the compilation.

Here's an example of what that file looks like:

```json
[
  {
    "outputFile": "wwwroot/site.css",
    "inputFile": "Styles/site.scss"
  },
  {
    "outputFile": "wwwroot/someotherfile.css",
    "inputFile": "Styles/someotherfile.scss",
    "sourceMap": "true",
    "style": "Compact"
  }
]
```
Default values for `sassconfig.json` can be found in the `sassconfig.json.defaults` file in the same location. 