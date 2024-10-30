# Коллоквиум по АиСД

### <ins> 1. Оценка сложности алгоритма по времени </ins> 

По сути подсчет кол-ва операций (арифметические операции,
операции сравнения, операции присваивания) в алгоритме.

Оценка по времени бывает сверху - O, снизу - Ω и точная (**существует тогда
и только тогда, когда верхняя и нижняя оценки совпадают**) - θ.

Сложность по времени зависит в большей степени от программиста.

**<ins>Определение</ins>** Пусть f(n) — функция. Тогда, функция
g(n) = O(f(n)), если существуют такие константы c и
*n<sub>0</sub>*, что g(n) < c·f(n) для всех n≥n<sub>0</sub>.

В контексте анализа алгоритмов, фраза «алгоритм работает за
O(f(n)) операций» означает, что начиная с какого-то n он делает
не более чем за c⋅f(n) операций.

### <ins> 2. Оценка сложности алгоритма по памяти </ins>

По сути подсчет кол-ва переменных в алгоритме:

- 1 переменная - 1 пункт сложности
- Массив размером n - n пунктов сложности
- Строка длинной n - n пунктов сложности

Сложность по памяти более предсказуема, чем сложность по времени
и чаще всего определена размером входных данных.

### <ins> 3. Сортировка вставками (Insertion sort) </ins>

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
```
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

### <ins> 4. Сортировка слиянием (Merge Sort) </ins>

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

Оценка по времени: O(n⋅log(n)), т.к. проход по массиву ⋅ рекурсия разбиения (постоянно
делим массив пополам)

Оценка по памяти: O(n), т.к. создается массив res

Код:
```
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

### <ins> 5. Быстрая сортировка (Quick Sort) </ins>

