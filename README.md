# Лабораторна робота №3
## "Реалізація алгоритмів k-NN та k-means із застосуванням декораторів функцій відстані"

### Мета роботи
Реалізувати алгоритми k-NN (k-найближчих сусідів) та k-means (k-середніх) без використання спеціальних бібліотек, застосовуючи декоратори для гнучкої зміни метрик відстані.

### Теоретичні відомості

#### Алгоритм k-NN (k-найближчих сусідів)
k-NN — це простий алгоритм класифікації, який ґрунтується на припущенні, що об'єкти, близькі у просторі ознак, мають належати до одного класу.

**Принцип роботи:**
1. Для кожної точки з тестового набору даних:
   - Обчислити відстань до кожної точки з тренувального набору
   - Вибрати k найближчих точок (сусідів)
   - Присвоїти класифікованій точці клас, який найчастіше зустрічається серед k найближчих сусідів

**Псевдокод:**
```
функція k_nn(X_train, y_train, X_test, k):
    для кожної точки x в X_test:
        обчислити відстань від x до кожної точки в X_train
        знайти k точок з X_train з найменшою відстанню
        визначити найпоширеніший клас серед цих k точок
        присвоїти цей клас точці x
    повернути прогнозовані класи для всіх точок в X_test
```

#### Алгоритм k-means (k-середніх)
k-means — це алгоритм кластеризації, який розбиває набір даних на k різних груп (кластерів), мінімізуючи відстань між точками всередині кластера.

**Принцип роботи:**
1. Ініціалізація: вибір k початкових центрів кластерів
2. Повторювати до збіжності:
   - Призначення: кожна точка присвоюється найближчому центру
   - Оновлення: центри перераховуються як середнє значення всіх точок у кластері
3. Повертає розподіл точок за кластерами та остаточні центри кластерів

**Псевдокод:**
```
функція k_means(X, k, max_iterations):
    вибрати k випадкових точок з X як початкові центри
    повторювати max_iterations разів або до збіжності:
        для кожної точки x в X:
            призначити x до найближчого кластера
        для кожного кластера:
            перерахувати центр як середнє всіх точок кластера
    повернути кластери та їх центри
```

#### Метрики відстані
Вибір метрики відстані значно впливає на результати обох алгоритмів. Найчастіше використовуються:

1. **Евклідова відстань**: 
   ```
   d(x, y) = √(Σ(xᵢ - yᵢ)²)
   ```

2. **Манхеттенська відстань**: 
   ```
   d(x, y) = Σ|xᵢ - yᵢ|
   ```

3. **Косинусна відстань**: 
   ```
   d(x, y) = 1 - (x·y)/(||x||·||y||)
   ```

4. **Відстань Чебишева**: 
   ```
   d(x, y) = max|xᵢ - yᵢ|
   ```

### Завдання

#### Обов'язкова частина

1. **Базові векторні операції** 
   - Реалізувати наступні функції для роботи з векторами (векторами є звичайні списки чисел):
     ```
     dot_product(vec1, vec2)         # скалярний добуток
     vector_subtract(vec1, vec2)     # віднімання векторів
     vector_add(vec1, vec2)          # додавання векторів
     vector_multiply(vec, scalar)    # множення вектора на скаляр
     vector_norm(vec)                # евклідова норма вектора
     vector_mean(vectors)            # середнє значення набору векторів
     ```

   1. **dot_product(vec1, vec2)** — використовується в:
      - Косинусній відстані: `cosine_distance(x, y)` обчислює скалярний добуток векторів
      - Можливо знадобиться для реалізації відстані Махаланобіса в додатковій частині

   2. **vector_subtract(vec1, vec2)** — використовується в:
      - Усіх метриках відстані, де потрібно обчислити різницю між векторами
      - Реалізації k-means при оновленні центрів та перевірці збіжності

   3. **vector_add(vec1, vec2)** — використовується в:
      - Алгоритмі k-means при обчисленні нових центрів кластерів (накопичення суми векторів)

   4. **vector_multiply(vec, scalar)** — використовується в:
      - Обчисленні середніх центрів у k-means (множення суми на 1/n)
      - Зваженій відстані, де ознаки множаться на відповідні ваги

   5. **vector_norm(vec)** — використовується в:
      - Евклідовій відстані
      - Косинусній відстані
      - Перевірці збіжності в k-means

   6. **vector_mean(vectors)** — використовується в:
      - Алгоритмі k-means для обчислення нових центрів кластерів


