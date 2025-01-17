---
title: "Как реализовать дизайн сайта, который будет выглядеть на любых экранах так, как ты задумывал"
createdAt: 2023-07-20T12:18:00+03:00

head:
  meta:
    - name: 'author'
      content: 'Александр Вокалёк'
    - name: 'keywords'
      content: 'резиновый дизайн сайта, адаптивный дизайн сайта'
---

Существует множество подходов для вёрстки современных сайтов, а&nbsp;также их&nbsp;вариации. Подробнее об&nbsp;этом можно почитать&nbsp;в [статье MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design). Подытожу, что всё сводится к&nbsp;использованию брейкпоинтов через медиавыражения и&nbsp;резиновому лейауту, а&nbsp;также их&nbsp;комбинации.

Однако существуют конкретные практические подходы для достижения адаптивности у&nbsp;фронтендеров, и&nbsp;чаще всего это css-фреймворки, либо&nbsp;же устоявшиеся техники.

Проблематика при проектировании таких систем кроется в&nbsp;том, что у&nbsp;каждого разработчика есть своё виденье адаптивности, когда дело доходит до&nbsp;переноса дизайна из&nbsp;картинки на&nbsp;устройство. Разработчику дают макет, состоящий из&nbsp;страниц сайта. Каждая страница скорее всего имеет десктопную версию и&nbsp;мобильную. Допустим, 1440px и&nbsp;375px соответственно.

![Пример дизайна одной страницы сайта в&nbsp;Figma](/static/articles/adaptive-design/page-example.png){width="926" height="539"}

Иногда от&nbsp;дизайнера требуют промежуточные варианты отображения по&nbsp;ширине устройства, но&nbsp;чаще всего никто таким, к&nbsp;счастью, не&nbsp;занимается.

Дальше перед разработчиком встаёт дилемма, как макет должен отображаться под при изменении разрешения. Кто-то использует брейкпоинты, кто-то резинит макет, кто-то и&nbsp;то, и&nbsp;другое. Кто-то делает mobile first, кто-то предпочитает показать сначала готовую десктопную версию менеджеру и&nbsp;затем возиться с&nbsp;мобилкой. Подходы и&nbsp;рассуждения могут меняться в&nbsp;зависимости от&nbsp;продукта и&nbsp;системы, под которую он&nbsp;собирается.

В&nbsp;этой статье я&nbsp;сразу постараюсь применять универсальный подход для достижения адаптивности и&nbsp;добьюсь того уровня адаптивности, который прекрасно будет работать на&nbsp;абсолютно любых устройствах и&nbsp;экранах, которые будут появляться в&nbsp;дальнейшем.

## Принципы

### Deskbile first

