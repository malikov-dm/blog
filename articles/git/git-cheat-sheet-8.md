# Шпаргалка по использованию Git 8

## Перемещение коммитов

### Rebase ветки

В случае, если появилось желание или необходимость начать ветку с другого коммита (тем самым, добавив в нее изменения, внесенные после разделения веток), можно произвести перебазирование ветки:

```bash
git checkout feature                            # перейти на ветку, которую необходимо перенести
git rebase master                               # перенести текущую ветку на вершину ветки master
```

При возникновении конфликтов возможны следующие действия:

```bash
git rebase --continue                           # нужно устранить конфликты и продолжить rebase
git rebase --abort                              # откат изменений в первоначальное состояние
git rebase --quit                               # выход из процесса перебазирование на текущем шаге
git rebase --skip                               # пропуск коммита при перебазировании
```

Если появилась необходимость вернуть все как было, то есть фактически отменить перенос, можно воспользоваться ссылкой `ORIG_HEAD`

```bash
git reset --hard ORIG_HEAD                      # передвинет указатель на вершину ветки по ссылке ORIG_HEAD
```

### Тестирование команды `git rebase`

Применение команды `git rebase` можно протестировать, используя ключ `-x`:

```bash
git rebase -x '<testing_script>' master         # если testing_script отработает с ошибкой, rebase не выполнится
```

### Перенос части ветки

Можно перенести часть ветки: `fix` - ветка, которую мы переносим, `master` - ветка, куда мы переносим ветку `fix`, `feature` - ветка, с которой начинается отсчет коммитов ветки `fix`:

```bash
git checkout fix
git rebase --onto master feature
```

### Перебазирование коммитов слияний

По умолчанию `rebase` пропускает коммиты слияния. Это может привести к ситуациям, когда ветка переносится не полностью, копируется лишняя ветка и другим проблемам. Избежать этого поможет флаг `-p` или `--preserve-merges':

```bash
git rebase -p master                             # создается новый коммит слияния, а лишняя ветка не переносится
```

***Если же коммит слияния содержал в себе исправления, то эти исправления перенесены не будут.***

### Итерактивное слияние

Если запустить перемещение в режиме интерактива `git rebase -i master`, появится возможность составить скрипт действий для каждого коммита, выполняемых при перемещении.
