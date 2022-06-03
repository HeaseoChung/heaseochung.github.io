---
title: "정해서 | AI Researcher"
keywords: sample homepage
tags: 
sidebar: mydoc_sidebar
permalink: index.html
summary: 
---

<!-- ![heaseo.jpeg](images/heaseo.jpeg) -->

## Contact

Email: Heaseochung@gmail.com

Phone: 010-6401-1789

## Channel

Github: [http://github.com/heaseochung](http://github.com/heaseochung)

Blog: [https://velog.io/@heaseo](https://velog.io/@heaseo)

## Introduce

2년 차 AI 개발자입니다. Image generation, restoration, object detection, body pose estimation 등 컴퓨터 비전 모델을 연구하고 있습니다. 비즈니스 가치를 창출하기 위해 선행연구 벤치마킹, 응용, 모델 학습 및 추론속도 개선을 서비스에 적용한 경험이 있습니다. 동시에 팀 내 리더로 프로젝트 설계, 코어, 파트 매니징을 업무를 맡고 있습니다.

스스로 발전하는 사람이 되기 위해 부족한 부분을 찾아 기술 및 논문리뷰 등을 개인 블로그에 포스트를 작성하고 배운 것을 팀원과 공유하는 것을 좋아합니다.

## Experience

2020.05.11 ~

 재직중

1년 8개월

**Espresomedia**

주임연구원

- Object detection 모델 연구 및 상업화
- GAN 기반 얼굴복원 모델 연구와 학습 파이프라인 개발 및 상업화
- CNN & GAN 기반 super-resolution 모델 연구와 학습 파이프라인 개발
- Model Optimization
- Edge AI (Nvidia Jetson, Xaiver) 환경에서 모델 구현 및 서비스 개발
- 다중 GPU 학습 및 추론 로직 개발
- Skill Keywords
    
    
    | Programming Language | Python |  |  |  |
    | --- | --- | --- | --- | --- |
    | AI Frameworks | PyTorch | TensorFlow | TensorRT | ONNX |
    | Model Optimizations | Quantization | Modifying Architecture | Pruning |  |
    | Other Libs | OpenCV | FFmpeg |  |  |
    | Utilizations | Git | Docker | Hydra | Wandb |

## Project

2021.11.01 ~

 2022.01.01

**실시간 테니스 공 트래킹 인공지능 모델 경량화 및 서비스**

- 볼 트래킹 선행연구 모델 탐색
- Unet 기반의 새로운 tiny ball tracking 모델 개발
- 볼 트래킹실시간 서비스 응용개발
    - 모델 추론 테스트 및 OpenCV를 사용한 공의 잔상을 구현
- 볼 트래킹 모델 경량화
    - PTQ 양자화 기법으로 추론시간 약 4배 감소
    - TensorRT 변환을 통해 추론시간 약 2배 감소
- 실시간 볼 트래킹 서비스 상용화

2021.10.25 ~

 2021.11.30

**GAN 기반 얼굴복원 모델 연구**

- StyleGAN prior를 decoder로 사용하는 U-net 기반 얼굴복원 모델 개발
- 학습 파이프라인 설계
    - Distributed data parallel 학습코드 구현으로 학습속도 최대 5배 개선
    - 데이터 전처리
        - Degradation 구현
        - Data augmentation 구현
    - 손실함수 코드로 구현
    - Wandb 으로 모델 학습 모니터링 구현
    - Hydra와 DVC 으로 모델 버전 관리

2021.06.01 ~

 2021.09.30

GAN **기반 이미지 복원 모델 연구**

- 이미지 복원 모델 연구 및 선행연구 모델 벤치마킹
    - RealESRGAN
    - BSRGAN
- 학습 파이프라인 설계
    - Distributed data parallel 학습코드 구현으로 학습속도 최대 5배 개선
    - 데이터 전처리
        - Degradation 구현
        - Data augmentation 구현
    - 손실함수 코드 구현
    - Wandb 으로 모델 학습 모니터링 구현
    - Hydra와 DVC 으로 모델 버전 관리
- 모델 테스트 코드 설계
    - FFmpeg-python 을 사용한 모델 테스트 구현

2021.04.01 ~

 2021.06.31

**CNN 기반 super-resolution 모델 연구 및 경량화**

- EDSR 모델 경량화 및 추론속도 개선

2020.12.02 ~

 2021.03.30

**Edge AI super-resolution 모델 연구 및 응용프로그램 개발**

- EDSR 모델 경량화 모델 “LDSR” 모델 개발
    - 모델 아키택처 개선
        - EDSR의 residual blocks 의 수 감소 및 head & tail 부분에 residual block 추가. 그 결과 기존 EDSR 대비 파라미터 수 1.5배 감소
        - RGB 3개의 채널 대신 YCBCR 중 “Y” 1개의 채널만 사용해서 파라미터 수 감소
    - 손실함수 개선
        - Pixelwise 와 GAN Loss 함수를 사용해서 학습 진행해서 기존 EDSR 모델보다 약 20% 개선된 PSNR 수치 확보
    - 모델 최적화 모듈 사용
        - PyTorch 모델을 ONNX 변환 후 TensorRT 모델로 변경
- Super-resolution 응용프로그램 개발
    - Pyqt5 으로 UI 설계 및 keyboard & mouse interaction 구현
    - OpenCV 으로 동영상 및 이미지 소스 처리
    - 테스트 코드 구현

2020.07.01 ~

 2020.10.31

**EDSR super-resolution 모델을 Graphcore AI 장비에 이식**

- Graphcore의 PopArt SDK를 사용해서 모델 이식 작업 진행
    - PyTorch 모델을 ONNX 로 변환
    - PopArt builder 생성을 통해 ONNX 모델 로드
    - 모델 가중치 float32 에서 float16 으로 변환
    - Builder로 생성된 모델을 추론하기 위해 session으로 생성
    - StepIO에 실제 input 및 empty output 데이터 생성
    - Session을 통해 모델 추론

## 개인 프로젝트

2022.01.01 ~

**Golf AI Coaching APP with Unity**

- 사용자의 key-points 및 골프 클럽 궤도 estimation 모델 연구
- 데이터 수집, 전처리, 학습 코드 구현
- ONNX를 이용한 Restful AI 추론서버 개발
- 추론서버에서 전달한 관절 및 클럽 궤도 위치정보를 유니티 클라이언트에서 구현

## Education

2022.01.10 ~

 2023.05.30

**McMaster University, 캐나다**

*소프트웨어 엔지니어링*

2017.09.01 ~

 2020.04.30

**George Brown College, 캐나다**

*게임프로그래밍 졸업 (GPA: 3.85 / 4.0)*

# **ETC**

2017.09.01 ~

 2020.04.30

**Dean’s Honor list award**

George Brown College에서 *6학기 모두 우등생 명단에 오름*

2012.12.04 ~

 2014.09.03

**육군 병장 만기 전역**

*운전병*

2007.09.01 ~

 2012.06.30

**캐나다 유학**

*캐나다 중.고등학교 졸업*

<!-- 
{% include note.html content="If you're cloning this theme, you're probably writing documentation of some kind. I have a blog on technical writing here called <a alt='technical writing blog' href='http://idratherbewriting.com'>I'd Rather Be Writing</a>. If you'd like to stay updated with the latest trends, best practices, and other methods for writing documentation, consider <a href='https://tinyletter.com/tomjoht'>subscribing</a>. I also have a site on <a href='http://idratherbewriting.com/learnapidoc'>writing API documentation</a>." %}

## Build the Theme

Follow these instructions to build the theme.

### 1. Download the theme

First, download or clone the theme from the [Github repo](https://github.com/tomjoht/documentation-theme-jekyll). Most likely you won't be pulling in updates once you start customizing the theme, so downloading the theme (instead of cloning it) probably makes the most sense. In Github, click the **Clone or download** button, and then click **Download ZIP**.

### 2. Install Jekyll

If you've never installed or run a Jekyll site locally on your computer, follow these instructions to install Jekyll:

* [Install Jekyll on Mac][mydoc_install_jekyll_on_mac]
* [Install Jekyll on Windows][mydoc_install_jekyll_on_windows]

### 3. Install Bundler

In case you haven't installed Bundler, install it:

```
gem install bundler
```

You'll want [Bundler](http://bundler.io/) to make sure all the Ruby gems needed work well with your project. Bundler sorts out dependencies and installs missing gems or matches up gems with the right versions based on gem dependencies.

### 4. Option 1: Build the Theme (*without* the github-pages gem) {#option1}

Use this option if you're not planning to publish your Jekyll site using [Github Pages](https://pages.github.com/).

Bundler's Gemfile specifies how project dependencies are managed. Although this project includes a Gemfile, this theme doesn't have any dependencies beyond core Jekyll. The Gemfile is used to list gems needed for publishing on Github Pages. **If you're not planning to have Github Pages build your Jekyll project, delete these two files from the theme's root directory:**

* Gemfile
* Gemfile.lock

If you've never run Jekyll on your computer (you can check with `jekyll --version`), you may need to install the jekyll gem:

```
gem install jekyll
```

Now run jekyll serve (first change directories (`cd`) to where you downloaded the project):

```
jekyll serve
```

### 4. Option 2: Build the Theme (*with* the github-pages gem) {#option2}

If you *are* in fact publishing on Github Pages, leave the Gemfile and Gemfile.lock files in the theme.The Gemfile tells Jekyll to use the github-pages gem. **However, note that you cannot use the normal `jekyll serve` command with this gem due to dependency conflicts between the latest version of Jekyll and Github Pages** (which are noted [briefly here](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)).

You need Bundler to resolve these dependency conflicts. Use Bundler to install all the needed Ruby gems:

```
bundle update
```

Then *always* use this command to build Jekyll:

```
bundle exec jekyll serve
```

If you want to shorten this long command, you can put this code in a file such as jekyll.sh (on a Mac) and then simply type `. jekyll.sh` to build Jekyll.

## Running the site in Docker

You can also use Docker to directly build and run the site on your local machine. Just clone the repo and run the following from your working dir:
```
docker-compose build --no-cache && docker-compose up
```
The site should now be running at [http://localhost:4000/](http://localhost:4000/).

This is perhaps the easiest way to see how your site would actually look.

## Configure the sidebar

There are several products in this theme. Each product uses a different sidebar. This is the essence of what makes this theme unique -- different sidebars for different product documentation. The idea is that when users are reading documentation for a specific product, the sidebar navigation should be specific to that product. (You can read more of my thoughts on why multiple sidebars are important in this [blog post](http://idratherbewriting.com/2016/03/23/release-of-documentation-theme-for-jekyll-50/).)

The top navigation usually remains the same, because it allows users to navigate across products. But the sidebar navigation adapts to the product.

In each page's frontmatter, you must specify the sidebar you want that page to use. Here's an example of the page frontmatter showing the sidebar property:

<pre>
---
title: Alerts
tags: [formatting]
keywords: notes, tips, cautions, warnings, admonitions
last_updated: July 3, 2016
summary: "You can insert notes, tips, warnings, and important alerts in your content. These notes are stored as shortcodes made available through the linksrefs.hmtl include."
<span class="red">sidebar: mydoc_sidebar</span>
permalink: mydoc_alerts
---
</pre>

The `sidebar: mydoc_sidebar` refers to the \_data/sidebars/mydoc_sidebar.yml file.

Note that your sidebar can only have 2 levels (expand the **Tag archives** option to see an example of the second level). Given that each product has its own sidebar, this depth should be sufficient (it's really like 3 levels). Deeper nesting goes against usability recommendations.

You can optionally turn off the sidebar on any page (e.g. landing pages). To turn off the sidebar for a page, you should set the page frontmatter tag as `hide_sidebar: true`.

If you don't declare a sidebar, the `home_sidebar` file gets used as the default because this is the default specified in the config file:

```yaml
-
  scope:
    path: ""
    type: "pages"
  values:
    layout: "page"
    comments: true
    search: true
    sidebar: home_sidebar
    topnav: topnav
```

If you want to set different sidebar defaults based on different folders for your pages, specify your defaults like this:

```
-
  scope:
    path: "pages/mydoc"
    type: "pages"
  values:
    layout: "page"
    comments: true
    search: true
    sidebar: mydoc_sidebar
    topnav: topnav
```

This would load the `mydoc_sidebar` for each file in **pages/mydoc**. You could set different defaults for different path scopes.

For more detail on the sidebar, see [Sidebar navigation][mydoc_sidebar_navigation].

## Top navigation

The top navigation works just like the sidebar. You can specify which topnav data file should load by adding a `topnav` property in your page, like this:

```yaml
topnav: topnav
```

Here the topnav refers to the `_data/topnav.yml` file.

Because most topnav options will be the same, the `_config.yml` file specifies the topnav file as a default:

```yaml
-
  scope:
    path: ""
    type: "pages"
  values:
    layout: "page"
    comments: true
    search: true
    sidebar: home_sidebar
    topnav: topnav
```

## Sidebar syntax

The sidebar data file uses a specific YAML syntax that you must follow. Follow the sample pattern shown in the theme, specically looking at `mydoc_sidebar.yml` as an example: Here's a code sample showing all levels:

```yaml
entries:
- title: sidebar
  product: Jekyll Doc Theme
  version: 6.0
  folders:
  - title: Overview
    output: web, pdf
    folderitems:

    - title: Get started
      url: /index.html
      output: web, pdf
      type: homepage

    - title: Introduction
      url: /mydoc_introduction.html
      output: web, pdf

  - title: Release Notes
    output: web, pdf
    folderitems:

    - title: 6.0 Release notes
      url: /mydoc_release_notes_60.html
      output: web, pdf

    - title: 5.0 Release notes
      url: /mydoc_release_notes_50.html
      output: web, pdf

  - title: Tag archives
    output: web
    folderitems:

    - title: Tag archives overview
      url: /mydoc_tag_archives_overview.html
      output: web

      subfolders:
      - title: Tag archive pages
        output: web
        subfolderitems:

        - title: Formatting pages
          url: /tag_formatting.html
          output: web

        - title: Navigation pages
          url: /tag_navigation.html
          output: web

        - title: Content types pages
          url: /tag_content_types.html
          output: web
```

Each `folder` or `subfolder` must contain a `title` and `output` property. Each `folderitem` or `subfolderitem` must contain a `title`, `url`, and `output` property.

The two outputs available are `web` and `pdf`. (Even if you aren't publishing PDF, you still need to specify `output: web`).

The YAML syntax depends on exact spacing, so make sure you follow the pattern shown in the sample sidebars. See my [YAML tutorial](mydoc_yaml_tutorial) for more details about how YAML works.

{% include note.html content="If you have just one character of spacing off, Jekyll won't build due to the YAML syntax error. You'll see an error message in your console that says \"Error ... did not find expected key while parsing a block mapping at line 22 column 5. Error: Run jekyll build --trace for more information.\" If you encounter this, it usually refers to incorrect indentation or spacing in the YAML file. See the example mydoc_sidebar.yml file to see where your formatting went wrong." %}

Each level must have at least one topic before the next level starts. You can't have a second level that contains multiple third levels without having at least one standalone topic in the second level. If you need a hierarchy that has a folder that contains other folders and no loose topics, use a blank `-` item like this:

```yaml
entries:
- title: sidebar
  product: Jekyll Doc Theme
  version: 6.0
  folders:
  - title: Overview
    output: web, pdf
    folderitems:

    -

  - title: Release Notes
    output: web, pdf
    folderitems:

    - title: 6.0 Release notes
      url: /mydoc_release_notes_60.html
      output: web, pdf

    - title: 5.0 Release notes
      url: /mydoc_release_notes_50.html
      output: web, pdf

  - title: Installation
    output: web, pdf
    folderitems:

    - title: About Ruby, Gems, Bundler, etc.
      url: /mydoc_about_ruby_gems_etc.html
      output: web, pdf

    - title: Install Jekyll on Mac
      url: /mydoc_install_jekyll_on_mac.html
      output: web, pdf

    - title: Install Jekyll on Windows
      url: /mydoc_install_jekyll_on_windows.html
      output: web, pdf
```

To accommodate the title page and table of contents in PDF outputs, each product sidebar must list these pages before any other:

```yaml
- title:
  output: pdf
  type: frontmatter
  folderitems:
  - title:
    url: /titlepage
    output: pdf
    type: frontmatter
  - title:
    url: /tocpage
    output: pdf
    type: frontmatter
```

Leave the output as `output: pdf` for these frontmatter pages so that they don't appear in the web output.

For more detail on the sidebar, see [Sidebar navigation][mydoc_sidebar_navigation] and [YAML tutorial][mydoc_yaml_tutorial].

## Comments

The theme integrates [Commento.io](https://commento.io/) for comments below pages and posts. (This commenting service doesn't inject controversial tracking ads like Disqus does.) You will need to Commento.io account + plan ($5/month) to authorize Commento with your domain (no other configuration should be required). If you don't want comments, in the \_config.yml file, change the `comments: true` properties (under `defaults`) to `comments: false` in every instance. Then in the commento.html include file (inside \_includes), the `{% raw %}{% unless page.comments == false %} ... {% endunless %}{% endraw %}` logic will not insert the Commentio form.

## Relative links and offline viewing

This theme uses relative links throughout so that you can view the site offline and not worry about which server or directory you're hosting it. It's common with tech docs to push content to an internal server for review prior to pushing the content to an external server for publication. Because of the need for seamless transferrence from one host to another, the site has to use relative links.

To view pages locally on your machine (without the Jekyll preview server), they need to have the `.html` extension. The `permalink` property in the page's frontmatter (without surrounding slashes) is what pushes the files into the root directory when the site builds.

## Page frontmatter

When you write pages, include these same frontmatter properties with each page:

```yaml
---
title: "Some title"
tags: [sample1, sample2]
keywords: keyword1, keyword2, keyword3
last_updated: Month day, year
summary: "optional summary here"
sidebar: sidebarname
permalink: filename.html
---
```

(You will customize the values for each of these properties, of course.)

For titles, surrounding the title in quotes is optional, but if you have a colon in the title, you must surround the title with quotation marks. If you have a quotation mark inside the title, escape it first with a backlash `\`.

Values for `keywords` get populated into the metadata of the page for SEO.

Values for `tags` must be defined in your \_data/tags.yml list. You also need a corresponding tag file inside the tags folder pages/tags/ that follows the same pattern as the other tag files shown in the tags folder. (Jekyll won't auto-create these tag files.)

If you don't want the mini-TOC to show on a page (such as for the homepage or landing pages), add `toc: false` in the frontmatter.

The `permalink` value should be the same as your filename and include the ".html" file extension.

For more detail, see [Pages][mydoc_pages].

## Where to store your documentation topics

You can store your files for each product inside subfolders following the pattern shown in the theme. For example, product1, product2, etc, can be stored in their own subfolders inside the \_pages directory. Inside \_pages, you can store your topics inside sub-subfolders or sub-sub-folders to your heart's content. When Jekyll builds your site, it will pull the topics into the root directory and use the permalink for the URL.

Note that product1, product2, and mydoc are all just sample content to demonstrate how to add multiple products into the theme. You can freely delete that content.

For more information, see [Pages][mydoc_pages] and [Posts][mydoc_posts].

## Configure the top navigation

The top navigation bar's menu items are set through the \_data/topnav.yml file. Use the top navigation bar to provide links for navigating from one product to another, or to navigate to external resources.

For external URLs, use `external_url` in the item property, as shown in the example topnav.yml file. For internal links, use `url` the same was you do in the sidebar data files.

Note that the topnav has two sections: `topnav` and `topnav_dropdowns`. The topnav section contains single links, while the `topnav_dropdowns` section contains dropdown menus. The two sections are independent of each other.

## Generating PDF

If you want to generate PDF, you'll need a license for [Prince XML](http://www.princexml.com/). You will also need to [install Prince](http://www.princexml.com/doc/installing/).  You can generate PDFs by product (but not for every product on the site combined together into one massive PDF). Prince will work even without a license, but it will imprint a small Prince image on the first page, and you're supposed to buy the license to use it.

If you're on Windows, install [Git Bash client](https://git-for-windows.github.io/) rather than using the default Windows command prompt.

Open up the css/printstyles.css file and customize the email address (`youremail@domain.com`) that is listed there. This email address appears in the bottom left footer of the PDF output. You'll also need to create a PDF configuration file following the examples shown in the pdfconfigs folder, and also customize some build scripts following the same pattern shown in the root: pdf-product1.sh

See the section on [Generating PDFs][mydoc_generating_pdfs] for more details about setting the theme up for this output.

## Blogs / News

For blog posts, create your markdown files in the \_posts folder following the sample formats. Post file names always begin with the date (YYYY-MM-DD-title).

The news/news.html file displays the posts, and the news_archive.html file shows a yearly history of posts. In documentation, you might use the news to highlight product features outside of your documentation, or to provide release notes and other updates.

See [Posts][mydoc_posts] for more information.

## Markdown

This theme uses [kramdown markdown](http://kramdown.gettalong.org/). kramdown is similar to Github-flavored Markdown, except that when you have text that intercepts list items, the spacing of the intercepting text must align with the spacing of the first character after the space of a numbered list item. Basically, with your list item numbering, use two spaces after the dot in the number, like this:

```
1.  First item
2.  Second item
3.  Third item
```

When you want to insert paragraphs, notes, code snippets, or other matter in between the list items, use four spaces to indent. The four spaces will line up with the first letter of the list item (the <b>F</b>irst or <b>S</b>econd or <b>T</b>hird).

```
1.  First item

    ```
    alert("hello");
    ```

2.  Second item

    Some pig!

3.  Third item
```

See the topics under "Formatting" in the sidebar for more information.

## Automated links

If you want to use an automated system for managing links, see [Automated Links][mydoc_hyperlinks.html#automatedlinks]. This approach automatically creates a list of Markdown references to simplify linking.

## Other instructions

The content here is just a getting started guide only. For other details in working with the theme, see the various sections in the sidebar.

{% include links.html %} -->
