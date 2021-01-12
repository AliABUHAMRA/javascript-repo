# The Document Object Model

> مؤسف جدا! نفس القصة القديمة! بمجرد الانتهاء من بناء منزلك ، تلاحظ أنك قد تعلمت عن طريق الخطأ شيئًا كان يجب أن تعرفه حقًا - قبل أن تبدأ

> Friedrich Nietzsche, Beyond Good and Evil

![](.gitbook/assets/image%20%287%29.png)

عندما تفتح صفحة ويب في متصفحك ، يأخذ المتصفح نص HTML للصفحة ويفسرها ، تمامًا مثل ما فعله المحلل اللغوي لتحليل التطبيقات في الفصل  [Chapter 12](https://eloquentjavascript.net/12_language.html#parsing). ينشئ المستعرض نموذجًا لهيكل المستند ويستخدمه لعرض المستند على الشاشة.

عرض المستند هذا هو أحد الأدوات التي يمكن لتطبيق JavaScript الوصول إليها في وضع الحماية الخاص به. هيكل بيانات يمكن قراءته أو تغييره ؛ هيكل البيانات الحية: عند حدوث تغيير ، يتم أيضًا تحديث الشاشة المعروضة على الشاشة لتعكس التغييرات.

### Document structure

يمكنك اعتبار مستند HTML على أنه مجموعة من المستطيلات المتداخلة. تتضمن العلامات مثل  و &lt;/ body&gt; علامات أخرى تحتوي بدورها على علامات أو نصوص أخرى. على سبيل المثال ، ترى مستندًا من الفصل السابق:

```text
<!doctype html>
<html>
  <head>
    <title>My home page</title>
  </head>
  <body>
    <h1>My home page</h1>
    <p>Hello, I am Marijn and this is my home page.</p>
    <p>I also wrote a book! Read it
      <a href="http://eloquentjavascript.net">here</a>.</p>
  </body>
</html>
```

هيكل هذه الصفحة كما يلي:

![](.gitbook/assets/image%20%2816%29.png)

يتبع هيكل البيانات الذي يستخدمه المتصفح لعرض هذا المستند هذا النمط. لكل مستطيل ، هناك كائن يمكننا التفاعل معه للحصول على أشياء مثل علامة HTML الخاصة به أو العلامات والنص الذي يحتوي عليه.                                                                                  تسمى هذه الشاشة  _Document Object Model_، أو اختصارًا DOM. 

يسمح متغير المستند العام بالوصول إلى هذه الكائنات. تشير خاصية documentElement إلى الكائن الذي يمثل علامة &lt;html&gt; . نظرًا لأن كل مستند HTML يحتوي على رأس وجسم ، فإنه يحتوي أيضًا على خصائص رأس وجسم تشير إلى تلك العناصر.

### Trees

فكر في العودة إلى شجرة بناء الجملة من الفصل [Chapter 12](https://eloquentjavascript.net/12_language.html#parsing) للحظة. تشبه هياكلها بشكل لافت للنظر بنية مستند المتصفح. قد تشير كل node الى nodes اخرى , children, اي ربما يمكن ان يكون لهم اطفالهم childern. هذا الشكل عبارة عن بنية متداخلة نموذجية يمكن أن تحتوي فيها العناصر على عناصر فرعية متشابهة.

تسمى بنية البيانات tree عندما يكون لها فروع بدون دائرة \(قد لا تحتوي node على نفسها ، بشكل مباشر أو غير مباشر\), وله جذر واحد محدد جيدًا. في حالة DOM ، يعمل document.documentElement كجذر.

تظهر الأشجار كثيرًا في علوم الكمبيوتر. بالإضافة إلى تمثيل recursive structures مثل مستندات أو برامج HTML ، غالبًا ما يتم استخدامها للحفاظ على مجموعات البيانات المصنفة لأنه يمكن عادةً العثور على العناصر أو إدراجها بشكل أكثر كفاءة من المصفوفة المسطحة.

تحتوي الشجرة النموذجية على أنواع مختلفة من nodes. تحتوي شجرة بناء الجملة  [the Egg language](https://eloquentjavascript.net/12_language.html) على معرفات وقيم وعقد تطبيق. يمكن أن يكون nodes التطبيق أطفال ، بينما تتكون المعرفات والقيم من الأوراق ؛ يشير إلى nodesالتي ليس لها أطفالchildren .

نفس الإجراء ينطبق على DOM. تحدد العقد ، كعناصر تمثل علامات HTML ، بنية المستند. يمكن أن تحتوي هذه nodesعلى  child nodes . . مثال على هذه العقدة هو document.body. يمكن أن يكون بعض هؤلاء children nodes ورقية ، مثل أجزاء من النص أو عقد تعليق.

لكل كائن عقدة DOM خاصية تسمى nodeType تحتوي على كود \(رقمي\) يحدد نوع العقده اوnode.تحتوي العناصرعلى الكود1،تعرف على أنها خاصية ثابتة .ELEMENT\_NODE Text node,والتي تمثل جزءًا من النص في المستند لها الرمز3 \(`Node.TEXT_NODE`\). التعليقات لها رمز 8 \(`Node.COMMENT_NODE`\).

![](.gitbook/assets/image%20%2813%29.png)

الأوراق عبارة عن عقد نصية ، وتمثل الأسهم العلاقات بين العُقد.

### The standard <a id="standard"></a>

إن استخدام الرموز الرقمية المشفرة لتمثيل نوع العقد ليس شائعًا في JavaScript. في بقية هذا الفصل ، نرى أن الأجزاء الأخرى من واجهة DOM تخلق أيضًا إحساسًا غير مألوف. هذا لأن DOM ليس مصممًا لـ JavaScript فقط. بدلاً من ذلك ، تكون الواجهة محايدة اللغة بحيث يمكن استخدامها بشكل جيد على الأنظمة الأخرى - ليس فقط في HTML ولكن أيضًا في XML ، وهو تنسيق عام للبيانات ذات قواعد تشبه HTML.

وهذا أمر مؤسف. المعايير غالبا ما تكون مفيدة. ولكن في هذه الحالة ، فإن الميزة \(التناسق بين اللغات\) ليست مقنعة تمامًا.. سيوفر لك وجود واجهة مدمجة بشكل صحيح مع اللغة التي تستخدمها وقتًا أطول من وجود واجهة مألوفة عبر اللغات.

كمثال على هذا التكامل الضعيف ، ضع في اعتبارك خاصية childNodes التي تمتلكها elements nodes في DOM, . تحتوي هذه الخاصية على كائن مصفوفة له خاصية تسمى length ، بالإضافة إلى الخصائص التي تم تصنيفها بأرقام للوصول إلى العقد الفرعية. لكن هذا مثال على NodeList ، وليس مصفوفة حقيقية ، لذلك لا تحتوي على طرق مصفوفة مثل slice and map.

هناك أيضًا مشاكل في التصميم السيئ. على سبيل المثال ، لا توجد طريقة لإنشاء عقدة جديدة مع عقد فرعية وسمات في خطوة واحدة. بدلاً من ذلك ، يجب عليك أولاً إنشائه ثم إضافة الأطفال والسمات واحدة تلو الأخرى ، باستخدام التأثيرات الجانبية. تميل الشفرة التي تتفاعل بشكل كبير مع DOM إلى أن تصبح طويلة ومتكررة وقبيحة.

لكن هذه ليست مشاكل قاتلة. لأن JavaScript يسمح لنا بكتابة الملخصات الخاصة بنا ، يمكننا تصميم طرق أفضل لأداء عمليات برنامجك. توفر العديد من المكتبات التي تم إنشاؤها للبرمجة المتعلقة بالمتصفح هذه الأداة.

### Moving through the tree

تحتوي عُقد DOM على روابط عديدة لعقد أخرى قريبة. يوضح الرسم البياني التالي هذا:

![](.gitbook/assets/image%20%2821%29.png)

ومع ذلك ، يعرض هذا الرسم البياني ارتباطًا واحدًا 1  فقط لكل نوع, لكن كل node لها property تسمى parentNode  ، والتي ، إن وجدت ، تشير إلى node هو جزء منها. وبالمثل ، كل element node \(نوع العقدة 1\) له خاصية childNodes التي تشير إلى كائن يشبه المصفوفة يحمل اطفاله.

من الناحية النظرية ، يمكنك التحرك في أي مكان على الشجرة باستخدام الروابط بين parent وchild . لكن JavaScript يمنحك روابط ملائمة أخرى. تشير خصائص firstChild و lastChild إلى أول وآخر عنصر فرعي أو تحتوي على قيمة null للعقد التي ليس لها اطفال. PreviousSibling و nextSibling يشيران إلى nodes المجاورة ، وهي node تقع تحت أحد الوالدين المشتركين وتقع قبل العقدة أو بعدها مباشرة. بالنسبة ل firstChild ، سيكون الشقيق السابق\(previousSibling\) فارغًا ، وبالنسبة ل lastChild ، سيكون الأخ التالي \(nextSibling\) فارغًا.

هناك ايضا خاصيه تسمى children ,مشابهه لخاصيه childNodes ولكنه يحتوي فقط على العناصر الفرعية  \(النوع 1\).لا يشمل الأنواع الأخرى من العقد الفرعية. يمكن استخدام هذه الميزة عندما لا تحتاج إلى node text.

غالبًا ما تكون وظائف الاسترجاع مفيدة عند العمل باستخدام بنية بيانات متداخلة مثل هذه. تبحث دالة المثال التالي عن مستند للعثور على العقد النصية التي لها سلسلة محددة ، وترجع صحيحًا إذا تم العثور عليها.

```text
function talksAbout(node, string) {
  if (node.nodeType == Node.ELEMENT_NODE) {
    for (let child of node.childNodes) {
      if (talksAbout(child, string)) {
        return true;
      }
    }
    return false;
  } else if (node.nodeType == Node.TEXT_NODE) {
    return node.nodeValue.indexOf(string) > -1;
  }
}

console.log(talksAbout(document.body, "book"));
// → true
```

نظرًا لأن childNodes ليس مصفوفة حقيقية ، فلا يمكنك استخدام لـ for/of  للتنقل فيه ، ولكن يجب عليك إما استخدام حلقة for عادية أو استخدام Array.from.

تحتفظ خاصية nodeValue ل text node بسلسلة النص التي تمثلها node.

### Finding elements

غالبًا ما يكون التنقل في هذه الروابط بين الآباء والأطفال والأشقاء مفيدًا. ولكن إذا أردنا العثور على عقدة معينة في المستند ، فإن الوصول إليها من خلال البدء في document.body واتباع مسار ثابت للخصائص يعد فكرة سيئة. يؤدي القيام بذلك إلى وضع افتراضات في برنامجنا حول البنية الدقيقة للوثيقة - وهي بنية قد ترغب في تغييرها لاحقًا. عامل آخر معقد هو أن text nodes  يتم إنشاؤها حتى للمسافة البيضاء بين nodes.                                  على سبيل المثال ، في علامة  للمستند ، لا تحتوي على ثلاثة توابع فقط \( body ,h1, p\) ، ولكن في الواقع لديها سبعة أبناء. هذه العناصر الثلاثة جنبًا إلى جنب مع المساحات الفارغة قبل وبعد ورؤيتهم.

لذلك إذا أردنا الحصول على خاصية href للرابط في المستند ، فلا نريد أن يكون لدينا أمر مشابه لـ "الحصول على الطفل الثاني من الطفل السادس من نص المستند". سيكون من الأفضل أن يكون لدينا أمر مشابه لـ "الحصول على الرابط الأول في المستند". ويمكننا فعل ذلك.

```text
let link = document.body.getElementsByTagName("a")[0];
console.log(link.href);
```

تحتوي جميع elements nodes على طريقة تسمى getElementsByTagName تجمع كل elements  التي تم تصنيفها وهي مجموعة فرعية من تلك nodes \(الأطفال المباشرون أو غير المباشرون\) لتلك nodes وإرجاعهم ككائن يشبه المصفوفة.

للعثور على node معينه يمكننا منحها سمه id  ومنحها معرف يدعى document.getElementById بدل من ذلك بالاعلى :

```text
<p>My ostrich Gertrude:</p>
<p><img id="gertrude" src="img/ostrich.png"></p>

<script>
  let ostrich = document.getElementById("gertrude");
  console.log(ostrich.src);
</script>
```

والطريقه الثالثه المشابهه هي getElementsByClassName اي انها تشبه document.getElementById,  يبحث في محتويات  element node  ويسترجع جميع العناصر التي تحتوي على السلسلة المحددة في سمة class الخاصة بهم.

### Changing the document

يمكن تغيير كل شيء تقريبًا حول بنية بيانات DOM, يمكن تعديل شكل شجرة الوثيقة بتغيير العلاقات بين parent  وchild . ل nodes  طريقة تسمى remove يمكن استخدامها لإزالةnode من أصلها الحالي parent node.                                                لإضافةnode childإلى element node ، يمكننا استخدام appendChild, مما سوف يضعه في قائمه ال children.

 او استخدم  insertBefore  الذي يُدرج node كأول وسيطة قبل node كوسيطة ثانية.

```text
<p>One</p>
<p>Two</p>
<p>Three</p>

<script>
  let paragraphs = document.body.getElementsByTagName("p");
  document.body.insertBefore(paragraphs[2], paragraphs[0]);
</script>
```

يمكن أن توجد node في المستند في مكان واحد فقط. وبالتالي ، فإن إدخال الفقرة الثالثة أمام الفقرة الأولى سيؤدي أولاً إلى إزالتها من نهاية المستند ثم إدخالها امام الفقره الاولى في المقدمة ،مما يودي الى 3/2/1 . جميع العمليات التي تضع node في مكان ما كأثر جانبي تؤدي إلى إزالة تلك node من موقعها الحالي أولاً \(إذا كانت في مكان ما\)

يتم استخدام طريقة replaceChild لاستبدال a child node  بأخرى.  تأخذ هذه الطريقة tow nodes  كوسيطتين : new node  وnode  التي يجب استبدالها. يجب أن تكون ال node المستبدلة تابعة للعنصر الذي يتم استدعاء الطريقة عليه. لاحظ أن كلا من replaceChild و insertBefore توقع أن تكون node الجديدة الوسيطة الأولى.

### Creating nodes

لنفترض أننا نريد كتابة نص برمجي يستبدل جميع الصور الموجودة في المستند \(`<img>` \) بنص في سمة النص البديل alt الخاصة به. الذي يحدد تمثيلًا نصيًا بديلاً للصورة.

```text
<p>The <img src="img/cat.png" alt="Cat"> in the
  <img src="img/hat.png" alt="Hat">.</p>

<p><button onclick="replaceImages()">Replace</button></p>

<script>
  function replaceImages() {
    let images = document.body.getElementsByTagName("img");
    for (let i = images.length - 1; i >= 0; i--) {
      let image = images[i];
      if (image.alt) {
        let text = document.createTextNode(image.alt);
        image.parentNode.replaceChild(text, image);
      }
    }
  }
</script>
```

من خلال إعطاء سلسلة ، تمنحنا وظيفة createTextNode عقدة نصية يمكننا وضعها في المستند لعرضه على الشاشة.

الحلقة التي تمر فوق الصور تبدأ في نهاية قائمة المهام. يعد هذا ضروريًا لأن قائمة node التي يتم إرجاعها بواسطة طريقة مثل getElementsByTagName \(أو خاصية مثل childNodes\) هي قائمة مباشرة. أي أنه يتم تحديثه مع تغير المستند. إذا بدأنا من البداية ، فقد يؤدي حذف الصورة الأولى إلى فقدان القائمة لعنصرها الأول ، وفي هذه الحالة ستتوقف عن العمل بتكرار الحلقة مرة ثانية حيث i  يكون 1 ، سيتوقف لأن طول المجموعة أصبح الآن أيضًا 1.

```text
let arrayish = {0: "one", 1: "two", length: 2};
let array = Array.from(arrayish);
console.log(array.map(s => s.toUpperCase()));
// → ["ONE", "TWO"]
```

لإنشاء element node  ، يمكنك استخدام طريقة document.createElement.  تقوم هذه الطريقة بإرجاع اسم مميز وإرجاع node  فارغة من النوع المحدد.

في المثال أعلاه ، يتم إنشاء وظيفة تطبيق تسمى elt والتي تنشئ element node وتعتبر وسيطاتها الأخرى بمثابة توابع لتلك node . ثم يتم استخدام نفس الوظيفة لإضافة سمة إلى علامة اقتباس.

```text
<blockquote id="quote">
  No book can ever be finished. While working on it we learn
  just enough to find it immature the moment we turn away
  from it.
</blockquote>

<script>
  function elt(type, ...children) {
    let node = document.createElement(type);
    for (let child of children) {
      if (typeof child != "string") node.appendChild(child);
      else node.appendChild(document.createTextNode(child));
    }
    return node;
  }

  document.getElementById("quote").appendChild(
    elt("footer", "—",
        elt("strong", "Karl Popper"),
        ", preface to the second edition of ",
        elt("em", "The Open Society and Its Enemies"),
        ", 1950"));
</script>
```

### Attributes

يمكن الوصول إلى بعض سمات العناصر ، مثل href للروابط ، من خلال خاصية تحمل الاسم نفسه في كائن DOM الخاص بالعنصر. هذا هو الحال بالنسبة للسمات القياسية الأكثر استخدامًا.

لكن HTML يسمح لك بإضافة أي سمة تريدها إلى العقد. يمكن أن يكون هذا مفيدًا لأنه يسمح لك بتخزين المزيد من المعلومات في مستند واحد. ومع ذلك ، إذا قمت بإنشاء اسم السمة الخاص بك ، فلن تكون هذه السمات متاحة كخصائص كائن عقدة. يجب عليك استخدام أساليب getAttribute و setAttribute للعمل معهم.

```text
<p data-classified="secret">The launch code is 00000000.</p>
<p data-classified="unclassified">I have two feet.</p>

<script>
  let paras = document.body.getElementsByTagName("p");
  for (let para of Array.from(paras)) {
    if (para.getAttribute("data-classified") == "secret") {
      para.remove();
    }
  }
</script>
```

من المستحسن أن تبدأ في تسمية مثل هذه الخصائص الاصطناعية مع بيانات البادئة- للتأكد من أنها لا تتداخل مع الخصائص الأخرى.

هناك سمة شائعة الاستخدام ، calss، وهي كلمة رئيسية في لغة جافا سكريبت.لأسباب تاريخية - لم تتمكن بعض تطبيقات JavaScript القديمة من معالجة أسماء الخصائص المطابقة للكلمات الرئيسية - تُسمى الخاصية المستخدمة للوصول إلى هذه السمة className. يمكنك أيضًا الوصول إليه باسمه الحقيقي ، "class" ،                   باستخدام أساليب getAttribute و setAttribute.

### Layout

ربما لاحظت أن أنواعًا مختلفة من العناصر يتم وضعها بشكل مختلف. بعض ، مثل الفقرات \(p\) أو العناوين \(h1\) ، تشغل عرض المستند بالكامل ويتم تقديمها في أسطر منفصلة.       هذه تسمى عناصر الكتلة elements block. يتم عرض العناصر الأخرى ، مثل الروابط \(a\) أو العنصر\(strong\)  ، على نفس السطر مع النص المحيط بها. . تسمى هذه العناصر بالعناصر المضمنة elements inline.

لكل مستند معين ، يمكن للمتصفحات حساب مخطط بناءً على موضع وحجم كل عنصر. تم استخدام هذا التصميم لاحقًا لتصوير المستند.

يمكن الحصول على حجم العنصر وموضعه من خلال JavaScript. تُظهر خصائص offsetWidth و offsetHeight المساحة التي يشغلها عنصر بالبكسل. البكسل هو وحدة القياس الأساسية في المتصفح. تقليديا ، ترتبط هذه الوحدة بأصغر نقطة يمكن للشاشة التقاطها ، ولكن في الشاشات الحديثة القادرة على التقاط نقاط صغيرة جدًا ، لم تعد ذات صلة ، وقد يغطي بكسل المتصفح عدة نقاط على الشاشة.

وبالمثل ، يمنحك كل من clientWidth و clientHeight حجم المساحة داخل العنصر ، متجاهلين عرض الحدود.

```text
<p style="border: 3px solid red">
  I'm boxed in
</p>

<script>
  let para = document.body.getElementsByTagName("p")[0];
  console.log("clientHeight:", para.clientHeight);
  console.log("offsetHeight:", para.offsetHeight);
</script>
```

الطريقة الأكثر فعالية للحصول على الموضع الدقيق لعنصر ما على الشاشة هي استخدام طريقة getBoundingClientRect. تقوم هذه الطريقة بإرجاع كائن يحتوي على الخصائص top وbottom وleft  وright  التي تحدد موضع كل جانب من عناصر العنصر بالنسبة إلى أعلى ويسار الشاشة متعددة البكسل. إذا كنت بحاجة إلى هذه الأرقام للمستند بأكمله ، فأنت بحاجة إلى إضافة موضع التمرير الحالي إليه. يمكن الحصول على هذا الموضع في متغيرات pageXOffset و pageYOffset.

يمكن أن يكون تصميم مستند يتطلب الكثير من العمل. من أجل السرعة ، لا تقوم محركات المستعرض بإعادة تخطيط المستند فورًا في كل مرة تقوم فيها بتغييره ولكن تنتظر قدر الإمكان. عندما ينتهي تشغيل برنامج JavaScript غيّر المستند ، سيتعين على المستعرض حساب تخطيط جديد لرسم المستند الذي تم تغييره على الشاشة. عندما يطلب أحد البرامج موقع أو حجم شيء ما عن طريق قراءة خصائص مثل offsetHeight أو استدعاء getBoundingClientRect ، فإن توفير المعلومات الصحيحة يتطلب أيضًا حساب مخطط.

يفرض البرنامج الذي يتناوب بشكل متكرر بين قراءة معلومات تخطيط DOM وتغيير DOM الكثير من حسابات التخطيط ، وبالتالي سيعمل ببطء شديد. الكود التالي هو مثال على ذلك. يحتوي على برنامجين مختلفين يقومان ببناء سطر من أحرف X يبلغ عرضه 2000 بكسل ويقيس الوقت الذي يستغرقه كل منهما.

```text
<p><span id="one"></span></p>
<p><span id="two"></span></p>

<script>
  function time(name, action) {
    let start = Date.now(); // Current time in milliseconds
    action();
    console.log(name, "took", Date.now() - start, "ms");
  }

  time("naive", () => {
    let target = document.getElementById("one");
    while (target.offsetWidth < 2000) {
      target.appendChild(document.createTextNode("X"));
    }
  });
  // → naive took 32 ms

  time("clever", function() {
    let target = document.getElementById("two");
    target.appendChild(document.createTextNode("XXXXX"));
    let total = Math.ceil(2000 / (target.offsetWidth / 5));
    target.firstChild.nodeValue = "X".repeat(total);
  });
  // → clever took 1 ms
</script>
```

### Styling

لقد رأينا أن عناصر HTML المختلفة يتم رسمها بشكل مختلف. يتم عرض بعضها على شكل blocks ، والبعض الآخر inline . يضيف البعض نمطًا —  يجعل المحتوى غامقًا و  يجعله أزرق ويضع خطًا تحته.

الطريقة التي تعرض بها علامة  صورة &lt;img &gt; أو علامة  تتسبب في اتباع ارتباط&lt;a&gt; عند النقر عليه مرتبطة بشدة بنوع العنصر \(element type\), ولكن يمكننا تغيير التصميم المرتبط بالعنصر ،مثل لون النص أو التسطير.

```text
<p><a href=".">Normal link</a></p>
<p><a href="." style="color: green">Green link</a></p>
```

يمكن أن تحتوي خاصية النمط على تعريف واحد أو أكثر يتضمن خاصية \(such as color\) تأتي بنقطتين وقيمته. عندما يكون هناك أكثر من تصريح واحد ، يجب فصل كل إعلان بفاصلة منقوطة ، مثل "color: red؛ border: none".

يمكن أن تتأثر الكثير من جوانب المستند بالتصميم. على سبيل المثال ، تتحكم خاصية display في ما إذا كان سيتم عرض العنصر ككتلة أو عنصر مضمن. هذا النص أنا

```text
This text is displayed <strong>inline</strong>,
<strong style="display: block">as a block</strong>, and
<strong style="display: none">not at all</strong>.
```

تنتهي علامة الحظر في سطر خاص بها لأن عناصر الحظر لا يتم عرضها في السطر بجوار النص المحيط بها. لا يتم عرض العلامة الأخيرة على الإطلاق - عرض: لا شيء يمنع عنصر من الظهور على الشاشة. هذه طريقة لإخفاء العناصر. يُفضل عادةً استخدام هذه الطريقة بدلاً من إزالة عنصر تمامًا من المستند لأنه من الأسهل استعادته في المستقبل.

يمكن أن تعالج تعليمات JavaScript البرمجية نمط عنصر بشكل مباشر من خلال خاصية نمط العنصر. تحتوي هذه الخاصية على كائن له خصائص كل خصائص النمط الممكنة. قيم هذه الخصائص هي سلاسل ، يمكننا الكتابة إليها لتغيير جانب معين من نمط العنصر.

```text
<p id="para" style="color: purple">
2
  Nice text
3
</p>
4
​
5
<script>
6
  let para = document.getElementById("para");
7
  console.log(para.style.color);
8
  para.style.color = "magenta";
9
</script>
```

تحتوي بعض أسماء خصائص النمط على واصلات ، مثل مجموعة الخطوط. نظرًا لأن أسماء الخصائص هذه يصعب التعامل معها في JavaScript \(يجب أن تقول style \["font-family"\]\) ، تتم إزالة واصلات أسماء الخصائص في كائن النمط لهذه الخصائص وتكتب الأحرف التي تليها بأحرف كبيرة \(style.fontFamily\).

### Cascading styles

يسمى نظام التصميم الخاص بـ HTML بـ CSS ، لأوراق الأنماط المتتالية. ورقة الأنماط هي مجموعة من القواعد لكيفية تصميم العناصر في المستند. يمكن وضعها داخل علامة&lt;style&gt; .

```text
<style>
  strong {
    font-style: italic;
    color: gray;
  }
</style>
<p>Now <strong>strong text</strong> is italic and gray.</p>
```

 ا_cascading_   تعني أن القواعد الهرمية يتم دمجها لإنتاج النمط النهائي للعنصر. في المثال أعلاه ، النمط الافتراضي للعلامات هو &lt;strong&gt;,  حيث تم تعريف font-weight: bold ، تم تغييرها بواسطة أمر آخر في العلامة&lt;style&gt;  ونمط الخط واللون المضافة.

عندما تحدد قواعد متعددة قيمة لنفس الخاصية ، فإن أحدث قاعدة تمت قراءتها لها أولوية أعلى وتفوز.لذلك إذا تضمنت القاعدة في العلامة  وزن الخط: عادي ، بما يتعارض مع قاعدة وزن الخط الافتراضية ، فسيكون النص عاديًا وليس غامقًا. الأنماط في سمة النمط المطبقة مباشرةً على العقدة لها الأسبقية الأعلى وتفوز دائمًا.

من الممكن استهداف أشياء أخرى غير أسماء العلامات في قواعد CSS. تنطبق قاعدة .abc على جميع العناصر التي تحتوي على "abc" في سمة الفئة الخاصة بها. تنطبق قاعدة \#xyz على العنصر بسمة معرف "xyz" \(والتي يجب أن تكون فريدة داخل المستند\).

```text
.subtle {
  color: gray;
  font-size: 80%;
}
#header {
  background: blue;
  color: white;
}
/* p elements with id main and with classes a and b */
p#main.a.b {
  margin-bottom: 20px;
}
```

تنطبق قاعدة الأسبقية التي تفضل أحدث قاعدة معرّفة فقط عندما يكون للقواعد نفس الخصوصية. خصوصية القاعدة هي مقياس لمدى دقة وصفها لعناصر المطابقة ، والتي يتم تحديدها من خلال عدد ونوع \(class or id\) لجوانب العنصر التي تتطلبها. على سبيل المثال ، القاعدة التي تستهدف p.a هي أكثر تحديدًا من القواعد التي تستهدف p أو فقط .a وبالتالي ستكون لها الأسبقية عليها.

يطبق التدوين p&gt; a {…} الأنماط المحددة على جميع  العلامات &lt;a&gt; التي تعتبر فرعية مباشرة لعلامات&lt;p&gt; . وبالمثل ، ينطبق p a {…} على جميع  العلامات داخل علامات  ، سواء كانت تابعة مباشرة &lt;p&gt; أو غير مباشرة.

### Query selectors

لن نستخدم الكثير من أوراق الأنماط في هذا الكتاب. يعد فهمها مفيدًا للبرمجة في المتصفح ، ولكن نطاق مناقشة الأسلوب واسع جدًا لدرجة أنها تتطلب كتابًا منفصلاً.

السبب الرئيسي الذي دفعني إلى تقديم صيغة المحدد - التدوين المستخدم في أوراق الأنماط لتحديد العناصر التي تنطبق عليها مجموعة من الأنماط - هو أنه يمكننا استخدام نفس اللغة المصغرة كطريقة فعالة للعثور على عناصر DOM.

يستقبل الأسلوب querySelectorAll ، الموجود في كل من كائن document و element node ، سلسلة محدد ويعيد NodeList‍ الذي يحتوي على جميع العناصر المطابقة.

```text
<p>And if you go chasing
  <span class="animal">rabbits</span></p>
<p>And you know you're going to fall</p>
<p>Tell 'em a <span class="character">hookah smoking
  <span class="animal">caterpillar</span></span></p>
<p>Has given you the call</p>

<script>
  function count(selector) {
    return document.querySelectorAll(selector).length;
  }
  console.log(count("p"));           // All <p> elements
  // → 4
  console.log(count(".animal"));     // Class animal
  // → 2
  console.log(count("p .animal"));   // Animal inside of <p>
  // → 2
  console.log(count("p > .animal")); // Direct child of <p>
  // → 1
</script>
```

على عكس الأساليب مثل getElementsByTagName ، فإن الكائن الذي تم إرجاعه بواسطة querySelectorAll ليس مباشرًا. لن يتغير عند تغيير المستند. ومع ذلك ، فهي لا تزال غير مصفوفة حقيقية ، لذلك لا تزال بحاجة إلى استدعاء Array.from إذا كنت تريد التعامل معها على أنها واحدة.

تعمل طريقة querySelector \(بدون جزء الكل\) بطريقة مماثلة. هذا مفيد إذا كنت تريد عنصرًا واحدًا محددًا. سيعود فقط العنصر المطابق الأول أو القيمة الفارغة عند عدم تطابق أي عنصر.

### Positioning and animating <a id="animation"></a>







