add_library(videocommon
  AbstractFramebuffer.cpp
  AbstractStagingTexture.cpp
  AbstractTexture.cpp
  AsyncRequests.cpp
  AsyncShaderCompiler.cpp
  BoundingBox.cpp
  BPFunctions.cpp
  BPMemory.cpp
  BPStructs.cpp
  CPMemory.cpp
  CommandProcessor.cpp
  Debugger.cpp
  DriverDetails.cpp
  Fifo.cpp
  FPSCounter.cpp
  FramebufferManagerBase.cpp
  GeometryShaderGen.cpp
  GeometryShaderManager.cpp
  HiresTextures.cpp
  HiresTextures_DDSLoader.cpp
  ImageWrite.cpp
  IndexGenerator.cpp
  LightingShaderGen.cpp
  OnScreenDisplay.cpp
  OpcodeDecoding.cpp
  PerfQueryBase.cpp
  PixelEngine.cpp
  PixelShaderGen.cpp
  PixelShaderManager.cpp
  PostProcessing.cpp
  RenderBase.cpp
  RenderState.cpp
  ShaderCache.cpp
  ShaderGenCommon.cpp
  Statistics.cpp
  TextureCacheBase.cpp
  TextureConfig.cpp
  TextureConversionShader.cpp
  TextureConverterShaderGen.cpp
  TextureDecoder_Common.cpp
  VertexLoader.cpp
  VertexLoaderBase.cpp
  VertexLoaderManager.cpp
  VertexLoader_Color.cpp
  VertexLoader_Normal.cpp
  VertexLoader_Position.cpp
  VertexLoader_TextCoord.cpp
  VertexManagerBase.cpp
  VertexShaderGen.cpp
  VertexShaderManager.cpp
  VideoBackendBase.cpp
  VideoConfig.cpp
  VideoState.cpp
  XFMemory.cpp
  XFStructs.cpp
)

target_link_libraries(videocommon
PUBLIC
  core
PRIVATE
  png
  xxhash
)

if(NOT ANDROID)
  target_sources(videocommon PRIVATE
    UberShaderCommon.cpp
    UberShaderPixel.cpp
    UberShaderVertex.cpp
  )
endif()

if(_M_X86)
  target_sources(videocommon PRIVATE
    TextureDecoder_x64.cpp
    VertexLoaderX64.cpp
  )
elseif(_M_ARM_64)
  target_sources(videocommon PRIVATE
    VertexLoaderARM64.cpp
    TextureDecoder_Generic.cpp
  )
else()
  target_sources(videocommon PRIVATE
    TextureDecoder_Generic.cpp
  )
endif()

if(FFmpeg_FOUND)
  target_sources(videocommon PRIVATE AVIDump.cpp)
  target_link_libraries(videocommon PRIVATE
    FFmpeg::avcodec
    FFmpeg::avformat
    FFmpeg::avutil
    FFmpeg::swscale
  )
endif()
