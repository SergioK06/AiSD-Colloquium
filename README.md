# Коллоквиум по АиСД

## <ins> 1. Оценка сложности алгоритма по времени </ins> 

По сути подсчет кол-ва операций (арифметические операции,
операции сравнения, операции присваивания) в алгоритме.

Оценка по времени бывает сверху - O, снизу - Ω и точная (**существует тогда
и только тогда, когда верхняя и нижняя оценки совпадают**) - θ.

Сложность по времени зависит в большей степени от программиста.

**Определение** Пусть f(n) — функция. Тогда, функция
g(n) = O(f(n)), если существуют такие константы c и
*n<sub>0</sub>*, что g(n) < c·f(n) для всех n≥n<sub>0</sub>.

В контексте анализа алгоритмов, фраза «алгоритм работает за
O(f(n)) операций» означает, что начиная с какого-то n он делает
не более чем за c⋅f(n) операций.

## <ins> 2. Оценка сложности алгоритма по памяти </ins>

По сути подсчет кол-ва переменных в алгоритме:

- 1 переменная - 1 пункт сложности
- Массив размером n - n пунктов сложности
- Строка длинной n - n пунктов сложности

Сложность по памяти более предсказуема, чем сложность по времени
и чаще всего определена размером входных данных.

## <ins> 3. Сортировка вставками (Insertion sort) </ins>

Главные принципы:
- Первый элемент считаем отсортированной частью
- Начиная со второго: 
  - просматриваем элементы слева направо от i к i+1
  - выполняем вставку просматриваемого элемента в отсортированную часть
  - после вставки увеличиваем размер отсортированной части

Инвариант: все элементы до i-ого отсортированы

Устойчивая

Оценка по времени: 
- лучший: O(n)
- средний/худший: O(n<sup>2</sup>)

Оценка по памяти: O(1)

Код:
```c++
for (int i=1; i<n; i++) {
    int key = a[i];
    int j = i-1;
    while(a[j] > key and j >= 0) {
        a[j+1] = a[j];
        j -= 1;
    }
    a[j+1] = key;
}
```

## <ins> 4. Сортировка слиянием (Merge Sort) </ins>

Главные принципы:
- "Разделяй и властвуй"/Метод декомпозиции (Задача разбивается на подзадачи):
  - Задача разделяется на подзадачи и выбран размер подзадач, до которого
идет разбиение (1 элемент - отсортирован относительно самого себя)
  - Корректный сбор решения подзадач, чтобы в итоге решить изначальную задачу
  - Рекурсия - эффективный способ реализовать этот подход
- Объединяем решение по парам: например, два одно элементных в двух элементные
- Не всегда разбиения будут четными => можем объединять одно и двух 
элементные
- Рекурсия: на спуске - разбиваем, на подъеме - сортируем в 
рамках подзадач и объединяем решения

Инвариант: при слиянии (подъеме) оба массива отсортированы

Устойчивая

Оценка по времени: O(n ⋅ log(n)), т.к. проход по массиву ⋅ рекурсия разбиения (постоянно
делим массив пополам)

Оценка по памяти: O(n), т.к. создается массив res

Код:
```c++
void merge(int left, int mid, int right) {
    int l = 0;
    int r = 0;
    int res[right-left];
    while (left + l < mid && mid + r < right) {
        if (a[left+l] <= a[mid + r]) {
            res[l+r]=a[left+l];
            l++;
        }
        else {
            res[r+l]=a[mid+r];
            r++;
        }
    }
    while (left + l < mid) {
        res[l+r] = a[left+l];
        l++;
    }
    while (mid + r < right) {
        res[r+l] = a[mid+r];
        r++;
    }
    for (int i = 0; i < l+r; i++) {
        a[left+i] = res[i];
    }
}

void mergeSortRecursive(int left, int right) {
    if (left+1 >= right) {
        return;
    }
    int mid = (left + right) / 2;
    mergeSortRecursive(left, mid);
    mergeSortRecursive(mid, right);
    merge(left, mid, right);
}
```

## <ins> 5. Быстрая сортировка (Quick Sort) </ins>

Главные принципы:
- Разбиение:
  - выбираем опорный элемент
  - делим массив, относительно опорного элемента, на две части, слева - 
меньше него, а справа больше
- Запускаем процесс разбиения отдельно в левой и правой частях полученного
массива ("разделяй и властвуй")

В основном под Быстрой сортировкой (Quick Sort) подразумевают сортировку
Хоара, но еще возможна и сортировка Ломуто.

**Разбиение Ломуто:**
- Опорным выбирается последний элемент в рассматриваемой части массива
- Необходимо 2 индекса:
  - i – отвечает за границу элементов меньших опорного, на старте считаем, что
таких элементов нет
  - j - для просмотра массива
- Просматриваем массив, и каждый раз, когда находится элемент, меньший
либо равный опорному, увеличиваем границу элементов меньших опорного (индекс i)
и вставляем найденный элемент перед опорным в зону меньших опорного

