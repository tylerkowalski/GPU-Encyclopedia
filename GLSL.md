# GLSL 

### Layout Qualifiers
  - Defines where the storage of a variable comes from and user-facing properties in a shader
  - `layout(qualifier1​, qualifier2​ = value, ...) variable definition`
  - `location=x` qualifer in ***vertex shader*** refers to an attribute index *x*. These are defined at pipeline creation with `VkVertexInputAttributeDescription`
  - In the ***fragment shader***, `location=x` qualifer refers to colour attachment *x* defined by `VkRenderingInfo::pColorAttachments[x]` used in RenderPass creation
  - [For more information](https://www.khronos.org/opengl/wiki/Layout_Qualifier_(GLSL))
