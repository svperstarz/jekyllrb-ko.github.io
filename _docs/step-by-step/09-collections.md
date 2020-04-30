---
layout: step
#title: Collections
title: 컬렉션
position: 9
---
<!--
Let's look at fleshing out authors so each author has their own page with a
blurb and the posts they've published.
-->
이제 저자 정보에 살을 붙여 각 저자별로 짤막한 설명과 그들의 포스트 목록이
담긴 페이지를 만드는 방법을 알아봅시다.

<!--
To do this you'll use collections. Collections are similar to posts except the
content doesn't have to be grouped by date.
-->
컬렉션을 사용할 것입니다. 컬렉션은 포스트와 비슷하지만 날짜별로
그룹지어질 필요가 없다는 차이점이 있습니다.

<!--
## Configuration
-->
## 환경설정

<!--
To set up a collection you need to tell Jekyll about it. Jekyll configuration
happens in a file called `_config.yml` (by default).
-->
컬렉션 설정방법은 Jekyll 에 컬렉션의 존재를 알려주는 것입니다. `_config.yml`
(기본값) 이라는 파일에서 Jekyll 환경설정을 합니다.

<!--
Create `_config.yml` in the root with the following:
-->
루트에 `_config.yml` 을 만들어 다음과 같이 입력합니다:

```yaml
collections:
  authors:
```

<!--
To (re)load the configuration, restart the jekyll server. Press `Ctrl`+`C` in your terminal to stop the server, and then `jekyll serve` to restart it.
-->
환경설정을 (다시) 불러오려면, Jekyll 서버를 재시작합니다. 터미널에서 `Ctrl`+`C` 를 눌러 서버를 중지하고, `jekyll serve` 로 재시작합니다.

<!--
## Add authors
-->
## 저자 추가

<!--
Documents (the items in a collection) live in a folder in the root of the site
named  `_*collection_name*`. In this case, `_authors`.
-->
문서 (컬렉션에 속한 항목) 는 사이트 루트에 있는  `_*컬렉션_이름*` 이라는 폴더에
존재합니다. 지금과 같은 경우에는, `_authors` 입니다.

<!--
Create a document for each author:
-->
각 저자마다 문서를 생성합니다:

`_authors/jill.md`:

```markdown
---
short_name: jill
name: Jill Smith
position: Chief Editor
---
Jill is an avid fruit grower based in the south of France.
```

`_authors/ted.md`:

```markdown
---
short_name: ted
name: Ted Doe
position: Writer
---
Ted has been eating fruit since he was baby.
```

<!--
## Staff page
-->
## 스태프 페이지

<!--
Let's add a page which lists all the authors on the site. Jekyll makes the
collection available at `site.authors`.
-->
사이트의 모든 저자 목록 페이지를 추가해봅시다. Jekyll 은 `site.authors` 로
이 컬렉션을 사용할 수 있게 해줍니다.

<!--
Create `staff.html` and iterate over `site.authors` to output all the staff:
-->
`staff.html` 을 생성한 뒤 `site.authors` 를 순회해서 모든 스태프를 출력합니다:

{% raw %}
```liquid
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2>{{ author.name }}</h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```
{% endraw %}

<!--
Since the content is markdown, you need to run it through the
`markdownify` filter. This happens automatically when outputting using
{% raw %}`{{ content }}`{% endraw %} in a layout.
-->
컨텐츠가 마크다운으로 작성되어있기 때문에, `markdownify` 필터를
사용해야 합니다. 레이아웃에서 {% raw %}`{{ content }}`{% endraw %} 를 사용해
출력할 때는 자동으로 이 작업이 실행됩니다.

<!--
You also need a way to navigate to this page through the main navigation. Open
`_data/navigation.yml` and add an entry for the staff page:
-->
기본 네비게이션에서 이 페이지로 이동할 방법도 필요할 것입니다.
`_data/navigation.yml` 을 열고 이 스태프 페이지에 대한 항목을 추가합니다:

```yaml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
- name: Staff
  link: /staff.html
```

<!--
## Output a page
-->
## 페이지 출력

<!--
By default, collections do not output a page for documents. In this case we
want each author to have their own page so let's tweak the collection
configuration.
-->
디폴트 환경설정에서, 컬렉션은 문서에 대해 페이지를 출력하지 않습니다. 지금 여기서 우리는
각 저자마다 페이지를 하나씩 만들어주고 싶으니 컬렉션 환경설정을 살짝
고쳐봅시다.

<!--
Open `_config.yml` and add `output: true` to the author collection
configuration:
-->
`_config.yml` 을 열고 저자 컬렉션 환경설정에 `output: true` 를
추가합니다:

```yaml
collections:
  authors:
    output: true
```

