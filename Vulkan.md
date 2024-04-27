# Vulkan
### Vertex Shader
  - `VkVertexInputBindingDescription` tells the driver how to pass sections of the vertex buffer to interface with each vertex shader invocation (e.g inputRate, stride, etc)
  - `VkVertexInputAttributeDescription` describes how each vertex shader invocation should handle the section of the vertex buffer interfacing with a specific invocation (e.g location id per attribute, offsets, etc)
    
*Both the above are required structures at pipeline creation*
  - Vertex buffers for a graphics pipeline are bound at command buffer recording time with the command `vkCmdBindVertexBuffers(...)`

### Fragment Shader
  - `layout(location = x)` output variable is associated *only* with colour attachments, defined in `VkRenderingInfo::pColorAttachments[x]`. Dynamic rendering uses a similar structure except it is set at command buffer recording as opposed to at pipeline creation (pipeline creation needs a RenderPass object if not using dynamic rendering)

### FrameBuffer
  - Container of all the `VkImageView`s which **represent** the attachments [defines which `VkImageView` is to be which attachment]
  - When `vkCmdBeginRenderPass` is called at the start of command buffer recording, `vkRenderPassBeginInfo` requires both the FrameBuffers and RenderPass objects [the `VkFramebuffer` in conjunction with the `VkRenderPass` define the render target]

### RenderPass
 - *RenderPass*: a chunk of metadata describing the outputs of a larger set of draw calls, similar to a C++ declaration for which you provide the implementation later; it defines which attachments will be drawn into.

  - *Subpass*: rendering operations that depend on the contents of framebuffers in previous passes (e.g a sequence of post-processing effects). Using subpasses instead of multiple renderpasses allows for operations to be re-ordered and for memory bandwidth to be *potentially* conserved

  - *Subpass dependencies*: describes the execution order between subpasses, forming a dependency DAG (logically equiv. to pipeline barrier between 2 subpasses). Otherwise, subpasses are asynchronous.

### Buffers, Images, & Memory
 - `VkDeviceMemory` represents actual memory allocated on the GPU
 - `VkBuffer` is a handle to a location within `VkDeviceMemory` and metadata (linear array of data)
 - `VkImage` is also a handle to a location with `VkDeviceMemory` and metadata(structured image data)
 - `VkImageView` defines which part of a `VkImage` to use

### Misc. Definitions
  - *Attachment*: *only* a description of needed frame image outputs and temporaries (no underlying memory) [*description* of a resource that is a result of rendering]

 
