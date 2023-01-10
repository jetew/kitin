---
title: "Hugo 主题 Zzo 修改"
slug: "style-zzo"
date: 2022-11-15T11:33:02+08:00
lastmod: 2022-11-15T11:33:02+08:00
description: Hugo 之 Zzo 主题简单修改过程
enableTocContent: true
hidden: false
pinned: false
tags:
- hugo
- zzo
- blog
categories:
- 学习
image: /images/style-zzo/0.png
---

## 前言

之前在 [《使用 Hugo 搭建个人博客》](/tags/hugo/) 的一系列文章中讲了搭建个人博客的一些基本操作。因为我是用的是 Zzo 主题，所以我以这个主题来讲下个人的修改过程。

<!--more-->

### Zzo 主题

Zzo 是 Hugo 的博客主题。 它几乎包含了一个博客主题应该具备的所有功能。 搜索、图库等。 它看起来很简单，但一旦你深入研究它，你就会发现很多功能。Zzo 主题虽说很久没有更新，但是它作为博客主题功能十分全面，于官方主题模板中，在 Github 上的收藏数也是比较高的。

---

## 配置与修改

### 安装主题

- <a href="https://github.com/zzossig/hugo-theme-zzo" target="_blank">Github 项目地址</a>

你可以直接从项目地址下载主题并解压至 **~/themes/zzo** 或者通过 **git clone** 克隆仓库：

```bash
git clone https://github.com/zzossig/hugo-theme-zzo.git themes/zzo
```

或者你也可以将主题添加为子模块：

```bash
git submodule add https://github.com/zzossig/hugo-theme-zzo.git themes/zzo
```

主题更新：

```bash
git submodule update --remote --merge
```

### 配置主题

打开博客根目录下的 **config/_default** 文件夹，对下列文件依次进行修改：

{{< expand "config.toml" >}}

```toml
baseURL = "http://example.com/" # 网址
title = "Hugo Zzo Theme" # 标题
theme = "zzo"

defaultContentLanguage = "zh" # 默认语言
defaultContentLanguageInSubdir = true # baseURL/en/, baseURL/kr/ ...
hasCJKLanguage = true # 设置为 `true` 支持中/日/韩语言

summaryLength = 70 # 首页文章摘要字数
buildFuture = true # if true, we can use future date for talks page

copyright = "©{year}, All Rights Reserved" # 版权信息
timeout = 10000
enableEmoji = true
paginate = 13 # 首页文章数
rssLimit = 100

enableGitInfo = false # When true, the modified date will appear on a summary and single page. Since GitHub info needs to be fetched, this feature will slow down to build depending on a page number you have
googleAnalytics = ""

[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      hardWraps = true
      unsafe = true
      xHTML = true
  [markup.highlight]
    codeFences = true
    lineNos = true
    lineNumbersInTable = true
    noClasses = false
  [markup.tableOfContents]
    endLevel = 3
    ordered = false
    startLevel = 2

[outputs]
  home = ["HTML", "RSS", "SearchIndex"]
  section = ["HTML", "RSS", "SearchIndex"]
  taxonomyTerm = ["HTML", "RSS", "SearchIndex"]
  taxonomy = ["HTML", "RSS", "SearchIndex"]

[outputFormats]
  [outputFormats.SearchIndex]
    mediaType = "application/json"
    baseName = "index"
    isPlainText = true
    notAlternative = true
    
[taxonomies]
  category = "categories"
  tag = "tags"
  series = "series"

```

{{< /expand >}}

{{< expand "languages.toml" >}}

- 如果你只需要中文，你可以这样设置：

```toml
[zh]
  title = ""
  languageName = "中文"
  weight = 1
  languagedir = "ltr"
  contentdir = "content"
```

- 如果你需要多语言，你可以这样设置：

```toml
[zh]
  title = "" # 标题
  languageName = "中文"
  weight = 1
  languagedir = "ltr"
  contentdir = "content/zh/"

[en]
  title = ""
  languageName = "English"
  weight = 2
  languagedir = "ltr"
  contentdir = "content/en"
```

