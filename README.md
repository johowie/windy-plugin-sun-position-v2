<p align="center"><img src="https://www.windy.com/img/logo201802/logo-full-windycom-gray-v3.svg"></p>

A Windy plugin that shows sun and moon positions on the map and gives details about sunset and sunrise times as well as other sun and moon details. This is a full rewrite of [version 1](https://github.com/jacobsjo/windy-plugin-sun-position) to work with the new windy plugin api and includes a number of improvements.

To use it on desktop go to https://windy.com, then go to `Menu` -> `Install Windy Plugin` -> `Sun and Moon position` -> `Install plugin`. You can then access it by right-clicking on the map and selecting `Sun Position`. The plugin will stay available even after a reload. (Mobile TBD)

![Sun detail pane](src/screenshot.png?raw=true "Screenshot of a dial showing directions and an info window")

### Dial
The dial displays the current sun and moon azimuth on the map using a black line from the picker position. Additionally, dashed lines show the azimuth of sunrise and sunset and dotted lines show the azimuth of moonrise and moonset.

### Detail pane
The detail pane on the right shows current azimuth and altitude of the sun and moon and their rise and set times. It also shows the current sunlight condition (such as golden hour), and moon phase. Optionally, a timeline can be activated, that shows the timing of astronomical and nautical twilight, and blue and golden hour.

The top of the pane shows a diagram of the sun and moon altitudes over the curse of the current day.

## Changelog
See https://github.com/jacobsjo/windy-plugin-sun-position for the changelog of version 1
### v2.0.0-alpha.8 (compared to v0.3.6)
- Completely rewritten plugin for windy plugin api v2.
- Using svelte to make plugin more modular
- more compact design
- the dial is no longer bound to the picker and can be moved independently
- added weather information to the timeline
- separate small mobile ui with 3 tabs
- removed the option to hide specific parts of the timeline

### V2.0.0-beta.1
- support for radar and satellite timelines
- fix bug when clicking on nadir time in timeline

## Development
Contributions are welcome!

To develop, run:
```sh
npm i
npm run start
```
then accept the self-signed certificate by going to https://localhost:9999/plugin.js. Finally, go to https://www.windy.com/developer-mode to load the plugin.

See https://docs.windy-plugins.com/getting-started/ for how to develop a windy plugin. 


========================
JOHN'S NOTES BELOW
=========================
- CHANGES
--run `npm audit fix`
-- changes to npm dependency : @windycom\plugin-devtools\index.mjs to fix loading of a file (see below)
-- changes to AltitudeDiagram.svelte to adjust the sun/moon azimuth graph
-- TODO: bug : crash when dragged over dateline (and maybe too higher an latitude)
--- related to use of https://www.npmjs.com/package/tz-lookup in ./src/plugin.svelte




==============================
Changes made to @windycom\plugin-devtools\index.mjs
===================================================
lines 210-246
changed the regex to remove the file name and slash from the path
this corrected  the following error:
Error while opening and parsing D:\dev\windy-plugin-sun-position-v2\src\plugin.svelte/pluginConfig.ts Error: ENOENT: no suc.svelte\pluginConfig.ts'


/**   line 210
 * Creates a Rollup plugin that transforms code to ESM format with our enhancements
 * @returns The Rollup plugin.
 */
export function transformCodeToESMPlugin() {
    let shouldGenerateSourcemaps = false;
    let outputDirectory = null;
    return {
        name: 'transform-to-esm-plugin',
        options(options) {
            const outputOptions = Array.isArray(options.output)
                ? options.output[0]
                : options.output ?? {};

            shouldGenerateSourcemaps = Boolean(outputOptions.sourcemap);
            outputDirectory = outputOptions.file.replace(/\/[^/]+$/, '');

            return undefined;
        },
        renderChunk(code, chunk) {
            const { facadeModuleId } = chunk;
            const pathOfTheFile = facadeModuleId.replace(/[^\\]+\\?$/, '');

            if (!fs.existsSync(outputDirectory)) {
                fs.mkdirSync(outputDirectory, { recursive: true });
            }

            return transformCode(
                code,
                shouldGenerateSourcemaps,
                this,
                pathOfTheFile,
                outputDirectory,
            );
        },
    };
}





===================

`npm audit fix` was run after this message
===============================

Microsoft Windows [Version 10.0.19045.5247]
(c) Microsoft Corporation. All rights reserved.

D:\dev\windy-plugin-sun-position-v2>npm i
npm warn deprecated sourcemap-codec@1.4.8: Please use @jridgewell/sourcemap-codec instead
npm warn deprecated @types/rollup@0.54.0: This is a stub types definition for rollup (https://github.com/rollup/rollup). rollup provides its own type definitions, so you don't need @types/rollup installed!

added 447 packages, and audited 448 packages in 30s

117 packages are looking for funding
  run `npm fund` for details

10 vulnerabilities (1 low, 6 moderate, 3 high)

To address issues that do not require attention, run:
  npm audit fix

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
npm notice
npm notice New major version of npm available! 10.9.0 -> 11.0.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.0.0
npm notice To update run: npm install -g npm@11.0.0
npm notice

D:\dev\windy-plugin-sun-position-v2>npm audit
# npm audit report

braces  <3.0.3
Severity: high
Uncontrolled resource consumption in braces - https://github.com/advisories/GHSA-grv7-fg5c-xmjg
fix available via `npm audit fix`
node_modules/braces

cross-spawn  7.0.0 - 7.0.4
Severity: high
Regular Expression Denial of Service (ReDoS) in cross-spawn - https://github.com/advisories/GHSA-3xgq-45jj-v275
fix available via `npm audit fix`
node_modules/cross-spawn

micromatch  <4.0.8
Severity: moderate
Regular Expression Denial of Service (ReDoS) in micromatch - https://github.com/advisories/GHSA-952p-6rrq-rcjv
fix available via `npm audit fix`
node_modules/micromatch

nanoid  <3.3.8
Infinite loop in nanoid - https://github.com/advisories/GHSA-mwcw-c2x4-8c55
fix available via `npm audit fix`
node_modules/nanoid

postcss  <8.4.31
Severity: moderate
PostCSS line return parsing error - https://github.com/advisories/GHSA-7fh5-64p2-3v2j
fix available via `npm audit fix --force`
Will install @windycom/plugin-devtools@1.0.6, which is a breaking change
node_modules/autoprefixer/node_modules/postcss
node_modules/less-plugin-autoprefixer/node_modules/postcss
  autoprefixer  1.0.20131222 - 9.8.8
  Depends on vulnerable versions of postcss
  node_modules/autoprefixer
  less-plugin-autoprefixer  *
  Depends on vulnerable versions of autoprefixer
  Depends on vulnerable versions of postcss
  node_modules/less-plugin-autoprefixer
    @windycom/plugin-devtools  >=1.0.7
    Depends on vulnerable versions of less-plugin-autoprefixer
    node_modules/@windycom/plugin-devtools

rollup  4.0.0 - 4.22.3
Severity: high
DOM Clobbering Gadget found in rollup bundled scripts that leads to XSS - https://github.com/advisories/GHSA-gcx4-mw62-g8wm
fix available via `npm audit fix`
node_modules/rollup

svelte  <4.2.19
Severity: moderate
Svelte has a potential mXSS vulnerability due to improper HTML escaping - https://github.com/advisories/GHSA-8266-84wp-wv5c
fix available via `npm audit fix`
node_modules/svelte

10 vulnerabilities (1 low, 6 moderate, 3 high)

To address issues that do not require attention, run:
  npm audit fix

To address all issues (including breaking changes), run:
  npm audit fix --force