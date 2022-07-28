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
# 从gstbuffer中获取数据的方法

## gstreamer的插件如何复制数据
[原文链接](https://quantum6.blog.csdn.net/article/details/84541902)
工作中，使用了gstreamer和nvidia的DeepStream插件。如何从nvidia插件中获取数据，这个之前吾有博文专门论述。因为这个很少介绍，也不好找。吾当时也是运气好，从别的代码中得到启示，才找到了正确的解决办法。

　　那么，普通的gstreamer插件如何获取数据呢？比如说，从decoder（包括nvidia decoder插件）中，获取原始的h264数据代码是怎样的？这个很简单，这里分享出来，这样可以在本博客中都能找到，更方便。

　　当然，这里的结构体的意思，一看就明白，自行更改即可。

```
 
static int get_probe_h264_data(const GstPadProbeInfo * pProbeInfo, DataBuffer* pBuffer)
{
    GstBuffer *gst_buf = (GstBuffer *) pProbeInfo->data;
    GstMapInfo map_info;
    int size = 0;
 
    if (gst_buf == NULL || !gst_buffer_map (gst_buf, &map_info, GST_MAP_READ))
    {
        g_print ("gst_buffer_map() error!");
        return -1;
    }
 
    size        = gst_buffer_get_size( gst_buf );
    databuffer_check(pBuffer, size);
    gst_buffer_extract (gst_buf, 0, pBuffer->data, size);
    pBuffer->size = size;
 
    gst_buffer_unmap (gst_buf, &map_info);
 
    return 0;
}
 
 
/**
 获取H264数据，并保存。
 */
static GstPadProbeReturn callback_osd_sink_pad_buffer_probe_decoder (GstPad * pad, GstPadProbeInfo * probe_info, gpointer u_data)
{
    AiTask *pAiTask = (AiTask *)u_data;
 
    get_probe_h264_data(probe_info, &(pAiTask->h264_buffer));
 
    /*GH_LOG_INFO("size=%d, data=[0x%02X, 0x%02X, 0x%02X, 0x%02X, 0x%02X,]",
            pAiTask->h264_buffer.size,
            pAiTask->h264_buffer.data[0],
            pAiTask->h264_buffer.data[1],
            pAiTask->h264_buffer.data[2],
            pAiTask->h264_buffer.data[3],
            pAiTask->h264_buffer.data[4]
            );
    */
    fwrite(pAiTask->h264_buffer.data, 1, pAiTask->h264_buffer.size, pAiTask->h264_file);
 
    return GST_PAD_PROBE_OK;
}
 
 
int main()
{
    video_decoder = gst_element_factory_make("nvdec_h264", element_name);
 
    osd_pad = gst_element_get_static_pad(video_decoder, TEXT_SINK);
    if (osd_pad)
    {
        osd_probe_id = gst_pad_add_probe(osd_pad, GST_PAD_PROBE_TYPE_BUFFER,
                                callback_osd_sink_pad_buffer_probe_decoder, pTask, NULL);
    }
 
}
```

## GstBuffer中data实际的存储地址

前段时间刚开始学Gstreamer，还没学多少就要干活了，最近想用gdb查看GstBuffer的data地址是总很麻烦，要先用gst_buffer_map先获得data，所以就深入的了解了一下GstBuffer中data所存放的地方。下面就和大家分享一下吧！

通常我们需要获取GstBuffer的data数据是通过接口gst_buffer_map得到的，进入gst_buffer_map接口的具体实现，我们可以发现，Gstreamer通过_get_merged_memory函数得到GstBuffer所对应的GstMemory，再深入后，我们可以发现GstBuffer只是暴露给我们用户的信息（通过GstBuffer是找不到我们想要的data的），真正的信息是存储在GstBufferImpl这个结构体中的，此结构体第一个成员即GstBuffer，而后会包含一个GstMemory指针数组（大小为16），我们想要的data就存储在这里面（通常我们只用到了mem[0]）。

_get_merged_memory函数是根据你传的flag（即GST_MAP_READ或GST_MAP_WIRTE）来判断是否要拷贝一份数据。如果你去GstMemory中查找我们想要的data，还是找不到，先别急。Gstreamer会用gst_memory_map来得到对应的data，而进入此函数，我们会发现Gstreamer会用到GstMemory中的allocator成员的mem_map函数来获得data。如果你不深入到Gstreamer框架是比较难找到这个mem_map函数指针的定义的。
不过，没事，你有我，这部分工作我帮你做吧！在Gstreamer中，我们发现其实GstMemory和GstBuffer一样，只暴露了一部分的信息，具体的信息是存储在GstMemorySystem这个结构体里。而GstMemory所对应的GstAllocator中的函数指针是在gst_allocator_sysmem_init函数中实现的（当然这个函数是可以覆盖的，如gst_xvimage_allocator_init中就覆写了），其中mem_map函数指针指针向的是_sysmem_map函数，在此函数中，我们要以看到，我们最终获取的信息是GstMemorySystem结构体中的data。

通过上面的分析，你应该清楚了Gstreamer是如何获得GstBuffer的data的了吧。所以，在用gdb调试的时候，假如我们想要打印buffer的data地址，可以这样：

 ------p ((GstMemorySystem *)((GstBufferImpl *)buffer)->mem[0])->data-------


# gst-launch-1.0使用
1、播放MP4音视频
MP4文件一般有2个流：音频流和视频流，部分文件会有字幕流。一般我们只处理2个流就够了。一般视频流采用h264编码，音频流采用aac编码。一开始由于不熟悉gst-inspect的用法，导致音视频解码器找不到，浪费了很多时间。

gst-inspect-1.0 | grep h264 找到h264解码器avdec_h264

gst-inspect-1.0 | grep aac 找到aac解码器avdec_aac。也可以用faad解码，faad输出为16位音频，avdec_aac输出为32位音频

gst-launch-1.0中关于demux的用法也摸索了好久，mp4文件要用到qtdemux(quick time demux)，用names属性分离管道，正确用法如下

gst-launch-1.0 filesrc location=gongye.mp4 ! qtdemux name=demuxer demuxer. ! avdec_h264 ! xvimagesink 播放视频

gst-launch-1.0 filesrc location=gongye.mp4 ! qtdemux name=demuxer demuxer. ! avdec_aac ! audioconvert ! audioresample ! alsasink  播放音频(audioresample可选)

gst-launch-1.0 filesrc location=gongye.mp4 ! qtdemux name=demuxer demuxer. ! queue ! avdec_aac ! audioconvert ! alsasink demuxer. ! queue ! avdec_h264 ! xvimagesink  播放音视频

demuxer. 后面可以指定流名称，如 demuxer.video_0，demuxer.audio_0，流名称必须与文件中的流名称对应。

其他的文件格式，如flv，ogg，mpeg等文件都可以采用类似的方式，先用gst-inspect-1.0查找对应的demux和音视频解码器，然后构建管道即刻播放。

对于元件中的request pad，gst-launch-1.0也可以指定request pad的连接，典型如tee，nvstreammux等元件需要request pad，连接方式如下：

gst-launch-1.0 filesrc location=sample_720.h264 ! h264parse ! nvv4l2decoder ! smuxer.sink_0 nvstreammux name=smuxer  width=1920 height=1080 batch-size=1 batched-push-timeout=4000000 ! nvinfer config-file-path=dstest1_pgie_path.txt ! nvvideoconvert ! nvdsosd ! nvvideoconvert ! nvv4l2h264enc ! h264parse ! qtmux ! filesink location=test.mp4

以上命令是对deepstream-test1的命令行模拟，但是缺少了osd探针函数，所以并不完整，但也可以运行，主要用于展示nvstreammux request pad（sink_%u）的用法。其中nvv4l2decoder是nvidia的硬解码元件，nvstreammux是deepstream队列元件，在使用nvinfer进行推理之前必须要使用该队列元件添加nvinfer所需要的数据。nvinfer是推理元件，配置文件为dstest1_pgie_path.txt。有关deepstream的资料，请另行查阅。

2、编码
gst-launch-1.0 v4l2src device=/dev/video0 ! video/x-raw,format=YUY2,width=640,height=480,framerate=30/1 ! videoconvert ! x264enc ! h264parse ! qtmux ! filesink location=1.mp4 -e

注意，尾部的-e不能省，表示按下ctrl-c键后会向视频流发送EOS标识，视频流才能完整编码。

3、rtp推流
 发送端：gst-launch-1.0 v4l2src ! video/x-raw,format=YUY2,width=1280,height=720,framerate=10/1 ! videoconvert ! x264enc ! rtph264pay ! udpsink host=127.0.0.1 port=5600

接收端：gst-launch-1.0 udpsrc port=5600 caps='application/x-rtp,media=(string)video,clock-rate=(int)90000,encoding-name=(string)H264' ! rtph264depay ! avdec_h264 ! videoconvert ! xvimagesink

4、摄像头采集
（1）实时显示

gst-launch-1.0 v4l2src device = /dev/video0 ! xvimagesink &

（2）如果桌面显示不了的话，可以按照如下存下YUV数据

gst-launch-1.0 v4l2src device = /dev/video0 ! filesink location=video.yuv &
