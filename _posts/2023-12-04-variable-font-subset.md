---
layout: post
title:  "Variable Font Subset"
tags: [ web,font,woff2,optimization,ux ]
featured_image_thumbnail: 
featured_image:
featured: false
---

Fonts tend to affect UX when loading web pages quite a bit: they're usually loaded asynchronously with the rest of HTML and applied after the initial render, resulting in unpleasant text "twitching" after applied; alternatively, it's possible to delay the initial render until the font is downloaded, but in this case user will see no text at all for a bit. The behaviour is controlled with [font-display](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display) CSS property.

No matter the render strategy, you'd want to reduce font file size as much as possible. Modern web font formats, such as `WOFF2` already use best possible compression (brotli), so the only way to optimize further is to strip the font content.

In many cases, variable fonts come with lots of extra, which you many not need on your website: charsets (languages), ligatures, weights, etc. Each such extra (especially charsets) occupy extra space, which we can strip to reduce final font file size.

Let's take [JetBrains Mono](https://www.jetbrains.com/lp/mono) font for example. Say we want to use it for code snippets on a documentation website. We get a variable variant of the font in the downloaded archive, which is nice, but it's quite hefty: **300KB**. This is because it contains charsets for 148 languages, as proudly featured on the landing. Not sure why JB decided a font designed for code needs to include anything but English, but we most likely won't need those other 147 languages inside our code snippets.

Thanks to [fonttools](https://github.com/fonttools/fonttools) utility we can easily strip the font from all the extra chars and/or features.

The tool requires Python, so make sure it's installed. Then install fonttools and brotli packages (later is required to encode woff2):

```
pip install fonttools brotli
```

Now we can feed the source font and specify the chars we want to keep in the font:

```
pyftsubset jb.ttf --output-file=jb.woff2 --flavor=woff2  --layout-features=* --unicodes="U+0000-00FF,U+0131,U+0152-0153,U+02BB-02BC,U+02C6,U+02DA,U+02DC,U+2000-206F,U+2074,U+20AC,U+2122,U+2191,U+2193,U+2212,U+2215,U+FEFF,U+FFFD"
```

The resulting file is **45KB**, 6 times smaller than the original.

You can as well disable some layout features to reduce it further. Find the available options in the [tool docs](https://fonttools.readthedocs.io/en/latest/subset).