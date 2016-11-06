
#Flaskとtensorflowとherokuで「hello world」を動かしてみた

http://qiita.com/music431per/items/2ce11bc3def42b5bcad1

echo を使ってファイルを作るとおかしくなる。

# Flask Install

```bash
pip install Flask
```

# Gunicorn Install
```bash

pip install gunicorn
```


# create hello.py
```python
import tensorflow as tf
import multiprocessing as mp

from flask import Flask

app = Flask(__name__) 

@app.route('/')
def hello_world():
    core_num = mp.cpu_count()
    config = tf.ConfigProto(
        inter_op_parallelism_threads=core_num,
        intra_op_parallelism_threads=core_num )
    sess = tf.Session(config=config)

    hello = tf.constant('hello, tensorflow!')
    return sess.run(hello)

if __name__ == '__main__':
    app.run()
```

ローカルで実行する

$ python hello.py
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

Procfile

web: gunicorn hello:app --log-file -


pip freeze > requirements.txt


.gitignore を作る


*.pyc
*.pyo
tensorflow-hello

## herokuにデプロイ

$ git init
$ git add .
$ git commit -m "Initial Commit"
$ heroku apps:create flask-hello-2121
$ heroku buildpacks:add heroku/python
$ git push heroku master

## webページを開く

$ heroku open