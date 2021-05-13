---
title: Github 블로그 Author_profile에 category 추가하기
hidden: false
publish: true
search: true
categories:
  - Github Blog
tags:
  - Github Blog
  - Category
  - author_profile
last_modified_at: 2021-05-13T09:58:00-05:00
toc: true
---
<span style="font-size: 15px"> 이 내용은 [Minimal Mistakes Jekyll](https://github.com/mmistakes/minimal-mistakes)theme를 이용하시는 경우에 해당하는 내용입니다. </span>

# Github Blog의 Author-profile 영역
현재 제 블로그 상의 좌측에 보이는 부분이 Author-profile 영역입니다.

해당 영역에 표시되는 것은 프로필 이미지(author avatar), 이름(author name), 소개(author bio), 지역(author location), 개인 링크(links) 등이 표시됩니다.

이 영역은 _includes 폴더의 author-profile.html에 명시되어있습니다.

제가 구현하고자 했던 것은, 해당 영역 아래에 category 리스트들을 한번에 볼 수 있도록 하고싶었습니다.

## 가장 유사한 UI를 갖고있는 파일
Category들을 모두 뿌려주는 화면이 무엇이 있나 찾아보다보니 `categories` 페이지를 따로 구현하는 것을 확인했습니다.

<!-- ![이미지](/assets/img/posts_imgs/20210513/img_01.png "Categories 페이지") -->

<img src = "/assets/img/posts_imgs/20210513/img_01.png" alt="test" style="border: 1px solid white;">
해당 페이지 구현 방법은 [해당링크](#)를 참고해주시면 되겠습니다.

해당 페이지의 layout은 이미 _layout폴더의 categories에 구현이 되어있습니다.

categories 파일의 코드 중 아래의 코드와 동일한 부분이 있는데
```liquid {% raw %}
{% assign categories_max = 0 %}
{% for category in site.categories %}
  {% if category[1].size > categories_max %}
    {% assign categories_max = category[1].size %}
  {% endif %}
{% endfor %}

<ul class="taxonomy__index">
  {% for i in (1..categories_max) reversed %}
    {% for category in site.categories %}
      {% if category[1].size == i %}
        <li>
          <a href="#{{ category[0] | slugify }}">
            <strong>{{ category[0] }}</strong> <span class="taxonomy__count">{{ i }}</span>
          </a>
        </li>
      {% endif %}
    {% endfor %}
  {% endfor %}
</ul>
{% endraw %}
```

이 부분을 응용해서 구현하게 되었습니다.

해당 코드대로라면 Post 갯수가 가장 많은 것 부터 내림차순으로 list가 출력이 되는데, 제 생각에도 이게 차라리 나을 것 같아서 그대로 이용하게 되었습니다.


## 구현 코드
맨 처음에 author-profile 영역에 대해 말씀드렸습니다.

해당 영역은 author-profile.html에 구현되어있는 상태입니다.

_includes 폴더를 보니 author-profile-custom-links.html파일이 따로 있는 것을 확인했고, 기본 author-profile.html은 건들지 않고 제가 원하는 대로 커스텀 할 수 있는 것을 확인했습니다.

해당 파일에 다음 코드를 넣어주었습니다.
```liquid  {% raw %}
<hr>
<tag>
  <a href="/categories/" itemprop="sameAs" rel="nofollow noopener noreferrer">
    Categories
  </a>
</tag>

{% assign categories_max = 0 %}
{% for category in site.categories %}
  {% if category[1].size > categories_max %}
    {% assign categories_max = category[1].size %}
  {% endif %}
{% endfor %}

{% for i in (1..categories_max) reversed %}
  {% for category in site.categories %}
    {% if category[1].size == i %}
      <ul style="padding-top: 0px; padding-bottom: 0px;">
        <a href="/categories/#{{ category[0] | slugify }}">
          <strong style="font-size: 15px;">{{ category[0] }}</strong> <span class="taxonomy__count" style="font-size: 13px;">{{ i }}</span>
        </a>
      </ul>
    {% endif %}
  {% endfor %}
{% endfor %}
{% endraw %}
```


## 왜 이렇게 구현을 하는가?
각 카테고리를 명시적으로 나누고 하위 카테고리로 만들까 고민도 했습니다.

예를 들면 iOS라는 카테고리 하위에 Swift, Objective-C, RxSwift 등이 포함되는 형태처럼요.

근데 명시적으로 구현하자니 새로운 category가 추가될 때마다 새로 수정을 해줘야 하는 부분이 불편할 것 같아서 지금처럼 구현하게 되었습니다.

그리고 카테고리 하위에 다른 카테고리를 넣게 되는 경우 생각보다 지저분해질 수 있다고 생각이 들었던 것도 지금 UI로 만들게 된 이유 중 하나입니다.

차후 더 나은 방안이 있다면 구현해보고 추가로 작성해보도록 하겠습니다.