**Разбиение Хоара:**
- Опорным выбирается элемент в середине массива, относительно него идет
разбиение
- Необходимо 2 индекса (i в начале массива, j – в конце), которые приближаются
друг к другу, пока не найдется пара элементов:
  - по i: элемент больше опорного и расположен перед ним (до среднего элемента)
  - по j: элемент меньше опорного и расположен после него (после среднего элемента)
- Найденные элементы меняются местами
- Обмен происходит до тех пор, пока индексы i и j не пересекутся

Разбиение Хоара эффективнее разбиения Ломуто, т. к. происходит в среднем в 3 раза
меньше обменов (swap) элементов, и разбиение эффективнее даже когда все элементы
равны

Следует заметить, что конечная позиция опорного элемента необязательно совпадает с его
начальной позицией (в середине массива)

Инвариант: элементы левее опорного меньше него, а правее - больше или равны

Неустойчивая

Оценка по времени:
- лучший/средний: O(n ⋅ log(n)), т.к. проход по массиву ⋅ рекурсия разбиения
- худший: O(n<sup>2</sup>), при постоянном разбиении на 1 и n-1

Оценка по памяти: 
- лучший/средний: O(log(n)), т.к. log n - глубина рекурсии
- худший: O(n), при постоянном разбиении на 1 и n-1

Модификации Quick Sort:
- среднее значение между крайними
- выбор опорного элемента с помощью random
- медиана из 3-х

Код (Хоара + модификация(среднее значение между крайними)):
```c++
void QuickSort(int l, int r) {
    int i = l, j = r;
    int mid = (a[l] + a[r]) / 2;

    while (i <= j) {
        while (a[i] < mid) {
            i++;
        }
        while (a[j] > mid) {
            j--;
        }
        if (i <= j) {
            swap(a[i], a[j]);
            i++;
            j--;
        }
    }
    if (j > l) {
        QuickSort(l, j);
    }
    if (r > i) {
        QuickSort(i, r);
    }
};
```

## <ins> 6. Сортировка подсчетом (Counting Sort) </ins>

Главные принципы:
- Исходная последовательность чисел длины n , а в конце отсортированная, 
хранится в массиве A. Также используется вспомогательный массив C с индексами
от 0 до k−1, изначально заполняемый нулями
- Последовательно пройдём по массиву A и запишем в C[i] количество чисел,
равных i
- Теперь достаточно пройти по массиву C и для каждого number∈{0,...,k−1}
в массив A последовательно записать число C[number] раз.

Инвариант: (не ебу)

Для чисел:
- Неустойчивая
- Оценка по времени: O(n + k), где k - мощность алфавита (количество уникальных
элементов в начальном массиве A, по сути массив C), n - размер массива A
- Оценка по памяти: O(k)

Сортировать можно не только числа, но и различные объекты, для этого нужно
создать вспомогательный массив B (по длине равный массиву A).

Для различных объектов:
- Устойчивая
- Оценка по времени: O(n), n - длина массива A
- Оценка по памяти: O(k + n), где k - размер доп. массива C, n - размер доп.
массива B

Модификации:
1) Неизвестен диапазон чисел
   - Если диапазон значений не известен заранее, то его можно найти с
помощью линейного поиска минимума и максимума в исходном
массиве, что не повлияет на асимптотику алгоритма, так как поиск
выполняется за O(n).

2) Отрицательные числа
   - Нужно учитывать, что минимум может быть отрицательным, в то
время как в массиве C индексы от 0 до k−1. Поэтому при работе с
массивом C из исходного A[i] необходимо вычитать минимум, а при
обратной записи в B[i] прибавлять его.

Псевдокод для чисел:
```javascript
function simpleCountingSort(A: int[n]): 
    for number = 0 to k - 1
        C[number] = 0 
    for i = 0 to n - 1
        C[A[i]] = C[A[i]] + 1;     
    pos = 0;
    for number = 0 to k - 1
        for i = 0 to C[number] - 1
            A[pos] = number;
            pos = pos + 1;
```

Псевдокод для различных объектов:
```javascript
function complexCountingSort(A: int[n], B: int[n]):
    for i = 0 to k - 1
        C[i] = 0;         
    for i = 0 to length[A] - 1
        C[A[i].key] = C[A[i].key] + 1;     
    carry = 0;
    for i = 0 to k - 1
        temporary = C[i];
        C[i] = carry;
        carry = carry + temporary;     
    for i = 0 to length[A] - 1
        B[C[A[i].key]] = A[i];
        C[A[i].key] = C[A[i].key] + 1;
```

## <ins> 7. Цифровая сортировка (Radix Sort) </ins>

Главные принципы:
- Будем называть элементы, из которых состоят сортируемые
объекты, разрядами. Тогда алгоритм состоит в последовательной
сортировке объектов какой-либо устойчивой сортировкой по
каждому разряду, в порядке от младшего разряда к старшему:
  - для чисел уже существует понятие разряда
  - для строк можно в качестве разрядов использовать отдельные символы,
которые сравниваются по таблице кодировок

Существуют два вида Radix Sort:
1) LSD - от младшего разряда к старшему
2) MSD - от старшего разряда к младшему

