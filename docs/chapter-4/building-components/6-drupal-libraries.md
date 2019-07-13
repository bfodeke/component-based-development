# Drupal Libraries

The CSS styles we've written work great in the style-guide but currently they are not available in Drupal. Although we have compiled each component's styles into individual CSS stylesheets, Drupal is not aware of them. To fix this we need to create libraries for each components which we later will attach to the corresponding component for Drupal to load those styles or Javascript.

In Drupal 8, stylesheets \(CSS\) and JavaScript \(JS\) are loaded through the same system for modules \(code\) and themes, for everything: [asset libraries](https://www.drupal.org/node/2274843).

Drupal uses a high-level principle: assets \(CSS or JS\) are still only loaded if you tell Drupal it should load them. Drupal does not load all assets on every page because it slows down front-end performance.

[Learn more about Drupal libraries.](https://www.drupal.org/docs/8/theming-drupal-8/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-theme)

In the context of the project we are building, we are going to create a library for each individual component we build. Each library will have all the CSS and Javascript \(if any\), the component needs to render as expected.

{% hint style="warning" %}
**IMPORTANT**

Drupal libraries are only intended to work in Drupal. They have no effect in the style-guide. How are we then passing styles and javascript to the components in the style-guide? Good question; we have a gulp task in place which compiles all CSS into a single stylesheet \(`/dist/all/all.css`\). This stylesheet is automatically appended to the style-guide so all styles are available when a component is rendered in the style-guide.

As for Javascript, how are we applying it to the style-guide? Even a better question. You are good! 😄 This unfortunately is a manual step, but not a difficult one. We'll do this shortly.
{% endhint %}

## Structure of a library

In your editor, open `nitflex_dev_theme.libraries.yml` \(located in your theme's root\). You will notice the global library already declared which includes all of the global theme styles that apply to all pages on the site \(i.e. font color, font-size, font-family, line-height, etc.\). The global library looks something like this:

```text
global:
  css:
    base:
      dist/css/global.css: {}
```

1. **Library name**: In our case this name will always be the name of our component to easily identify what a library is for.
2. **Asset**: The asset we want to include in the library. Usually `css` and/or `js`.
3. **Loading ordering**: Ordering category, in this case `base`, is loaded before other categories. Drupal 8 loads stylesheets based on the [SMACSS](https://smacss.com) ordering: `base`, `layout`, `component`, `state`, and `theme`.  All of the components we create will use the `component` ordering.
4. **The path**:  The path to the asset relative to the root of the theme. All assets in the **nitflex\_dev\_theme** theme are compiled into `dist/css` or `dist/js`. This means for each component we will have an asset stylesheet \(i.e. `dist/css/heading.css`\), and if we have a Javascript behavior for the heading component we will also have a JS assets in `dist/js/heading.js`.

There is a lot more to Drupal libraries and we encourage you to learn more about them in the URL above.

**NOTE:** We've created other libraries which you will see in the **nitflex\_dev\_theme.libraries.yml**.

## Creating the Movie Card component's library

1. In your editor open `nitflex_dev_theme.libraries.yml`
2. Add the following code at the bottom of the page:

```yaml
movie-card:
  css:
    component:
      dist/css/movie-card.css: {}
```

Libraries are great because Drupal only loads what we need when we needed to avoid having to load assets that our page or component may never use. This helps with site's performance.

**CLEAR DRUPAL'S CACHE**

Don't forget to clear your caches when adding new libraries to your theme.

* Use the admin menu to flush all caches
* Or run this command:

```bash
lando drush cr
```

## Attaching a library

Now that the Movie Card component's library is ready, we need to make Drupal aware of it so it can use it.

1. In your editor, open `src/_patterns/01-patterns/movie-card/movie-card.twig`
2. Edit the file by adding the following code at the first line in the file:

```text
{{ attach_library('nitflex_dev_theme/movie-card') }}
```

The `attach_library` function takes a path parameter which we are declaring by using the theme's namespace \(the namespace is created by the [Components Libraries module](https://www.drupal.org/project/components)\), then the name of the library we want to attach \(i.e. `nitflex_dev_theme/movie-card`\).

With the code above, we are telling Drupal that whenever we render the Movie Card component, its library should be attached so the styles for the component can be applied.

## Adding Javascript to Pattern Lab

So far we have not written any javascript. The instructions below only apply when a component needs javascript.

As explained before, Drupal libraries have no effect on Pattern Lab. In order for the components to make use of the javascript we've written we need to manually add the script to the style guide.

1. In your text editor, open `nitflex_dev_theme/src/_meta/_01-foot.twig`
2. Scroll to the very bottom of the file where you will see other Javascripts references.
3. A good place to add your newly created script is between the **START** and **END** of **component JS** comments.

Example of a custom script to add to the style-guide:

```text
<script src="../../dist/js/movie-card.js"></script>
```

As you can see adding a new script to the style-guide is nothing more than specifying the script tags with a path to the javascript file. The Compile Gulp tasks automatically compiles all javascript code found in the components into individual files inside `dist/js`.

**Note**: Confirm the right path for your scripts.

## Creating libraries for each component

Now that we know how Drupal Libraries work and how to create them, go ahead and create a new library for each component you have built thus far, then attach each library to the corresponding component.

Ordering libraries alphabetically is helpful to quickly find the right library as your components catalog grows.

## Resources

Mediacurrent's Director of Front-End Development, Zack Hawkins, explains libraries in detail: [https://www.youtube.com/watch?v=V8hnfxSx4Ck](https://www.youtube.com/watch?v=V8hnfxSx4Ck)

