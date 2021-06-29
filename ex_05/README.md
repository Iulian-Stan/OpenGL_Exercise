# OpenGL Shading Language (GLSL)

## Takeaways

* OpenGL Shading Language (GLSL) is a high-level shading language with a syntax based on the C programming language. It gives developers more direct control of the graphics pipeline without having to use ARB assembly language or hardware-specific languages.

* In order to use GLSL a program object needs to be created. The process consists of the following steps:
  1. Create program object using `glCreateProgram`
  2. Attach shaders (shader cretion is described bellow)
  3. Link the program object using `glLinkProgram`
  4. Validate the program object using `glValidateProgram`
  5. Install program object as part of current rendering state using `glUseProgram`

**Note:** after step 3 and 4 it is recomended to verify execution status using `glGetProgramiv`. In case of an error, corresponding error message can be obtained using `glGetProgramInfoLog`.

* Shader object creation is similar to program object and has the following steps:
  1. Create shader object using `glCreateShader`
  2. Set shader object source code using `glShaderSource`, the source code is loaded as char array read from a text file
  3. Compile dhader object using `glCompileShader`
  4. Attach shader object to the program using `glAttachShader`

**Note:** shader object compilation status can be verified using `glGetShaderiv`. In case of an error, corresponding error message can be obtained using `glGetShaderInfoLog`.

* This example includes 2 shaders - vertex shader and fragment shader.
  1. Vertex shader is defined in `shader.vs` file and handles the processing of individual vertices
  ```
  #version 330

  layout (location = 0) in vec3 Position;
  layout (location = 1) in vec3 inColor;

  out vec4 Color;

  void main()

  {
    gl_Position = vec4(Position, 1.0);
    Color = vec4(inColor, 1.0);
  }
  ```
  This shader takes in 2 variables corresponding to the 2 vetrex attribute buffers (position and color). It sets a predefined variable `gl_Position` by appending an additional `1.0` value to the position vector as the engine expects a 4 dimensional array. Similar operation is performed on the color and the resulting vector is assigned to an output variable `Color` that is passed down the pipe line to the fragment shader.

  2. Fragment shader is defined in `shader.fs` file and will process a Fragment generated by the Rasterization (each pixel covered by a primitive) into a set of colors and a single depth value.
  ```
  #version 330

  in vec4 Color;

  out vec4 FragColor;

  void main()
  {
    FragColor = Color;
  }
  ```