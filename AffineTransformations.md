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
מטריצת הרוטציה (אם אני לא טועה אותה היא נלמדת בלינארית 2):

   
 <img src='images/rotationmatrix1.png' style="width: 60%; height: auto;"/> <br>
   
   במבט ראשון הכלי נראה לכאורה מושלם לביצוע הרוטציה שאנחנו צריכים עבור התמונה. בואו ננסה להשתמש בה. 
   
  <br>
   
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
