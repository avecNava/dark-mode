# Dark mode

Three ways to enable dark mode in web application.

## 1. Using JS to toggle class

This method uses a switch to manually toggle between light (default) and dark modes. This method uses javascript to switch css classes on the fly. Cookies or local storage can be utilized to remember the last mode selected for a better user experience.

```css
<style>
        
        html {font-size: 100%;}

        body {
            font-weight: 400;
            line-height: 1.75;
            color: #3f0d7d;
        }

        .dark-mode {
            background: #3e3939;
            color: white;
        }

</style>
```

The following js script toggles dark mode class to the body element.

```js
    //this function is called when the user clicks on dark mode
    function switch_mode(){
        const element = document.body;
        element.classList.toggle('dark-mode');
    }

```

Please note that, the mode will remain only in the current page and unless refreshed. Once the user refreshes the page or navigates to other page, the default (light mode) will be activated.

Hence, we can improve the user experience by saving the selected mode to local storage (or cookies, sesssions) and loading the last mode selected on page load.

```js
<script>
    
    //runs on every page load, checks local storage for last mode and applies dark mode if found
    document.body.classList.toggle('dark-mode', localStorage.getItem('darkmode') === 'true');

    //the function must be called when dark mode is clicked by the user
    function switch_mode(){
        
        const dark = localStorage.getItem('darkmode') === 'true';
        localStorage.setItem('darkmode', !dark);

        const element = document.body;
        element.classList.toggle('dark-mode', !dark);

        }
</script>
```

## 2. No JS. Only CSS media query (using theme ```prefers-color-scheme```)

You can use ```prefers-dark-scheme``` media query to detect if the OS and/or browser has some preferred theme (light or dark).

In Windows 10, the preference can be set under via Windows settings, Personalization, Color, Choose your color (light, dark, custom).

Similarly, browsers also let you choose color mode. In Edge, Settings, Appearance, Default Theme (Light, Dark, System default)

The following CSS defines two variables ```background``` and ```text-color```. The media query detects dark mode (if any) and updates the variables for dark mode. Afterwards, the variables are utilized inside body tag eg, ```color:var(--text-color)``)

```css
<style>
        :root {
            --background: #fff;
            --text-color: #0f1031;
        }

        @media (prefers-color-scheme: dark) {
            :root {
                --background: #0f1031;
                --text-color: #fff;
            }
        }

        body, html {
            height: 100%;
            color: var(--text-color);
            background: var(--background);
        }
</style>
```

## 3. [Using separate styles for light and dark mode](1)

The previous ways used internal CSS to switch between light and dark modes. Here, we will see how external css can be utilized for a cleaner approach.

We can check if the current browser supports dark mode by checking if the media query ```prefers-color-scheme``` matches at all. We then load different css files for each mode.

- ```style.css``` default rule
- ```dark.css``` dark mode specific rule
- ```light.css``` light mode specific rule

```js
//checking if browser supports dark mode
if (window.matchMedia('(prefers-color-scheme)').media !== 'not all') {
  console.log('Dark mode is supported');
}
```

We can include the code to load different CSS according to the mode.

```js

<script>
  // If `prefers-color-scheme` is not supported, fall back to light mode.
  if (window.matchMedia('(prefers-color-scheme: dark)').media === 'not all') {
    document.documentElement.style.display = 'none';
    document.head.insertAdjacentHTML(
        'beforeend',
        '<link rel="stylesheet" href="/light.css" onload="document.documentElement.style.display = \'\'">'
    );
  }
</script>

<link rel="stylesheet" href="/dark.css" media="(prefers-color-scheme: dark)">
<link rel="stylesheet" href="/light.css" media="(prefers-color-scheme: light)">
<!-- The main stylesheet -->
<link rel="stylesheet" href="/style.css">
```

### Reference

1: <https://web.dev/prefers-color-scheme>
