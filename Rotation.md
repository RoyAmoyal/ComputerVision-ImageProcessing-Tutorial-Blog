---
title: "Let's Build Together a Computer Vision Library!"
---
<head>
<style>
.button {
  background-color: #00c2c2;
  border: none;
  color: white;
  padding: 3px 5px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 12px;
  margin: 1px 1px;
  cursor: pointer;
  font-weight : bold ;

}
</style>
</head>

<div dir="rtl">
   <h1 align=center>Affine Transformations</h1>
  <h2> Image Rotation </h2> 
    אז בחלק הזה נלמד איך לעשות רוטציות לתמונה. ראשית אציג את הדרך הנאיבית בה היווצרו חורים בתמונה (בשל הזזת הפיקסלים) ולאחר מכן נפתור את הבעייה באמצעות שימוש באינטרפולציה שעליה פירטתי בהרחבה בפוסט הבא <a href="index.md">interpolation</a>. <br> 
    <b>בחלק הבונוס:</b>
    נלמד איך לבצע את הרוטציה ע״י ממרכז התמונה על ידי שינוי ראשית הצירים שבדיפולט נמצאת בפינה השמאלית העליונה של התמונה.
      
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
   <br>
   <b>הערה:</b>
   בקוד הבא ביצענו רוטציה אך ורק לתמונות עם צבע, כאמור 
   BGR.
   בהמשך נדאג גם לבצע את הרוטציה לתמונות 
   Grayscale
   <br>
   <br>

    

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
<b> וזו התוצאה שנקבל:</b>
<br>
<img src='images/badlion.png' style="width: 90%; height: auto;"/> <br>
<br>
<b>
כמו שניתן לראות, אמנם הצלחנו לבצע את הרוטציה אבל אך לא הצלחנו לשמר את איכות התמונה ולמעשה נוצרו חורים בין הפיקסלים.
בעיה זו נקראת Aliasing.
</b>
<br>
<br>


<h4> הסבר לקוד: </h4>
<div dir="ltr">
{% highlight cpp%}
rotatedX = round(x * cos(angle * toRadian) - y * sin(angle * toRadian));
rotatedY = round(x * sin(angle * toRadian) + y * cos(angle * toRadian));
cv::Point2i dstPixel((int) rotatedX, (int) rotatedY);

{% endhighlight %}
</div>

