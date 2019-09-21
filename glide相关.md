
1、Glide 缓存策略是怎么做的？
```
两种缓存：内存缓存、磁盘缓存

内存缓存（也是默认缓存）：
RequestOptions options = new RequestOptions()
        // 内存缓存
        .skipMemoryCache(false)
        // 磁盘不缓存
        .diskCacheStrategy(DiskCacheStrategy.NONE);
        
磁盘缓存
RequestOptions options = new RequestOptions()
        // 既缓存原始图片，又缓存转化后的图片
        .diskCacheStrategy(DiskCacheStrategy.ALL);
```
2、Glide 硬盘缓存的方式有哪几种？

```
1，DiskCacheStrategy.NONE// 表示不缓存任何内容

2，DiskCacheStrategy.DATA// 表示只缓存原始图片

3，DiskCacheStrategy.RESOURCE// 表示只缓存转换过后的图片

4，DiskCacheStrategy.ALL // 表示既缓存原始图片，也缓存转换过后的图片

5，DiskCacheStrategy.AUTOMATIC//表示让Glide根据图片资源智能地选择使用哪一种缓存策略（默认选项）
```
3、怎么清除缓存？
```
清除所有的内存缓存
Glide.get(this).clearMemory();

清除所有的磁盘缓存
Glide.get(this).clearDiskCache();

除单个缓存
Glide.with(this).clear(imageView);
```

4、Glide 怎么和Activity 和 Fragment 绑定生命周期的？
```
LifecycleListener

```

5、Glide 中 override 与 submit 都是重置图片大小，他们有什么区别？
```
RequestOptions override(int width, int height) 
override 是在RequestOptions 中的方法，主要是重置图片大小，用于显示用

FutureTarget<TranscodeType> submit(int width, int height)
在 RequestBuilder中，（与into 一样，但是into 过时了）
主要用于后台进程中下载，下载下来备用


```
6、怎么通过一个地址判断图片的格式？
```
String filePath = file.getPath();
BitmapFactory.Options options = new BitmapFactory.Options();
options.inJustDecodeBounds = true;
BitmapFactory.decodeFile(filePath, options);

String mimeType = options.outMimeType;

outMimeType就是获取图片的格式

例子：
Glide.with(contentView.getContext())
                .downloadOnly()
                .load(imgUrl)
                .listener(getFileRequestListener(loadingProgress, longImg))
                .into(new SimpleTarget<File>() {
                    @Override
                    public void onResourceReady(@NonNull File file, @Nullable Transition<? super File> transition) {
                        String filePath = file.getPath();
                        BitmapFactory.Options options = new BitmapFactory.Options();
                        options.inJustDecodeBounds = true;
                        BitmapFactory.decodeFile(filePath, options);
                        int bmpWidth = options.outWidth;
                        int bmpHeight = options.outHeight;

                        //outMimeType是以--”image/png”、”image/jpeg”、”image/gif”…….这样的方式返回的
                        String mimeType = options.outMimeType;
                        
                        }
                       }

```