Мобильные устройства это обыденность, они есть у&nbsp;любого. Мобильный трафик на&nbsp;сайтах составил [60% в&nbsp;2022 году](https://www.allconnect.com/blog/mobile-vs-desktop) и&nbsp;его доля будет только увеличиваться.

Но&nbsp;даже зная это, нельзя выбирать &laquo;что верстать первым&raquo;. Верстать следует сразу десктоп и&nbsp;мобилку, без вариантов, по&nbsp;блокам, двигаясь сверху вниз. Плюсы в&nbsp;том, что при таком подходе генерируется меньше багов при вёрстке путём правильного формирования базы для дизайна при первых шагах проектирования. Минусы состоят в&nbsp;том, что менеджеру придётся объяснять рациональность этого подхода, при котором первую страницу он&nbsp;увидит позже, чем он&nbsp;думает. Но&nbsp;зато дальше пойдёт быстрее и&nbsp;в&nbsp;целом это экономия времени, а&nbsp;не&nbsp;его трата.

### Резиновый &gt; отзывчивый

Потому что это по&nbsp;умолчанию более лёгкая для реализации вариация дизайна, которая обеспечивает лучшую адаптивность для самых разных лейаутов.

Брейкпоинты и&nbsp;медиавыражения используются для пограничных случаев или когда дизайнер явно даёт понять, что блок обязан изменить своё представление на&nbsp;мобильное.

Например, самый популярный css фреймворк Tailwind работает с&nbsp;фиксированными размерами по&nbsp;брейкпоинтам с&nbsp;использованием [единицы измерения rem](https://www.freecodecamp.org/news/what-is-rem-in-css/). Подход, который я&nbsp;презентую, будет несколько другим и&nbsp;базируется именно на&nbsp;резиновости нежели чем на&nbsp;отзывчивости.

![Поиск по&nbsp;ключевому слову &laquo;fluid&raquo; на&nbsp;сайте css-фреймворка Tailwind](/static/articles/adaptive-design/tailwind-fluid.png){width="789" height="305"}

### Rem hell

Прекрасная статья &laquo;[Don&rsquo;t use rem/em for paddings, margins and more](https://medium.com/@sascha.wolff/dont-use-rem-em-for-paddings-margins-and-more-94e19026b000)&raquo; расскажет вам, почему rem для отступов и&nbsp;всего прочего это плохо, какие проблемы с&nbsp;адаптивностью и&nbsp;доступностью там есть. Я&nbsp;лишь дополню, что работать с&nbsp;rem крайне неудобно, ты&nbsp;постоянно обязан что-то высчитывать, переводить пиксели из&nbsp;дизайна в&nbsp;rem. Tailwind, о&nbsp;котором я&nbsp;говорил ранее, использует базовые 16px для html, которые затем высчитывать бывает проблематично и&nbsp;не&nbsp;думаю, что кто-то радовался от&nbsp;этого процесса. А&nbsp;смена базы на&nbsp;10px для html несёт другие свои проблемы для доступности. Поэтому rem я&nbsp;не&nbsp;использую.

![Вы точно не&nbsp;получите удовольствие от&nbsp;адаптации стилей и&nbsp;заучивании значений](/static/articles/adaptive-design/rem-hell.png){width="409" height="715"}

## Реализация

Дизайн система для разработчика должна быть понятной, легка в&nbsp;использовании, обеспечивать адаптивность и&nbsp;доступность.

Для обеспечения этих условий и&nbsp;выше оговорённых принципов, я&nbsp;использую [единицу измерения vw](https://www.freecodecamp.org/news/learn-css-units-em-rem-vh-vw-with-code-examples/#what-are-vw-units), которая подходит для всего наилучшим образом, кроме обеспечения доступности. Но&nbsp;об&nbsp;этом позже.

Есть замечательная [css функция](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp) `clamp()`{lang="css"}, которая занимается ограничением вычисляемого значения для свойства в&nbsp;верхней и&nbsp;нижней границе. В&nbsp;[этой статье](https://www.smashingmagazine.com/2022/01/modern-fluid-typography-css-clamp/) демонстрируют работу с&nbsp;этой функцией для достижения резиновой типографии. Рассмотрим её&nbsp;вкратце.

```css
clamp({мин. значение}, {вычисляемая функция}, {макс. значение})
```

_Минимальное и&nbsp;максимальное значение_ &mdash; это ограничитель для вычисляемой функции.

_Вычисляемая функция_ &mdash; специально подобранная функция, которая будет вычислять значение свойства для каждого размера экрана, например, от&nbsp;375px до&nbsp;1440px.

Как подобрать эту вычисляемую функцию? Посмотрим сначала на&nbsp;графический смысл, как это будет работать:

![Уравнение прямой с&nbsp;угловым коэффициентом для поиска fluid значения по&nbsp;viewport&rsquo;у](/static/articles/adaptive-design/line-eq.png){width="900" height="600"}

Всё предельно просто. Ищем функцию от&nbsp;14px до&nbsp;22px при изменениях экрана с&nbsp;375px до&nbsp;1440px. Обычное уравнение прямой с&nbsp;угловым коэффициентом имеет вид $y = kx + b$. Как найти&nbsp;k? Как найти&nbsp;b?

Для этого легче всего воспользоваться каноническим уравнением прямой:

$$
{\frac{x-x_a}{x_b-x_a} = \frac{y-y_a}{y_b-y_a}}
$$

Выражаем $y$:

$$
y = \frac{(y_b-y_a)(x-x_a)}{x_b-x_a} + y_a
$$

Подставляем наши значения:

$$
y = \frac{(22-14)(x-375)}{1440-375} + 14
$$

Вычисляем:

$$
y = \frac{8(x-375)}{1065} + 14
$$

$$
y = \frac{8x}{1065} - \frac{3000}{1065} + 14
$$

Вычисляем ещё и&nbsp;получаем&nbsp;то, что можно использовать в&nbsp;css `calc()`

$$
y = 0.0075x + 11.18
$$

Собственно, это и&nbsp;есть искомое уравнение вида $y = kx + b$. Но&nbsp;что здесь будет $x$? Нам ведь нужна зависимость от&nbsp;вьюпорта? Так как вьюпорт по&nbsp;умолчанию поделён на&nbsp;100 (1vw = 1%&nbsp;от&nbsp;ширины окна), значит нам надо умножить $k$ на&nbsp;100. Получаем:

```css
font-size: calc(0.75vw + 11.18px);
```

Используем функцию `clamp` (`calc` можно опустить внутри неё):

```css
font-size: clamp(14px, 0.75vw + 11.18px, 22px);
```

Супер. Теперь у&nbsp;нас есть резиновая типография, которая резинится от&nbsp;14px до&nbsp;22px при разрешениях 375px до&nbsp;1440px.

Но&nbsp;я&nbsp;говорил про адаптивный дизайн, который будет смотреться прекрасно для всех устройств и&nbsp;размеров экрана. Сейчас, если ты&nbsp;верстаешь в&nbsp;rem или&nbsp;px (по&nbsp;сути то&nbsp;же самое), ты&nbsp;получаешь дизайн, который поддерживается до&nbsp;того значения, которое ты&nbsp;проконтролировал. Как правило, это дизайнерские 1440px. Иногда макет дизайнят до&nbsp;1920px.

Так в&nbsp;чём проблема? Вот в&nbsp;этом:

![«Вот как YouTube хочет, чтобы я&nbsp;потреблял контент на&nbsp;своем 32-дюймовом мониторе 4k»
\[@TylerGlaiel на&nbsp;сайте twitter.com\]](/static/articles/adaptive-design/large-monitors-problem.jpeg){width="3815" height="1951"}

Проблема в&nbsp;том, что в&nbsp;мире существуют мониторы больше 1920px. Откуда дизайнеры берут 1440px? Я&nbsp;думаю, что из&nbsp;Figma.

![Desktop пресеты размеров экранов в&nbsp;Figma при создании нового фрейма](/static/articles/adaptive-design/figma-new-frame.png){width="238" height="325"}

Забавно тут&nbsp;то, что другие типы пресетов, в&nbsp;точности&nbsp;&mdash; мобильные, Figma обновляет оперативно. Но&nbsp;для обычного Desktop они выбирают 1440px. Что странно, ведь глобальная доля распространения экранов 1920px составляет [наибольшие 23.22%](https://www.hobo-web.co.uk/best-screen-size/), а&nbsp;1440px&nbsp;&mdash; 5.93%.

Поэтому дизайнить макеты надо до&nbsp;1920px, а&nbsp;Figma, скорее всего, будет обновлять в&nbsp;скором времени свои пресеты.

Пользователи активно переходят на&nbsp;мониторы и&nbsp;телевизоры 4К. Уже сейчас есть мониторы 5К&nbsp;и&nbsp;6К, и&nbsp;будут ещё больше. Наивно предполагать, что прогресс остановится на&nbsp;1920px мониторах будучи дизайнером интерфейсов и&nbsp;сайтов. С&nbsp;телевизоров смотрят сайты через браузеры Smart TV&nbsp;и&nbsp;приставки (xbox, steam deck). Вы&nbsp;действительно хотите забить на&nbsp;эти устройства и&nbsp;говорите себе &laquo;мне всё равно, что будет с&nbsp;сайтом на&nbsp;экранах больше 1920px&raquo;?

Вот ещё несколько примеров сайтов, которые не&nbsp;адаптируются под Apple Studio Display&nbsp;27&Prime; 5K&nbsp;и&nbsp;его нативное разрешение 5120&times;2880. Можете открыть во&nbsp;весь экран и&nbsp;посмотреть, удобно&nbsp;ли пользователям будет работать с&nbsp;таким интерфейсом :emoji{text="😅"}

![instagram.com](/static/articles/adaptive-design/instagram.com.png){width="5120" height="2880"}

![google.com](/static/articles/adaptive-design/google.com.png){width="5120" height="2880"}

![twitter.com](/static/articles/adaptive-design/twitter.com.png){width="5120" height="2880"}

Но&nbsp;на&nbsp;самом деле не&nbsp;всё так плохо. По&nbsp;умолчанию 4К&nbsp;телевизоры, в&nbsp;том числе и&nbsp;UltraWide, устанавливают масштаб всего интерфейса системы на&nbsp;130%. Можно и&nbsp;самому поставить больше, и&nbsp;всё в&nbsp;системе подстроится к&nbsp;масштабу, в&nbsp;том числе и&nbsp;браузер. А&nbsp;Smart TV&nbsp;сами устанавливают масштаб браузера побольше, как раз по&nbsp;этой причине.

Однако пользователь может хотеть поставить масштаб 100% или 120% для системы, но&nbsp;при таком масштабе в&nbsp;браузере всё будет мелким. Так что в&nbsp;этом плане это не&nbsp;вы&nbsp;решаете как будет выглядеть ваш сайт, а&nbsp;система пользователя (даже не&nbsp;он&nbsp;сам).

А&nbsp;ведь будут ещё другие устройства, VR-гарнитуры! Умные часы становятся умнее, их&nbsp;экран становится детализированнее и&nbsp;больше! И&nbsp;кто знает, где сайты ещё будут просматриваться.

Эту проблему как раз решают vw&nbsp;единицы, которые мы&nbsp;уже посмотрели, поскольку они полагаются на&nbsp;ширину экрана. Корректным отображением сайта должны заниматься&nbsp;мы, а&nbsp;не&nbsp;система, её&nbsp;настройки или глобальные пользовательские установки. Мы&nbsp;должны продумать, как выглядит наш интерфейс и&nbsp;ответственность за&nbsp;его кривое отображение лежит в&nbsp;любом случае на&nbsp;нас, дизайнерах и&nbsp;разработчиках.

Так как дизайнер верстает макет до&nbsp;1920px и&nbsp;никто не&nbsp;будет его продумывать на&nbsp;разрешениях 4К&nbsp;или выше, то&nbsp;можно ведь просто скейлить размеры дизайна, взятые на&nbsp;макете 1920px до&nbsp;любого другого большего разрешения. Как это сделать?

![Обновлённый графический смысл](/static/articles/adaptive-design/line-eq-new.png){width="900" height="600"}

Синяя линия показывает графический смысл адаптивности, который можно использовать на&nbsp;разрешениях более 1440px (в&nbsp;наших примерах 1440px, но&nbsp;как я&nbsp;сказал, правильнее верстать макеты сайтов сразу до&nbsp;1920px).

Всё то&nbsp;же самое, что и&nbsp;с&nbsp;каноническим уравнением, но&nbsp;мы&nbsp;опускаем max значение для clamp, а&nbsp;после 1920px брейкпоинта всё должно скейлится равномерно, чтобы на&nbsp;2К&nbsp;и&nbsp;на&nbsp;5К&nbsp;сайт выглядел одинаково, как на&nbsp;разрешении 1920px.

Ищем значение прямой, проходящей через точку $(0,\ 0)$ и&nbsp;точку $(1920,\ 22)$. Через начало координат прямая пропускается по&nbsp;той причине, что если этого не&nbsp;сделать&nbsp;и, например, дальше использовать на&nbsp;отрезке $[1920, \infin]$ функцию `calc(0.75vw + 11.18px)`{lang=&quot;css&quot;}, то&nbsp;скейлинг размеров будет происходить неравномерно.

Подставляем наши значения:

$$
y = \frac{(22-0)(x-0)}{1440-0} + 0
$$

$$
y = \frac{22x}{1440}
$$

$$
y = 0.015277x
$$

Получаем простое значение для нашего свойства:

```css
font-size: 1.5277vw
```

Помните, что мы&nbsp;используем это значение для ширины экрана &gt; 1440px.

Теперь, если вы&nbsp;изменяете разрешение экрана с&nbsp;1440px на&nbsp;4К&nbsp;или&nbsp;5К, да&nbsp;хоть 400К, всё будет смотреться прекрасно.

![Переключаем туда-сюда и&nbsp;смотрим, что масштаб текста и&nbsp;отступов сохраняется](/static/articles/adaptive-design/adaptive-fluid-test.gif){width="1337" height="795"}

Отлично, теперь всё выглядит и&nbsp;резинится так, как задумано в&nbsp;дизайне и&nbsp;на&nbsp;любых экранах, даже тех, которые мы&nbsp;не&nbsp;учитывали.

Мы&nbsp;абстрагируемся от&nbsp;того, какой dpi у&nbsp;пользователя, какой масштаб установлен у&nbsp;него в&nbsp;системе, сколько дюймов у&nbsp;него экран&nbsp;&mdash; всё будет выглядеть именно так, как в&nbsp;дизайне.

## Полностью резиновый дизайн

По&nbsp;моему коммерческому опыту я&nbsp;часто сталкивался с&nbsp;тем, что тебе дают десктопную версию и&nbsp;мобильную (в&nbsp;лучшем случае), без промежуточных. Промежуточные ты&nbsp;должен сам вывести и&nbsp;адаптировать, если это необходимо. В&nbsp;таком подходе применять fluid-first будет наилучшим решением, чем работать по&nbsp;брейкпоинтам.

Остаётся только применить этот подход ко&nbsp;всем размерам, отступам, текстам и&nbsp;получить полностью резиновый дизайн для всех устройств и&nbsp;всех экранов.

## Доступность и костыли

У&nbsp;подхода размеров по&nbsp;vw&nbsp;есть одна существенная проблема: доступность. Вернее, её&nbsp;нет вообще. Мы&nbsp;не&nbsp;можем корректно менять масштаб страницы в&nbsp;браузерах привычными способами (`ctrl` + `+`), а&nbsp;ведь это входит в&nbsp;требование [WCAG 2.1 Resize text&nbsp;&mdash; Level AA](https://www.w3.org/WAI/WCAG21/quickref/?showtechniques=144#resize-text).

Подробно исследование этой проблемы написана в&nbsp;статье &laquo;[Responsive Type and Zoom](https://adrianroselli.com/2019/12/responsive-type-and-zoom.html)&raquo;. Изначально опубликована в&nbsp;декабре 2019 и&nbsp;её&nbsp;дополнение ведётся по&nbsp;сей день!

Коротко говоря, использование vw&nbsp;в&nbsp;fluid-системе дизайна сайта _всегда_ проваливает проверку WCAG 2.1 Resize text&nbsp;&mdash; Level&nbsp;AA, потому что если размер текста составляет 50px на&nbsp;100% увеличении, то&nbsp;при 200% увеличении он&nbsp;не&nbsp;будет равен 100px, хотя и&nbsp;должен&nbsp;бы.

Для решения этой проблемы можно прибегнуть к&nbsp;созданию собственного раздела сайта с&nbsp;настройками размера шрифта и&nbsp;общего масштаба сайта, где пользователь будет волен самостоятельно выбирать и&nbsp;регулировать отдельно любой из&nbsp;параметров.

![Пример вёрстки настроек сайта](/static/articles/adaptive-design/fluid-settings.png){width="520" height="480"}

Конечно, для обеспечения всего этого подхода нужны существенные изменения в&nbsp;css сайта и&nbsp;я&nbsp;бы не&nbsp;рекомендовал переписывать css существующего проекта на&nbsp;такой подход, но&nbsp;он&nbsp;прекрасно подходит для новых проектов.

Как правило, пользователь устанавливает эти настройки один раз и&nbsp;навсегда.

Вот мой sass-миксин, которым я&nbsp;пользуюсь, когда определяю fluid-систему:

```sass [fluid-typo.sass]
@use 'sass:math'
@use '@sass-collective/strip-unit'

@mixin fluid-typo($property, $font-max, $font-min, $screen-max: 1920, $screen-min: 320)
    $font-max: strip-unit.strip($font-max)
    $font-min: strip-unit.strip($font-min)

    $k: math.div($font-max - $font-min, $screen-max - $screen-min)
    #{$property}: calc((#{$k * 100}vw + #{$font-min - $k * $screen-min}px) * var(--font-scale, 1) * var(--site-scale, 1))

    @media (max-width: #{$screen-min}px)
        #{$property}: calc(#{$font-min}px * var(--font-scale, 1) * var(--site-scale, 1))

    @media (min-width: #{$screen-max}px)
        #{$property}: calc(#{math.div($font-max, $screen-max) * 100}vw * var(--font-scale, 1) * var(--site-scale, 1))
```

И&nbsp;использовать его можно таким образом:

```sass
h1
  @include fluid-typo('font-size', 48px, 30px)
```

В&nbsp;css переменных `&mdash;font-scale` и `&mdash;site-scale`, которые применяются к&nbsp;тегу `html` указываются целые числа: 1, 1.2, 1.4, на&nbsp;которые умножается значение свойства.

К&nbsp;сожалению, браузеры не&nbsp;поддерживают отслеживание изменения масштаба по&nbsp;событиям и&nbsp;мы&nbsp;не&nbsp;можем это контролировать. Поэтому собственные настройки сайта/приложения являются приоритетным вариантом при использовании vw&nbsp;единиц, если вы&nbsp;думаете о&nbsp;доступности.

Можно подписаться на&nbsp;событие нажатия `ctrl` + `+` и `ctrl` + `mouseUp`, чтобы показывать модальное окно с&nbsp;настройками.

В&nbsp;итоге мы&nbsp;получаем полностью резиновый сайт на&nbsp;любых разрешениях:

![Готовый результат главной страницы моего сайта, который резинется абсолютно на&nbsp;всех разрешениях](/static/articles/adaptive-design/end-result.gif){width="1367" height="838"}

Спасибо за&nbsp;внимание!
