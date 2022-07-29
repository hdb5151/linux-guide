/usr/lib/x86_64-linux-gnu/gstreamer-1.0/

GST_DEBUG_DUMP_DOT_DIR=. gst-launch-1.0
dot -Tpng -o pipeline_playing.png 0.00.00
gst-launch-1.0
gst-inspect-1.0 -b

gst-inspect-1.0 > 3.txt
gst-inspect-1.0 videotestsrc
gst-inspect-1.0 fakesink

gst-inspect-1.0 nvidscamera
gst-inspect-1.0 nvdepcamera

rm ~/.cache/gstreamer-1.0/registry.x86_64.bin

查看gstreamer-1.0库路径
pkg-config --cflags --libs gstreamer-1.0

echo $GST_PLUGIN_PATH

gst-inspect-1.0 v4l2src
locate libgstvideo4linux2.so
/usr/lib/x86_64-linux-gnu/gstreamer-1.0/libgstvideo4linux2.so

gst-inspect-1.0 videotestsrc
locate libgstvideotestsrc.so

通过bin形式播放
gst-launch-1.0 playbin uri=file:///media/gk/DATA/cari_data/common/audio/test01.mp3
gst-launch-1.0 playbin uri=file:///media/gk/DATA/cari_data/common/video/movie01.mp4
gst-launch-1.0 playbin uri=rtmp://58.200.131.2:1935/livetv/hunantv
gst-launch-1.0 playbin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm

测试视频
gst-launch-1.0 videotestsrc ! xvimagesink
gst-launch-1.0 videotestsrc ! 'video/x-raw' ! videoconvert ! ximagesink
gst-launch-1.0 videotestsrc ! 'video/x-raw,width=(int)1280,height=(int)720,format=(fourcc)I420' ! xvimagesink
gst-launch-1.0 videotestsrc ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m batch-size=1 width=640 height=480 ! nveglglessink
gst-launch-1.0 videotestsrc ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m batch-size=1 width=1280 height=720 ! nveglglessink
gst-launch-1.0 videotestsrc ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m batch-size=1 width=1280 height=720 ! nvinfer config-file-path=/home/gk/proj/vision/deep_stream/demo/deepstream-test1/dstest1_pgie_config.txt ! nvvideoconvert ! nvdsosd ! nveglglessink

gst-launch-1.0 videotestsrc ! videoflip method=clockwise ! videoconvert ! ximagesink

gst-launch-1.0 videotestsrc ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m batch-size=1 width=1024 height=768 ! nveglglessink


# YUV图像文件
```
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/movie01_i420_640x480_35s_01.yuv ! videoparse width=640 height=480 ! xvimagesink

```
```
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/movie01_i420_1280x720_27s_01.yuv ! videoparse width=1280 height=720 ! xvimagesink

```

# 视频文件
gst-launch-1.0 filesrc location=/home/gk/proj/vision/deep_stream/samples/streams/sample_720p.mp4 ! qtdemux ! h264parse ! nvv4l2decoder ! m.sink_0 nvstreammux name=m batch-size=1 width=1280 height=720 ! nvvideoconvert ! nvdsosd ! nveglglessink

gst-launch-1.0 filesrc location=/home/gk/proj/vision/deep_stream/samples/streams/sample_720p.mp4 ! qtdemux ! h264parse ! nvv4l2decoder ! m.sink_0 nvstreammux name=m batch-size=1 width=1280 height=720 ! nvinfer config-file-path=/home/gk/proj/vision/deep_stream/ds-test/deepstream-test1/dstest1_pgie_config.txt ! nvvideoconvert ! nvdsosd ! nveglglessink

gst-launch-1.0 filesrc location=/home/gk/proj/vision/deep_stream/samples/streams/sample_720p.mp4 ! qtdemux ! h264parse ! nvv4l2decoder ! m.sink_0 nvstreammux name=m batch-size=1 width=1280 height=720 ! nvinfer config-file-path=/home/gk/proj/vision/deep_stream/ds-test/deepstream-test1/dstest1_pgie_config.txt ! nvvideoconvert ! dsexample processing-width=1280 processing-height=720 full-frame=1 ! nvdsosd ! nveglglessink


gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/movie01_5m.mp4 ! qtdemux ! h264parse ! avdec_h264 ! videoconvert ! xvimagesink

gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/movie01_h264_480p_31s_01.mp4 ! qtdemux ! h264parse ! avdec_h264 ! videoconvert ! xvimagesink



