---
title: "Let's Build Together a Computer Vision Library!"
---
<head>
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-213391535-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'UA-213391535-1');
</script>
</head>

<div dir="rtl" style="font-size:19px;">
   <h1 align=center>Basics</h1>
  <h2 align=center><u> Necessary math for taking these tutorials </u></h2> 
  לכל חלק במדריכים אשתדל לכוון אילו קורסים מתמטיים אקדמאיים תצטרכו עבורו. 
  <br>
  <ul>
  <li> <u><b>Image Processing: Affine Transformations</b></u> -  הקורס אלגברה לינארית 1 אמור להספיק אך רצוי להיות לאחר אלגברה 2.</li>
  <li> <u><b>Image Processing: Histogram Equalization</b></u> - אינפי/חדווא 1-2, הסתברות/מבוא להסתברות למדעי המחשב.</li>
  <li> Computer Vision: Optical Flow - TODO</li>
</ul>
   <br>

  <h2> Basics methods and objects of OpenCV </h2> 
  כדי להתמקד בחלקים הרלוונטים בכל מדריך בבלוג, שחלקם גם ככה לא פשוטים, נשתמש בפונקציות בסיסיות ואובייקטים בסיסיים של
  OpenCV. 
  <br>
  בכל מדריך אמנע משימוש ב- 
 <mark style="background-color:rgba(220, 220, 220,0.6)">
  using namespace cv;
</mark> 
 זאת על מנת להקל עליכם להבין מתי אנחנו משתמשים באובייקטים ובפונקציות של הספרייה ומתי אנו רושמים קוד אותנטי שלנו.
  <br><br>
  
  <b>הערה:</b> אל תטעו, תהיה לנו מספיק עבודה קשה גם במתמטיקה וגם בקוד כדי לבצע את הנדרש.
  <br>
    <h2><u><b> האובייקטים: </b></u></h2> 
  <ul>
  <li><b><u>Mat</u></b>- למעשה זוהי המטריצה שתאכסן לנו את התמונה. בהתאם לסוג התמונה שנרצה לאחסן, נצטרך להגדיר את המטריצה שלנו בהתאם. 
  לדוגמה אם המטריצה מכילה תמונת
  Grayscale, 
  כלומר לכל פיקסל-תא במטריצה, יש ערך אחד ויחיד של עוצמת הצבע בין 0-255, אך אם היא תכיל תמונת 
  BGR/RGB
  כל תא במטריצה יכיל וקטור באורך 3 שכל מקום בו יאחסן את הערך המתאים של עוצמת הצבע הכחול/אדום/ירוק לכל פיקסל.</li>
<br>
  <li> <u><b>imread(ImgPath)</b></u> -  פונקציית קריאת התמונה שלנו מהמחשב. היא תמיד תקבל את מיקום התמונה במחשב אותה אנחנו רוצים לקרוא ובאופן דיפולטיבי, היא תקרא את התמונה בפורמט 
  BGR.
  <ul>
      <li><b>imread(ImgPath,IMREAD_GRAYSCALE)</b>- קריאת התמונה ב GrayScale</li>
    </ul>
   </li>
  <br>
  <li><b><u>imshow("Window Name",img)</u></b> - פונקציית הצגת התמונה שלנו במסך. היא תמיד תקבל את שם החלון החדש הנפתח במחשב עבור התמונה ואובייקט מסוג
  Mat 
  שמכיל לנו את התמונה אותה אנחנו רוצים להציג.
   <ul>
      <li><b>waitKey(time)</b> - כדי להצליח להציג את התמונה, נצטרך להשתמש בפונקציה הזאת לאחר 
      imshow. 
      המשתנה 
      time 
      בעצם קובע לכמה זמן להציג את התמונה במילי-שניות.
      <br>
      אם נרצה להציג את התמונה ללא מגבלת זמן עד לחיצת מקש במקלדת נזין את הערך 0.
      אם נרצה להציג למשך מילי-שנייה את התמונה אז נשים את הערך 1. (יהיה רלוונטי כאשר נעבוד על סרטונים בהם נצטרך להציג כל את הפריימים של הסרטון אחד לאחר השני כדי למעשה להציג את הסרטון עצמו.</li>
    </ul>
  
  </li>
  <br>
  <li>wait - פונקציית הצגת התמונה שלנו במסך. היא תמיד תקבל את שם החלון החדש הנפתח במחשב עבור התמונה ואובייקט מסוג
  Mat 
  שמכיל לנו את התמונה אותה אנחנו רוצים להציג.</li>
  <li>Computer Vision: Optical Flow*- TODO</li>
</ul>








  </div>