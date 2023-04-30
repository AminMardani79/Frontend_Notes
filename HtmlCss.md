### Optimization for image

```html
<picture>
  <!-- if window width >= 900px -->
  <source media="(min-width: 900px)" srcset="img-large.jpg">
  
  <!-- if window width >= 600px -->
  <source media="(min-width: 600px)" srcset="img-medium.jpg">

  <!-- default -->
  <img src="img-small.jpg">
</picture>

```
