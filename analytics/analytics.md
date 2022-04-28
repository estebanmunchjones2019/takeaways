How to get access to old style analytics dashboard?

When adding a new Analytics, check **advanced options** when creating a new page, and use the old version **"universal Analytics"**. Then go to **firebase** and **disconnect the analytics linked to the project**, and in the index.html, **add the script**, and **take out the firebase.analytics() call**.

````html
 //index.html
 
 <!-- Global site tag (gtag.js) - Google Analytics -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-FJLEQ7EM3N"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());

    gtag('config', 'G-FJLEQ7EM3N');
  </script>
````

