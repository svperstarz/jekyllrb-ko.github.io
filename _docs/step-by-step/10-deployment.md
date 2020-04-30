---
layout: step
#title: Deployment
title: 배포
position: 10
---
<!--
In this final step we'll get the site ready for production.
-->
마지막 단계로 사이트를 운영환경에 배포할 준비를 할 것입니다.

## Gemfile

<!--
It's good practice to have a [Gemfile](/docs/ruby-101/#gemfile) for your site.
This ensures the version of Jekyll and other gems remains consistent across
different environments.
-->
사이트에 [Gemfile](/docs/ruby-101/#gemfile) 을 만드는 것은 따라야 할 좋은 관행
중 하나입니다. 이렇게 하면 다른 환경에서도 Jekyll 과 기타 젬들의 버전을
일관성있게 유지할 수 있습니다.

<!--
Create `Gemfile` in the root with the following:
-->
루트에 `Gemfile` 을 만들고 다음과 같이 입력합니다:

```ruby
source 'https://rubygems.org'

gem 'jekyll'
```

<!--
Then run `bundle install` in your terminal. This installs the gems and
creates `Gemfile.lock` which locks the current gem versions for a future
`bundle install`. If you ever want to update your gem versions you can run
`bundle update`.
-->
이제 터미널에서 `bundle install` 을 실행합니다. 필요한 젬들을 설치하고
이후 `bundle install` 에 영향을 받지 않게 `Gemfile.lock` 를 만들어 현재
젬 버전을 고정시킵니다. 업데이트하고 싶은 젬이 있다면 `bundle update` 를
실행합니다.

<!--
When using a `Gemfile`, you'll run commands like `jekyll serve` with
`bundle exec` prefixed. So the full command is:
-->
`Gemfile` 을 사용한다면, `jekyll serve` 같은 명령어를 실행할 때 앞에 `bundle exec`
를 추가해 실행합니다. 따라서 전체 명령어는 다음과 같습니다:

```sh
bundle exec jekyll serve
```

<!--
This restricts your Ruby environment to only use gems set in your `Gemfile`.
-->
이것으로 오직 `Gemfile` 에 지정된 젬만 사용하도록 루비 환경을 제한할 수 있습니다.

<!--
## Plugins
-->
## 플러그인

<!--
Jekyll plugins allow you to create custom generated content specific to your
site. There's many [plugins](/docs/plugins/) available or you can even
write your own.
-->
Jekyll 플러그인으로 당신의 사이트에 꼭 맞는 커스텀 컨텐츠를 생성할 수
있습니다. 이미 많은 [플러그인](/docs/plugins/)이 존재하며 심지어 직접
자신의 플러그인을 만들 수도 있습니다.

<!--
There's three official plugins which are useful on almost any Jekyll site:
-->
거의 모든 Jekyll 사이트에 유용하게 사용될 수 있는 공식 플러그인이 세 가지 있습니다:

<!--
* [jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap) - Creates a sitemap
file to help search engines index content
* [jekyll-feed](https://github.com/jekyll/jekyll-feed) - Creates an RSS feed for
your posts
* [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) - Adds meta tags to help
with SEO
-->
* [jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap) - 검색엔진의 사이트
내용 인덱싱을 돕는 사이트맵 파일 생성
* [jekyll-feed](https://github.com/jekyll/jekyll-feed) - 포스트에 대한 RSS 피드
생성
* [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) - SEO 에 도움이 되는
메타 태그 추가

<!--
To use these first you need to add them to your `Gemfile`. If you put them
in a `jekyll_plugins` group they'll automatically be required into Jekyll:
-->
이 플러그인들을 사용하려면 먼저 `Gemfile` 에 추가해야 합니다. `jekyll_plugins`
그룹에 플러그인들을 추가하면 자동으로 Jekyll 에 의해 사용됩니다:

```ruby
source 'https://rubygems.org'

gem 'jekyll'

group :jekyll_plugins do
  gem 'jekyll-sitemap'
  gem 'jekyll-feed'
  gem 'jekyll-seo-tag'
end
```

<!--
Then add these lines to your `_config.yml`:
-->
다음 `_config.yml` 에 아래 내용을 추가합니다:

```yaml
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
```

<!--
Now install them by running a `bundle update`.
-->
이제 `bundle update` 를 실행해 이 플러그인들을 설치합니다.

<!--
`jekyll-sitemap` doesn't need any setup, it will create your sitemap on build.
-->
`jekyll-sitemap` 은 따로 셋업이 필요하지 않습니다. 사이트 빌드 시 사이트맵을 생성할 것입니다.

<!--
For `jekyll-feed` and `jekyll-seo-tag` you need to add tags to
`_layouts/default.html`:
-->
`jekyll-feed` 와 `jekyll-seo-tag` 를 위해서는
`_layouts/default.html` 에 태그를 추가해야 합니다.:

{% raw %}
```liquid
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
    {% feed_meta %}
    {% seo %}
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```
{% endraw %}

<!--
Restart your Jekyll server and check these tags are added to the `<head>`.
-->
Jekyll 서버를 재시작하고 `<head>` 에 이 태그들이 추가되었는지 확인합니다.

<!--
## Environments
-->
## 환경

<!--
Sometimes you might want to output something in production but not
in development. Analytics scripts are the most common example of this.
-->
가끔 운영환경에는 필요하지만 개발환경에는 출력할 필요가 없는 것들이
있습니다. 가장 흔한 예시들 중 하나는 Analytics 스크립트입니다.

<!--
To do this you can use [environments](/docs/configuration/environments/). You
can set the environment by using the `JEKYLL_ENV` environment variable when
running a command. For example:
-->
이를 해결하려면 [환경](/docs/configuration/environments/)을 사용합니다.
명령어를 실행할 때 `JEKYLL_ENV` 환경변수로 환경을 지정할 수 있습니다.
예를 들면 다음과 같습니다:

```sh
JEKYLL_ENV=production bundle exec jekyll build
```

<!--
By default `JEKYLL_ENV` is development. The `JEKYLL_ENV` is available to you
in liquid using `jekyll.environment`. So to only output the analytics script
on production you would do the following:
-->
`JEKYLL_ENV` 의 디폴트값은 development 입니다. Liquid 의 `jekyll.environment` 를
사용해서 `JEKYLL_ENV` 의 값을 사용할 수 있습니다. 따라서, 오직 운영환경에서만
Analytics 스크립트를 출력하려면 다음과 같이 입력합니다:

{% raw %}
```liquid
{% if jekyll.environment == "production" %}
  <script src="my-analytics-script.js"></script>
{% endif %}
```
{% endraw %}

<!--
## Deployment
-->
## 배포

<!--
The final step is to get the site onto a production server. The most basic way
to do this is to run a production build:
-->
마지막 단계는 운영 서버에 사이트를 배포하는 것입니다. 가장 단순한 방법은
운영환경 빌드를 실행하는 것입니다:

```sh
JEKYLL_ENV=production bundle exec jekyll build
```

<!--
And copy the contents of `_site` to your server.
-->
그 다음 `_site` 의 내용물을 서버에 복사합니다.

<!--
A better way is to automate this process using a [CI](/docs/deployment/automated/)
or [3rd party](/docs/deployment/third-party/).
-->
더 좋은 방법은 [CI](/docs/deployment/automated/) 나 [서드 파티](/docs/deployment/third-party/)를
사용해서 이 과정을 자동화하는 것입니다.

<!--
## Wrap up
-->
## 요약

<!--
That brings us to the end of this step-by-step tutorial and the beginning of
your Jekyll journey!
-->
이것으로 단계별 튜토리얼이 끝났습니다. Jekyll 로의 당신만의 여행이 시작됩니다.

<!--
* Come say hi to the [community forums](https://talk.jekyllrb.com)
* Help us make Jekyll better by [contributing](/docs/contributing/)
* Keep building Jekyll sites!
-->
* [커뮤니티 포럼](https://talk.jekyllrb.com)에 방문해보세요
* Jekyll 을 더 좋게 만들 수 있게 [기여](/docs/contributing/)하세요
* 계속 Jekyll 을 사용해서 사이트를 만드세요!
