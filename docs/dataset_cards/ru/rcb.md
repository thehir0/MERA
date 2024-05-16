# rcb

## Описание задачи

**Russian Commitment Bank (RCB)** - это набор из естественных контекстов, в которых содержится или отсутствует причинно-следственная связь, а также размечены дополнительные дискурсивные характеристики - вопрос, модальность, отрицание предшествующих условий.

Датасет входит в бенчмарк [Russian SuperGLUE](https://russiansuperglue.com/tasks/task_info/RCB) [1], был переведен в инструктивный формат и перепроверен. Исходный сет является аналогом английского [датасета CommitmentBank (CB)](https://github.com/mcdm/CommitmentBank) [2, 3].

**Ключевые слова:** Оценка здравого смысла, причинно-следственные связи, RTE (Recognizing Textual Entailment),  многоклассовая классификация

**Авторы:** Татьяна Шаврина, Алена Феногенова, Валентин Малых, Екатерина Артемова, Владислав Михайлов, Мария Тихонова, Денис Шевелёв, Антон Емельянов, Андрей Евлампиев

### Мотивация

Датасет позволяет оценить насколько модели умеют разрешать логическое следствие, задача (textual entailment). Датасет построен таким образом, чтобы учитывать дискурсивные характеристики. Датасет в бенчмарке Russian SuperGLUE один из немногих, для которых всё ещё сохраняется значительный разрыв между оценками моделей и человеческой.

## Описание данных

### Поля датасета

Каждый пример данных датасета представляет собой некоторую ситуацию предпоссылки

- instruction — инструктивный промпт заданный под текущее задание;
- inputs — словарь, содержащий следующую информацию:
    - premise — текст посылки c изначальной ситуацией;
    - hypotesis — текст гипотезы, для которой требуется определить, следует ли она из посылки или нет;
- outputs — информация об ответе;
- meta — метаинформация о задаче:
    - id — номер примера из датасета;
    - negation — информация об отрицании;
    - genre — жанр текста;
    - verb — глагол действия, по которому подбирались тексты.

### Пример из датасета

```jsx
{
    "instruction": "Приведено описание ситуации и гипотеза. Ситуация: \\"{premise}\\" Гипотеза: \\"{hypothesis}\\". Определи отношение гипотезы к ситуации, выбери один из трех вариантов: 1 - гипотеза следует из ситуации, 2 - гипотеза противоречит ситуации, 3 - гипотеза независима от ситуации. В ответ напиши только цифру 1, 2 или 3, больше ничего не добавляй.",
    "inputs": {
		"premise": "Сумма ущерба составила одну тысячу рублей. Уточняется, что на место происшествия выехала следственная группа, которая установила личность злоумышленника. Им оказался местный житель, ранее судимый за подобное правонарушение.",
		"hypothesis": "Ранее местный житель совершал подобное правонарушение."
},
    "outputs": "1",
    "meta": {
	"verb": "судить",
        "negation": "no_negation",
        "genre": "kp",
        "id": 0
    }
}
```

Варианты ответа пишутся в поле outputs(строковые значения): 1*- гипотеза следует из ситуации,*  2 - *гипотеза противоречит ситуации,* или 3 - *гипотеза независима от ситуации.*

### Разбиение датасета

Количество обучающих примеров в датаcете 438 тренировочных, 220 валидационных примеров и 438 тестовых.  Количество предложений всего сета равно 2715, а количество токенов: 3.7 · 10^3.

### Промпты

Промпты (всего 9 штук) представлены в виде инструкции, в которой даны ситуация и гипотеза. Необходимо определить есть ли логическая связь между ситуацией и гипотезой.

Пример промта:

```jsx
"Ситуация: \\"{premise}\\" Гипотеза: \\"{hypothesis}\\". Определи логическое отношение гипотезы к ситуации, возможен один из трех вариантов: 1 - гипотеза следует из ситуации, 2 - гипотеза противоречит ситуации, 3 - гипотеза независима от ситуации. В ответ напиши только цифру 1, 2 или 3, больше ничего не добавляй."
```

### Создание датасета

Все примеры были собраны из открытых новостных источников и литературных журналов, на основе корпуса Taiga [4], а затем вручную перепроверены и дополнены человеческой оценкой на Yandex.Toloka.

## Оценка

### Метрики

В качестве метрики для оценки используется точность (Accuracy) и Макро усредненная F1.

### Человеческая оценка

Человеческая оценка производилась с помощью платформы Яндекс.Толока с перекрытием разметчиков равным 3.

Финальная оценка точности человека: **0.587.**

Финальная оценка усреденной макро F1 метрики у человека: **0.565.**

## Список литературы

[1] Tatiana Shavrina, Alena Fenogenova, Emelyanov Anton, Denis Shevelev, Ekaterina Artemova, Valentin Malykh, Vladislav Mikhailov, Maria Tikhonova, Andrey Chertok, and Andrey Evlampiev. 2020. RussianSuperGLUE: A Russian Language Understanding Evaluation Benchmark. In *Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP)*, pages 4717–4726, Online. Association for Computational Linguistics.

[2] Original CB paper: Marie-Catherine de Marneffe, Mandy Simons, and Judith Tonhauser (2019). The CommitmentBank: Investigating projection in naturally occurring discourse. Proceedings of Sinn und Bedeutung 23.

[3] Wang A. et al. Superglue: A stickier benchmark for general-purpose language understanding systems //Advances in Neural Information Processing Systems. – 2019. – С. 3261-3275.

[4] Shavrina, Tatiana, and Olga Shapovalova. "To the methodology of corpus construction for machine learning:“Taiga” syntax tree corpus and parser." *Proceedings of “CORPORA-2017” International Conference.* 2017.