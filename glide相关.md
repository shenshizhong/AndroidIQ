
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
7、Glide 怎么绑定页面生命周期的？

```
简单说明过程：Glide -> RequestManagerRetriever -> RequestManagerFragment
-> ActivityFragmentLifecycle -> RequestManager -> RequestTracker

1、在 Glide 的get方法中调用 RequestManagerRetriever 的get方法，
   在get中会创建一个空的Fragment，还有RequestManager，并且将Fragment 的生命周期
   传递给 RequestManager。

2、空的Fragment 就是RequestManagerFragment 会在构造方法中创建ActivityFragmentLifecycle
3、在空的Fragment 的生命周期中会调用 ActivityFragmentLifecycle 的方法，ActivityFragmentLifecycle
   就去调用 RequestManager 的方法，RequestManager 又会去调用 RequestTracker
   
4、RequestTracker 其实就是Glide的请求了，生命周期就控制了，什么时候开始请求，什么时候取消

总的来说，就是创建了空的Fragment，通过它的生命周期去调用 ActivityFragmentLifecycle，然后
ActivityFragmentLifecycle 再去调用 RequestManager 再去调用Glide请求。从而达到，生命周期
与Glide请求的绑定。页面消失，那么Glide 也结束了。

备注：
RequestManagerRetriever 是作为 RequestManagerFragment 和 RequestManager 的桥梁

```


