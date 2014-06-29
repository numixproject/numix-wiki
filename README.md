# numix-kit

## Introduction

`numix-kit` generates complete icons from predefined SVG templates and symbols.

## Directory structure

    |-- input
    |   |-- symbols
    |   |   |-- file-manager.svg
    |   |   |-- text-editor.svg
    |   |   |-- web-browser.svg
    |   `-- templates
    |       |-- circle
    |       |-- hexagon
    |       `-- square
    |           |-- background.svg
    |           |-- blur.svg
    |           |-- clip.svg
    |           |-- overlay.svg
    |           |-- shadow.svg
    |           `-- template.meta
    `-- output
        |-- circle
        |-- hexagon
        `-- square
            |-- png
            `-- svg

## Templates and symbols

The templates and symbols reside under the `input` directory. The symbols are common to all templates, while the templates are categorized under different folders according to their name.

The SVG files in the `template` directory can have any name, but they must be referenced inside the `template.meta` file.

Each file in the directory are passed to the script as a layer. For example, `background.svg` will be passed as a layer named `background`.

### template.meta

Each template must contain a `template.meta` file, which describes how to generate the resulting icon.

The syntax follows a simple approach. **Select a layer, and modify it.**

An example `template.meta` file looks like,

    background -> fill(symbol[color])
    symbol -> %drop -> fill(#000) -> filter="blur.svg" -> opacity="0.5" -> clip-path="clip.svg"
    symbol -> transform="translate(0,-1)" -> clip-path="clip.svg"
    shadow -> +background -> +drop -> +symbol -> +overlay -> write()

So, let's see what's happening.

In the first line, we select the `background` layer and perform a `fill` operation on it. `fill` is a built-in function in the script.

Here the data passed to `fill` is `symbol[color]`. `symbol[color]` tells the script to take the `color` data from the `symbol`.

Any SVG file can contain these types of data in a simple syntax. For example, the `color` data can be defined inside the symbol as,

    <!-- color: #d64937 -->

Coming to the next line, we are generating a shadow for the symbol here. So, we select the `symbol` and copy into a new layer named `drop`. The `%` in front of the name tells the script to create a new layer.

Then we do a `fill` operation on it with the color `#000`.

Next, we tell the script to add the attribute `filter="blur.svg`. Here the script sees that `blur.svg` is a SVG file name and includes that file in appropriate format. If it's not a file name, it'll directly add that attribute to the layer, as it occurs in the next step with `opacity="0.5"`.

In the last line, we tell the script to take the `shadow` layer, then add the `background` layer to it, then `drop` and so on. The `+` in front of the name tells the script to add the layer.

Lastly, we tell the script to `write` the result with `write()`. The `write` function writes the SVG (and PNG if specified) files to the output folder with the same name as the `symbol` layer.

### Resources

1. [SVG attribute index](http://www.w3.org/TR/SVG/attindex.html)
2. [SVG filter effects](http://www.w3.org/TR/SVG/filters.html)

## Ouput folder

The `ouput` folder holds the generated icons categorized under the template name and file type (SVG or PNG).

## Usage

The usage can accessed from the script with `-h` or `--help`.