{{< /expand >}}

{{< expand "menus.zh.toml" >}}

```toml
[[main]]
  identifier = "archive"
  name = "归档"
  url = "archive"
  weight = 1

[[main]]
  identifier = "categories"
  name = "分类"
  url = "categories"
  weight = 2

[[main]]
  identifier = "tags"
  name = "标签"
  url = "tags"
  weight = 3
    
[[main]]
  identifier = "about"
  name = "关于"
  url = "about"
  weight = 4
```

> 如果你是多语言，那么就应该继续创建 **menus.en.toml** 并进行配置

{{< /expand >}}

{{< expand "params.toml" >}}

```toml
logoText = "Zzo" # Logo text that appears in the site navigation bar.
logoType = "short" # long, short -> short: squre shape includes logo text, long: rectangle shape not includes logo text
logo = true # Logo that appears in the site navigation bar.
description = "The Zzo theme for Hugo example site." # for SEO
custom_css = [] # custom_css = ["scss/custom.scss"] and then make file at root/assets/scss/custom.scss
custom_js = [] # custom_js = ["js/custom.js"] and then make file at root/assets/js/custom.js
useFaviconGenerator = false # https://www.favicon-generator.org/
languagedir = "ltr" # ltr / rtl

themeOptions = ["dark", "light", "hacker", "solarized", "kimbie"] # select options for site color theme
notAllowedTypesInHome = ["contact", "talks", "about", "showcase"] # not allowed page types in home page. type can be set in front matter or default to folder name.
notAllowedTypesInHomeSidebar = ["about", "archive", "showcase"] # not allowed page types in home page sidebar(recent post titles).
notAllowedTypesInArchive = ["about", "talks", "showcase"] # not allowed page types in archive page
notAllowedTypesInHomeFeed = ["about", "archive", "contact", "talks", "showcase", "publication", "presentation", "resume", "gallery"]
enablePinnedPosts = true # show pinned posts first in the main view

viewportSize = "normal" # widest, wider, wide, normal, narrow
enableUiAnimation = true
hideSingleContentsWhenJSDisabled = false
minItemsToShowInTagCloud = 1 # Minimum items to show in tag cloud

# appbar
enableAppbarSearchIcon = false
enableAppbarLangIcon = false

# header
homeHeaderType = "text" # text, img, slide, typewriter
hideHomeHeaderWhenMobile = false

# menu
showMobileMenuTerms = ["tags", "categories", "series"]

# navbar
enableThemeChange = true # site color theme

# body
enableBreadcrumb = true # breadcrumb for list, single page
enableSearch = true # site search with Fuse
enableSearchHighlight = true # when true, search keyword will be highlighted
enableGoToTop = true # scroll to top
enableWhoami = true # at the end of single page
summaryShape = "classic" # card, classic, compact
archiveGroupByDate = "2006" # "2006-01": group by month, "2006": group by year
archivePaginate = 13 # items per page
paginateWindow = 1 # setting it to 1 gives 7 buttons, 2 gives 9, etc. If set 1: [1 ... 4 5 6 ... 356] [1 2 3 4 5 ... 356] etc
talksPaginate = 8 # items per page
talksGroupByDate = "2006" # "2006-01": group by month, "2006": group by year

# whoami: usage - home page sidebar, single page bottom of post. all values can be empty
myname = "zzossig"
email = "zzossig@gmail.com"
whoami = "Web Developer"
bioImageUrl = "" # image url like "http//..." or "images/anyfoldername/mybioimage.jpg" If not set, we find a avatar image in root/static/images/whoami/avatar.(png|jpg|svg)
useGravatar = false # we use this option highest priority
location = "Seoul, Korea"
organization = "Hugo"
link = "https://github.com/zzossig/hugo-theme-zzo"

# sidebar
enableBio = true # in home page sidebar
enableBioImage = true # in home page sidebar
enableSidebar = true # Set to false to create the full width of the content.
enableSidebarTags = true # if you want to use tags.
enableSidebarSeries = true
enableSidebarCategories = true
enableHomeSidebarTitles = true
enableListSidebarTitles = true
enableToc = true # single page table of contents, you can replace this param to toc(toc = true)
hideToc = false # Hide or Show toc
tocPosition = "inner" # inner, outer
tocFolding = false
enableTocSwitch = true # single page table of contents visibility switch
itemsPerCategory = 5 # maximum number of posts shown in the sidebar.
sidebarPosition = "right" # bio, profile component layout position
tocLevels = ["h2", "h3", "h4"] # minimum h2, maximum h4 in your article
enableSidebarPostsByOrder = false # another lists in the sidebar

# footer
showPoweredBy = true # show footer text: Powered by Hugo and Zzo theme
showFeedLinks = true # RSS Feed 
showSocialLinks = true # email, facebook, twitter ...
enableLangChange = true # show button at bottom left of footer.

# service
googleTagManager = "" # GTM-XXXXXX
baiduAnalytics = "" # alternative of google analytics
enableBusuanzi = false # if set true, total page view, total unique visitors show up in the footer.
busuanziSiteUV = true # unique visitors (total number of visitors)
busuanziSitePV = true # site total page view count
busuanziPagePV = true # post view count

# comment
enableComment = true
disqus_shortname = "" 
commento = false

[gitment]          # Gitment is a comment system based on GitHub issues. see https://github.com/imsun/gitment
  owner = ""              # Your GitHub ID
  repo = ""               # The repo to store comments
  clientId = ""           # Your client ID
  clientSecret = ""       # Your client secret

[utterances]       # https://utteranc.es/
  owner = ""              # Your GitHub ID
  repo = ""               # The repo to store comments
  message = ""      # Optional
  link = ""         # Optional

[gitalk]           # Gitalk is a comment system based on GitHub issues. see https://github.com/gitalk/gitalk
  owner = ""              # Your GitHub ID
  repo = ""               # The repo to store comments
  clientId = ""           # Your client ID
  clientSecret = ""       # Your client secret

# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
[valine]
  enable = false
  appId = '你的appId'
  appKey = '你的appKey'
  notify = false  # mail notifier , https://github.com/xCss/Valine/wiki
  verify = false # Verification code
  avatar = 'mm' 
  placeholder = '说点什么吧...'
  visitor = false

[changyan]
  changyanAppid = ""        # Changyan app id             # 畅言
  changyanAppkey = ""       # Changyan app key

[livere]
  livereUID = ""            # LiveRe UID                  # 来必力

# Isso: https://posativ.org/isso/
[isso]
  enable = false
  scriptSrc = "" # "https://isso.example.com/js/embed.min.js"
  dataAttrs = "" # "data-isso='https://isso.example.com' data-isso-require-author='true'"

[socialOptions] # if set, social icons will show up.
  email = "mailto:your@email.com"
  phone = ""
  facebook = "http://example.org/"
  twitter = "http://example.org/"
  github = "https://github.com/zzossig/hugo-theme-zzo"
  stack-overflow = ""
  instagram = ""
  google-plus = ""
  youtube = ""
  medium = ""
  tumblr = ""
  linkedin = ""
  pinterest = ""
  stack-exchange = ""
  telegram = ""
  steam = ""
  weibo = ""
  douban = ""
  csdn = ""
  gitlab = ""
  mastodon = ""
  jianshu = ""
  zhihu = ""
  signal = ""
  whatsapp = ""
  matrix = ""
  xmpp = ""
  dev-to = ""
  gitea = ""
  google-scholar = ""
  twitch = ""

[donationOptions]
  enable = false # if set, the donation button will show up on the single page.
  alipay = "" # Alipay QR Code image (example path: images/donation/alipay-qrcode.png) and put your file at root/static/images/donation/
  wechat = "" # Wechat pay QR Code image (example path: same as above)
  paypal = "" # Paypal URL
  patreon = "" # Patreon URL
  bitcoin = "" # example path: images/donation/bitcoin-code-image.png

[copyrightOptions]
  enableCopyrightLink = false # if set, you can add copyright link
  copyrightLink = ""
  copyrightLinkImage = ""
  copyrightLinkText = ""

# possible share name: "facebook","twitter", "reddit", "linkedin", "tumblr", "weibo", "douban", "line", "whatsapp", "telegram"
[[share]]
  name = "facebook"
[[share]]
  name = "twitter"
  username = ""

[[footerLinks]]
  name = ""
  link = ""
[[footerLinks]]
  name = ""
  link = ""
```

