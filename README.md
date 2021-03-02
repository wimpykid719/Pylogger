# Pylogger

It out put error log to log.txt file

エラーをlog.txtに出力する。

# Demo

Script run then create log.txt.

スクリプトを走らせるとlog.txtが生成される。

![logtxt](https://user-images.githubusercontent.com/23703281/109614845-80ab4f00-7b76-11eb-813b-0e01e1639850.png)

If Script gets error then error out put to log.txt.
エラーがあるとログに書き込まれる。

![lotxtError](https://user-images.githubusercontent.com/23703281/109615060-c7994480-7b76-11eb-8c2f-ec3ce7ffb683.png)


# Features

It can use in the case of can't use console but want to check the error.

コンソールでエラーを確認出来ないが、エラーを確認したい場合に使えます。

エラーをテキストにして残して起きたい場合。


# Requirement

- sys
- logging

# Installation

Sorry for doesn't use pip

copy and paste

まだpip等の設定は出来ないから必要なRequirementインストールして

コピー・ペーストで動かして😅



# Usage

```Python
'''
同じ階層にmodulesフォルダを生成してそこにcreatelog.py
という名前でスクリプトを保存して
'''
from modules import createlog
```

Create modules folder at same directory which you want to make check error then, create `createlog.py` in modules.

and write a code import `createlog` which you want to check error script.

エラーをチェックしたいスクリプトにimportして使う。


# Author

# License

Pylogger is under [MIT license](https://en.wikipedia.org/wiki/MIT_License).

Mit License

コードのレビューをして下さると助かります。

# 解説

プログラムを書くまでに勉強した内容です。これさえ読めばプログラムが何をしているのか分かります。

## 最初に

普段は `print()` でデバッグを済ましており、それだけだと対応仕切れない場面が出てきたのでloggingの使い方を学ぼうと思う。例：Pythonスクリプトをmacで実行出来る形式にした際にコンソールに出力されなくなり、必要になった。

## ログレベル

Pythonのロギングには5つのレベルがある。デフォルトは値は `warning` になっており、 `critical, error, warning` までが出力される。

`info`, `debug` はコンソールに出力されない

```python
import logging

logging.critical('critical')
logging.error('error')
logging.warning('warning')
logging.info('info')
logging.debug('debug')

#実行結果
CRITICAL:root:critical
ERROR:root:error
WARNING:root:warning
```

`info`, `debug` も出力したい場合は `basicConfig` でログレベルを下に変更する。

```python
import logging

# ログレベルを DEBUG に変更
logging.basicConfig(level=logging.DEBUG)

logging.critical('critical')
logging.error('error')
logging.warning('warning')
logging.info('info')
logging.debug('debug')

#実行結果
CRITICAL:root:critical
ERROR:root:error
WARNING:root:warning
INFO:root:info
DEBUG:root:debug
```

## ログのフォーマットを変更する。

個人的にはログが出てこればそれで十分なのだが一応見ていこうと思う。

```python
import logging

# ログレベルを DEBUG に変更
logging.basicConfig(level=logging.DEBUG)

# 従来の出力
logging.info('error{}'.format('outputting error'))
logging.info('warning %s %s' % ('was', 'outputted'))
# logging のみの書き方
logging.info('info %s %s', 'test', 'test')
```

まぁテンプレートリテラルみたいなの使えますよってだけだった。

## ログをファイルに出力

これを今回行いたかった。コンソールに出力されないのでログをファイルに書き出す。

```python
import logging

# ログレベルを DEBUG に変更
logging.basicConfig(filename='logfile/logger.log', level=logging.DEBUG)
```

filenameにパスを指定してあげれば生成されるみたいだ。フォルダは生成されないのであらかじめ作って置かないとパスエラーになる。

## ロガーの設定

```python
import logging

# ロギングの基本設定(infoレベルを指定)
logging.basicConfig(level=logging.INFO)
logging.info('info')

# 現在のロギングの情報を取得(引数はファイル名)
logger = logging.getLogger(__name__)
# ロギングの設定を DEBUG に変更
logger.setLevel(logging.DEBUG)
logger.debug('debug')

#実行結果
INFO:root:info
DEBUG:__main__:debug
```

`__name__` を引数にする事で現在実行中のPythonファイル名を取得している。それはわかるんだけどいまいち上記のコードではただ `debug` という文字列が出力されるだけでデバッグの意味をなしていない気がする。 `logging.info('文字列')` , `logging.debug('文字列')` とする事でそのレベルでログの出力が許可されていたら文字列を出力するくらいの機能にしかなってない。

どんなエラーなのかまでは分からない。そもそもエラー出なくても出力されているから上記だけでは意味ないよな。

## ロギング ハンドラとは

ハンドラ：特定の処理に対して、処理を行うプログラムを指す。

例をあげるとtry except構文もエラーの発生に対して処理を実行するからハンドラーに該当する。

main処理に記述するbasiConfigのfilenameにログのパスを記述すればログが書き込まれる事については上記に記してある。

main処理以外のログ情報をファイルに出力する為には、ハンドラを使う必要がある。

`logging.FileHandler('logfile/logger.log')` を使用する事でコンソールではなくファイルにログを残せる。

上記で先に紹介した `logging.basicConfig(filename='logfile/logger.log', level=logging.DEBUG)` との違いは上記はデフォルトのロギング設定を変更するものであり、 `FileHandler` はまず、 `logging.getLogger(__name__)` でloggerと呼ばれる物を生成してそれの設定を変更してるイメージなのかもしれない。一段階壁を置くみたいな感じどんな意味があるかよく分からない。別の設定でロガーを生成したり汎用性が高くなるのかもしれない。

```python
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

##ハンドラ取得
get_handler = logging.FileHandler('logfile/logger.log')
logger.addHandler(get_handler)

def do_something():
    logger.info('from logger_lesson2')
    logger.info('from logger_lesson2_debug')
```

これをデバックしたいスクリプトに書いてmain.py 等 のメイン処理にimportして実行する。ことで個別にデバックレベル等設定出来る。

上記の情報を元にオリジナルのロガー関数を作成してみた。

```python
import logging

def get_logger(logger_name, log_file, f_fmt='%(message)s'):

	"""ロガーを取得"""
	# ロガー作成
	logger = logging.getLogger(logger_name)
	logger.setLevel(logging.DEBUG)

	# ファイルハンドラ作成
	file_handler = logging.FileHandler(log_file, mode='a', encoding='utf-8')
	file_handler.setLevel(logging.DEBUG)
	file_handler.setFormatter(logging.Formatter(f_fmt))

	# ロガーに追加
	logger.addHandler(file_handler)

	return logger
```

この関数を使用したいスクリプトで `import` して

```python
import sys
# 同じ階層のmodulesフォルダにcreatelog.pyで保存している。
from modules import createlog

lg = createlog.get_logger(__name__, 'log.txt')
lg.debug('ロギング 開始')

try:
	#デバックしたい処理
except:
	lg.exception(sys.exc_info())#エラーをlog.txtに書き込む
```

実行するとスクリプトを実行した階層にlog.txtが生成される。

## 終わりに

結局、実行形式にしたアプリに導入したが、ファイルを生成する事が出来ない（バグか仕様？）ので機能しなかった。普通のPythonスクリプトではもちろん使えるので今後、使用する機会があれば積極的に使っていきたい。

### 参照

[Pythonでロギングを学ぼう - Qiita](https://qiita.com/__init__/items/91e5841ed53d55a7895e)

[Pythonでログを出力するコード例【logging】](https://srbrnote.work/archives/1656)