---
layout: default
title: Archive
parent: Git
---

# Git Archive

"Exportando" um projeto do git em um arquivo compactado sem o diretório .git.

_OBS: o arquivo não conterá o diretório .git, mas conterá outros arquivos específicos do git ocultos, como .gitignore, .gitattributes, etc._

---

- [stackoverflow.com/questions/160608/do-a-git-export-like-svn-export](https://stackoverflow.com/questions/160608/do-a-git-export-like-svn-export/163769#163769)

```sh
git archive --format zip --output /full/path/to/zipfile.zip master
```