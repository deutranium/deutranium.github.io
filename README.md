# Hugo Blog
[![forthebadge](https://forthebadge.com/images/badges/powered-by-black-magic.svg)](https://forthebadge.com) [![forthebadge](https://forthebadge.com/images/badges/made-with-markdown.svg)](https://forthebadge.com)


Static site: [deutranium/blog](https://github.com/deutranium/blog)

Modified Theme: [deutranium/hugo-theme-mini](https://github.com/deutranium/hugo-theme-mini)

## How to setup
- Clone the repo and navigate to its root directory
``` bash
git clone https://github.com/deutranium/hugo-blog.git
cd hugo-blog
```

- Remove the cached theme submodule to add all the files again
``` bash
git rm -r --cached themes/
```

- Remove the local submodule files
``` bash
rm -r themes/
```

- Add the submodule for our copy of the theme
``` bash
git submodule add https://github.com/deutranium/hugo-theme-mini.git themes/mini
```

- Run the server and view your blog at http://localhost:1313 (most probably)
``` bash
hugo server
```

This would set up your hugo server along with the slightly modified copy of `mini` in `themes/mini`.


## Create a blog post
Use the following command to create a file with some data like datetime and title already entered
``` bash
hugo new content/posts/this-is-a-title.md
```
For example, the above command will create an `.md` file with title "This Is A Title" and date as the datetime of running this command. It should be noted the file created will have the **draft** status which should be set to `false` when the post is ready for publishing

## Generate static site
To generate the static site, run the following command
``` bash
hugo -D
```
which will add the static site content in `public/`. This folder can be used as a separate [repo](https://github.com/deutranium/blog) of its own and hosted on Github Pages or any other platform

An alternative way would be to create a `public/` folder as a separate repo within this repo and set its repo for remote origin. After this, just running `./deploy.sh /<commit message/>` would be enough as it will push the required content to both the repos (hugo content and static site).

*P.S. for this step, the `deploy.sh` file would require execution permissions, which can be given using `chmod +x deploy.sh`*

*P.P.S. You should already have the public folder with the static site contents*
