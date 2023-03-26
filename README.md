## Задача поиска ассоиативных правил

В задаче применяются два алгоритма: APriory и FT-Growth (более эффективен на больших данных).

Задача относится к классу задач обучения без учителя. Дан бинарный набор признаков и нет ответов.

Для обучения важно какие признаки совместно встречаются у объекта (конъюнкция признаков, пересечение множеств). Выявляются такие подмножества признаков, у которых поддержка больше или равна минимальной поддержке (MinSupp).

Ассоциативные правила -  если встречается определенный набор признаков, то для него в большинстве случаев характерен признак (y). 

### Алгоритм APriori

<b>Вход:</b> обучающая выборка.

<b>Выход:</b> список ассоциативных правил.

Выделяется множество всех однопризнаковых подмножеств (отдельных признаков), которые являются частыми наборами (остальные признаки выбрасываются).

Строится множество всех частых наборов мощности и к каждому набору добавляется еще один признак так, чтобы подмножество было частым набором.

Каждый из наборов расщепляется на две непересекающиеся части - посылку и следствие и проверяется, являются ли они ассоциативным правилом (рекурсивная функция AssocRules).

<b>AssocRules</b> - простой алгоритм, выполняемый быстро в оперативной памяти. 

AssocRules(R, f, y), где R - набор ассоциативных правил, (f, y) - ассоциативное правило. на первой итерации f - набор признаков, который был найден, а y - пустой и AssocRules будет по одному переносить признаки из подмножества f в подмножество y, т.е. будет делать из них посылку и следствие, где посылка f, а следствие y.

Не самый эффективный алгорим для больших наборов данных. Используются различные методы оптимызации для применения алгоритма к большим наборам:

- более эффективные структуры данных для быстрого поиска частых признаков
- алгоритмы, учитывающие иерархию признаков
- инкрементные и декрементные алгоритмы

### Алгоритм FT-Growth (префиксное FP-дерево) (FP - frequent pattern)

Такая структура данных, в которой можно сохранить всю транзакционную базу таким образом, чтобы там сохранилась информация о поддержках всех частых наборах.

Структуре передается набор признаков и она отвечает является ли он частым.

В каждой вершине дерева задается признак и множество дочерних вершин.

Путь от вершины дерева до корня - подмножество признаков. Прохождение по пути от вершины - поддержка определенного набора признаков, которые будут встречены по пути. 

Дерево состоит из уровней, на каждом из которых находится вершина только одного признака.

Дерево содержит полную информацию о всех частых наборах.

Все признаки в дереве упорядочиваются по убываию (т.е по тому, как часто они встречаются в выборке).

Построчный проход по матрице, построение дерева:

[Построение дерева](/img/tree.jpeg)

Считается совстречаемость признаков, например d и с.

<b>Вход:</b> обучающая выборка.

<b>Выход:</b> FP-дерево T, состоящее из <f, c, s>, где f - признак, c - поддержка набора признаков вплоть до корня, s - дочернее множество вершин для текущей вершины.

Признаки упорядочиваются по убыванию.

Дерево строится линейным проходом по базе транзакций.

Ищется такой путь в дереве, по которому признаки пройдут по тем, которые уже были раньше.

Если нет дочерней вершины, то она создается, если вершина есть, то в нее добавляется 1, т.е. увеличивается счетчик.

Далее выполняется рекурсивный поиск частых наборов по FP-дереву (FP-find).

<b>FP-find</b> - принимает на вход FP-дерево, набор признаков и список частых наборов (изначально пустой, но наполняющийся по мере работы функции частыми наборами). Идет по уровням дерева снизу вверх.

FP-find(T, f, R) - в R добавляются частые наборы, содержащие f.

Внутри функции строится условное FP-дерево. FP-дерево по подвыборке объектов, у которых признак равен 1 (например, все чеки, в которых куплено молоко).

По такой просеянной выборке заново строится FP-дерево (как раз условное).

В рекурсивный вызов FP-find передается условное дерево.

Дерево, получающееся, если бы оно строилось по всем вершинам, содержащим "e" (вершины этого признака также отбрасываются):

[Условное дерево](/img/conditionsl_tree.png)

### Исходные данные

[Заказы на покупки в продуктовых магазинах](https://www.kaggle.com/datasets/heeraldedhia/groceries-dataset?resource=download&select=Groceries_dataset.csv)