USB摄像头
gst-launch-1.0 v4l2src ! videoconvert ! xvimagesink
如果画面卡顿或出现“警告：来自组件 /GstPipeline:pipeline0/GstXvImageSink:xvimagesink0：许多缓冲被丢弃。”，则需要降低帧率：
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480,framerate=15/1' ! videoconvert ! xvimagesink

gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! xvimagesink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480' ! videoconvert ! xvimagesink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480,framerate=15/1' ! videoconvert ! valve drop=true ! xvimagesink

gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=480,framerate=15/1' ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=720,height=540' ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=1280,height=960' ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=1440,height=1080' ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=480' ! nvvideoconvert ! nvdsosd ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m live-source=1 batch-size=1 width=1280 height=960 ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m live-source=1 batch-size=1 width=720 height=540 ! queue ! nvvideoconvert ! nvinfer config-file-path=/home/gk/proj/vision/deep_stream/demo/deepstream-test1/dstest1_pgie_config.txt ! queue ! nvvideoconvert ! nvdsosd ! nveglglessink

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480,framerate=25/1' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink

gst-launch-1.0 v4l2src ! 'video/x-raw,width=640,height=480,framerate=25/1' ! gamma gamma=2.0 ! videoconvert ! ximagesink
gst-launch-1.0 v4l2src ! videobalance saturation=0.0 ! videoconvert ! ximagesink
gst-launch-1.0 v4l2src ! videoflip method=clockwise ! videoconvert ! ximagesink

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480,framerate=15/1' ! videoconvert ! queue min-threshold-time=2000000000 leaky=1 ! xvimagesink


USB摄像头延时
gst-launch-1.0 v4l2src device=/dev/video0 ! xvimagesink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480,framerate=15/1' ! videoconvert ! queue2 use-buffering=TRUE max-size-time=4000000000 low-watermark=0.1 high-watermark=0.6 ! xvimagesink

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,format=NV12,width=640,height=480,framerate=15/1' ! videoconvert ! delayqueue ! xvimagesink
gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=480,framerate=15/1' ! delayqueue ! nveglglessink
gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=720,height=540,framerate=15/1' ! delayqueue ! nveglglessink


gst-launch-1.0 v4l2src device=/dev/video0 ! delayqueue ! xvimagesink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480,framerate=15/1' ! delayqueue ! xvimagesink
gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw,width=640,height=480' ! delayqueue ! xvimagesink

----------------
# 工业相机IDSCamera
## 最简 结构
```
gst-launch-1.0 \
	nvidscamera dev_idx=0  ! 'video/x-raw,format=NV12,width=1024,height=768' ! \
	nveglglessink

```

## 使用 streammux
```
GST_DEBUG_DUMP_DOT_DIR=. gst-launch-1.0 \
	nvidscamera dev_idx=0 ! 'video/x-raw,format=NV12,width=1024,height=768' ! nvvideoconvert ! \
	'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m live-source=1 batch-size=1 width=800 height=600 ! \
	nveglglessink

```

## 使用 streammux + nvinfer + nvdsosd
```
gst-launch-1.0 \
	nvidscamera dev_idx=0  ! 'video/x-raw,format=NV12,width=1024,height=768' ! nvvideoconvert ! \
	'video/x-raw(memory:NVMM),format=NV12' ! m.sink_0 nvstreammux name=m live-source=1 batch-size=1 width=400 height=300 ! \
	queue ! nvvideoconvert ! nvinfer config-file-path=/hom_stream/ds-test/deepstream-test1/dstest1_pgie_config.txt ! \
	queue ! nvvideoconvert ! nvdsosd ! \
	nveglglessink

```

## fcp 实际结构： 基本结构 + nvstreammux
```
gst-launch-1.0 \
    nvidscamera dev_idx=0 ! 'video/x-raw,format=NV12,width=1024,height=768,framerate=16/1' ! videoconvert ! \
    tee ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=1024,height=768,framerate=16/1' ! \
    m.sink_0 nvstreammux name=m width=640 height=480 batch-size=1 live-source=1 batched-push-timeout=4000 gpu-id=0 ! \
    videoconvert ! nveglglessink

```

## fcp 实际结构： 基本结构 + nvstreammux + nvinfer + nvdsosd
```
gst-launch-1.0 \
    nvidscamera dev_idx=0 ! 'video/x-raw,format=NV12,width=1024,height=768,framerate=16/1' ! videoconvert ! \
    tee ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=1024,height=768,framerate=16/1' ! \
    m.sink_0 nvstreammux name=m width=640 height=480 batch-size=1 live-source=1 batched-push-timeout=40000 ! \
    queue ! nvvideoconvert ! nvinfer config-file-path=./config_infer_cvy_bfm.cfg ! \
    queue ! nvvideoconvert ! nvdsosd ! \
    fakesink

```

