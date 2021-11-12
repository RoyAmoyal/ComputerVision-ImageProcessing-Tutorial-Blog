---
title: "Let's Build Together a Computer Vision Library!"
---

<div dir="rtl">
   <h1 align=center>Affine Transformations</h1>
  <h2> Image Rotation </h2> 
    אז בחלק הזה נלמד איך לעשות רוטציות לתמונה. ראשית אציג את הדרך הנאיבית בה היווצרו חורים בתמונה (בשל הזזת הפיקסלים) ולאחר מכן נפתור את הבעייה באמצעות שימוש באינטרפולציה שעליה פירטתי בהרחבה בפוסט הבא <a href="index.md">interpolation</a>. <br> 
    <b>בחלק הבונוס:</b>
    נלמד איך לבצע את הרוטציה ע״י שינוי ראשית הצירים לבמרכז התמונה מראשית הצירים המקורית של התמונה הנמצאת בפינה השמאלית העליונה של התמונה.
  
      
 <figure>
    <img src='lions1.jpeg' alt='missing' />
    <figcaption> רוטציה  </figcaption>
</figure>
  
  <h3> תאוריה </h3>
   כשמבצעים רוטציה לתמונה מבצעים את הרוטציה בזווית &alpha; שבדרך מתקבלת כפרמטר מהמשתמש. <br>
   לדגומה בתמונה הבאה, אנחנו מבצעים רוטציה בזווית &alpha; מהפיקסל (u,v) אל הפיקסל (x,v).


   <img src='https://user-images.githubusercontent.com/69425073/138815394-abd74c44-6e61-4942-a842-86e06a214842.png' alt='missing' style="width: 30%; height: auto;" />
        <br>
   
   כעת עלינו להכיר כמה כלים מתמטיים שישמשו אותנו עבור ביצוע הרוטציה. 
  <br>
מטריצת הרוטציה (ככל הנראה נתקלתם באזכור שלה באחד מקורסי הלינארית): 
 <br><br>

   
 <img src='images/rotationmatrix1.png' style="width: 40%; height: auto;"/> 
 <br>
  <br>
   באופן כללי עבור הזזת הפיקסל 
   (x,y):
<br>
  <img src='images/rotationmatrix3.png' style="width: 80%; height: auto;"/> 
  <br>
  <img src='images/rotationmatrix2.png' style="width: 70%; height: auto;"/> 

   <br><br>
    <b> לכאורה </b> 
  במבט ראשון הכלי נראה 
   מושלם לביצוע הרוטציה שאנחנו צריכים עבור התמונה. בואו ננסה להשתמש בה. 
   <br><br>
    

<b> ראשית נייבא את הספריות הדרושות לנו: </b>
<div dir="ltr">
{% highlight c++%}
#include <iostream>
#include <cmath>
#include "opencv2/opencv.hpp"
{% endhighlight %}
</div><br>

<b>
לאחר מכן נכתוב את התוכנית המרכזית להצגת התמונות: (אם משהו לא מובן בחלק זה, נא לקרוא את
  <a href="https://royamoyal.github.io/ComputerVision-ImageProcessing-Tutorial-Blog/Basics.html">המדריך הבסיסי לבלוג</a>
)
</b>
<div dir="ltr"> 
<br>
{% highlight cpp%}
int main() {
    cv::Mat img = cv::imread("../lion.jpeg");
    
    cv::Mat rotatedImage(img.rows,img.cols,CV_8UC3);

    // Rotating
    NaiveRotation(img,rotatedImage,30);
    // End of Rotating

    // Show the images
    cv::imshow("window1",img);
    cv::imshow("window2",rotatedImage);
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
</div><br>

<b> וזו התוצאה שנקבל:<b>
<br>
<img src='images/badlion.png' style="width: 90%; height: auto;"/> <br>


כמו שניתן לראות, אמנם הצלחנו לבצע את הרוטציה אבל אך לא הצלחנו לשמר את איכות התמונה ולמעשה נוצרו חורים בין הפיקסלים.
בעיה זו נקראת Aliasing.
<br>
<br>


<h4> :הסבר לקוד </h4>
{% highlight cpp%}
rotatedX = round(x * cos(angle * toRadian) - y * sin(angle * toRadian));
rotatedY = round(x * sin(angle * toRadian) + y * cos(angle * toRadian));
cv::Point2i dstPixel((int) rotatedX, (int) rotatedY);

{% endhighlight %}

למעשה באמצעות לולאה מקוננת נעבור על כל פיקסל בתמונה ונחשב לאן היא אמורה לעבור לתמונת יעד שלנו.
<br>
<ol>
הערות:
  <li>.כמו בחישוב במחשבון, נצטרך להעביר את הזווית שהמשתמש הכניס לרדיאנים</li>
  <li>מכיוון שפיקסלים מיוצגים במחשב ע״ מספרים טבעיים והחישוב שלנו עלול לתת ערכים חיוביים שאינם שלמים, נצטרך לעגל לערך הקרוב ביותר כדי לבחור את הפיקסל המתאים. (מתקשר לבעיית ה
  aliasing.</li>
</ol>


{% highlight cpp%}
if (dstPixel.x < 0 || dstPixel.x > src.cols - 1 || dstPixel.y < 0 || dstPixel.y > src.rows - 1)
                dst.at<cv::Vec3b>(cv::Point(x, y)) = 0;
            else { // In case everything is good
                dst.at<cv::Vec3b>(dstPixel) = src.at<cv::Vec3b>(cv::Point(x, y));
{% endhighlight %}

מכיוון שאנו מזיזים את התמונה, ישנם פיקסלים שערך הפיקסל החדש בתמונה היעד, יצא למעשה מגבולות התמונה, כלומר באופן מעשי הוא אמור להיעלם.
בפועל באופן דיפולטיבי ב
OpenCV 
, ברגע שמנסים להזין ערך לפיקסל מחצה את גבולות התמונה, למעשה הערך יוזן לפיקסל שנמצא בגבולות התמונה אליו נגיע באופן מעגלי מתחילת אותה העמודה או השורה אותה חצינו.
<br>
לדוגמה:







<h4>Aliasing</h4>


   הדרך הנאיבית:
   הדרך הנאיבית, נשלח כל פיקסל באמצעות הטרנספורמציה שהדגמנו קודםם.
   
   
   
  
   
   
   <br>
לפי טריגונומטריה 
   
 


   
   
   

   
   
   
