# GLSL 

**Layout Qualifiers**
  - Defines where the storage of a variable comes from and user-facing properties in a shader
  - `layout(qualifier1​, qualifier2​ = value, ...) variable definition`
  - `location=x` qualifer in ***vertex shader*** refers to an attribute index *x*. These are defined at pipeline creation with `VkVertexInputAttributeDescription`
  - [For more information](https://www.khronos.org/opengl/wiki/Layout_Qualifier_(GLSL))
