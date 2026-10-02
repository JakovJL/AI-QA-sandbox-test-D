# Состояние наших тестовых данных после проверки 2026-10-02

Все перечисленные ниже состояния прочитаны из интерфейса после полной загрузки соответствующей страницы.

| Объект | Проверенное состояние | Доказательство |
|---|---|---|
| QA Test Product 2026-09-28 (`prod_01M3KVZF7B6GHT2QQTY14R9R2Q`) | Draft | [Текст карточки](oct2-restored-product.txt) |
| Default variant (`variant_01M3KVZFJWDY9TJN1C8DY43HGJ`) | SKU пуст, учёт запасов выключен, цена €5 | [Текст карточки](oct2-restored-variant.txt) |
| QA Inventory Oct2 (`iitem_01M3XRK2TW3A0PZN1SW0DFTV0P`) | In stock 10, Reserved 0, Available 10; резервов и связанных вариантов нет | [Текст карточки](oct2-restored-inventory.txt) |
| Корзина medusa-sandbox-qa | Cart (0), тестовый товар удалён | [Текст страницы](oct2-restored-cart.txt) |

Отдельная QA-позиция оставлена для повторения ISSUE-028/029. Новые заказы в этой проверке не создавались.
