# install
```
os:debian
$ sudo npm install -g markmap-cli
$ markmap 
```
# use
```
Usage: markmap [options] <input>

Create a markmap from a Markdown input file

Options:
  -V, --version          output the version number
  --no-open              do not open the output file after generation
  --no-toolbar           do not show toolbar
  -o, --output <output>  specify filename of the output HTML
  --offline              Inline all assets to allow the generated HTML to work offline
  -w, --watch            watch the input file and update output on the fly, note that this feature is for development only
  -h, --help             display help for command
```
```
markmap xxx.md --offline -o xx.html
```

# ref
https://markmap.js.org/docs/packages--markmap-cli
