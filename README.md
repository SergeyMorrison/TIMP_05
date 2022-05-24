[![Coverage Status](https://coveralls.io/repos/github/SergeyMorrison/TIMP_05/badge.svg?branch=master)](https://coveralls.io/github/SergeyMorrison/TIMP_05?branch=master)
# Изучение фреймворков для тестирования на примере GTest

Создайте CMakeList.txt для библиотеки banking.
----
Создаем файл CMakeLists.txt и пишем код.
``` nano CMakeLists.txt ```
<img width="464" alt="image" src="https://user-images.githubusercontent.com/99497212/170107203-dde85f6b-3f7a-4480-99c3-5963ae930347.png">

Создайте модульные тесты на классы ``Transaction`` и `Account`.
---
Cоздаем модульные тесты. 
Создаем папку "tests" и переключаемся на нее.
``` 
mkdir test
cd tests
```

Создаем файл "tests.cpp"
```nano test.cpp```

<img width="467" alt="image" src="https://user-images.githubusercontent.com/99497212/170108269-e25aeb18-dcca-40fa-ae65-a8e2431ac708.png">
<img width="467" alt="image" src="https://user-images.githubusercontent.com/99497212/170108358-0573d05f-6754-4599-b4e2-6f21ab31059b.png">
<img width="481" alt="image" src="https://user-images.githubusercontent.com/99497212/170108451-ca5f498c-e7f2-489c-9ddf-d5327541ff9e.png">
После чего коммитим и пушим все в наш репозиторий на GitHub
Также добавляем в репозиторий папку banking из данного нам репозитория по условию.

Настройте сборочную процедуру на TravisCI.
---
Настраиваем GitHub Actions
```
name: Bank
on: [ push ]
jobs:
  BuildTest:
    runs-on: ubuntu-latest
    timeout-minutes: 3

    steps:
      - name: checkout
        uses: actions/checkout@v3

      - name: prepare
        run: |
          sudo apt-get install lcov
      - name: building
        run: |
          mkdir build && cd build
          cmake -DBUILD_TESTS=ON -DCOVERALLS=ON ..
          make
      - name: test
        run: |
          cd build
          ctest
      - name: coverage
        run: |
          mkdir coverage
          lcov -d . -t bank_test -o coverage/lcov.info  -b . -c --no-external
          lcov --remove coverage/lcov.info '*/gmock/*' '*/gtest/*' '*/include/*' -o coverage/lcov.info
      - name: Publish to coveralls.io
        uses: coverallsapp/github-action@master
        with:
          github-token: ${{ github.token }}
```

Настройте Coveralls.io.
---
Подключаем coveralls к нашему репозиторию, создаем файл .coveralls.yml и загружаем свой токен.
Проверяем на сайте покрытие кода и видим результат. [![Coverage Status](https://coveralls.io/repos/github/SergeyMorrison/TIMP_05/badge.svg?branch=master)](https://coveralls.io/github/SergeyMorrison/TIMP_05?branch=master)
