# GLSL 
[Click here to go back to main](README.md)
### Layout Qualifiers
  - Defines where the storage of a variable comes from and user-facing properties in a shader
  - `layout(qualifier1​, qualifier2​ = value, ...) variable definition`
  - `location=x` qualifer in ***vertex shader*** input refers to an attribute index *x*. These are defined at pipeline creation with `VkVertexInputAttributeDescription`
  - In the ***fragment shader*** output, `location=x` qualifer refers to colour attachment *x* defined by `VkRenderingInfo::pColorAttachments[x]` used in RenderPass creation
  - [For more information](https://www.khronos.org/opengl/wiki/Layout_Qualifier_(GLSL))

### Misc. Definitions
  - *Vertex Attributes (Aka. Attributes)*: per vertex read-only data availble to the vertex shader in the form of variables