{{< /expand >}}

接着修改一下文章默认模板，位于 **~/archetypes/default.md**

{{< expand "default.md" >}}

```yaml
---
title: "{{ replace .Name "-" " " | title }}"
slug: "{{ .Name }}"
date: {{ .Date }}
lastmod: {{ .Date }}
description: 
enableTocContent: true
hidden: false
pinned: false
tags:
-
categories:
-
image:
---
```

{{< /expand >}}

---

### 修改主题

#### 标题中文显示

更改分类、标签、系列页面中文显示

**content** 目录下新建 **categories/_index.md**

```yaml
---
title: "分类"
---
```

**content** 目录下新建 **tags/_index.md**

```yaml
---
title: "标签"
---
```

**content** 目录下新建 **series/_index.md**

```yaml
---
title: "系列"
---
```

#### 字体修改

你可以通过以下方式修改字体：

##### 使用预设字体

1. 创建 **~/data/font.toml** 文件

```toml
title_font = "\"Montserrat\", sans-serif" # 标题字体
content_font = "\"muli\", sans-serif" # 文章字体
```

2. 主题自带预设字体如下：

```toml
"\"Montserrat\", sans-serif" 
"\"Merriweather\", serif"
"\"muli\", sans-serif"
"\"Courgette\", cursive"
"\"Source Sans Pro\", sans-serif"
"\"League Gothic\", sans-serif"
```

