# How to (set up this website)?

I use a [fork](https://github.com/nodejh/hugo-theme-mini) of the Hugo theme [mini by nodejh](https://github.com/nodejh/hugo-theme-mini), only becuase I want to make a few customisations. Please feel free to use either as the setup steps would be the same :)

## 1. Pre-requisites
Please ensure you have `hugo` installed on your system. If not, you may follow the installation instruction [here](https://gohugo.io/installation/)

## 2. Setup
- Create an empty repo and clone it
``` git
git clone https://github.com/<your username>/<your repo>.git
```
- Navigate to your cloned repo and install the theme as a submodule. You might have to replace `deutranium` with `nodejh` depending on which theme you're using.
```
git submodule add https://github.com/deutranium/hugo-theme-mini.git themes/mini
```
- Now that we have the theme in place, we need to create the directory structure to store out content.
### 2.1 Copy exampleSite from the theme itself (easier and recommended)
- Simply copy the exampleSite provided with the theme itself
``` bash
cp -r ./themes/mini/exampleSite/ ./
```
### 2.2 Create directory structure from scratch
- Use a bunch of `mkdir` and `touch` statements to create the following directory structure:
```
.
├── LICENSE
├── README.md
├── config.yaml
├── content
│   ├── about
│   │   └── _index.md
│   └── posts
│       └── test.md
├── layout
└── static
```
Recall that `mkdir <folder name>` and `touch <file name>` create the folders and files with the respective names.

## 3. Run the server
- Simply run the server from your project root using
``` bash
hugo server
```
and you should be able to see your website at the URL shown in your terminal window (Probably [http://localhost:1313/about/](http://localhost:1313/))

Please note that the website would have some palceholder content if you followed `2.1`, and empty otherwise.

## 4. Enjoy :)
Have some cold lemonade or hot chocolate depending on the state of sun outside your window!