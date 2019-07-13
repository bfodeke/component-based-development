# Components Variations

Component variations are new instances of a component with small or big changes. Variations are great ways to use an existing component and extend it to change its look and/or behavior. [More on components variations here](http://bradfrost.com/blog/post/pattern-variations/).

Based on our prototype, we have at least two different buttons. One with red outline and one with white outline. Text color and background color on hover is also different between the two. We will write the required styles for each button, but we will rely on Pattern Lab to create variations for us. This is a huge advantage of using tools like Pattern Lab because it does all the heavy lifting for us. Without Pattern Lab we would need to create a new version of the button component which would require we duplicate a lot of the code we already wrote, but Pattern Lab takes the code we wrote in the previous exercise and extends it for the new variation. Let's take a look at how this works.

1. Inside the **button** directory create a new file called **button~primary.yml**. \(Notice the tilde \("`~`"\) in the file name\).
2. Add the following code to `button~primary.yml`:

```yaml
button:
  modifier: button--primary
  text: "Primary button"
  url: "#"
```

## How Pattern Lab handles variations

* Pattern lab use [pseudo patterns](https://patternlab.io/docs/pattern-pseudo-patterns.html) to create pattern variations. This is accomplished by creating a new data file \(**.yml**\), which includes the name of the pattern we want to create a variation for, as well as the variation name.  For example, when creating a variation named **primary** for the button pattern, our new data file would be `button~primary.yml`.
* You may have noticed the file name we just created uses a tilde symbol \(`~`\) between the words `button` and `primary`. This tells Pattern Lab that this will be a variation of the button pattern called "primary".
* The value for **modifier** is what we use as the CSS modifier class \(i.e. **button--primary**\).
* If we wanted another variation for a large button for example, we would create a new `.yml` file with the name `button~large.yml`, and the value for **modifier** would be `button--large`.

## Updating the button styles

Now that we have a new button variation let's add css styles to it.

* Edit `button.scss` so the styles look like this :

  ```css
  // Import site utilities
  @import '../../00-global/utils/init';

  .button {
    background-color: $color-black;
    border: 2px solid $color-tundora;
    border-radius: 44px;
    color: $color-white;
    cursor: pointer;
    display: inline-block;
    font-size: 14px;
    font-weight: 600;
    padding: 14px 40px;
    text-decoration: none;
    transition: border 0.125s ease, background-color 0.125s ease;
    white-space: nowrap;

    &:hover,
    &:focus {
      border-color: $color-white;
    }

    // Primary button styles begin here.
    &.button--primary {
      border-color: $color-bright-red;

      &:hover,
      &:focus {
        background: $color-bright-red;
      }
    }
  }
  ```

Styles for `.button--primary` start on line **20** above. These styles will apply when both, `button` and `button--primary` classes are present \(i.e. `<a class="button button--primary">My button</a>`\), and when they do we are changing background and border color to red. Before we can see the new variation in action, we need to update the pattern's twig template so it's aware when a variation is needed.

* Now let's edit the twig file to look like this:

```php
{% if button.url %}
  <a href="{{ button.url }}"
    class="button{% if button.modifier %} {{ button.modifier|lower }}{% endif %}">
    {{ button.text }}
  </a>
{% else %}
  <button class="button{% if button.modifier %} {{ button.modifier|lower }}{% endif %}">
    {{ button.text }}
  </button>
{% endif %}
```

* First we check whether the **modifier** key has a value, and if it does we pass the value to the button \(i.e. `{% if button.modifier %} {{ button.modifier|lower }}{% endif %}`\).
* Note: we're using the `|lower` twig filter to ensure that the modifier text that's passed in will be lowercase. 

## Compiling the code

Now that we have written all the necessary code to build the button' variations, it's time to see the component in the style-guide. Let's compile our project first.

* In your command line, navigate to `/themes/custom/nitflex_dev_theme`.
* Run this command:

```bash
lando npm run build && lando php patternlab/core/console --generate
```

**What does this command do?**

_The command above runs all gulp tasks found inside the gulp-tasks directory in the theme. Keep in mind, we are using the word **lando** because our local environment was built with lando. Typically the build command would be **npm run build**._

## Viewing the component

* Open your Drupal site and point to the URL below:

  [http://nitflex.lndo.site/themes/custom/nitflex\_dev\_theme/dist/style-guide/?p=viewall-patterns-button](http://nitflex.lndo.site/themes/custom/nitflex_dev_theme/dist/style-guide/?p=viewall-patterns-button)

Under the Components category you should see the new Button component.