למעשה באמצעות לולאה מקוננת נעבור על כל פיקסל בתמונה ונחשב לאן היא אמורה לעבור לתמונת יעד שלנו.
<br>
הערות:
<ol>
  <li>.כמו בחישוב במחשבון, נצטרך להעביר את הזווית שהמשתמש הכניס לרדיאנים</li>
  <li>מכיוון שפיקסלים מיוצגים במחשב ע״ מספרים טבעיים והחישוב שלנו עלול לתת ערכים חיוביים שאינם שלמים, נצטרך לעגל לערך הקרוב ביותר כדי לבחור את הפיקסל המתאים. (מתקשר לבעיית ה
  aliasing.</li>
</ol>

<div dir="ltr">
{% highlight cpp%}
if (dstPixel.x < 0 || dstPixel.x > src.cols - 1 || dstPixel.y < 0 || dstPixel.y > src.rows - 1)
                dst.at<cv::Vec3b>(cv::Point(x, y)) = 0;
            else { // In case everything is good
                dst.at<cv::Vec3b>(dstPixel) = src.at<cv::Vec3b>(cv::Point(x, y));
{% endhighlight %}
</div>

מכיוון שאנו מזיזים את התמונה, ישנם פיקסלים שערך הפיקסל החדש בתמונה היעד, יצא למעשה מגבולות התמונה, כלומר באופן מעשי הוא אמור להיעלם.
בפועל באופן דיפולטיבי ב
OpenCV 
, ברגע שמנסים להזין ערך לפיקסל מחצה את גבולות התמונה, למעשה הערך יוזן לפיקסל שנמצא בגבולות התמונה אליו נגיע באופן מעגלי מתחילת אותה העמודה או השורה אותה חצינו.
<br>
לדוגמה:



<br>
<br>
<h3>
כעת נפתור את הבעיה באמצעות
<a href="index.md">אינטרפולציה</a>
</h3>
<br>


<b> כמו קודם נייבא את הספריות הדרושות לנו: </b>
<div dir="ltr">
{% highlight c++%}
#include <iostream>
#include <cmath>
#include "opencv2/opencv.hpp"
{% endhighlight %}
</div><br>


<b>נשתמש ב
- enums
כדי להקל על בחירת איכות התמונה למשתמש</b>


<div dir="ltr">
{% highlight c++%}
using namespace std;

enum interpolation_type{
    INTERPOLATION_CUBIC,
    INTERPOLATION_LINEAR,
    INTERPOLATION_NEAREST_NEIGHBOR
};
{% endhighlight %}
</div><br>

<b> כעת נשתמש בפונקציות האינטרפולציה שלנו 
(ראו את 
<a href="index.md">המדריך</a>
לגביו) 
</b>
<button id="InterpolationButton" class="button" onclick="myFunction()">הראה קוד</button>
<div dir="ltr" style="display: none" id="InterpolationDiv">
{% highlight c++%}

void NearestNeighbor_Interpolation_Helper(const cv::Mat& src, cv::Mat& dst, const cv::Point2d& srcPoint, cv::Point2i& dstPixel)
{
    // Find Nearest Neighbor
    int NearestNeighborX = (int) round(srcPoint.x);
    int NearestNeighborY = (int) round(srcPoint.y);
    cv::Point NearestNeighborPixel(NearestNeighborX,NearestNeighborY);
    if (src.channels() > 1)
    { //RGB image
        if (NearestNeighborPixel.x < 0 || NearestNeighborPixel.x > src.cols - 1 || NearestNeighborPixel.y < 0 || NearestNeighborPixel.y > src.rows - 1)
            dst.at<cv::Vec3b>(dstPixel) = 0;
        else
            dst.at<cv::Vec3b>(dstPixel) = src.at<cv::Vec3b>(NearestNeighborPixel);
    }
    else// GrayScale Image
    {
        if (NearestNeighborPixel.x < 0 || NearestNeighborPixel.x > src.cols - 1 || NearestNeighborPixel.y < 0 || NearestNeighborPixel.y > src.rows - 1)
            dst.at<uchar>(dstPixel) = 0;
        else
            dst.at<uchar>(dstPixel) = src.at<uchar>(NearestNeighborPixel);
    }
}



void Linear_Interpolation_BGRandRGBHelper(const cv::Mat& src, cv::Mat& dst, const cv::Point2d& srcPoint, cv::Point2i& dstPixel)
{

    // Lets find the 4-Nearest Neighbors of our "landing" spot.
    // (r,c),(r,c+1),(r+1,c),(r+1,c+1)
    int leftUpperNeighborX = floor(srcPoint.x);
    int leftUpperNeighborY = floor(srcPoint.y);
    // (r,c)
    cv::Point2i leftUpperNeighbor(leftUpperNeighborX, leftUpperNeighborY);
    // (r,c+1)
    cv::Point2i leftBottomNeighbor(leftUpperNeighborX, leftUpperNeighborY + 1);
    // (r+1,c)
    cv::Point2i rightUpperNeighbor(leftUpperNeighborX + 1, leftUpperNeighborY);
    // (r+1,c+1)
    cv::Point2i rightBottomNeighbor(leftUpperNeighborX + 1, leftUpperNeighborY + 1);

    // ratioX = Alpha , ratioY = Beta
    // 0 <= ratioX,ratioY <= 1
    double Alpha = srcPoint.x - (double) leftUpperNeighborX;
    double Beta = srcPoint.y - (double) leftUpperNeighborY;

    if (leftUpperNeighbor.x < 0 || leftUpperNeighbor.x >= src.cols - 1 || leftUpperNeighbor.y < 0 || leftUpperNeighbor.y >= src.rows - 1)
        dst.at<cv::Vec3b>(dstPixel) = 0;  // ratio with the left upper neighbor
    else {
        int B = int((1 - Alpha) * (1 - Beta) * src.at<cv::Vec3b>(leftUpperNeighbor)[0] +
                    (1 - Alpha) * (Beta) * src.at<cv::Vec3b>(rightUpperNeighbor)[0] +
                    (Alpha) * (1 - Beta) * src.at<cv::Vec3b>(leftBottomNeighbor)[0] +
                    (Alpha) * (Beta) * src.at<cv::Vec3b>(rightBottomNeighbor)[0]);

        int G = int((1 - Alpha) * (1 - Beta) * src.at<cv::Vec3b>(leftUpperNeighbor)[1] +
                    (1 - Alpha) * (Beta) * src.at<cv::Vec3b>(rightUpperNeighbor)[1] +
                    (Alpha) * (1 - Beta) * src.at<cv::Vec3b>(leftBottomNeighbor)[1] +
                    (Alpha) * (Beta) * src.at<cv::Vec3b>(rightBottomNeighbor)[1]);

        int R = int((1 - Alpha) * (1 - Beta) * src.at<cv::Vec3b>(leftUpperNeighbor)[2] +
                    (1 - Alpha) * (Beta) * src.at<cv::Vec3b>(rightUpperNeighbor)[2] +
                    (Alpha) * (1 - Beta) * src.at<cv::Vec3b>(leftBottomNeighbor)[2] +
                    (Alpha) * (Beta) * src.at<cv::Vec3b>(rightBottomNeighbor)[2]);

        cv::Vec3b newVal(B, G, R);
        dst.at<cv::Vec3b>(dstPixel) = newVal;
    }
}


void Linear_Interpolation_GRAYHelper(const cv::Mat& src, cv::Mat& dst, const cv::Point2d& srcPoint, cv::Point2i& dstPixel)
{
    // Lets find the 4-Nearest Neighbors of our "landing" spot.
    // (r,c),(r,c+1),(r+1,c),(r+1,c+1)
    int leftUpperNeighborX = floor(srcPoint.x);
    int leftUpperNeighborY = floor(srcPoint.y);
    // (r,c)
    cv::Point2i leftUpperNeighbor(leftUpperNeighborX, leftUpperNeighborY);
    // (r,c+1)
    cv::Point2i leftBottomNeighbor(leftUpperNeighborX, leftUpperNeighborY + 1);
    // (r+1,c)
    cv::Point2i rightUpperNeighbor(leftUpperNeighborX + 1, leftUpperNeighborY);
    // (r+1,c+1)
    cv::Point2i rightBottomNeighbor(leftUpperNeighborX + 1, leftUpperNeighborY + 1);

    // ratioX = Alpha , ratioY = Beta
    // 0 <= ratioX,ratioY <= 1
    double Alpha = srcPoint.x - (double) leftUpperNeighborX;
    double Beta = srcPoint.y - (double) leftUpperNeighborY;

    if (leftUpperNeighbor.x < 0 || leftUpperNeighbor.x > src.cols - 1 || leftUpperNeighbor.y < 0 || leftUpperNeighbor.y > src.rows - 1)
        dst.at<uchar>(dstPixel) = 0;
    else {

        int greyValue = int((1 - Alpha) * (1 - Beta) * src.at<uchar>(leftUpperNeighbor) +
                            (1 - Alpha) * (Beta) * src.at<uchar>(rightUpperNeighbor) +
                            (Alpha) * (1 - Beta) * src.at<uchar>(leftBottomNeighbor) +
                            (Alpha) * (Beta) * src.at<uchar>(rightBottomNeighbor));

        dst.at<uchar>(dstPixel) = greyValue;


    }
}



void Interpolation_Calculator(const cv::Mat& src, cv::Mat& dst, const cv::Point2d& srcPoint, cv::Point2i& dstPixel, interpolation_type inter_type){
    // The origin pixels for the currPixel in the newImage depends on the interpolation type

        switch(inter_type) {
            case INTERPOLATION_NEAREST_NEIGHBOR: {
                NearestNeighbor_Interpolation_Helper(src,dst,srcPoint,dstPixel);
            }
            case INTERPOLATION_LINEAR:
                if(src.channels() > 1) //if its BGR/RGB Image (3 channel Image)
                    Linear_Interpolation_BGRandRGBHelper(src,dst,srcPoint,dstPixel);
                else // GrayScale/Binary Image
                    Linear_Interpolation_GRAYHelper(src,dst,srcPoint,dstPixel);
            case INTERPOLATION_CUBIC:
                if(src.channels() > 1) //if its BGR/RGB Image (3 channel Image)
                    Linear_Interpolation_BGRandRGBHelper(src,dst,srcPoint,dstPixel);
                else // GrayScale/Binary Image
                    Cubic_Interpolation_GRAYHelper(src,dst,srcPoint,dstPixel);
    }
}
{% endhighlight %}
</div><br>

<br>
<b>
נבצע את הרוטציה שלנו ע״י הטרנספורמציה ההופכית למטריצת הרוטציה:
</b>

<div dir="ltr">
{% highlight c++%}
void RotationFunction(const cv::Mat& src, cv::Mat& dst, int angle, interpolation_type inter_type) {
    // The pixels in the new image we want to find right origin pixel for his value.
    if (src.channels() != dst.channels())
        throw std::invalid_argument(
                "Source Image and Destination Image have different channels. (Probably one is greyscale and the other BGR/RGB)");

    double rotatedX;
    double rotatedY;

    double toRadian = 3.141592653589 / 180;

    for (int x = 0; x < dst.cols; x++) // We can use the width instead of the cols.
    {
        for (int y = 0; y < dst.rows; y++) // We cna use the height instead of the rows.
        {

            rotatedX = x * cos(angle * toRadian) + y * sin(angle * toRadian);
            rotatedY = x * (-sin(angle * toRadian)) + y * cos(angle * toRadian);

            cv::Point2d srcPoint(rotatedX, rotatedY);
            cv::Point2i dstPixel(x, y);
            Interpolation_Calculator(src, dst, srcPoint, dstPixel, inter_type);
        }
    }
}
{% endhighlight %}
</div><br>
<div id="space1"><br></div>

<b> לבסוף נכתוב את הקוד הדרוש לקריאה והצגת התמונות. </b>
<br>
<br>
<div dir="ltr">
{% highlight c++%}
int main() {
    cv::Mat img = cv::imread("../lion.jpeg");
    //if(img.channels() < 3)
      //  img_chan = GRAYSCALE;

    cv::Mat rotatedImage(img.rows,img.cols,CV_8UC3);

    RotationFunction(img,rotatedImage,30,INTERPOLATION_LINEAR);

    // Show the images
    cv::imshow("Original Image",img);
    cv::imshow("Rotated Image",rotatedImage);
    cv::waitKey(0);

{% endhighlight %}
</div><br>

<b> התוצאה שקיבלנו: </b>
<br>

אינטרפולציה לשכן הכי קרוב:
<figure>
    <img src='images/rotatedImageNearest.png' alt='missing' />
    <figcaption>   </figcaption>
</figure>

<br>
<br>
אינטרפולציה לינארית:
<figure>
    <img src='images/rotatedImageGood.png' alt='missing' />
    <figcaption>   </figcaption>
</figure>








</div>




<h4>Aliasing</h4>


   הדרך הנאיבית:
   הדרך הנאיבית, נשלח כל פיקסל באמצעות הטרנספורמציה שהדגמנו קודםם.
   
   
   
  
   
   
   <br>
לפי טריגונומטריה 
   
 


   
   asdasdasdasdasdsad
   

   
   
   
<script>
function myFunction() {
  var x = document.getElementById("InterpolationDiv");
  var space = document.getElementById("space1");
  var button = document.getElementById("InterpolationButton");
  if (x.style.display === "none") {
    x.style.display = "block";
    button.textContent = "הסתר קוד";
    space.style.display = "none";
  } else {
    x.style.display = "none";
    button.textContent = "הראה קוד";
    space.style.display = "block";
  }
}
</script>
