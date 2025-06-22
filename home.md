# KNU 2025 sever weather summer school: Land surface model

**By [Junhong Lee (이준홍)][junhong_homepage], Kyungpook national university**

This lecture introduces the fundamentals of land surface models (LSMs), which are essential components in weather and climate models. LSMs simulate the exchange of energy, water, and carbon between the land surface and atmosphere.


`````{admonition} Through this lecture, you will:
:class: tip

- Learn the basic concepts and principles of land surface modeling
- Understand key processes like surface energy balance, abd soil moisture dynamics
- Develop a simple "toy" land surface model to gain hands-on experience
`````

The lecture combines theoretical foundations with practical coding exercises using Python. By the end, you'll have both conceptual understanding and practical skills in land surface modeling.


<br>
<br>
<br>

- [To view the book online, go here][book].
- The book content are all in [github repository][repo].
- 강의 자료는 [이곳][git_page]에서 오른쪽 상단의 다운로드 버튼을 누르면 pdf를 받을 수 있습니다(Fig. 1).
<!-- git page 링크 수정 필요 -->

<figure>
 <img src="./_static/images/pdf_download.png" alt="PDF 강의자료 다운 방법">
 <figcaption>Figure 1. PDF 강의자료 다운 방법</figcaption>
</figure>

<br>
<br>
<br>
<hr>

### 1. 강사 소개
**[Junhong Lee (이준홍)][junhong_homepage]**

##### 학력
- 2008 - 2012: B.S. in Earth Science Education & Atmospheric Sciences, Kongju National University, Republic of Korea
- 2012 - 2019: PhD. in Atmospheric Sciences, Yonsei University, Seoul, Republic of Korea

##### 경력
- Feb. 2019 - Sep. 2019: Postdoctoral Researcher, Department of Atmospheric Sciences, Yonsei University, Republic of Korea
- Sep. 2019 - Feb. 2025: Wissenschaftler (scientist), Max Plank Institute for Meteorology, Hamburg, Germany
- Mar. 2025 - Present: Research Professor, Kyungpook National University, Daegu, Republic of Korea

##### 모델 개발 경력
- Yonsei University Surface Layer (YSL) scheme in the WRF model {cite:p}`lee2020implementation`.
- 3-D Smagorinsky turbulence scheme in the ICON model {cite:p}`lee2022climatic`.
- Kilometer-scale Earth System Modeling (ICON & IFS models) {cite:p}`segura2025nextgems`.

<br>
<br>
<br>
<hr>

### 2. 강의 대상

- 대학원생
- 학부 3~4학년

##### 추천 선행 지식 
- 전반적인 대기과학 기본 지식
- Python에 대한 기본 지식

<br>
<br>
<br>
<hr>

### 3. 강의 목표
`````{admonition} 강의 목표:
:class: tip

1. 지면 모수화 (land surface model, LSM)에 대하여 이해한다.
2. 'Toy'<sup>[<a href="#footnote-1">1</a>]</sup> land surface model을 만들 수 있다.
`````

<a name="footnote-1"><sup>[1]</sup></a> 'Toy model'은 매우 단순화된 모델로, 복잡도를 낮추어 개념 설명/이해 및 이상 실험에 쓰인다.

<br>
<br>
<br>
<hr>

### 4. 강의 수강

##### 강의 자료 받기
1. [이곳][git_page]에서 오른쪽 상단의 다운로드 버튼을 누르면 pdf를 받을 수 있습니다(Fig. 1).
<!-- git page 링크 수정 필요 -->

<figure>
  <img src="./_static/images/pdf_download.png" alt="PDF 강의자료 다운 방법">
  <figcaption>Figure 1. PDF 강의자료 다운 방법</figcaption>
</figure>


2. 강의 jupyter notebook 받기
[이곳][repo]에서 받거나, sever에서 다음 명령어를 통하여 받을 수 있습니다:
```{code-block} bash
:linenos:
git clone git@github.com:junhonglee89/KNU_2025_summer_school_LSM.git
```