## 有问题 不能工作！！！
```
gst-launch-1.0 \
	nvidscamera dev_idx=0 ! 'video/x-raw,format=NV12,width=1024,height=768' ! videoconvert ! \
    tee ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=1024,height=768,framerate=16/1' \
    nvvideoconvert ! 'video/x-raw,format=NV12,width=1024,height=768,framerate=16/1'! \
    ! m.sink_0 nvstreammux name=m live-source=1 batch-size=1 width=800 height=600 ! \
	nveglglessink 

```

-------------
# 深度相机

## 深度图 基本结构 -1
```
gst-launch-1.0 \
    nvdepcamera access-mode=0 stream-type=1 resolution-type=0 ! 'video/x-raw,format=GRAY16_LE,width=1280,height=960,framerate=16/1' ! \
    nvdeprender ! 'video/x-raw,format=NV12,width=1280,height=960,framerate=16/1' ! queue ! videoconvert ! xvimagesink

```

## 深度图 基本结构 -1
```

```



-------------
# 播放音频
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/audio/test01.mp3 ! audioconvert ! audioresample ! osssink
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/audio/test01.mp3 ! mad ! audioconvert ! audioresample ! osssink
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/audio/test01.wav ! wavparse ! audioresample ! pulsesink
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/audio/test01.wav ! wavparsedemo ! audioresample ! pulsesink

播放音视频
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/sintel_trailer-480p.ogv ! oggdemux name=demux ! queue ! vorbisdec ! autoaudiosink demux. ! queue ! theoradec ! videoconvert ! autovideosink

拉流（播放）RTSP
gst-launch-1.0 playbin uri=rtsp://localhost:8554/
gst-launch-1.0 playbin uri=rtsp://localhost:8554/ds-test
gst-launch-1.0 playbin uri=rtsp://localhost:8554/rtsp_test

推流RTSP
cd ~/proj/vision/deep_stream/gstreamer/rtsp/rtsp-server/gst-rtsp-server/examples

./test-mp4 /home/gk/Dataset/movie01.mp4


./test-launch '( v4l2src device=/dev/video0 ! videoconvert ! queue ! x264enc tune="zerolatency" byte-stream=true bitrate=10000 ! rtph264pay name=pay0 pt=96 )'



客户端启动播放时出现卡顿
./test-launch "v4l2src device=/dev/video0 ! videoconvert ! video/x-raw,format=YV12,width=640,height=480,framerate=15/1 ! videoconvert ! x264enc profile=main tune=zerolatency threads=4 ! rtph264pay name=pay0 pt=96"

客户端启动播放时出现卡顿，和“x264 [error]: baseline profile doesn't support 4:2:2”
./test-launch "v4l2src device=/dev/video0 ! videoconvert ! queue ! x264enc tune=zerolatency threads=4 bitrate=500 ! rtph264pay pt=96 name=pay0"

./test-launch "v4l2src device=/dev/video0 ! videoconvert ! video/x-h264,framerate=(fraction)15,width=640,height=480,profile=main,stream-format=(string)=byte-stream ! x264enc profile=main tune=zerolatency threads=4 bitrate=500 ! rtph264pay pt=96 name=pay0"

./test-launch "v4l2src device=/dev/video0 ! videoconvert ! video/x-h264,framerate=(fraction)15,width=640,height=480,profile=main,stream-format=(string)=byte-stream ! x264enc profile=main tune=zerolatency threads=4 bitrate=500 ! rtph264pay pt=96 name=pay0 alsasrc ! audioconvert ! audio/x-raw ! voaacenc ! queue ! rtpmp4apay pt=97 name=pay1"


---- 编码 ----
采集USB摄像头并保存视频
gst-launch-1.0 v4l2src ! videoconvert ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4

gst-launch-1.0 v4l2src ! tee name=t ! queue ! videoconvert ! xvimagesink t. ! queue ! videoconvert ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4

