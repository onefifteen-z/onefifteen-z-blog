---
title: 【Hexo】Part 3. How to Customize Your NexT
abbrlink: 54115
date: 2020-03-15 20:35:11
tags:
  - Hexo
  - NexT
  - Google Adsense
categories:
  - Development
  - Hexo
---
### Quick Start

According to the [official document](https://theme-next.org/docs/advanced-settings) of `NexT`, we can implement the customization of `NexT` by `theme_inject`. This blog will talk about how to customize your `NexT` with the config and create your own inject step by step as well.

### Using Injects

#### `custom_file_path`

First, uncomment the code in the `themes/next/_config.yml`( If you are using the submodule and CI tool metioned in the previous blog, please remember to change the `deploy/_config.yml` as well). `NexT` has already created the 10 injection points as following. Of cause, you can change the path here to your own customized path.

<!-- more -->

```yml
custom_file_path:
  head: source/_data/head.swig
  header: source/_data/header.swig
  sidebar: source/_data/sidebar.swig
  postMeta: source/_data/post-meta.swig
  postBodyEnd: source/_data/post-body-end.swig
  footer: source/_data/footer.swig
  bodyEnd: source/_data/body-end.swig
  variable: source/_data/variables.styl
  mixin: source/_data/mixins.styl
  style: source/_data/styles.styl
```

Then, create the file with the path set in the config file and add your own code there.

#### Insert Your Own Injecttion

First, create a new `js` file under the `scripts` folder. All the `js` files will be excuted when `Hexo` server has been started.

`scripts/gitter.js`

```js
hexo.extend.filter.register('theme_inject', function(injects) {
  injects.head.file('gitter', 'views/gitter.swig', {}, {cache: true});
});
```

Then create the view file with the path above:

`views/gitter.swig`

```js
<script>
  ((window.gitter = {}).chat = {}).options = {
    room: 'your-room-name'
  }});
</script>
<script src="https://sidecar.gitter.im/dist/sidecar.v1.js" async defer></script>
```

Start the server, now you can see the `gitter` right bottom. In addition, you can also inject the style of `gitter` as well.

`scripts/gitter.js`

```diff
hexo.extend.filter.register('theme_inject', function(injects) {
   injects.head.file('gitter', 'views/gitter.swig', {}, {cache: true});
+  injects.style.push('styles/gitter.styl'); // Injection of style
});
```

`styles/gitter.styl`

```css
.gitter-open-chat-button {
  background-color: slateblue;
  margin-bottom: .8rem;
  margin-right: 1rem;
  padding: .4rem .8rem;
  border-radius: .6rem;
  box-shadow: 0 0 .4rem #111;
  opacity: .9;
}

.gitter-open-chat-button:focus,.gitter-open-chat-button:hover {
  background-color: slateblue;
  box-shadow: 0px 0px 0.8rem #111;
}

.gitter-open-chat-button.is-collapsed {
  transform: translateY(150%);
}

.sidebar-toggle {
  margin-bottom: 18px;
}
```

### Example: Add Google Adsense in Your Blog

I recommend to use the automatic adv of `Google Adsense`. It has no need to think about where to put the advertisement and easier to import. Just one line in `source/_data/header.swig`.

```html
<script data-ad-client="{CLIENT-ID}" async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
```

### Example: Customize the Header Image of Your Blog

`NexT` is a Minimalism theme. That's why we choose it. Since it has been more and more popular, sometimes we'd like to change something in our blog to make it look a little different with others.

![My Blog](/images/20200315-how-to-customize-next-theme.jpg)

I have added the header image and changed the style of web title and menus. In addition, to avoid the footer being overlapped with the `Addthis` plugin on mobile, I expanded the `padding-bottom` of `footer`.

```css
.header {
    background-image: url(../images/header_background.jpg);
    background-size: cover;
    background-position: center;
}

.header-inner {
    margin: 0 auto;
    padding: 100px 0 70px;
}

.site-subtitle {
    color: #f5f5f5;
}

.site-nav-toggle .toggle-line.toggle-line {
    background: #f5f5f5;
}


.menu {
    margin-top: 20px;
    padding-left: 0;
    text-align: center;
    background: rgba(255,255,255,0.65);
    margin-left: auto;
    margin-right: auto;
    width: 500px;
    border-radius: initial;
}

.menu .menu-item a {
    display: block;
    font-size: 16px;
    text-transform: capitalize;
    line-height: inherit;
    border-bottom: 1px solid transparent;
    transition-property: border-color;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
    transition-delay: 0s;
}

footer.footer {
    padding: 20px 0 50px 0;
}

@media (max-width: 767px) {
    .menu {
        text-align: left;
        width: 100%;
    }

    .menu .menu-item {
        display: block;
        margin: 0 10px;
        vertical-align: top;
    }

    .menu .menu-item a {
        padding: 5px 10px;
    }
}
```
