---
author: AA
category:
- Three
cover: https://images.unsplash.com/photo-1472377723522-4768db9c41ce?ixid=M3w2NzEyNTB8MHwxfHNlYXJjaHwxfHxidWlsZGluZyUyMGNvbW1pY3xlbnwwfDB8MXx8MTczMDU1Mjc1N3ww&ixlib=rb-4.0.3&w=1960&h=1102&auto=format&fit=crop&q=60
coverAlt: white and brown concrete house near trees at daytime
description: Learn how to use images in Astro.
pubDate: 2024-02-01 00:00:00
slug: assets-guides-images
tags:
- documentation
- CSharp
- Stylus
title: Images 
---

Astro provides several ways for you to use images on your site, whether they are stored locally inside your project, linked to from an external URL, or managed in a CMS or CDN!




See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.

Where to store images
---------------------

[Section titled Where to store images](#where-to-store-images)
### `src/` vs `public/`

[Section titled src/ vs public/](#src-vs-public)
We recommend that local images are kept in `src/` when possible so that Astro can transform, optimize and bundle them. Files in the `/public` directory are always served or copied into the build folder as\-is, with no processing.


Your local images stored in `src/` can be used by all files in your project: `.astro`, `.md`, `.mdx`, `.mdoc`, and other UI frameworks. Images can be stored in any folder, including alongside your content.


Store your images in the `public/` folder if you want to avoid any processing or to have a direct public link to them.


### Remote images

[Section titled Remote images](#remote-images)
You can also choose to store your images remotely, in a [content management system (CMS)](/en/guides/cms/) or [digital asset management (DAM)](/en/guides/media/) platform.
Astro can fetch your data remotely using APIs or display images from their full URL path.


For extra protection when dealing with external sources, Astro’s image components and helper function will only process (e.g. optimize, transform) images from [authorized image sources specified in your configuration](#authorizing-remote-images). Remote images from other sources will be displayed with no processing.


Images in `.astro` files
------------------------

[Section titled Images in .astro files](#images-in-astro-files)
In `.astro` files, a local image must be imported from its relative path. This import provides the `src` value for your image.


Remote and `public/` images do not require importing, and instead require a URL (full, or relative path on your site) for `src`.


Import and use Astro’s native [`<Image />`](#display-optimized-images-with-the-image--component) and [`<Picture />`](#create-responsive-images-with-the-picture--component) components for optimized images. Astro syntax also supports [writing an HTML `<img>` tag directly](#display-unprocessed-images-with-the-html-img-tag), which skips image processing.




src/pages/blog/my\-images.astro


```
---import { Image } from 'astro:assets';import localBirdImage from '../../images/subfolder/localBirdImage.png';---<Image src={localBirdImage} alt="A bird sitting on a nest of eggs." /><Image src="/images/bird-in-public-folder.jpg" alt="A bird." width="50" height="50" /><Image src="https://example.com/remote-bird.jpg" alt="A bird." width="50" height="50" />
<img src={localBirdImage.src} alt="A bird sitting on a nest of eggs."><img src="/images/bird-in-public-folder.jpg" alt="A bird."><img src="https://example.com/remote-bird.jpg" alt="A bird.">
```



See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.




**Related recipe:**
[Dynamically import images](/en/recipes/dynamically-importing-images/) 


### Display optimized images with the `<Image />` component

[Section titled Display optimized images with the \&lt;Image /\&gt; component](#display-optimized-images-with-the-image--component)
Use the built\-in `<Image />` Astro component to display optimized versions of:


* your local images located within the `src/` folder
* [configured remote images](#authorizing-remote-images) from authorized sources


`<Image />` can transform a local or authorized remote image’s dimensions, file type, and quality for control over your displayed image. The resulting `<img>` tag includes `alt`, `loading`, and `decoding` attributes and infers image dimensions to avoid Cumulative Layout Shift (CLS).


What is Cumulative Layout Shift?

[Cumulative Layout Shift (CLS)](https://web.dev/cls/) is a Core Web Vital metric for measuring how much content shifted on your page during loading. The `<Image />` component optimizes for CLS by automatically setting the correct `width` and `height` for your images.




src/components/MyComponent.astro


```
---// import the Image component and the imageimport { Image } from 'astro:assets';import myImage from '../assets/my_image.png'; // Image is 1600x900---
<!-- `alt` is mandatory on the Image component --><Image src={myImage} alt="A description of my image." />
```






```
<!-- Output --><!-- Image is optimized, proper attributes are enforced --><img  src="/_astro/my_image.hash.webp"  width="1600"  height="900"  decoding="async"  loading="lazy"  alt="A description of my image."/>
```

The `<Image />` component accepts [several component properties](/en/reference/modules/astro-assets/#image-properties) as well as any attributes accepted by the HTML `<img>` tag.


The following example provides a `class` to the image component which will apply to the final `<img>` element.




src/pages/index.astro


```
---import { Image } from 'astro:assets';import myImage from '../assets/my_image.png';---
<!-- `alt` is mandatory on the Image component --><Image src={myImage} alt="" class="my-class" />
```






```
<!-- Output --><img  src="/_astro/my_image.hash.webp"  width="1600"  height="900"  decoding="async"  loading="lazy"  class="my-class"  alt=""/>
```

Tip

You can also use the `<Image />` component for images in the `public/` folder, or remote images not specifically configured in your project, even though these images will not be optimized or processed. The resulting image will be the same as using the HTML `<img>`.

However, using the image component for all images provides a consistent authoring experience and prevents Cumulative Layout Shift (CLS) even for your unoptimized images.


### Create responsive images with the `<Picture />` component

[Section titled Create responsive images with the \&lt;Picture /\&gt; component](#create-responsive-images-with-the-picture--component)

**Added in:**
`astro@3.3.0`



Use the built\-in `<Picture />` Astro component to display a responsive image with multiple formats and/or sizes.




src/pages/index.astro


```
---import { Picture } from 'astro:assets';import myImage from '../assets/my_image.png'; // Image is 1600x900---
<!-- `alt` is mandatory on the Picture component --><Picture src={myImage} formats={['avif', 'webp']} alt="A description of my image." />
```






```
<!-- Output --><picture>  <source srcset="/_astro/my_image.hash.avif" type="image/avif" />  <source srcset="/_astro/my_image.hash.webp" type="image/webp" />  <img    src="/_astro/my_image.hash.png"    width="1600"    height="900"    decoding="async"    loading="lazy"    alt="A description of my image."  /></picture>
```



See details about [the `<Picture />` component properties](/en/reference/modules/astro-assets/#picture-properties) in the `astro:assets` reference.

### Display unprocessed images with the HTML `<img>` tag

[Section titled Display unprocessed images with the HTML \&lt;img\&gt; tag](#display-unprocessed-images-with-the-html-img-tag)
The [Astro template syntax](/en/basics/astro-syntax/) also supports writing an `<img>` tag directly, with full control over its final output. These images will not be processed and optimized. It accepts all HTML `<img>` tag properties, and the only required property is `src`.


Local images must be imported from the relative path from the existing `.astro` file, or you can configure and use an [import alias](/en/guides/imports/#aliases). Then, you can access the image’s `src` and other properties to use in the `<img>` tag.


Imported image assets match the following signature:







```
interface ImageMetadata {  src: string;  width: number;  height: number;  format: string;}
```

The following example uses the image’s own `height` and `width` properties to avoid Cumulative Layout Shift (CLS) and improve Core Web Vitals:




src/pages/posts/post\-1\.astro


```
---// import local imagesimport myDog from '../../images/pets/local-dog.jpg';---// access the image properties<img src={myDog.src} width={myDog.width} height={myDog.height} alt="A barking dog." />
```

#### Images in `public/`

[Section titled Images in public/](#images-in-public)
For images located within `public/` use the image’s file path relative to the public folder as the `src` value:







```
<img src="/images/public-cat.jpg" alt="A sleeping cat." >
```

#### Remote images

[Section titled Remote images](#remote-images-1)
For remote images, use the image’s full URL as the `src` value:







```
<img src="https://example.com/remote-cat.jpg" alt="A sleeping cat." >
```

### Choosing `<Image />` vs `<img>`

[Section titled Choosing \&lt;Image /\&gt; vs \&lt;img\&gt;](#choosing-image--vs-img)
The `<Image />` component optimizes your image and infers width and height (for images it can process) based on the original aspect ratio to avoid CLS. It is the preferred way to use images in `.astro` files whenever possible.


Use the HTML `<img>` element when you cannot use the `<Image />` component, for example:


* for unsupported image formats
* when you do not want your image optimized by Astro
* to access and change the `src` attribute dynamically client\-side


### Setting Default Values

[Section titled Setting Default Values](#setting-default-values)
Currently, there is no way to specify default values for all `<Image />` or `<Picture/>` components. Required attributes should be set on each individual component.


As an alternative, you can wrap these components in another Astro component for reuse. For example, you could create a component for your blog post images that receives attributes as props and applies consistent styles to each image:




src/components/BlogPostImage.astro


```
---import { Image } from 'astro:assets';
const { src, ...attrs } = Astro.props;---<Image src={src} {...attrs} />
<style>  img {    margin-block: 2.5rem;    border-radius: 0.75rem;  }</style>
```

### Authorizing remote images

[Section titled Authorizing remote images](#authorizing-remote-images)
You can configure lists of authorized image source URL domains and patterns for image optimization using [`image.domains`](/en/reference/configuration-reference/#imagedomains) and [`image.remotePatterns`](/en/reference/configuration-reference/#imageremotepatterns). This configuration is an extra layer of safety to protect your site when showing images from an external source.


Remote images from other sources will not be optimized, but using the `<Image />` component for these images will prevent Cumulative Layout Shift (CLS).


For example, the following configuration will only allow remote images from `astro.build` to be optimized:




astro.config.mjs


```
export default defineConfig({  image: {    domains: ["astro.build"],  }});
```

The following configuration will only allow remote images from HTTPS hosts:




astro.config.mjs


```
export default defineConfig({  image: {    remotePatterns: [{ protocol: "https" }],  }});
```

Using Images from a CMS or CDN
------------------------------

[Section titled Using Images from a CMS or CDN](#using-images-from-a-cms-or-cdn)
Image CDNs work with [all Astro image options](#images-in-astro-files). Use an image’s full URL as the `src` attribute in the `<Image />` component, an `<img>` tag, or in Markdown notation. For image optimization with remote images, also [configure your authorized domains or URL patterns](#authorizing-remote-images).


Alternatively, the CDN may provide its own SDKs to more easily integrate in an Astro project. For example, Cloudinary supports an [Astro SDK](https://astro.cloudinary.dev/) which allows you to easily drop in images with their `CldImage` component or a [Node.js SDK](https://cloudinary.com/documentation/node_integration) that can generate URLs to use with an `<img>` tag in a Node.js environment.




See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.

Images in Markdown files
------------------------

[Section titled Images in Markdown files](#images-in-markdown-files)
Use standard Markdown `![alt](src)` syntax in your `.md` files. This syntax works with Astro’s [Image Service API](/en/reference/image-service-reference/) to optimize your local images stored in `src/`. Remote images and images stored in the `public/` folder are not optimized.




src/pages/post\-1\.md


```
# My Markdown Page
<!-- Local image stored in src/assets/ --><!-- Use a relative file path or import alias -->![A starry night sky.](../assets/stars.png)
<!-- Image stored in public/images/ --><!-- Use the file path relative to public/ -->![A starry night sky.](/images/stars.png)
<!-- Remote image on another server --><!-- Use the full URL of the image -->![Astro](https://example.com/images/remote-image.png)
```

The `<img>` tag is not supported for local images, and the `<Image />` and `<Picture />` components are unavailable in `.md` files.


If you require more control over your image attributes, we recommend using [Astro’s MDX integration](/en/guides/integrations-guide/mdx/) to add support for`.mdx` file format. MDX allows adding components to Markdown and there are additional [image options available in MDX](#images-in-mdx-files).


Images in MDX files
-------------------

[Section titled Images in MDX files](#images-in-mdx-files)
You can use Astro’s `<Image />` and `<Picture />` components in your `.mdx` files by importing both the component and your image. Use them just as they are [used in `.astro` files](#images-in-astro-files). The JSX `<img />` tag is also supported for unprocessed images and [uses the same image import as the HTML `<img>` tag](#display-unprocessed-images-with-the-html-img-tag).


Additionally, there is support for [standard Markdown `![alt](src)` syntax](#images-in-markdown-files) with no import required.




src/pages/post\-1\.mdx


```
---title: My Page title---import { Image } from 'astro:assets';import rocket from '../assets/rocket.png';
# My MDX Page
// Local image stored in the the same folder![Houston in the wild](houston.png)
// Local image stored in src/assets/<Image src={rocket} alt="A rocketship in space." /><img src={rocket.src} alt="A rocketship in space." />![A rocketship in space](../assets/rocket.png)
// Image stored in public/images/<Image src="/images/stars.png" alt="A starry night sky." /><img src="/images/stars.png" alt="A starry night sky." />![A starry night sky.](/images/stars.png)
// Remote image on another server<Image src="https://example.com/images/remote-image.png" /><img src="https://example.com/images/remote-image.png" />![Astro](https://example.com/images/remote-image.png)
```



See the full API reference for the [`<Image />`](/en/reference/modules/astro-assets/#image-) and [`<Picture />`](/en/reference/modules/astro-assets/#picture-) components.

Images in content collections
-----------------------------

[Section titled Images in content collections](#images-in-content-collections)
Images in content collections will be processed the same way they are in [Markdown](#images-in-markdown-files) and [MDX](#images-in-mdx-files) depending on which file type you are using.


Additionally, you can declare an associated image for a content collections entry, such as a blog post’s cover image, in your frontmatter using its path relative to the current folder:




src/content/blog/my\-post.md


```
---title: "My first blog post"cover: "./firstpostcover.jpeg" # will resolve to "src/content/blog/firstblogcover.jpeg"coverAlt: "A photograph of a sunset behind a mountain range."---
This is a blog post
```

The `image` helper for the content collections schema lets you validate the image metadata using Zod.




src/content/config.ts


```
import { defineCollection, z } from "astro:content";
const blogCollection = defineCollection({  schema: ({ image }) => z.object({    title: z.string(),    cover: image().refine((img) => img.width >= 1080, {      message: "Cover image must be at least 1080 pixels wide!",    }),    coverAlt: z.string(),  }),});
export const collections = {  blog: blogCollection,};
```

The image will be imported and transformed into metadata, allowing you to pass it as a `src` to `<Image/>`, `<img>`, or `getImage()`.


The example below shows a blog index page that renders the cover photo and title of each blog post from the schema above:




src/pages/blog.astro


```
---import { Image } from "astro:assets";import { getCollection } from "astro:content";const allBlogPosts = await getCollection("blog");---
{  allBlogPosts.map((post) => (    <div>      <Image src={post.data.cover} alt={post.data.coverAlt} />      <h2>        <a href={"/blog/" + post.slug}>{post.data.title}</a>      </h2>    </div>  ))}
```

Images in UI framework components
---------------------------------

[Section titled Images in UI framework components](#images-in-ui-framework-components)
The `<Image />` component, like any other Astro component, is unavailable inside UI framework components.


But, you can pass the static content generated by `<Image />` to a framework component inside a `.astro` file as children or using a [named `<slot/>`](/en/guides/framework-components/#can-i-use-astro-components-inside-my-framework-components):




src/components/ImageWrapper.astro


```
---import ReactComponent from './ReactComponent.jsx';import { Image } from 'astro:assets';import stars from '~/stars/docline.png';---
<ReactComponent>  <Image src={stars} alt="A starry night sky." /></ReactComponent>
```

You can also use the framework’s own image syntax to render an image (e.g. `<img />` in JSX, `<img>` in Svelte).


[Local images must first be imported](#display-unprocessed-images-with-the-html-img-tag) to access their image properties such as `src`.




src/components/ReactImage.jsx


```
import stars from "../assets/stars.png";
export default function ReactImage() {  return (    <img src={stars.src} alt="A starry night sky." />  )}
```



src/components/SvelteImage.svelte


```
<script>  import stars from '../assets/stars.png';</script>
<img src={stars.src} alt="A starry night sky." />
```

Generating images with `getImage()`
-----------------------------------

[Section titled Generating images with getImage()](#generating-images-with-getimage)
The `getImage()` function is intended for generating images destined to be used somewhere else than directly in HTML, for example in an [API Route](/en/guides/endpoints/#server-endpoints-api-routes). When you need options that the `<Picture>` and `<Image>` components do not currently support, you can use the `getImage()` function to create your own custom `<Image />` component.




See more in the [`getImage()` reference](/en/reference/modules/astro-assets/#getimage).




**Related recipe:**
[Build a custom image component](/en/recipes/build-custom-img-component/) 


Alt Text
--------

[Section titled Alt Text](#alt-text)
Not all users can see images in the same way, so accessibility is an especially important concern when using images. Use the `alt` attribute to provide [descriptive alt text](https://www.w3.org/WAI/tutorials/images/) for images.


This attribute is required for both the `<Image />` and `<Picture />` components. If no alt text is provided, a helpful error message will be provided reminding you to include the `alt` attribute.


If the image is merely decorative (i.e. doesn’t contribute to the understanding of the page), set `alt=""` so that screen readers know to ignore the image.


Default image service
---------------------

[Section titled Default image service](#default-image-service)
[Sharp](https://github.com/lovell/sharp) is the default image service used for `astro:assets`. You can further configure the image service using the [`image.service`](/en/reference/configuration-reference/#imageservice) option.


Note

When using a [strict package manager](https://pnpm.io/pnpm-vs-npm#npms-flat-tree) like `pnpm`, you may need to manually install Sharp into your project even though it is an Astro dependency:



Terminal window


```
pnpm add sharp
```

### Configure Squoosh

[Section titled Configure Squoosh](#configure-squoosh)
If you would prefer to use [Squoosh](https://github.com/GoogleChromeLabs/squoosh) to transform your images, update your config with the following:




astro.config.mjs


```
import { defineConfig, squooshImageService } from 'astro/config';
export default defineConfig({  image: {    service: squooshImageService(),  },});
```

### Configure no\-op passthrough service

[Section titled Configure no\-op passthrough service](#configure-no-op-passthrough-service)
If your [adapter for `server` or `hybrid` mode](https://astro.build/integrations/?search=&categories%5B%5D=adapters) does not support Astro’s built\-in Squoosh and Sharp image optimization (e.g. Deno, Cloudflare), you can configure a no\-op image service to allow you to use the `<Image />` and `<Picture />` components. Note that Astro does not perform any image transformation and processing in these environments. However, you can still enjoy the other benefits of using `astro:assets`, including no Cumulative Layout Shift (CLS), the enforced `alt` attribute, and a consistent authoring experience.


Configure the `passthroughImageService()` to avoid both Squoosh and Sharp image processing:




astro.config.mjs


```
import { defineConfig, passthroughImageService } from 'astro/config';
export default defineConfig({  image: {    service: passthroughImageService()  }});
```

Community Integrations
----------------------

[Section titled Community Integrations](#community-integrations)
There are several third\-party [community image integrations](https://astro.build/integrations?search=images) for optimizing and working with images in your Astro project.


Learn