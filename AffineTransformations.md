---
title:  "Roy's Computer Vision Library Blog"
---
<style> h1 {text-align: center;} </style>

<div dir="rtl">
   <h1 align=center>Affine Transformations </h1>
  <h2> Image Rotation </h2>
    אז בחלק הזה נלמד איך לעשות רוטציות לתמונה. ראשית אציג את הדרך הנאיבית בה היווצרו חורים בתמונה (בשל הזזת הפיקסלים) ולאחר מכן נפתור את הבעייה באמצעות שימוש באינטרפולציה שעליה פירטתי בהרחבה בפוסט הבא <a href="index.md">interpolation</a>. <br> 
  
      
 <figure>
    <img src='lions1.jpeg' alt='missing' />
    <figcaption> יובל יאריה </figcaption>
</figure>
  
   
   
  <h3> תאוריה </h3>
   כשמבצעים רוטציה לתמונה מבצעים את הרוטציה בזווית a שבדרך מתקבלת כפרמטר מהמשתמש. <br>
   לדגומה בתמונה הבאה, אנחנו מבצעים רוטציה בזווית a מהפיקסל (u,v) אל הפיקסל (x,y).
   
   <img src='https://user-images.githubusercontent.com/69425073/138815394-abd74c44-6e61-4942-a842-86e06a214842.png' alt='missing' style="width: 70%; height: auto;" />


   
   
   

   
   
   
  </div>
