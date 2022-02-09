## Welcome to Aidan's Signed Distance Field Study

### Signed Distance Fields

  Lets talk about signed distance fields. Signed Distance fields(SDF) is a tecnique that uses raymarching to take a position as an input, and outputs the distance from that position to the nearest part of a shape. After we gather all the points that we need. Its time to use the raymarching algorithm to render the SDF. By Checking if you hit a point or not, you can determine the color of the pixel. If it hits a point you render the shape. if not then you render it transparent. You can then add a lighting pass, textures or anything else that would be passed through a HLSL Shader. Code for raymarching would look something like this in GLSL. 


```markdown
float depth = start;
for (int i = 0; i < MAX_MARCHING_STEPS; i++) {
    float dist = sceneSDF(eye + depth * viewRayDirection);
    if (dist < EPSILON) {
        // We're inside the scene surface!
        return depth;
    }
    // Move along the view ray
    depth += dist;

    if (depth >= end) {
        // Gone too far; give up
        return end;
    }
}
return end;
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/iXiphos/iXiphos.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
