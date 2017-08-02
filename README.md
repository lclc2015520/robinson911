
我写了一个tabview加载多张网络图片的demo，在模拟器中快速滑动，加载图片时，是好的。

问题：在手机上一运行，发现xcode自动打印这个log 错误“JPEGDecompressSurface : Picture decode failed: e00002d1 ”，同时真机上的图片加载不完全。



因为我这个加载多张网络图片的demo包含图片缓存到磁盘中，最终定位原因是多张图片写磁盘时，出现问题。

解决办法：将写图片到磁盘放到异步线程中去执行，然后就不报上面的错误了，同时真机上的图片也完全加载出来了。

dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0), ^{  
           BOOL a =  [[LJCacheDataManage shareInstance]writeFile:self.recieveData withName:self.imageUrl];  
           if (a) {  
               CHDebugLog(@"-----文件写入成功");  
           }  
       });  