2. **Функції метрик відстані** (25 балів)
   - Створити функції для обчислення різних метрик відстані:
     ```python
     import math 
     
     
     def euclidean_distance(x, y):
         """Обчислює евклідову відстань між двома векторами"""
         return math.sqrt(sum((a - b) ** 2 for a, b in zip(x, y)))
     
     def manhattan_distance(x, y):
         """Обчислює манхеттенську відстань між двома векторами"""
         return sum(abs(a - b) for a, b in zip(x, y))
     
     def cosine_distance(x, y):
         """Обчислює косинусну відстань між двома векторами"""
         dot = dot_product(x, y)
         norm_x = vector_norm(x)
         norm_y = vector_norm(y)
         return 1 - (dot / (norm_x * norm_y)) if norm_x * norm_y > 0 else 1
     
     def chebyshev_distance(x, y):
         """Обчислює відстань Чебишева між двома векторами"""
         return max(abs(a - b) for a, b in zip(x, y))
     ```

3. **Параметризовані алгоритми машинного навчання**
   - Реалізувати алгоритми, які приймають функцію відстані як параметр:
     ```python
     def k_nn(X_train, y_train, X_test, k=3, distance_function=euclidean_distance):
         """k-NN алгоритм з налаштовуваною функцією відстані"""
         predictions = []
         for test_point in X_test:
             # Обчислити відстані, використовуючи передану функцію відстані
             distances = [(distance_function(test_point, train_point), i) 
                          for i, train_point in enumerate(X_train)]
             # Решта алгоритму...
         return predictions
     
     def k_means(X, k=3, max_iters=100, tol=1e-4, distance_function=euclidean_distance):
         """k-means алгоритм з налаштовуваною функцією відстані"""
         # Ініціалізація...
         for iteration in range(max_iters):
             # Призначення точок до кластерів, використовуючи передану функцію відстані
             # ...
         return labels, centers
     ```

4. **Декоратори функцій відстані**
   - Створити декоратори, які змінюють функцію відстані для алгоритмів:
     ```python
     def with_manhattan_distance(func):
         """Декоратор, що змінює функцію відстані на манхеттенську"""
         def wrapper(*args, **kwargs):
             # Змінюємо функцію відстані в kwargs
             kwargs['distance_function'] = manhattan_distance
             return func(*args, **kwargs)
         return wrapper
     ```

5. **Тестування та аналіз**
   - Генерувати тестові дані
   - Порівнювати результати алгоритмів з різними метриками відстані
   - Візуалізувати результати

#### Додаткова частина 

1. **Додаткові декоратори відстані** 
   - Реалізувати один або кілька додаткових декораторів:
     ```
     with_weighted_distance(weights)    # зважена відстань з параметризацією ваг
     with_mahalanobis_distance(cov)     # відстань Махаланобіса
     ```


### Технічні вимоги

1. **Використовувані бібліотеки**:
   - `math` - для математичних функцій
   - `random` - для генерації випадкових даних
   - `matplotlib` - для візуалізації (опційно)
   - **ЗАБОРОНЕНО**: numpy, scipy, sklearn, pandas та інші бібліотеки для машинного навчання

2. **Структура даних**:
   - Усі вектори та матриці представлені у вигляді стандартних списків або списків списків Python

3. **Вимоги до реалізації**:
   - Усі функції повинні містити докстрінги з описом
   - Код повинен бути чітко структурований та супроводжуватись коментарями

4. **Для генерації даних та їх зчитування використайте скрипт generate_samples.py, знаходячись у директорії lab_3**:
```console
python src/generate_samples.py
```
або
```console
python3 src/generate_samples.py
```
### Приклади реалізації

#### Приклад використання різних метрик з функціями:

```python
# Використання функцій відстані безпосередньо
predictions_euclidean = k_nn(X_train, y_train, X_test, k=3, distance_function=euclidean_distance)
predictions_manhattan = k_nn(X_train, y_train, X_test, k=3, distance_function=manhattan_distance)
```

#### Приклад використання декораторів:

```python
# Використання декораторів для зміни функції відстані
@with_manhattan_distance
def manhattan_knn(X_train, y_train, X_test, k=3, distance_function=None):
    return k_nn(X_train, y_train, X_test, k, distance_function)

# Виклик з декоратором
predictions_manhattan = manhattan_knn(X_train, y_train, X_test, k=3)

# Динамічне застосування декоратора
predictions_cosine = with_cosine_distance(k_nn)(X_train, y_train, X_test, k=3)
```

### Критерії оцінювання

1. **Функціональність** (50%):
   - Правильна робота базових функцій
   - Коректна робота метрик відстані
   - Точність алгоритмів k-NN та k-means

2. **Якість коду** (30%):
   - Чіткість та структурованість коду
   - Правильне документування
   - Ефективність реалізації

3. **Аналіз та висновки** (20%):
   - Якість візуалізації
   - Глибина аналізу
   - Обґрунтованість висновків

### Термін здачі: 10.03.2025
