# yasio-docs-936

yasio 中文文档markdown仓库, 文档构建工具: [mkdocs](https://www.mkdocs.org/)

* 文档站点: https://yasio.org/
* 备用站点: http://docs.simdsoft.com:3003/yasio/

## Install mkdocs with mkdocs-material

- Install python3+
- pip install -r docs/requirements.txt

## pdf-export

- 插件: https://github.com/zhaoterryy/mkdocs-pdf-export-plugin
- ubuntu部署
  ```sh
  apt-get build-dep cairo
  pip3 install cffi
  pip3 install pytest-runner
  pip3 install WeasyPrint
  pip3 install mkdocs-pdf-export-plugin
  ```
- 参考: https://www.zhihu.com/question/20849824/answer/565654864

## 其他参考

- https://squidfunk.github.io/mkdocs-material/
- https://squidfunk.github.io/mkdocs-material/reference/admonitions/
- https://facelessuser.github.io/pymdown-extensions/
- https://cyent.github.io/markdown-with-mkdocs-material/
- https://github.com/jimporter/mike
