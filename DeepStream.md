> # GStreamer插件
>
>>* GstFileSrc:	-从文件中读取数据：视频数据或图像。 
>>* GstH264Parse:	-解析传入的H264流。对于H265编解码器，请使用H265Parse。
>>* GstRtpH264Pay:-	将H264编码的有效负载转换为RTP数据包（RFC 3984）。
>>* GstUDPSink:	-将UDP数据包发送到网络。与RTP有效负载（GstRtpH264Pay）配对时，它可以实现RTP流。
>>* GstCapsFilter:	-在不修改数据的情况下限制数据格式。
>>* GstV4l2Src:	-从v4l2设备捕获视频。
>>* GstQTMux：	-将流（音频和视频）合并到QuickTime（.mov）文件中。
>>* GstFileSink:	-将传入数据写入本地文件系统中的文件。
>>* GstURIDecodeBin：	-将数据从URI解码到原始媒体中。它选择可以处理给定“ uri”方案的源元素，并将其连接到解码器。


> # DeepStream的gst插件
>
>>* Gst-nvstreammux：	-在发送AI推理之前批处理流。
>>* Gst-nvinfer：	-使用TensorRT运行推理。
>>* Gst-nvvideo4linux2：	-使用硬件加速解码器（NVDEC）解码视频流；使用硬件加速编码器（NVENC）将I420格式的RAW数据编码为H264或H265输出视频流。
>>* Gst-nvvideoconvert：	-执行视频颜色格式转换。Gst-nvdsosd插件之前的第一个Gst-nvvideoconvert插件将流数据从I420转换为RGBA，Gst-nvdsosd插件将Gst-nvdsosd插件将数据从RGBA转换为I420。
>>* Gst-nvdsosd：	-绘制边界框，文本和关注区域（ROI）多边形。
>>* Gst-nvtracker：	-跟踪帧之间的对象。
>>* Gst-nvmultistreamtiler：	-从批处理缓冲区合成2D切片。
>>* Gst-nvv4l2decoder：	-解码视频流。
>>* Gst-Nvv4l2h264enc：	-编码视频流。
>>* Gst-NvArgusCameraSrc：	-提供使用Argus API控制ISP属性的选项。
>>* Gst-nvinferserver：	-使用对应于NGC容器20.03的NVIDIA®Triton Inference Server（以前称为TensorRT Inference Server）1.12.0版对输入数据进行推断。
>>* Gs-tracker：	-踪检测到的对象，并为每个新对象提供唯一的ID
>>* Gst-nvstreamdemux：	-批处理的帧多路分解为单独的缓冲区。它为批处理中的每个帧创建一个单独的Gst缓冲区
>>* Gst-Nvdevarper：	-该插件使摄像机输入分离
>>* Gs-Nvof：	-从dGPU Turing一代和Jetson Xavier一代开始的NVIDIA GPU包含用于计算光流的硬件加速器
>>* Gst-nvofvisual：	-可用于可视化运动矢量数据
>>* Gst-nvsegvisual：	-可显示细分结果。分割基于图像识别，区别在于分类发生在像素级别，而不是图像识别。
>>* Gst-Nvvideo4linux2：	-DeepStream扩展了开源V4L2编解码器插件（此处称为Gst-v4l2），以支持硬件加速的编解码器。
>>* Gst-nvegltransform：	-实现渲染osd输出。这个属性是在makefile文件中设置的。

> # CUDA
>
>> ## 结构体：cudaDeviceProp
```
struct cudaDeviceProp 
{
      char name[256];               // 识别设备的ASCII字符串（比如，"GeForce GTX 940M"）
      size_t totalGlobalMem;        // 全局内存大小
      size_t sharedMemPerBlock;     // 每个block内共享内存的大小
      int regsPerBlock;             // 每个block 32位寄存器的个数
      int warpSize;                 // warp大小
      size_t memPitch;              // 内存中允许的最大间距字节数
      int maxThreadsPerBlock;       // 每个Block中最大的线程数是多少
      int maxThreadsDim[3];         // 一个块中每个维度的最大线程数
      int maxGridSize[3];           // 一个网格的每个维度的块数量
      size_t totalConstMem;         // 可用恒定内存量
      int major;                    // 该设备计算能力的主要修订版号
      int minor;                    // 设备计算能力的小修订版本号
      int clockRate;                // 时钟速率
      size_t textureAlignment;      // 该设备对纹理对齐的要求
      int deviceOverlap;            // 一个布尔值，表示该装置是否能够同时进行cudamemcpy()和内核执行
      int multiProcessorCount;      // 设备上的处理器的数量
      int kernelExecTimeoutEnabled; // 一个布尔值，该值表示在该设备上执行的内核是否有运行时的限制
      int integrated;               //  
      int canMapHostMemory;         // 表示设备是否可以映射到CUDA设备主机内存地址空间的布尔值
      int computeMode;              // 一个值，该值表示该设备的计算模式：默认值，专有的，或禁止的
      int maxTexture1D;             // 一维纹理内存最大值
      int maxTexture2D[2];          // 二维纹理内存最大值
      int maxTexture3D[3];          // 三维纹理内存最大值
      int maxTexture2DArray[3]; // 二维纹理阵列支持的最大尺寸
      int concurrentKernels; // 一个布尔值，该值表示该设备是否支持在同一上下文中同时执行多个内核
｝
```
