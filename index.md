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

While this does the raymarching code. It does none of the lighting passes or texture passes. We go through and do that ourselves. Using some Signed Distance code for a fractal, we can render a fractal. Our Blueprint looks something like this, Just taking in the important pieces we need and passing them into a custom function that handles the raymarching, and the SDF math.

![Blueprint](/docs/assets/Blueprint.png)

Fractal Code
```
float mandelbulbSDF( float3 p, float power ) {
	float3 w = p;
    float m = dot(w,w);
	float dz = 1.0;

	for( int i=0; i<3; i++ )
    {
        dz = power*pow(sqrt(m), power - 1.0 )*dz + 1.0;

        float r = length(w);
        float b = power*acos( w.y/r);
        float a = power*atan2( w.x, w.z );
        w = p + pow(r,power) * float3( sin(b)*sin(a), cos(b), sin(b)*cos(a) );

        m = dot(w,w);
		if( m > 256.0 )
            break;
    }

    return 0.25*log(m)*sqrt(m)/dz;
}


float sceneSDF( float3 pos ) {
 	return mandelbulbSDF( pos, 7.0 );
}
```
So using these pieces of code we can render a fractal.


![Fractal](/docs/assets/Fractal.png)
![Fractal](/docs/assets/Fractal2.png)

This is a very small exploration of Signed Distance Functions and raymarching in Unreal. Hope you enjoy!