##### 自定义字体

1. 在 **assets/css** 文件夹下创建 **font.css** 文件

2. 修改 **config/_default/params.toml** 配置文件，在自定义 Css 处添加：

```toml
...
custom_css = ["css/font.css"]
...
```

3. 在 **font.css** 中添加如下：

```css
@font-face {
    font-family: 'Montserrat';
    src: url('../fonts/montserrat-black.woff2') format('woff2'),
        url('../fonts/montserrat-black.woff') format('woff');
    font-weight: 900;
    font-style: normal;
}

@font-face {
    font-family: 'Merriweather';
    src: url('../fonts/merriweather-regular.woff2') format('woff2'),
        url('../fonts/merriweather-regular.woff') format('woff');
    font-weight: 400;
    font-style: normal;
}
```

4. 将下载的字体文件放入 **~/static/fonts/** 文件夹中

5. 创建 **~/data/font.toml** 文件

```toml
title_font = "\"Montserrat\", sans-serif" # 标题字体
content_font = "\"muli\", sans-serif" # 文章字体
```

##### 导入字体

1. 创建 **~/layouts/partials/head/custom-head.html** 文件，并编辑(我这里是从 <a href="https://fonts.google.com/" target="_blank">Google Fonts</a> 导入的宋体)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300&family=Noto+Serif+SC:wght@300&display=swap" rel="stylesheet">
```

3. 创建 **~/data/font.toml** 文件

```toml
title_font = "\"Noto Sans SC\", sans-serif"
content_font = "\"Noto Serif SC\", serif"
```

#### 首页顶部修改

作者提供了几种首页顶部样式，可以参照下面的模板进行选择；文件位于 **~/content/_index.md**

{{< expand "_index.md" >}}

```yaml
---
header:
  - type: typewriter # 打字效果
    methods:
      - typeString: Hello world!
      - pauseFor: 2500
      - deleteAll: true
      - typeString: Strings can be removed
      - pauseFor: 2500
      - deleteChars: 7
      - typeString: <strong>altered!</strong>
      - pauseFor: 2500
    options:
      loop: true
      autoStart: false
    height: 190
    paddingX: 50
    align: center
    fontSize: 44
    fontColor: yellow
    
  - type: text
    height: 200
    paddingX: 50
    paddingY: 0
    align: center
    title:
      - HUGO
    subtitle:
      - The world’s fastest framework for building websites
    titleColor: 
    titleShadow: true
    titleFontSize: 44
    subtitleColor: 
    subtitleCursive: true
    subtitleFontSize: 18
    spaceBetweenTitleSubtitle: 16
  
  - type: text
    height: 200
    paddingX: 50
    paddingY: 0
    align: center
    title:
      - HUGO
    subtitle:
      - The world’s fastest framework for building websites
    titleColor: # #123456, red
    titleShadow: false
    titleFontSize: 44
    subtitleColor: # #123456, red
    subtitleCursive: false
    subtitleFontSize: 16
    spaceBetweenTitleSubtitle: 20
  
  - type: img # 图片
    imageSrc: images/header/background.jpg # your image file path: root/static/images/header/background.jpg
    imageSize: cover # auto|length|cover|contain|initial|inherit
    imageRepeat: no-repeat # repeat|repeat-x|repeat-y|no-repeat|initial|inherit
    imagePosition: center # x% y%| xpos ypos| left top| center bottom| ...
    height: 235
    paddingX: 50
    paddingY: 0
    align: center
    title:
      -
    subtitle:
      -
    titleColor:
    titleShadow: false
    titleFontSize: 44
    subtitleColor:
    subtitleCursive: false
    subtitleFontSize: 16
    spaceBetweenTitleSubtitle: 20

  - type: slide # 滚动图片
    height: 235
    options:
        startSlide: 0
        auto: 5000 # auto slide delay 5000ms(5sec)
        draggable: true # slide draggable
        autoRestart: true # restart after drag finished
        continuous: true # last to first
        disableScroll: true
        stopPropagation: true
    slide:
      - paddingX: 50
        paddingY: 0
        align: left
        imageSrc: images/header/background.jpg
        imageSize: cover
        imageRepeat: no-repeat
        imagePosition: center
        title:
          - header title1
        subtitle:
          - header subtitle1
        titleFontSize: 44
        subtitleFontSize: 16
        spaceBetweenTitleSubtitle: 20

      - paddingX: 50
        paddingY: 0
        align: center
        imageSrc: images/header/background.jpg
        imageSize: cover
        imageRepeat: no-repeat
        imagePosition: center
        title:
          - header title2
        subtitle:
          - header subtitle2
        titleFontSize: 44
        subtitleFontSize: 16
        spaceBetweenTitleSubtitle: 20

      - paddingX: 50
        paddingY: 0
        align: right
        imageSrc: images/header/background.jpg
        imageSize: cover
        imageRepeat: no-repeat
        imagePosition: center
        title:
          - header title3
        subtitle:
          - header subtitle3
        titleFontSize: 44
        subtitleFontSize: 16
        spaceBetweenTitleSubtitle: 20
---
```

{{< /expand >}}

设置好样式后，在 **~/config/_default/params.toml** 文件中的 **homeHeaderType** 选项设置对应的样式即可。

#### 代码高亮样式

创建 **~/data/skin.toml** 然后进行修改：

```toml
theme_light_chroma = "default"
theme_dark_chroma = "default"
theme_hacker_chroma = "default"
theme_solarized_chroma = "default"
theme_kimbie_chroma = "default"
```

然后设置对应皮肤的高亮，比如亮色主题：

```toml
theme_dark_chroma = "colorful"
```

代码高亮样式可以在 <a href="https://xyproto.github.io/splash/docs/all.html" target="_blank">这里</a> 查找

---

## 总结

Zzo 主题的修改与调整差不多就是这样了；其他的我觉得其实并没有什么需要修改的，正如前文所说： Zzo 是一个功能十分完善的主题；作者也提供了许多可客制化的内容，更多更详细的修改、定制可以到作者提供的文档中进行查阅。

## 参考资料

1. <a href="https://zzo-docs.vercel.app/zzo" target="_blank">Zzo 主题文档</a>