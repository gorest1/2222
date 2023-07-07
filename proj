#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int key;
    int value;
    int lastAccess; // Хранит информацию о последнем обращении
} CacheItem;

typedef struct {
    int capacity;
    int size;
    CacheItem* cache;
} Cache;

// Модуль 1: Создание и уничтожение кэша (отвечает за создание и уничтожение кэша (createCache, destroyCache)).

Cache* createCache(int capacity) {
    Cache* cache = (Cache*)malloc(sizeof(Cache)); // Выделяем память под кэш
    cache->capacity = capacity; // Задаем емкость кэша
    cache->size = 0; // Инициализируем размер кэша
    cache->cache = (CacheItem*)malloc(capacity * sizeof(CacheItem)); // Выделяем память под элементы кэша
    return cache;
}

void destroyCache(Cache* cache) {
    free(cache->cache); // Освобождаем память, занятую элементами кэша
    free(cache); // Освобождаем память, занятую структурой кэша
}

// Модуль 2: Операции с кэшем ( содержит операции над кэшем, такие как проверка заполненности кэша (isCacheFull), поиск элемента в кэше (findCacheItem), удаление элемента из кэша (removeFromCache) и добавление элемента в кэш (addToCache).)

int isCacheFull(Cache* cache) {
    return cache->size == cache->capacity; // Проверяем, полный ли кэш
}

int findCacheItem(Cache* cache, int key) {
    int i;
    for (i = 0; i < cache->size; i++) { // Ищем элемент в кэше по ключу
        if (cache->cache[i].key == key) {
            return i; // Возвращаем индекс элемента, если найден
        }
    }
    return -1; // Возвращаем -1, если элемент не найден
}

void removeFromCache(Cache* cache, int index) {
    int i;
    for (i = index; i < cache->size - 1; i++) {
        cache->cache[i] = cache->cache[i + 1]; // Сдвигаем элементы кэша для удаления элемента
    }
    cache->size--; // Уменьшаем размер кэша
}

void addToCache(Cache* cache, int key, int value) {
    if (isCacheFull(cache)) {
        // Если кэш полный, удаляем элемент с наибольшим временем последнего обращения
        int maxIndex = 0;
        int maxLastAccess = cache->cache[0].lastAccess;
        int i;
        for (i = 1; i < cache->size; i++) {
            if (cache->cache[i].lastAccess > maxLastAccess) {
                maxIndex = i; // Находим индекс элемента с наибольшим временем последнего обращения
                maxLastAccess = cache->cache[i].lastAccess;
            }
        }
        removeFromCache(cache, maxIndex); // Удаляем элемент с наибольшим временем последнего обращения
    }

    // Добавляем новый элемент в кэш
    CacheItem newItem;
    newItem.key = key;
    newItem.value = value;
    newItem.lastAccess = 0; // Устанавливаем время последнего обращения в 0

    cache->cache[cache->size++] = newItem; // Добавляем элемент в кэш и увеличиваем размер кэша
}

void updateLastAccess(Cache* cache, int index) {
    cache->cache[index].lastAccess = 0; // Обновляем время последнего обращения элемента
}

// Модуль 3: Вспомогательные функции (3 содержит вспомогательные функции, такие как печать содержимого кэша (printCache).)


void printCache(Cache* cache) {
    int i;
    printf("Cache: ");
    for (i = 0; i < cache->size; i++) { // Печатаем содержимое кэша
        printf("[%d: %d] ", cache->cache[i].key, cache->cache[i].value);
    }
    printf("\n");
}

// Модуль 4: Основная логика программы (одержит функцию accessData, которая осуществляет доступ к данным в кэше и 
// обновляет его состояние в соответствии с алгоритмом идеального кэширования (OPT). 
// Также в нем находится функция main, которая инициализирует кэш, 
// задает последовательность обращений к данным и вызывает функцию accessData для каждого элемента последовательности.)


void accessData(Cache* cache, int key) {
    int index = findCacheItem(cache, key);
    if (index != -1) {
        updateLastAccess(cache, index); // Обновляем время последнего обращения, если элемент найден
    } else {
        printf("Cache miss for key: %d\n", key);
        addToCache(cache, key, key); // В данном примере ключ и значение одинаковы
    }
}


// Дополнительные тесты
void runAdditionalTests() {
    Cache* cache = createCache(3);

    printf("Additional tests:\n");

    // Тест 1: Проверка заполненности кэша
    printf("Test 1: Cache full:\n");
    accessData(cache, 1);
    accessData(cache, 2);
    accessData(cache, 3);
    accessData(cache, 4);
    printCache(cache);

    // Тест 2: Доступ к существующему элементу
    printf("Test 2: Access existing element:\n");
    accessData(cache, 2);
    printCache(cache);

    // Тест 3: Доступ к несуществующему элементу
    printf("Test 3: Access non-existent element:\n");
    accessData(cache, 5);
    printCache(cache);

    destroyCache(cache);
}

int main() {
    int i;
    Cache* cache = createCache(3); // Создаем кэш с вместимостью 3 элемента

    // Предполагаемая последовательность обращений к данным
    int accessSequence[] = {1, 2, 1, 3, 4, 2, 4, 3, 5};

    for (i = 0; i < sizeof(accessSequence) / sizeof(int); i++) {
        accessData(cache, accessSequence[i]); // Выполняем доступ к данным
        printCache(cache); // Печатаем состояние кэша
    }



    destroyCache(cache); // Уничтожаем кэш
    return 0;
}