<!--
You can link to the output page using `author.url`.
-->
`author.url` 을 사용해서 출력된 페이지로 링크할 수 있습니다.

<!--
Add the link to the `staff.html` page:
-->
`staff.html` 페이지에 링크를 추가합니다:

{% raw %}
```liquid
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```
{% endraw %}

<!--
Just like posts you'll need to create a layout for authors.
-->
포스트 때와 똑같이 저자를 위한 레이아웃을 생성해야 합니다.

<!--
Create `_layouts/author.html` with the following content:
-->
`_layouts/author.html` 을 생성하고 다음과 같이 입력합니다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}
```
{% endraw %}

<!--
## Front matter defaults
-->
## 머리말 기본값

<!--
Now you need to configure the author documents to use the `author` layout. You
could do this in the front matter like we have previously but that's getting
repetitive.
-->
이제 저자관련 문서는 `author` 레이아웃을 사용하도록 설정해야 합니다. 이전과
동일하게 머리말에서 이 설정을 할 수 있지만 같은 작업이 반복되기
시작하네요.

<!--
What you really want is all posts to automatically have the post
layout, authors to have author and everything else to use the default.
-->
우리가 원하는 건 모든 포스트는 자동으로 포스트 레이아웃을, 저자정보는
저자 레이아웃을 사용하고 나머지들은 기본 레이아웃을 사용하게 하는 것입니다.

<!--
You can achieve this by using [front matter defaults](/docs/configuration/front-matter-defaults/)
in `_config.yml`. You set a scope of what the default applies to, then the
default front matter you'd like.
-->
이를 위해서는 `_config.yml` 에 [머리말 기본값](/docs/configuration/front-matter-defaults/)을
사용합니다. 기본값이 적용될 범위를 설정하고, 원하는 값을 머리말 기본값으로
설정할 수 있습니다.

<!--
Add defaults for layouts to your `_config.yml`,
-->
`_config.yml` 에 레이아웃 기본값을 추가합니다,

```yaml
collections:
  authors:
    output: true

defaults:
  - scope:
      path: ""
      type: "authors"
    values:
      layout: "author"
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
    values:
      layout: "default"
```

<!--
Now you can remove layout from the front matter of all pages and posts. Note
that any time you update `_config.yml` you'll need to restart Jekyll for the
changes to take effect.
-->
이제 모든 페이지와 포스트의 머리말에서 레이아웃 설정을 제거할 수 있습니다.
`_config.yml` 을 수정할 때마다, Jekyll 을 재시작해야 변경사항이 적용된다는
것을 기억하세요.

<!--
## List author's posts
-->
## 저자별 포스트 나열하기

<!--
Let's list the posts an author has published on their page. To do
this you need to match the author `short_name` to the post `author`. You
use this to filter the posts by author.
-->
저자 페이지에 그 저자가 게시한 포스트들을 나열해봅시다. 이를 위해서는
저자의 `short_name` 을 포스트의 `author` 와 비교해봅니다. 이 방식으로
저자별 포스트를 골라낼 수 있습니다.

<!--
Iterate over this filtered list in `_layouts/author.html` to output the
author's posts:
-->
`_layouts/author.html` 에서 필터링된 목록을 순회하여 각 저자의 포스트를
출력합니다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}

<h2>Posts</h2>
<ul>
  {% assign filtered_posts = site.posts | where: 'author', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
```
{% endraw %}

<!--
## Link to authors page
-->
## 저자 페이지로 링크하기

<!--
The posts have a reference to the author so let's link it to the author's page.
You can do this using a similar filtering technique in `_layouts/post.html`:
-->
포스트에 있는 저자 정보를 사용해서 저자 페이지로 링크해봅시다.
`_layouts/post.html` 에서의 필터링 기법과 유사한 방법을 사용하면 됩니다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.title }}</h1>

<p>
  {{ page.date | date_to_string }}
  {% assign author = site.authors | where: 'short_name', page.author | first %}
  {% if author %}
    - <a href="{{ author.url }}">{{ author.name }}</a>
  {% endif %}
</p>

{{ content }}
```
{% endraw %}

<!--
Open up <a href="http://localhost:4000" target="_blank" data-proofer-ignore>http://localhost:4000</a> and
have a look at the staff page and the author links on posts to check everything
is linked together correctly.
-->
<a href="http://localhost:4000" target="_blank" data-proofer-ignore>http://localhost:4000</a> 을
열어 스태프 페이지와 포스트의 저자 링크등 모든 것이 올바르게 링크되었는지
확인합니다.

<!--
In the next and final step of this tutorial, we'll add polish to the site and
get it ready for a production deployment.
-->
다음은 이 튜토리얼의 마지막 단계로서, 사이트를 다듬고 운영환경에 대한 배포 준비를
할 것입니다.
