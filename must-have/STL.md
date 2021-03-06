## Использование `vector` как динамического массива фиксированной длины, `push_back` и `emplace_back`
`std::vector` - стандартный контейнер, который позволяет работать с ним как с динамическим массивом. Вектор работает с блоком памяти (массив в куче), который он увеличивает при необходимости. Все элементы в нем лежат в памяти подряд --> по нему можно легко итерироваться

Для добавления элемента в конец используются `push_back`, `emplace_back`
* `push_back` - передается сам элемент
* `emplace_back` - передаются только параметры конструктора, с помощью которых `vector` сам создаст элемент в конце массива без использования лишних копирований и перемещений

При добавлении/удалении из `vector` может произойти инвалидация итераторов (элементы, по которым выполняется итерация, изменяют свой адрес или уничтожаются):
1. Из-за `переаллоцирования` - увеличение необходимой памяти
2. Из-за удаления элемента (`resize` или `pop_back`, еще `erase`)

## Использование `map` со стандартным компаратором, особенность `operator[]` (создаёт значение даже при чтении)
`std::map` - стандартный контейнер, хранит пары `<уникальный ключ, значение>` в отсортированном порядке (порядок по ключу). Порядок сортировки по умолчанию задается стандартным компаратором (если ключи можно сравнивать, то они будут в отсортированном порядке). Если же нельзя - надо задавать свой компаратор и передавать его в `map`.

В мапе все ключи уникальные. Если мы пытаемся добавить элемент по ключу, который уже есть, то:
* `operator []` - изменится то, что лежало по этому ключу
* `insert` - ничего не произойдет

В `map` не происходит инвалидация итераторов от добавления новых элементов, так как она построена на дереве поиска (при удалении инвалидируется только итератор на удаляемый элемент)

При вызове `оператора []` если нужного ключа не было, то все равно создастся объект с дефолтным конструктором (чтобы избежать этого, надо сначала проверить если ли объект в `map`). Проверить, что объект есть в `map` можно с помощью `std::map::find`

Создание с дефолтным конструктором может быть плохо в следующем случае - хотели проверить существование элемента, сравнив `int` с `0`. Все сработает, но мап изменился
