## Laboratory work IX

Данная лабораторная работа посвещена изучению процесса создания пакета на примере **Github Release**

```ShellSession
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [x] 4. Сгенерировать GPG ключ и добавить его к аккаунту сервиса **GitHub**
- [x] 5. Выполнить инструкцию учебного материала
- [x] 6. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_TOKEN=6ef94034c7842b283a4de661c910635108009509
$ export GITHUB_USERNAME=b2017-17viu16m
$ alias gsed=sed # for *-nix system
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ go get github.com/aktau/github-release
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09
```

```ShellSession
$ gsed -i 's/lab08/lab09/g' README.md
```

```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```

```ShellSession
$ travis login --auto
$ travis enable
```

```ShellSession
$ git tag -s v0.1.0.0
$ git tag -v v0.1.0.0
$ git push origin master --tags
```

```ShellSession
$ github-release --version
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ github-release release \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```

```ShellSession
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m` 
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz
$ github-release upload \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
```

```ShellSession
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
$ tar -ztf ${PACKAGE_FILENAME}
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=09
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```
## Result

Создан и добавлен в профиль GPG-ключ:
gpg: ключ 8A8833B3 помечен как абсолютно доверенный.
открытый и закрытый ключи созданы и подписаны.

gpg: проверка таблицы доверия
gpg: требуется 3 с ограниченным доверием, 1 с полным, модель доверия PGP
gpg: глубина: 0  верных:   1  подписанных:   0  доверие: 0-, 0q, 0n, 0m, 0f, 1u
gpg: срок следующей проверки таблицы доверия 2018-09-22
pub   4096R/8A8833B3 2018-03-26 [годен до: 2018-09-22]
      Отпечаток ключа = 4467 99F8 4C39 704A 0191  07B3 5FEA 9389 8A88 33B3
uid                  b2017-17viu16m <b2017-17viu16m@yandex.ru>
sub   4096R/32BF8FBE 2018-03-26 [годен до: 2018-09-22]

Изучен механизм Release на Github, созданы метки, опубликова релиз v0.1.0.0, для верификации меток и коммитов использован GPG-ключ, получен файл релиза print-Linux-x86_64.tar.gz.

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2017 Братья Вершинины
```
