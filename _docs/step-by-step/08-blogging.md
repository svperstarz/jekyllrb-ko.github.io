---
layout: step
#title: Blogging
title: 블로그
position: 8
---
<!--
You might be wondering how you can have a blog without a database. In true
Jekyll style, blogging is powered by text files only.
-->
어떻게 데이터베이스 없이 블로그를 만들 수 있는지 궁금할 것입니다. 진정한
Jekyll 스타일에서는, 텍스트 파일만으로 블로그를 만듭니다.

<!--
## Posts
-->
## 포스트

<!--
Blog posts live in a folder called `_posts`. The filename for posts have a
special format: the publish date, then a title, followed by an extension.
-->
블로그 포스트는 `_posts` 라는 폴더에 존재합니다. 포스트의 파일명은 특별한
형식을 가지고 있습니다: 게시일자, 그 다음 제목, 마지막으로 확장자.

<!--
Create your first post at `_posts/2018-08-20-bananas.md` with the
following content:
-->
다음과 같은 내용으로 당신의 첫 포스트를 `_posts/2018-08-20-bananas.md` 에
생성합니다:

```markdown
---
layout: post
author: jill
---
A banana is an edible fruit – botanically a berry – produced by several kinds
of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains",
distinguishing them from dessert bananas. The fruit is variable in size, color,
and firmness, but is usually elongated and curved, with soft flesh rich in
starch covered with a rind, which may be green, yellow, red, purple, or brown
when ripe.
```

<!--
This is like the `about.md` you created before except it has an author and
a different layout. `author` is a custom variable, it's not required and could
have been named something like `creator`.
-->
이 전에 생성한 `about.md` 와 비슷하지만 레이아웃이 다르고 작성자 정보가 있다는
차이점이 있습니다. `author` 는 사용자 변수로, 필수조건이 아니므로 `creator` 같이
다른 이름을 가져도 됩니다.

<!--
## Layout
-->
## 레이아웃

<!--
The `post` layout doesn't exist so you'll need to create it at
`_layouts/post.html` with the following content:
-->
`post` 레이아웃이 없으니 다음과 같은 내용의 파일을
`_layouts/post.html` 에 생성합니다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```
{% endraw %}

<!--
This is an example of layout inheritance. The post layout outputs the title,
date, author and content body which is wrapped by the default layout.
-->
이는 레이아웃 상속을 보여주는 예시입니다. 포스트 레이아웃은 기본 레이아웃으로
감싸져있고 제목과 날짜, 저자 정보를 출력합니다.

<!--
Also note the `date_to_string` filter, this formats a date into a nicer format.
-->
그리고 `date_to_string` 필터도 눈여겨 보세요. 날짜를 보기좋은 형식으로 변환합니다.

<!--
## List posts
-->
## 포스트 목록

<!--
There's currently no way to navigate to the blog post. Typically a blog has a
page which lists all the posts, let's do that next.
-->
현재로서는 블로그 포스트 사이를 이동할 방법이 없습니다. 블로그에는 보통 모든
포스트들을 나열하는 페이지가 있습니다. 한번 만들어봅시다.

<!--
Jekyll makes posts available at `site.posts`.
-->
Jekyll 은 `site.posts` 로 포스트들에 접근할 수 있게 해줍니다.

<!--
Create `blog.html` in your root (`/blog.html`) with the following content:
-->
다음 내용을 가진 `blog.html` 을 루트(`/blog.html`)에 생성합니다:

{% raw %}
```liquid
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
```
{% endraw %}

<!--
There's a few things to note with this code:
-->
이 코드에 대해 설명할 것이 몇 가지 있습니다:

<!--
* `post.url` is automatically set by Jekyll to the output path of the post
* `post.title` is pulled from the post filename and can be overridden by
setting `title` in front matter
* `post.excerpt` is the first paragraph of content by default
-->
* `post.url` 은 포스트의 출력 경로로서 Jekyll 이 자동으로 설정한다
* `post.title` 은 포스트의 파일명에서 추출된 제목이며 머리말에 `title` 을
설정하여 덮어쓸 수 있다
* `post.excerpt` 은 컨텐츠의 첫 문단 (기본값) 이다

<!--
You also need a way to navigate to this page through the main navigation. Open
`_data/navigation.yml` and add an entry for the blog page:
-->
기본 네비게이션에서 이 페이지로 이동할 방법도 필요할 것입니다.
`_data/navigation.yml` 을 열고 이 블로그 페이지에 대한 항목을 추가합니다:

```yaml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
```

<!--
## More posts
-->
## 추가 포스트

<!--
A blog isn't very exciting with a single post. Add a few more:
-->
포스트가 하나 밖에 없는 블로그는 그다지 재미있어 보이지 않습니다. 몇 개 더 추가해봅시다:

`_posts/2018-08-21-apples.md`:

```markdown
---
layout: post
author: jill
---
An apple is a sweet, edible fruit produced by an apple tree.

Apple trees are cultivated worldwide, and are the most widely grown species in
the genus Malus. The tree originated in Central Asia, where its wild ancestor,
Malus sieversii, is still found today. Apples have been grown for thousands of
years in Asia and Europe, and were brought to North America by European
colonists.
```

`_posts/2018-08-22-kiwifruit.md`:

```markdown
---
layout: post
author: ted
---
Kiwifruit (often abbreviated as kiwi), or Chinese gooseberry is the edible
berry of several species of woody vines in the genus Actinidia.

The most common cultivar group of kiwifruit is oval, about the size of a large
hen's egg (5–8 cm (2.0–3.1 in) in length and 4.5–5.5 cm (1.8–2.2 in) in
diameter). It has a fibrous, dull greenish-brown skin and bright green or
golden flesh with rows of tiny, black, edible seeds. The fruit has a soft
texture, with a sweet and unique flavor.
```

<!--
Open <a href="http://localhost:4000" target="_blank" data-proofer-ignore>http://localhost:4000</a>
and have a look through your blog posts.
-->
브라우저로 <a href="http://localhost:4000" target="_blank" data-proofer-ignore>http://localhost:4000</a>
을 열고 블로그 포스트들을 확인합니다.

<!--
Next we'll focus on creating a page for each post author.
-->
다음 단계에서는 포스트 저자들의 페이지를 만들어볼 것입니다.
