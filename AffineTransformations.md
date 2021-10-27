---
title:  "Roy's Computer Vision Library Blog"
---

<div dir="rtl">
   <h1 align=center>Affine Transformations </h1>
  <h2> Image Rotation </h2>
    אז בחלק הזה נלמד איך לעשות רוטציות לתמונה. ראשית אציג את הדרך הנאיבית בה היווצרו חורים בתמונה (בשל הזזת הפיקסלים) ולאחר מכן נפתור את הבעייה באמצעות שימוש באינטרפולציה שעליה פירטתי בהרחבה בפוסט הבא <a href="index.md">interpolation</a>. <br> 
  
      
 <figure>
    <img src='lions1.jpeg' alt='missing' />
    <figcaption> יובל יאריה </figcaption>
</figure>
  
  <h3> תאוריה </h3>
   כשמבצעים רוטציה לתמונה מבצעים את הרוטציה בזווית &alpha; שבדרך מתקבלת כפרמטר מהמשתמש. <br>
   לדגומה בתמונה הבאה, אנחנו מבצעים רוטציה בזווית &alpha; מהפיקסל (u,v) אל הפיקסל (x,y).   
ֿ
   <img src='https://user-images.githubusercontent.com/69425073/138815394-abd74c44-6e61-4942-a842-86e06a214842.png' alt='missing' style="width: 60%; height: auto;" />
        <br>
   
   כעת עלינו להכיר כמה כלים מתמטיים שישמשו אותנו עבור ביצוע הרוטציה. 
  <br>
מטריצת הרוטציה (אם אני לא טועה, היא נלמדת בלינארית 2):  <br>

   
 <img src='images/rotationmatrix1.png' style="width: 40%; height: auto;"/> <br>
   
   במבט ראשון הכלי נראה לכאורה מושלם לביצוע הרוטציה שאנחנו צריכים עבור התמונה. בואו ננסה להשתמש בה.   <br>


ראשית נייבא את הספריות הדרושות לנו. 
 <br>
  <br>
<div dir="ltr"> 
{% highlight c++%}
#include <iostream>
#include <cmath>
#include "opencv2/opencv.hpp"
{% endhighlight %}
</div><br>

לאחר מכן נכתוב את התוכנית המרכזית להצגת התמונות: <br>

<div dir="ltr"> 
{% highlight cpp%}
int main() {
    cv::Mat img = cv::imread("../lion.jpeg");
    image_channels img_chan = RGB;
    //if(img.channels() < 3)
      //  img_chan = GRAYSCALE;

    cv::Mat rotatedImage(img.rows,img.cols,CV_8UC3);
    cv::Mat rotatedImage2(img.rows,img.cols,CV_8UC3);

    // Rotating
    RotationFunction(img,rotatedImage2,30,INTERPOLATION_NEAREST_NEIGHBOR,img_chan);
    // End of Rotating
    // Show the images

    NaiveRotation(img,rotatedImage,30);

    cv::imshow("window1",img);
    cv::imshow("window2",rotatedImage);
    cv::imshow("window3",rotatedImage2);
    cv::imwrite("myfile.jpeg",rotatedImage);
    cv::waitKey(0);
    // End of Show the images

    return 0;
}

{% endhighlight %}
</div><br>

וכעת נתבונן בפונקציית הרוטציה הנאיבית שלנו:

<div dir="ltr"> 
{% highlight cpp%}
   void NaiveRotation(cv::Mat src, cv::Mat dst, int angle) {
    double rotatedX;
    double rotatedY;

    double toRadian = 3.141592653589 / 180;

    // ---------------------------- RGB HANDLER ----------------------------
    for (int x = 0; x < src.cols; x++) {
        for (int y = 0; y < src.rows; y++) {

            rotatedX = round(x * cos(angle * toRadian) - y * sin(angle * toRadian));
            rotatedY = round(x * sin(angle * toRadian) + y * cos(angle * toRadian));

            cv::Point2i dstPixel((int) rotatedX, (int) rotatedY);

            // Checking if the Interpolation calculations crossed the boundaries
            if (dstPixel.x < 0 || dstPixel.x > src.cols - 1 || dstPixel.y < 0 || dstPixel.y > src.rows - 1)
                dst.at<cv::Vec3b>(cv::Point(x, y)) = 0;
            else { // In case everything is good
                dst.at<cv::Vec3b>(dstPixel) = src.at<cv::Vec3b>(cv::Point(x, y));
            }
        }
    }
}
{% endhighlight %}

</div>
   
   
   
   
   
   
   
   
   הדרך הנאיבית:
   הדרך הנאיבית, נשלח כל פיקסל באמצעות הטרנספורמציה שהדגמנו קודםם.
   
   
   
   
   
   
   <br>
לפי טריגונומטריה 
   
 


   
   
   

   
   
   
  </div>
  &alpha;
