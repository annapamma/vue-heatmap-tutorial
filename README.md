# Getting started

Start with a basic html page.

Create this `index.html` file and open in your browser.

`index.html`
```html
<div id="app">
    <h1>Hello World</h1>
</div>
```

You should see this:
![Webpage with white background and "Hello World" in black letters](step-1.png)

## Mounting the Vue App

Add JavaScript and load the Vue library from a CDN with `<script>` tags.

```html
<div id="app">
    <h1>{{ message }}</h1>
</div>

<script src="https://unpkg.com/vue" ></script>

<script>
  const app = new Vue({
    el: '#app',
  });
</script>
```

The second script tag contains your newly mounted Vue app. This bit of code tells Vue to mount your app on the element with the id "app" (`#app`).

## Changing the message

Add a data object to the Vue instance. Add a data property with the key `message` and enter whatever message you want to replace Hello World.

```html
<script src="https://unpkg.com/vue" ></script>

<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello from Vue!'
    },
  });
</script>
```

Bind the message data to your html by replacing the 'Hello World' text with `{{ message }}`.
```html
<div id="app">
    <h1>{{ message }}</h1>
</div>
```

You should now see your text where 'Hello World' used to be:
![Webpage with white background and "Hello from Vue" in black letters](step-2.png)

## Optional: Exploring Data Binding

Open the JavaScript console, and enter `app` to see your instance of Vue. Enter `app.message` to see your custom message. 
![Webpage with white background and "Hello from Vue" in black letters and JavaScript console with app and app.message that reads "Hello from Vue"](step-3.png)

In your console, reset `app.message` to a new message. The text on your webpage should change to the new message.
![Webpage with white background and "How are vue doing?" in black letters and JavaScript console with app and app.message that reads "How are Vue doing?"](step-4.png)

Congratulations, you just witnessed Vue reactivity in action! As the data in your Vue instance updates, so does your user interface.

## Getting Started with ApexCharts

Install the ApexCharts libraty by adding the following script tags to `index.html`:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/apexcharts/3.6.12/apexcharts.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-apexcharts"></script>
```

Register an apxechart component with Vue before mounting your Vue app. This will give you app access to all of the data visualizations pre-built by ApexCharts.

```html
<script>
  // Register apexchart component
  Vue.component('apexchart', VueApexCharts);

  const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello from Vue!'
    },
  });
</script>
```

## Adding a heatmap with HTML and JavaScript

Now that you have registered your component, you can use it in the element where you mounted your Vue app (`#app`). Add an apexcharts component to your html.

```html
<div id="app">
    <h1>{{ message }}</h1>
    <apexchart type="heatmap" :options="options" :series="series"/>
</div>
```

An apexchart component has at least 3 properties: `type`, `options`, and `series`. The `type` property lets you specify what kind of visualization you would like, such as a heatmap, bar graph, etc. Do you see the colon (`:`) before `options` and `series`? That means these properties will be assigned with JavaScript. 

Let's get to the JavaScript then.

Add `options` and `series` properties ot your Vue object. For now, leave options empty, and add some sample heatmap data to `series`.

```html
<script>
  Vue.component('apexchart', VueApexCharts);

  const app = new Vue({
    el: '#app',
    data: {
      // Change to heatmap title
      message: 'My Super Awesome Heatmap',
      // Add heatmap options
      options: {},
      // Add sample heatmap data
      series: [{
        name: 'Sample Series',
        data: [10, 20, 30, 40, 50],
      }]
    },
  });
</script>
```

As you can see here, apexcharts heatmap data should be an array of objects. The entire heatmap is an array, and each track is an object with a `name` and a `data` property.

Save your index.html file and refresh the page. 

You should see something like this:
![Webpage with white background and "Hello from Vue!" in black letters and a chart with the numbers 10, 20, 30, 40, 50 in increasingly darker shades of blue"](step-5.png)

Notice that the colors get progressively darker as the values increase. Looks like the heatmap is working! Let's add a second track to test your new skills.

Add a second track object to the series array with a `name` and `data` values of your choosing.
```js
series: [{
        name: 'Sample Series',
        data: [10, 20, 30, 40, 50]
      },
      {
        name: 'Sample Series 2',
        data: [50, 10, 30, 20, 40]
      },]
```

Save and refresh `index.html`. You should see:
![Webpage with white background and "Hello from Vue!" in black letters and a heatmap with two tracks of different colors"](step-5.png)

Looking good! At the end of the day a heatmap is just data and colors. Now that we've got some data, let's work on our colors.

## Customizing the heatmap
ApexCharts offers a lot of great options for customizing your heatmap to suit your data. Let's start by customizing colors.

To your options data property, add a `plotOptions` with a nested `heatmap` object. This heatmap object is where you will place your customizations for the heatmap display.

```js
      options: {
        // add nested plotOptions object
        plotOptions: {
          // add nested heatmap object
          heatmap: {   
            // customizations will go here
          },
        }
      },
```

Let's start by adding a custom color scale. To the `heatmap` object, add another nested object with the `key` colorScale. Give this `colorScale` object a `ranges` object like this:
```js
options: {
  plotOptions: {
      heatmap: { 
        colorScale: {
          // custom color range
          ranges: [
          {
            from: 0,
            to: 29,
            color: '#00A100',
            name: 'low',
          },
          {
            from: 30,
            to: 50,
            color: '#FF0000',
            name: 'high',
          },
         ],
        },
    },
```

Save and refresh `index.html`. You should see:
![Webpage with white background and "Hello from Vue!" in black letters and a heatmap with two data tracks. All tracks with values from 0 to 29 are colored in a green gradient, and from 30 to 50 in a red gradient."](step-5.png)

You just classified your data into `low` and `high`, and this immediately gives you a nice sense of the patterns in your data. As you can see, ApexCharts accepts `colorScale` as an array of objects. The `from` and `to` keys assign the lowest and highest values in that color range, `color` is just that, and `name` is an optional property. Can you add a `middle` range and color to your scale?

Here's one way you could do that:
```js
options: {
  plotOptions: {
      heatmap: { 
        colorScale: {
          // custom color range
          ranges: [
          {
            from: 0,
            to: 19,
            color: 'green',
            name: 'low',
          },
          {
            from: 20,
            to: 39,
            color: 'yellow',
            name: 'low',
          },
          {
            from: 40,
            to: 50,
            color: 'red',
            name: 'high',
          },
         ],
        },
    },
```

Result:
![Webpage with white background and "Hello from Vue!" in black letters and a heatmap with two data tracks. All tracks with values from 0 to 19 are colored in a green gradient, from 20 to 39 in yellow, and from 40 to 50 in a red gradient."](step-5.png)

Congratulations, you have now made and customized a heatmap! This heatmap isn't too meaningful right now though, so let's try using some real data.


