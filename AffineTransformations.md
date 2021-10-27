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
   <img src='https://user-images.githubusercontent.com/69425073/138815394-abd74c44-6e61-4942-a842-86e06a214842.png' alt='missing' style="width: 70%; height: auto;"
        <br>
   
   כעת נבצע את הרוטציה בפועל:
   ראשית עלינו להכיר כמה כלים מתמטיים שישמשו אותנו עבור ביצוע הרוטציה. 
  <br>
מטריצת הרוטציה (אם אני לא טועה אותה מלמדים בלינארית 2):

 <img src='images/rotationmatrix1.png'/>  
   
   
   הדרך הנאיבית, נשלח כל פיקסל באמצעות הטרנספורמציה שהדגמנו קודם וכפי שהסברנו, נשים לב שנקבל חורים.
   
   
   
   
   
   
        />
   <br>
לפי טריגונומטריה 
   
 


   
   
   

   
   
   
  </div>
  &alpha;