<br>
<br>

##### 강의 자료 사용에 필요한 프로그램 설치
직접 toy model을 만들고, 돌려보고 싶은 분들은 다음과 같은 프로그램들을 설치하여야합니다.


> **필요 프로그램**
> 1. Git
> 2. Anaconda
> 3. 편집툴 (e.g., vim, VSCODE 등)

`````{admonition} Windows 사용자를 위한 git & anaconda 설치 방법
Cygwin을 통한 git 설치:
- [Cygwin 웹사이트](https://www.cygwin.com/)에서 설치 프로그램을 다운로드
- 설치 중 패키지 선택 단계에서 'git'을 검색하여 선택
- 설치 완료 후 Cygwin 터미널에서 git 명령어 사용 가능
- [참조 사이트][https://git-scm.com/book/ko/v2/시작하기-Git-설치]

anaconda 설치:
- [anaconda 웹사이트](https://www.anaconda.com/download)에서 설치 프로그램을 다운로드 후 설치

`````


`````{admonition} Linux 사용자를 위한 git & anaconda 설치 방법
git 설치:
    - [Git homepage 참조](https://git-scm.com/book/ko/v2/시작하기-Git-설치)

anaconda 설치:
```{code-block} bash
:linenos:
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh
bash Anaconda3-2024.02-1-Linux-x86_64.sh
~/anaconda3/bin/conda init
source ~/.bashrc
```
`````


`````{admonition} Mac 사용자를 위한 git & anaconda 설치 방법
Brew 설치:
```{code-block} bash
:linenos:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

Brew를 통한 git 설치:
```{code-block} bash
:linenos:
brew install git
```

Brew을 통한 anaconda 설치:
```{code-block} bash
:linenos:
brew install --cask anaconda
```
`````

<br>
<br>

##### 직접 toy model 수행해보기

1. jupyter notebook 파일 받기
```{code-block} bash
:linenos:
git clone git@github.com:junhonglee89/KNU_2025_summer_school_LSM.git
```

2. anaconda environment 설치
```{code-block} bash
:linenos:
cd ./KNU_2025_summer_school_LSM
conda env create --file environment.yml
conda activate KNU_2025_summer_school
```

3. jupyter notebook 실행
```{code-block} bash
:linenos:
jupyter notebook
```
웹 브라우저가 띄워진 후, 원하는 jupyter notebook 파일 실행.

<br>
<br>
<br>
<hr>

### 5. 기타 참고 사항

- This book is based on a textbook {cite:p}`stensrud2007parameterization`, and technical documentations of land surface models {cite:p}`reick2021jsbach` and {cite:p}`ek1991osu`.

- This book is inspired by [Climate Laboratory][climlab], a lecture in University at Albany.

- This book is powered by [JupyterBook][jupyterbook], and aims to be all of the following:
    1. **self-reproducing** *(most figures are self-generating in the notebooks)*
    2. **interactive** *(readers can run and modify code examples)*
    3. **living document** *(content will continue to evolve, and bug report is welcome)*

```{warning}
본 자료는 저작자의 명시적 동의 없이 복제, 배포, 수정, 전재, 2차 저작물 제작에 사용할 수 없습니다. 단, 출처를 명확히 밝히는 경우에 한하여 교육·연구 목적의 사용은 허용됩니다.
```


[junhong_homepage]: https://scholar.google.com/citations?user=CfzQ610AAAAJ&hl=en
[git_page]: https://junhonglee89.github.io/KNU_2025_summer_school_LSM/
[jupyterbook]: https://jupyterbook.org
[climlab]: https://github.com/climlab/climlab
[book]: https://junhonglee89.github.io/KNU_2025_summer_school_LSM/
[repo]: https://github.com/junhonglee89/KNU_2025_summer_school_LSM
[notebook]: https://jupyter-notebook.readthedocs.io/en/stable/