gst-launch-1.0 v4l2src ! videoconvert ! tee name=t ! queue ! xvimagesink ! t. ! queue ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4 -e

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=I420,width=1280,height=960,framerate=25/1' ! nvv4l2h264enc ! h264parse ! qtmux ! filesink location=output.mp4 -e

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=I420,width=640,height=480,framerate=15/1' ! nvv4l2h264enc ! h264parse ! qtmux ! filesink location=output.mp4 -e

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=I420,width=1280,height=960,framerate=25/1' ! tee name=t ! queue ! nveglglessink t. ! queue ! nvv4l2h264enc ! h264parse ! qtmux ! filesink location=output.mp4 -e

gst-launch-1.0 audiotestsrc ! tee name=t ! queue ! audioconvert ! autoaudiosink t. ! queue ! wavescope ! videoconvert ! autovideosink


gst-launch-1.0 v4l2src ! videoconvert ! queue min-threshold-time=2000000000 ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4 -e

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=I420,width=640,height=480,framerate=15/1' ! queue min-threshold-time=2000000000 ! nvv4l2h264enc ! h264parse ! qtmux ! filesink location=output.mp4 -e



读取测试视频并保存
gst-launch-1.0 videotestsrc ! videoconvert ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/sintel_trailer-480p.ogv ! oggdemux name=demux ! queue ! theoradec ! videoconvert ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4
gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/sintel_trailer-480p.ogv ! oggdemux name=demux ! queue ! theoradec ! videoconvert ! x264enc bitrate=50000 dct8x8=false threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output1.mp4

UDP流输出
gst-launch-1.0 videotestsrc ! video/x-raw ! x264enc ! rtph264pay ! udpsink host=127.0.0.1 port=5400
gst-launch-1.0 videotestsrc pattern=ball ! video/x-raw ! videoconvert ! x264enc tune=zerolatency ! rtph264pay ! udpsink host=127.0.0.1 port=5400
gst-launch-1.0 videotestsrc ! video/x-raw,width=800,height=600,codec=h264,type=video ! videoscale ! videoconvert ! x264enc tune=zerolatency ! rtph264pay ! udpsink host=127.0.0.1 port=5400

gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! videoscale ! "video/x-raw,is-live=true,format=NV12,width=640,height=480,framerate=15/1" ! x264enc ! h264parse ! rtph264pay name=pay0 pt=96 ! udpsink host=127.0.0.1 port=5400

gst-launch-1.0 v4l2src device=/dev/video0 ! 'video/x-raw' ! videoconvert ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=480,framerate=15/1' ! nvv4l2h264enc ! h264parse ! rtph264pay ! udpsink host=127.0.0.1 port=5400

gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/image/airplane.png ! pngdec ! videoconvert ! x264enc ! rtph264pay ! udpsink host=127.0.0.1 port=5400

gst-launch-1.0 filesrc location=/media/gk/DATA/cari_data/common/video/movie01.mp4 ! qtdemux ! h264parse ! rtph264pay pt=96 name=pay0 ! udpsink host=127.0.0.1 port=5400

UDP流接收和显示
gst-launch-1.0 udpsrc port=5400 ! 'application/x-rtp' ! rtph264depay ! avdec_h264 ! autovideosink
gst-launch-1.0 udpsrc port=5400 ! 'application/x-rtp,media=video,clock-rate=90000,encoding-name=H264' ! rtph264depay ! avdec_h264 ! autovideosink fps-update-interval=1000 sync=false
gst-launch-1.0 udpsrc port=5400 caps='application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264' ! rtph264depay ! avdec_h264 ! autovideosink fps-update-interval=1000 sync=false


x264enc主要选项
bitrate : 15000,	// kbps
threads : 4,
byte-stream : "TRUE",
rc-lookahead : 25,
key-int-max : 100,
pass : 5,
mb-tree : true,
speed-preset : 0,
dct8x8 : true,	// Profile: high, main

---- 多路处理 ----
音频播放和波形显示
gst-launch-1.0 audiotestsrc ! tee name=t ! queue ! audioconvert ! autoaudiosink t. ! queue ! wavescope ! videoconvert ! autovideosink
gst-launch-1.0 audiotestsrc ! tee name=t ! queue ! audioconvert ! pulsesink t. ! queue ! wavescope ! videoconvert ! xvimagesink
gst-launch-1.0 audiotestsrc ! tee name=t ! queue ! audioconvert ! pulsesink t. ! queue ! wavescope shader=0 style=3 ! videoconvert ! xvimagesink

元件连接图：
audiotestsrc -> tee -> queue -> audioconvert -> autoaudiosink(pulsesink)
		 |---> queue -> wavescope -> videoconvert -> autovideosink(xvimageink)

