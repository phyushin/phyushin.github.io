---
layout: default
title:  "Adding Captions To Images In Jekyll"
date:   2025-09-23 00:00:00 +0000
categories: Markdown
---
I wasn't quite happy with the way my last blogpost looked so I decided to add captions to the images and make them a little _prettier_ so I've documented what I did, (it wasn't super hard) but just in case anyone else wanted to do the same then they can checkout the [previous post][1] to see how it shakes out


## First Off
Firstly, you'll need to add a new file into your `_includes` directory (I chose `image.html`), if don't have an `_includes` directory, create one in the root of the directory, and then create a file named, (in my case) `image.html`:

```bash
mkdir /<myblog_repo_folder>/_includes
touch /<myblog_repo_folder>/_includes/image.html
```

## Image.html Contents
Open the file and add HTML like the below, note the use of the `include.url` and `include.description`:  

```html
<figure class="image">
    <img 
        src="{{site.url}}/{{ include.url }}" 
        alt="{{ include.description }}"
        class="img-responsive"
    >
    <figcaption>
        <center>
            <em>{{ include.description }}</em>
        </center>
        </figcaption>
</figure>
```

The `include.*` works in the same way as `props` in _react_ I guess, so what will happen here is when we add an image we require a caption on we can add `url` for the obvious, and `description` which we can use as alt text but also as a caption underneath it will look something like this in markdown:

{% raw %}
```markdown
{% include image.html url="assets/ios_mobile_app_testing_pt1/sideloadly.png" description="Sideloadly Screenshot showing upload of an IPA" %}
```
{% endraw %}

Once rendered, it will look like this:
{% include image.html url="assets/adding_captions_to_images_in_jekyll/image_with_caption.png" description="A Screenshot of a Screenshot... very meta" %}


## Other Quirks...
When writing this blog post, I noticed that when I tried to put the [Liquid][2] syntax, Jekyll kept trying to interpret it and gave me _weird_ results...
To fix this you can wrap the _offending_ code in the `raw` tag ending with the `endraw` tag... 

These are wrapped with with the {% raw %} `{% %}` {% endraw %} but trying to get that to display ends up with a loop of trying to render how we displayed each bit... so i'm not gonna do that :)


Hopefully, This was useful

Phyu


[1]: {% post_url ../2025-09-11-iOS-mobile-app-testing %}
[2]: https://jekyllrb.com/docs/liquid/