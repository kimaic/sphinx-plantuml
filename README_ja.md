# このDockerイメージについて

gitlab-ciでSphinxのドキュメントをビルドするためのイメージです。

よく使われるblockdiagやplantuml、有用なテーマを同梱しています。

毎回、gitlab-ci.ymlでpip instalやapt-get installを繰り返せば、このイメージを利用せずとも同じことが可能ですが、
時間効率のために、必要なパッケージを含めたDockerイメージとしてます。

sphinxのソースファイルを書くことに専念するために、ビルド環境についてはオールインワンにした、というところです。



## 単独での使い方

ソースファイルがカレントディレクトリの下、sourceフォルダにあると仮定して、これをビルドする例です。

```sh
docker run --rm -v `pwd`:/tmp/sphinx --name sphinx sphinx-plantuml sphinx-build -b html /tmp/sphinx/source /tmp/sphinx/build
```

## Gitlab-CIでの使い方

サンプルを示します。

```yaml
image: tsgkadot/sphinx-plantuml:latest

stages:
  - build

pages:
  stage: build
  script:
    - pip install -r requirements.txt -U
    - sphinx-build -b html ./source public
  artifacts:
    expire_in: 1 week
    paths:
      - public
  tags:
    - docker
```

実際にビルドに使用したスクリプトとその生成物については、以下にあります。

- [.gitlab-ci.yml](https://gitlab.com/tsgkdt/sphinx-plantuml/blob/master/.gitlab-ci.yml)
- [ビルドされた結果のHTMLたち => gitlab pages](https://tsgkdt.gitlab.io/sphinx-plantuml/) 

## base image

python:3.6-alpine (公式イメージ)

## 導入済みパッケージ

- sphinx 1.6.3
- sphinxcontrib-blockdiag 1.5.5
- sphinxcontrib-actdiag 0.8.5
- sphinxcontrib-nwdiag 0.9.5
- sphinxcontrib-seqdiag 0.8.5
- sphinxcontrib-plantuml 0.8.1
- recommonmark 0.4.0

## 導入済みソフトウェア

- java (plantumlで使用します）
- graphviz (plantumlで使用します）
- plantuml（Version 8057）
- git (#1)

plantumlでは、javaとgraphvizが必要です。

## 導入済みテーマ

見た目がナウいbootstrapのテーマを入れています。ご利用ください。

| theme | version | remarks |
|:------|---------|----------:|
| [sphinxbootstrap4theme](https://github.com/myyasuda/sphinxbootstrap4theme) | 0.5.0 | Bootstrapテーマです |
| [sphinx_materialdesign_theme](https://github.com/myyasuda/sphinx_materialdesign_theme) | 0.0.4 | マテリアルデザインに準じたテーマです |


テーマの利用イメージは下記のデモサイトをご覧ください。

- [Bootstrap Theme Demo Site](https://myyasuda.github.io/sphinxbootstrap4theme/)
- [Material Design Theme Demo Site](https://myyasuda.github.io/sphinx_materialdesign_theme/)


# plantuml location

plantumlのjarファイルは以下の場所にあります。conf.pyで指定するときの参考にしてください。

- /usr/local/plantuml/plantuml.jar

環境変素 $PLANTUML でjarファイルのパスを保持しています。

```sh
echo $PLANTUML

```

## Installed Fonts

- [TakaoFonts_00303.01](https://launchpad.net/takao-fonts)

Takaoフォントは以下にあります。　blockdiagで日本語を使用する場合など、ttfフォントの指定が必要となるときにお使いいただけます。

/usr/share/fonts/TakaoFonts/

# 参考情報

gitlab-ci.ymlやconf.pyの記載内容の参考にしてください。gitlab.ioでは、ビルドされた結果を表示しています。

- サンプルSphinxプロジェクトとgitlab-ciの例

  see [gitlab repogitory](https://gitlab.com/tsgkdt/sphinx-plantuml/)
