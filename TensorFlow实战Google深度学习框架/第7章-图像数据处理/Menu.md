# 第7章 图像数据处理(多线程处理输入数据的方案)

数据格式-->数据预处理-->多线程

## 7.1 TFRecord输入数据格式

TFRecord文件的数据都是通过Protocal Buffer的格式存储

### 7.1.1 TFRecord格式介绍

#### Protocal Buffer

```
message Example {
  Features features = 1;
};

message SequenceExample {
  Features context = 1;
  FeatureLists feature_lists = 2;
};

message Features {
  map<string, Feature> feature = 1;
};

message FeatureLists {
  map<string, FeatureList> feature_list = 1;
};

message Feature {
  oneof kind {
    BytesList bytes_list = 1;
    FloatList float_list = 2;
    Int64List int64_list = 3;
  }
};


message FeatureList {
  repeated Feature feature = 1;
};

message BytesList {
  repeated bytes value = 1;
}

message FloatList {
  repeated float value = 1 [packed = true];
}

message Int64List {
  repeated int64 value = 1 [packed = true];
}
```

### 7.1.2 TFRecord样例程序

#### 写tfrecord

```
writer = tf.python_io.TFRecordWriter(filename)
for index in range(num_examples):
    ... ...
    example = tf.train.Example(...)
    writer.write(example.SerializeToString())
writer.close()
```

#### 读tfrecord

```
features = tf.parse_single_example(serialized_example, features)
```

## 7.2 图像数据处理

### 7.2.1 TensorFlow图像处理函数

#### 图像编码处理

```
tf.image.decode_jpeg
tf.image.decode_and_crop_jpeg
tf.image.decode_bmp
tf.image.decode_gif
tf.image.decode_png
tf.image.decode_image

tf.image.convert_image_dtype
```

#### 图像大小调整

```
tf.image.resize_images
tf.image.resize_bilinear
tf.image.resize_nearest_neighbor
tf.image.resize_bicubic
tf.image.resize_area
tf.image.resize_image_with_pad
tf.image.resize_image_with_crop_or_pad
tf.image.central_crop
tf.image.crop_and_resize
tf.image.crop_to_bounding_box
tf.image.pad_to_bounding_box
tf.image.extract_glimpse
```

#### 图像翻转

```
# 上下翻转
tf.image.flip_up_down
# 以一定概率上下翻转
tf.image.random_flip_up_down
# 左右翻转
tf.image.flip_left_right
# 以一定概率左右翻转
tf.image.random_flip_left_right
# 对角线翻转
tf.image.transpose_image
# 逆时针旋转90度的整数倍
tf.image.rot90
```

#### 图像色彩调整

```
# 调整亮度
tf.image.adjust_brightness
# 随机调整亮度
tf.image.random_brightness
# 调整对比度
tf.image.adjust_contrast
# 随机调整对比度
tf.image.random_contrast
# 调整色相
tf.image.adjust_hue
# 随机调整色相
tf.image.random_hue
# 伽马校正
tf.image.adjust_gamma
# 调整饱和度
tf.image.adjust_saturation
# 随机调整饱和度
tf.image.random_saturation
# 标准化(将三维矩阵中的数字均值变为0，方差变为1)
tf.image.per_image_standardization
```

#### 处理标注框

```
# 加标注框
tf.image.draw_bounding_boxes
# 非极大值抑制
tf.image.non_max_suppression
# 随机截取图片
tf.image.sample_distorted_bounding_box
```

### 7.2.2 图像预处理完整样例

## 7.3 多线程输入数据处理框架

### 7.3.1 队列与多线程

### 7.3.2 输入文件队列

### 7.3.3 组合训练数据(batching)

### 7.3.4 输入数据处理框架