视频播放和保存
gst-launch-1.0 videotestsrc ! videoconvert ! tee name=t ! queue ! autovideosink t. ! queue ! autovideosink
gst-launch-1.0 videotestsrc ! videoconvert ! tee name=t ! queue ! autovideosink t. ! queue ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4
gst-launch-1.0 v4l2src ! videoconvert ! tee name=t ! queue ! xvimagesink t. ! queue ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4
gst-launch-1.0 v4l2src ! videoconvert ! tee name=t ! queue ! autovideosink t. ! queue ! x264enc threads=4 sliced-threads=TRUE tune=zerolatency ! matroskamux ! filesink location = output.mp4


==== 工具 ====
gst-inspect-1.0 alsasink
gst-inspect-1.0 videotestsrc
gst-inspect-1.0 nvdsgst_gigecamera

如果插件加载失败，查看黑名单
gst-inspect-1.0 -b
# 删除缓存
rm .cache/gstreamer-1.0/registry.x86_64.bin
# 通过这个方法，查看真正错误。
gst-inspect-1.0 GST_DEBUG=2,GST_PLUGIN_LOADING:5,GST_REGISTRY:5

#　注意，如果插件依赖so，不要复制到插件目录下，而是这个目录：
sudo cp 目标插件.so /usr/lib/x86_64-linux-gnu/

==== 调试 ====
Gstreamer使用GST_DEBUG_DUMP_DOT_DIR宏定义整个pipeline的拓扑结构图导出路径。拓扑结构图的格式为Dot，推荐使用GraphViz工具将Dot文件转成Png，便于查看。

GST_DEBUG_DUMP_DOT_DIR=/home/gk/proj/vision/gstreamer/dump gst-launch-1.0 videotestsrc ! autovideosink
GST_DEBUG_DUMP_DOT_DIR=/home/gk/proj/vision/gstreamer/v4l2 gst-launch-1.0 v4l2src ! videoconvert ! xvimagesink
GST_DEBUG_DUMP_DOT_DIR=/home/gk/proj/vision/gstreamer/dump gst-launch-1.0 playbin uri=file:///media/gk/DATA/cari_data/common/video/movie01.mp4

dot -Tpng -o pipeline_ready.png 0.00.00.042781691-gst-launch.NULL_READY.dot
dot -Tpng -o pipeline_playing.png 0.00.00.042781691-gst-launch.NULL_READY.dot
dot -Tpng -o pipeline_video_rec.png 0.00.00.042781691-gst-launch.NULL_READY.dot

gst-launch-1.0 v4l2src device=/dev/video0 ! video/x-raw,width=1280,height=720,framerate=20/1 ! autovideosink

gst-launch-1.0 v4l2src !video/x-raw-yuv,format=fourccYUY2,width=640,height=480,framerate=15/1 ! videorate ! videoscale ! ffmpegcolorspace ! xvimagesink


/usr/lib/x86_64-linux-gnu/gstreamer-1.0

gst-inspect-1.0 > 3.txt
gst-inspect-1.0 videotestsrc
gst-inspect-1.0 nvdsgigecamera

gst-launch-1.0 videotestsrc ! xvimagesink
gst-launch-1.0 videotestsrc ! autovideosink
gst-launch-1.0 videotestsrc width=640 height=480 framerate=25 ! videoconvert ! autovideosink
gst-launch-1.0 videotestsrc ! video/x-raw,width=1280,height=720,framerate=20/1 ! videoconvert ! autovideosink

GST_DEBUG_DUMP_DOT_DIR=/home/gk/proj/vision/gstreamer/doc/dump gst-launch-1.0 videotestsrc ! videoconvert ! autovideosink

gst-launch-1.0 -v 
gst-launch-1.0 edtpdvsrc ! ffmpegcolorspace ! autovideosink
gst-launch-1.0 nvdsgigecamera ! xvimagesink
gst-launch-1.0 nvdsgigecamera ! videoconvert ! autovideosink
gst-launch-1.0 nvdsgigecamera ! video/x-raw,width=640,height=480,framerate=20/1 ! videoconvert ! autovideosink
gst-launch-1.0 nvdsgigecamera ! video/x-raw,format=YUY2,width=640,height=480,framerate=20/1 ! videoconvert ! autovideosink

GST_DEBUG_DUMP_DOT_DIR=. gst-launch-1.0 nvdsgigecamera ! video/x-raw,width=640,height=480,framerate=20/1 ! videoconvert ! autovideosink
dot -Tpng -o pipeline_playing.png 0.00.00.131848655-gst-launch.PAUSED_PLAYING.dot



