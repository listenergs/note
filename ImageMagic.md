# ImageMagic

## 安装

>sudo apt-get install imagemagick

## 使用

* 查看图片
    >display input.jpg

* 格式转换
    >convert input.jpg output.png

* 质量压缩
    >convert -quality input.jpg output.jpg

* 压缩尺寸
    >convert -resize 100x100 input.jpg output.jpg \
    >convert -resize 60%x60% input.jpg output.jpg

* 查看图片质量信息
    > identify -verbose input.jpg

* 旋转图片
    >convert -rotate 45 input.jpg output.jpg
