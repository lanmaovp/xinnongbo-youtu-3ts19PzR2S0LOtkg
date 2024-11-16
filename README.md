
**一、介绍**　　　　今天是这个系列《C\+\+之 Opencv 入门到提高》得第五篇文章。这篇文章也不难，介绍如何图像的基本操作，比如：读取一张图片的像素值，如何修改一张图片中的像素值，如何读取一张图片，如何保存一张图片等等，这都是基础，为以后的学习做好铺垫。虽然操作很简单，但是背后有很多东西需要我们深究，才能做到知其然知其所以然。OpenCV 具体的简介内容，我就不多说了，网上很多，大家可以自行脑补。　　　　OpenCV 的官网地址：[https://opencv.org/](https://github.com)，组件下载地址：[https://opencv.org/releases/](https://github.com):[豆荚加速器](https://baitenghuo.com) 。　　　　OpenCV 官网学习网站：[https://docs.opencv.ac.cn/4\.10\.0/index.html](https://github.com)　　　　我需要进行说明，以防大家不清楚，具体情况我已经罗列出来。　　　　　　　　操作系统：Windows Professional 10（64位）　　　　　　　　开发组件：OpenCV – 4\.10\.0　　　　　　　　开发工具：Microsoft Visual Studio Community 2022 (64 位) \- Current版本 17\.8\.3　　　　　　　　开发语言：C\+\+（VC16）**二、知识学习**　　　　这些都是图像的基本操作，所以并不会很难，但是这也是学好 openCV的基础。内容很简单，就不说过多的废话了，所有讲解都在代码的注释中。


```
  1 #include 
  2 #include 
  3 #include 
  4 
  5 using namespace std;
  6 using namespace cv;
  7 
  8 /// 
  9 /// 图像的操作
 10 /// 1、读写图像
 11 /// 2、读写像素
 12 /// 3、修改像素值
 13 /// 
 14 /// 
 15 int main()
 16 {
 17     //1、读写图像
 18     //1.1、imread 可以指定加载灰度或者 RGB 图像
 19     //1.2、imwrite 可以保存图像，类型由扩展名决定。
 20     Mat src;
 21     src = imread("D:\\360MoveData\\Users\\Administrator\\Desktop\\TestImage\\demo-gril.png", IMREAD_UNCHANGED);
 22     if (src.empty())
 23     {
 24         cout << "图像加载失败！！！" << endl;
 25         return -1;
 26     }
 27 
 28     namedWindow("原图", WINDOW_AUTOSIZE);
 29     imshow("原图", src);
 30 
 31     //2、读写像素
 32     //2.1、都一个灰度（Gray）像素的像素值（CV_8UC1）
 33     //        Scalar intensity=src.at(row,col);
 34     //        Scalar intensity=src.at(Point(row,col));
 35     // 
 36     //2.2、读一个彩色（RGB）像素点的像素值。
 37     //        Vec3f intensity=src.at(row,col);
 38     //        float blue=intensity.val[0];
 39     //        float green=intensity.val[1];
 40     //        float red=intensity.val[2];
 41     //
 42     //        Vec3f 就是 float 类型的 RGB 数据
 43     // 
 44     //2.3、修改单通道灰度像素值
 45     //        src.at(row,col)=128;
 46     // 
 47     // 2.4、修改RGB 三通道像素值
 48     //        src.at(row,col)[0]=128;
 49     //        src.at(row,col)[1]=128;
 50     //        src.at(row,col)[2]=128;
 51     // 
 52     // 2.5、空白像素赋值
 53     //        src=Scalar(0);
 54     // 
 55     // 
 56     // 2.6、Vec3b 与 Vec3f
 57     // 2.6.1、Vec3b 对应三通道的顺序是 blue,green,red 的 uchar 类型数据
 58     // 2.6.2、Vec3f 对应三通道的顺序是 blue,green,red 的 float 类型数据
 59     // 2.6.3、把 CV_8UC1 转换为 CV32F1 实现如下：src.convertTo(dst,CV_32F);
 60     // 
 61     //2.1、读取单通道像素值，示例代码：
 62     Mat graySrc;
 63     cvtColor(src, graySrc, COLOR_BGR2GRAY);//将彩色的 RGB 3 通道的转换为灰度单通道的图片。
 64     namedWindow("单通道灰度图像", WINDOW_AUTOSIZE);
 65     imshow("单通道灰度图像", graySrc);
 66 
 67     int width = graySrc.cols;
 68     int height = graySrc.rows;
 69 
 70     for (int row = 0; row < height; row++)
 71     {
 72         for (int col = 0; col < width; col++)
 73         {
 74             //读取单通道、灰度的像素值
 75             int gray = graySrc.at(row, col);
 76             //修改单通道、灰度的像素值
 77             graySrc.at(row, col) = 255 - gray;
 78         }
 79     }
 80 
 81     imshow("修改后单通道灰度图像", graySrc);//可以直接使用显示图像，他会自动创建显示图片的窗口。
 82 
 83     //处理多通道、彩色的图像
 84     width = src.cols;
 85     height = src.rows;
 86     int channes = src.channels();
 87 
 88     Mat dst;
 89     dst.create(src.size(),src.type());
 90 
 91     for (int row = 0; row < height; row++)
 92     {
 93         for (int col = 0; col < width; col++)
 94         {
 95             if (channes == 1)
 96             {
 97                 //读取单通道、灰度的像素值
 98                 int gray = src.at(row, col);
 99                 //修改单通道、灰度的像素值
100                 src.at(row, col) = 255 - gray;
101             }
102             else
103             {
104                 //读取多通道、彩色像素值。Vec3b 就是指具有3个通道的 BGR 的数据结构
105                 Vec3b myvalue=src.at(row, col);
106                 int b = myvalue.val[0];
107                 int g = myvalue.val[1];
108                 int r = myvalue.val[2];
109 
110                 //修改多通道、彩色像素值,这是取反效果。
111                 /*dst.at(row, col)[0] = 255 - b;
112                 dst.at(row, col)[1] = 255 - g;
113                 dst.at(row, col)[2] = 255 - r;*/
114 
115                 //修改多通道、彩色像素值,这是只包含蓝色和绿色，青就是色。
116                 /*dst.at(row, col)[0] = b;
117                 dst.at(row, col)[1] = g;
118                 dst.at(row, col)[2] = 0;*/
119 
120                 //修改多通道、彩色像素值,这是只包含蓝色和红色，就是紫色。
121                 /*dst.at(row, col)[0] = b;
122                 dst.at(row, col)[1] = 0;
123                 dst.at(row, col)[2] = r;*/
124 
125                 //修改多通道、彩色像素值,这是只包含绿色和红色，就是黄色。
126                 dst.at(row, col)[0] = 0;
127                 dst.at(row, col)[1] = g;
128                 dst.at(row, col)[2] = r;
129 
130                 //graySrc.at(row, col) = min(r,min(b,g));取灰色第二个方法
131             }
132         }
133     }
134 
135     imshow("修改后单、多通道图像", dst);//可以直接使用显示图像，他会自动创建显示图片的窗口。
136     //imshow("单通道灰度图像", graySrc);
137     //opencv 接口也可以实现这样反差的效果
138     bitwise_not(src, dst);
139 
140     imshow("bitwise_not 修改后单、多通道图像", dst);//可以直接使用显示图像，他会自动创建显示图片的窗口。
141 
142     
143     waitKey(0);
144 
145     system("pause");
146     return 0;
147 }
```


 


　　　　代码很简单，就不多说了。


　　　　效果如图：


　　　　![](https://img2024.cnblogs.com/blog/1048776/202411/1048776-20241115105627763-1828432568.png)


　　　　![](https://img2024.cnblogs.com/blog/1048776/202411/1048776-20241115105734765-1900927633.png)


　　　　![](https://img2024.cnblogs.com/blog/1048776/202411/1048776-20241115105826873-383381961.png)


　　　　![](https://img2024.cnblogs.com/blog/1048776/202411/1048776-20241115105926111-669756906.png)


　　　　![](https://img2024.cnblogs.com/blog/1048776/202411/1048776-20241115110024340-1904527917.png)


　　　　以上就是图像修改后的效果图。


**三、总结**　　　　这是 C\+\+ 使用 OpenCV 的第五篇文章，其实也没那么难，感觉是不是还是很好入门的，那就继续。初见成效，继续努力。皇天不负有心人，不忘初心，继续努力，做自己喜欢做的，开心就好。
