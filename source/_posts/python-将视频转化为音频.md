---
title: python-将视频转化为音频
date: 2023-12-09 16:03:50
tags:
---

# 1. 故事背景
    最近沉迷🐟，看了非常多其他UP的优秀二创，所以想训练一个🐟的语音模型，方便后期创作。视频还是很容易取材的，那么音频该如何整理呢？了解到一个工具叫“FFmpeg”，是开源的音视频处理工具集，它可以用于处理、转换和流媒体解码/编码音视频文件。功能强大，但是需要下载安装，就显得没有那么轻便，所以想着自己用 python 写一个小工具，方便后续使用😀。好在python 有对应的库：ffmpeg3，决定使用该库实现我的小工具咯。

# 2. 安装
    1. 下载
    官方地址：https://ffmpeg.org/  
    {% asset_image 官网.png ffmpeg 官网 %}
    下载后解压到对应路径：
    {% asset_image 下载.png 下载对应文件 %}
    {% asset_image 安装包.png  Windows-安装包 %}

    2. 配置环境变量
    把 bin 文件夹路径贴进去即可。
    {% asset_image 配置环境变量.png 配置环境变量 官网 %}

    3. 检查安装情况
    {% asset_image 安装成功.png 安装成功 %}

    

# 3. 实践：ffmpeg3 库
    1. 安装 ffmpy3
    {% asset_image 安装ffmpy3.png 安装ffmpy3 %}
    2. 编写代码
```
# 1. 导包
## 读写文件需要用到
import os
from ffmpy3 import FFmpeg

# 2. 确定音视频文件路径
input_file_path = r"./vedio"
output_file_path = r"./audio"

# 3. 处理视频文件
vedio_name = os.listdir(input_file_path)
print("待处理视频文件为：")
print(vedio_name)
print('\n')

# 4. 处理音频文件
audio_name = os.listdir(output_file_path)
print("已完成的音频文件为：")
print(audio_name)
print('\n')

# 4. 文件转换（改文件名+格式转换）
for i in range(len(vedio_name)):
    # 改后缀
    change_file = input_file_path + '/' + vedio_name[i]
    # 支持多种视频格式，暂列两种
    change_name = vedio_name[i].replace('mp4', 'mp3').replace('flv', 'mp3')

    output_file = output_file_path + '/' + change_name
    if change_name in audio_name:
        continue
    print(change_file)

    # FFmpeg 转换
    fpg = FFmpeg(inputs={change_file: None},
                 outputs={output_file: '-vn -ar 44100 -ac 2 -ab 192 -f mp3'})
    print(fpg.cmd)
    fpg.run()

```

    3. 执行结果
    {% asset_image 执行结果v1.png 执行结果成功 %}

    4. 文件展示  
    
    {% asset_image 音视频.png 文件产出 %}