**LSD**:
- Инвариант: все разряды младше сортируемого уже отсортированы по порядку
- Устойчивая
- Оценка по времени: O(k ⋅ T(n)), где k - количество разрядов, а T(n) - время
работы устойчивой сортировки (она выбирается исходя из входящих данных)
- Оценка по памяти: O(M(n)), где M(n) - количество доп. памяти, которая
используется в устойчивой сортировке
- Код:
```c++
void radix_sort(A, d)
{
  for (int i = k - 1; i >= 0; i--)
  {
    // устойчивая сортировка (merge, counting и т.п.) по i-ому разряду 
  }
}
```

**MSD**:
- Инвариант: все разряды старше сортируемого уже отсортированы по порядку
- Устойчивая
- Оценка по времени: O(k ⋅ T(n)), где k - количество разрядов, а T(n) - время
работы устойчивой сортировки (она выбирается исходя из входящих данных)
- Оценка по памяти: O(M(n)), где M(n) - количество доп. памяти, которая
используется в устойчивой сортировке
- Код:
```javascript
function radixSort(int[] A, int l, int r, int d):
    if d > m or l >= r 
        return
    for j = 0 to k + 1
        cnt[j] = 0
    for i = l to r                              
        j = digit(A[i], d)
        cnt[j + 1]++
    for j = 2 to k
        cnt[j] += cnt[j - 1]
    for i = l to r
        j = digit(A[i], d)
        c[l + cnt[j]] = A[i]
        cnt[j]--
    for i = l to r
        A[i] = c[i]
    radixSort(A, l, l + cnt[0] - 1, d + 1)
    for i = 1 to k
        radixSort(A, l + cnt[i - 1], l + cnt[i] - 1, d + 1)
```

## <ins> 8. Стек (Stack) </ins>

Стек — структура данных, представляющая из себя упорядоченный набор
элементов, в которой добавление новых элементов и удаление существующих 
производится с одного конца, называемого вершиной стека. Притом первым из
стека удаляется элемент, который был помещен туда последним, то есть в
стеке реализуется стратегия «последним вошел — первым вышел» 
(last-in, first-out — LIFO). Примером стека в реальной жизни может
являться стопка тарелок: когда мы хотим вытащить тарелку, мы должны снять
все тарелки выше

Поддерживаемые операции:
- empty — проверка стека на наличие в нем элементов
- push (запись в стек) — операция вставки нового элемента
- pop (снятие со стека) — операция удаления нового элемента

Для стека с n элементами требуется O(n) памяти, т.к. она нужна лишь
для хранения самих элементов.

Стек можно реализовывать и на массиве (стат. и дин.), и на списках

## <ins> 9. Очередь (Queue) </ins>

Очередь — это структура данных, добавление и удаление элементов в которой
происходит путём операций push и pop соответственно. Притом первым из очереди
удаляется элемент, который был помещен туда первым, то есть в очереди
реализуется принцип «первым вошел — первым вышел» (англ. first-in, first-out
— FIFO). У очереди имеется голова (англ. head) и хвост (англ. tail).
Когда элемент ставится в очередь, он занимает место в её хвосте. Из очереди
всегда выводится элемент, который находится в ее голове. 

Поддерживаемые операции:
- empty — проверка очереди на наличие в ней элементов
- push (запись в очередь) — операция вставки нового элемента
- pop (снятие с очереди) — операция удаления нового элемента

Для очереди с n элементами требуется O(n) памяти, т.к. она нужна лишь для
хранения самих элементов.

Очередь можно реализовывать и на массиве (стат. и дин.), и на списках

## <ins> 10. Односвязный список </ins>

Связный список — структура данных, состоящая из элементов, содержащих
помимо собственных данных ссылки на следующий и/или предыдущий элемент
списка. С помощью списков можно реализовать такие структуры данных как стек
и очередь.

Односвязный список - простейшая реализация списка, в узлах которого хранятся 
данные и указатель на следующий элемент в списке.

Базовые операции:
- Вставка
- Поиск
- Удаление

Код:
```c++
struct Node{
    int key;
    Node* next;
};
struct Node* head
```

## <ins> 11. Двусвязный список </ins>

Связный список — структура данных, состоящая из элементов, содержащих
помимо собственных данных ссылки на следующий и/или предыдущий элемент
списка. С помощью списков можно реализовать такие структуры данных как стек
и очередь.

Двусвязный список - реализация списка, в узлах которого хранятся данные и
указатели на предыдущий и следующий элемент списка, благодаря чему становится
проще удалять и переставлять элементы.

Базовые операции:
- Вставка
- Поиск
- Удаление

Код:
```c++
struct Node{
    int key;
    Node* next;
    Node* prev;
};
struct Node* head
```

## <ins> 12. Циклический список </ins>

Связный список — структура данных, состоящая из элементов, содержащих
помимо собственных данных ссылки на следующий и/или предыдущий элемент
списка. С помощью списков можно реализовать такие структуры данных как стек
и очередь.

Циклический список - одно/двусвязный список, в котором первый элемент является
следующим для последнего элемента списка.

## <ins> 13. Стек на списках  </ins>