require 'fileutils'
require 'rake/clean'

CONCAT_FILE = "all.md"
HEADER_COMMENT = <<-EOB
（訳者注: これは、[JavaScript Stack from Scratch](https://github.com/verekia/js-stack-from-scratch)を翻訳し、まとめて読めるように1ファイルにしたものです。元の翻訳と各種ファイルについては、[日本語訳forkリポジトリ](https://github.com/takahashim/js-stack-from-scratch)を参照してください。また、原文が活発に更新されているため、訳文も追従して更新されます。ご了承ください。）

EOB

TOC = <<-EOB
[1 - Node、NPM、Yarn、そして package.json](#1---nodenpmyarnそしてpackagejson)

[2 - packageのインストールと使用](#2---パッケージのインストールと利用)

[3 - Babel と Gulp による ES6 のセットアップ](#3---babelとgulpによるes6のセットアップ)

[4 - ES6のクラス構文を使う](#4---es6のクラス構文を使う)

[5 - ES6モジュール構文](#5---es6モジュール構文)

[6 - ESLint](#6---eslint)

[7 - Webpackによるクライアントアプリ](#7---webpackによるクライアントアプリ)

[8 - React](#8---react)

[9 - Redux](#9---redux)

[10 - Immutable JS と Redux の改良](#10---immutable-jsとreduxの改良)

[11 - Mocha、Chai、Sinonによるテスティング](#11---mochachaisinonによるテスティング)

[12 - Flowによる型検査](#12---flow)

EOB

IMAGES = <<EOB
[![Yarn](https://qiita-image-store.s3.amazonaws.com/0/4293/2ee7be54-48f2-818a-8c81-4fc9d328b138.png)](https://yarnpkg.com/)[![React](https://qiita-image-store.s3.amazonaws.com/0/4293/23b5076b-0756-7e63-11f4-ca08cd5163e2.png)](https://facebook.github.io/react/)[![Gulp](https://qiita-image-store.s3.amazonaws.com/0/4293/12e41319-0fc3-36e6-14d9-e5f42d8b1e17.png)](http://gulpjs.com/)[![Redux](https://qiita-image-store.s3.amazonaws.com/0/4293/059d0798-395c-6b46-1853-02f515a9592e.png)](http://redux.js.org/)[![ESLint](https://qiita-image-store.s3.amazonaws.com/0/4293/ebf4eabb-78c8-0e85-6a27-fab4e5833da0.png)](http://eslint.org/)[![Webpack](https://qiita-image-store.s3.amazonaws.com/0/4293/dca06b8b-ca2b-e174-a08c-f325834f762d.png)](https://webpack.github.io/)[![Mocha](https://qiita-image-store.s3.amazonaws.com/0/4293/4182051d-cca7-f314-ffdb-58b243aa933c.png)](https://mochajs.org/)[![Chai](https://qiita-image-store.s3.amazonaws.com/0/4293/09ee8c5d-4472-3231-5440-0fc078353b5d.png)](http://chaijs.com/)[![Flow](https://qiita-image-store.s3.amazonaws.com/0/4293/02965c17-7816-dd23-e604-3c608e0c2c3b.png)](https://flowtype.org/)
EOB

def preface
  readme0 = File.read("README.md")
  readme0.gsub!(/(## 目次\n).*(## 今後の予定\n)/m){ $1 + TOC + $2 }
  readme0.gsub!(/(## 翻訳\n).*(## Credits\n)/m){ $2 }
  readme0.gsub!(%r{\[!\[Build Status\].*\(https://flowtype.org/\)\n}m){ IMAGES }
  HEADER_COMMENT + readme0
end

def trimming(file)
  content = File.read(file)
  content.gsub!(/^\s*次章.*$/m,"")
  content.gsub!(/^\s*\[前章.*$/m,"")
  content
end

def detect_chapnum(file)
  f1 = file.gsub(%r|tutorial/|, "")
  f2 = f1.gsub(/\-.*$/,"")
  f2.to_i
end

task :default => :concat

desc 'cocant all README.md files'
task :concat do
  chaps = []
  chaps[0] = preface
  Dir.glob("tutorial/*/README.md").each do |file|
    num = detect_chapnum(file)
    chaps[num] = trimming(file)
  end
  File.open(CONCAT_FILE,"w") do |f|
    f.write(chaps.join("\n\n"))
  end
end


