# ruMultiAr

## Описание задачи

**Russian Multistep Arithmetic (ruMultiAr)** **/ Многоступенчатая арифметика** — это математическая задача из [BIG-bench](https://github.com/google/BIG-bench/blob/main/bigbench/benchmark_tasks/multistep_arithmetic/README.md). Эта задача проверяет способность модели выполнять многоступенчатые арифметические операции, состоящие из сложения, вычитания, умножения и деления. Таким образом, мы можем измерить способность моделей мыслить последовательно.

**Ключевые слова:** арифметика, свободный ответ, математика, zero-shot

**Авторы:** Pablo Antonio Moreno Casares

### Мотивация

Эта задача относительно проста для человека, поскольку вычисление выполняется шаг за шагом. Соответственно задача направлена на проверку способности систем разбивать сложные задачи на более простые шаги и планировать действия. Более того, последовательное мышление является одним из навыков гибкого интеллекта в соответствии с теорией когнитивных способностей Кеттелла-Хорна-Кэрролла [1]. Этот тест направлен именно на проверку этого навыка.

## Описание датасета

Задача представляет собой арифметические выражения с несколькими уровнями вложенности и разной длиной содержимого внутри самой внутренней скобки.

### Поля датасета

- instruction — инструктивный промпт заданный под текущее задание;
- inputs — математическое выражение;
- outputs — целевая переменная, результат вычисления операций;
- meta — поле для дополнительной информации:
    - id — номер примера из датасета.

### Примеры данных

```jsx
{
    "instruction": "Вычисли результат выражения:\\n{inputs}"
    "inputs": "((-3) + 5) = "
    "outputs": "2"
    "meta": {"id": 1}
}
```

### Разбиение датасета

Набор данных состоит из обучающего сета с разметкой (1039 примеров) и тестового сета для оценки модели (1024 примеров).

### Промпты

Для датасета было подготовлено 6 промптов различной сложности. Пример:

```jsx
“Выполни следующие базовые арифметические операции в правильном порядке, в том числе учитывая порядок скобок, и напиши результат вычисления выражения в виде одного числа:\n{inputs}”
```

### Создание датасета

Данные в этой задаче генерируются автоматически скриптом. Скрипт генерирует примеры, перебирая различные конфигурации с разной глубиной вложенности и количеством аргументов в скобках, и фильтрует примеры с учетом следующих критериев.

Аргументы задачи генерируются из набора цифр [-9; 9]. random_seed для теста был выбран так, чтобы примеры максимально не пересекались с открытым сетом.

Оба сета были отфильтрованы таким образом, что:

- значения целевой переменной лежат в диапазоне от -1000 до 1000
- значения целевой переменной встречаются не чаще 10 раз в выборке
- дубликаты примеров удалены
- для примеров с делением отфильтрованы примеры с целочисленным результатом.

## Оценка

### Метрики качества

В качестве метрики качества используется Exact Match (EM).

Для каждого примера присваивается 1, если целевая последовательность ТОЧНО соответствует предсказанной последовательности. В противном случае присваивается 0. Общий балл равен средней точности по всем последовательностям.

### Человеческая оценка

Человеческая оценка замеряется на подмножестве из 600 примеров, просемплированных из тестовой выборки с разной сложностью (глубиной вложенности, длиной выражений внутри скобок) примерно по 50 на каждую конфигурацию. Запускался один пул на все подзадачи с перекрытием в 5 разметчиков.

Финальный результата человека равен 0.998.

## Ограничения

1. При оценке модели учитываются только числовые ответы (например, «4») вместо также допустимого текстового ответа (в данном случае «четыре»).
2. Текущая задача, однако, не позволяет нам отличить модель, выполняющую многошаговые операции, от модели с доступом к калькулятору / древовидным алгоритмам / скрипту для подсчета ответа.

## Ссылки

[1] Flanagan, D.P. & Dixon, S.G. (2014) The Cattell-Horn-Carroll theory of cognitive abilities. In C.R. Reynolds, K.J. Vannest and E. Fletcher-Janzen (eds.), *Encyclopedia of Special Education*. New York: Wiley Online.