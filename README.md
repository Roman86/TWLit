# Overview

This package allows us to utilize TailwindCSS from within LitElement components.

We need to set up the TailwindCSS build process as normal (either via running npx tailwindcss or by configuring it as a PostCSS plugin). See the TailwindCSS site for details on this.

Once you have tailwind setup to scan the LitElement for classes and produce a CSS file, TWLit then looks for changes to this file and then creates a JS file from it that can be imported to the Static Styles property of you LitElement. This gives us a nice DX in that Tailwind classes added to your LitElement are automatically generated and can be used with no manual build step required.

This approach also means we use the constructable style sheets functionality that LitElement provides and as such the style sheet will not be duplicated if more than one of our LitElement components are present in the application.

A Full working example of previous/forked version in use can be found here:
https://github.com/MarkJamesHoward/TWLitExampleUse

# Usage

### Once installed, run from the command line

`twlit --input ./tw.css --output ./twlit.js`

Watch mode is also available.

`twlit --input ./tw.css --output ./twlit.js --watch`

In this mode the process won't exit, it will constantly watch the input file and output a new JS file on each change.

### Or add to your tooling chain in package.json

```package.json
"scripts": {
    "build": "echo nil > src/twlit.ts & echo nil > src/tw_output.css & concurrently \"tailwindcss -i src/tw_global.css -o src/tw_output.css & twlit --input src/tw_output.css --output src/twlit.ts\" \"tsc\"",
    "build:watch": "echo nil > src/twlit.ts & echo nil > src/tw_output.css & concurrently \"tailwindcss -i src/tw_global.css -o src/tw_output.css --watch\" \"twlit --input src/tw_output.css --output src/twlit.ts --watch\" \"tsc --watch\"",
}
```

Echo commands are used to create the files if they don't exist. This is to prevent errors when running the build command for the first time.

Notice: example filenames use .ts extension, change to .js if you are using JavaScript.

# Parameters

### --input

Specify the location of your tailwind generated CSS file. In the above example this is the 'tw.css' file. This is the file spit out from running either npx tailwindcss or from your PostCSS setup of tailwind. Either way it contains all the class definitions that we need inside of our LitElement

### --output

The output is a JS file that contains all the Tailwind classes within a tagged template literal. This can now be imported into your LitElement

### --watch

If set the process won't exit and will continue to watch the input file and update the output file on each change.

# LitElement configuration:

We need to import the JS file that is spit out (from --output above)

`import { TWStyles } from "./tailwind/twlit.js";`

And then include this in the static Styles property of our LitElement:

` static styles = [css``, TWStyles];  `
