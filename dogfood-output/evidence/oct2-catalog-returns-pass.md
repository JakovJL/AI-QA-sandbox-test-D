# Каталог, checkout и возвраты: дополнительный проход 2026-10-02

Проверка выполнена через интерфейсы витрины и админки Chromium. Во всех сценариях использованы наши QA-товар, покупательские данные и заказы. В действиях fulfillment, shipment, delivery и return переключатель Send notification выключен.

## Проверенные сценарии

| Сценарий | Наблюдаемый результат | Доказательство |
|---|---|---|
| Сортировка Low -> High / High -> Low | QA-товар €5 оказался первым / последним среди товаров €10 | [Low](oct2-catalog-sort-low.txt), [High](oct2-catalog-sort-high.txt) |
| Две комбинации варианта | S / White и L / Black сохранились отдельными строками; после изменения количества в корзине 1 и 2 единицы | [Корзина после изменения](oct2-cart-quantity-changed.txt) |
| Смена страны | Меню Denmark → France сохранило корзину; адрес France → Denmark сохранился как Copenhagen 1100, Denmark в новом заказе | [Заказ №21 до дальнейших операций](oct2-order21-before-return.txt) |
| Изменение количества после выбора Manual Payment | Итог вырос с €30 до €40; оплата выбрана заново, заказ создан и захвачены €40 | [Review](oct2-checkout-recalculated.txt), [подтверждение](oct2-order21-confirmation.txt), [админка](oct2-order21-before-return.txt) |
| Дубликат опций | Попытка создания QA Duplicate Variant Oct2 с Default option value отклонена сообщением `Variant (Default variant) with provided options already exists.` | [После выхода остался один Default variant](oct2-catalog-restored-product.txt) |
| Превышение возврата | Для одной доставленной единицы №19 значение 2 отклонено; значение возвращено к 1, показана причина | Наблюдение интерфейса при проверке, успешный обычный приём 1 описан ниже |
| Обычный приём №19 | Принята одна единица L / Black, запас вырос с 999997 до 999998 | [До](oct2-return-inventory-before.txt), [текущее состояние заказа](oct2-order19-return-current.txt) |
| Отмена принятого возврата | Отклонена с `Can't cancel a return which has returned items`, повторное оприходование не выполнено | [Сообщение](oct2-order19-return-current.txt), [HTTP 400](oct2-received-return-cancel-network.txt) |
| Приём повреждённых товаров №15 | Для полного возврата введены 0 неповреждённых и 2 повреждённых; приём успешен, запас не увеличился | [Перед сохранением](oct2-return-two-damaged-valid.txt), [успешный приём](oct2-return-two-damaged-completed.txt), [запросы](oct2-return-damaged-network.txt) |
| Приём всего заказа №21 | Возвращены 2 L / Black и 1 S / White; Item Subtotal €0, Order Total €10, Outstanding -€30 | [Результат](oct2-order21-return-received.txt), [видео](../videos/oct2-order21-return-received.webm) |
| Возврат денег №21 | После выбора платежа форма предложила €30; возврат успешен, Paid Total €10 и Outstanding €0 | [Итог](oct2-order21-refunded.txt) |

## Наблюдения, которые не стали дефектами

В приёме возврата основное числовое поле относится к неповреждённым единицам, а повреждённые вводятся отдельно. При некорректной сумме количеств сервер отклоняет изменение; корректные 0 + 2 успешно принимаются без увеличения продаваемого запаса. Первоначальное подозрение на неправильное оприходование не подтверждено.

После подтверждения приёма Summary обновляется: у №21 сразу после появления записи Return received суммы уже €0 / €10. Первоначальный снимок старых сумм был снят во время обновления интерфейса. Отдельный дефект на его основе не создан.

Одно раннее зависание при быстрой смене варианта не удалось выделить в устойчивый сценарий. Последовательный выбор двух комбинаций и их добавление прошли правильно; отдельный issue не создан.

## Тестовые данные после прохода

| Объект | Конечное состояние | Доказательство |
|---|---|---|
| QA Test Product 2026-09-28, `prod_01M3KVZF7B6GHT2QQTY14R9R2Q` | Draft, один исходный вариант | [Карточка после полной загрузки](oct2-catalog-restored-product.txt) |
| `variant_01M3KVZFJWDY9TJN1C8DY43HGJ` | SKU пуст, Manage inventory off, цена €5 | [Карточка варианта](oct2-catalog-restored-variant.txt) |
| QA Inventory Oct2, `iitem_01M3XRK2TW3A0PZN1SW0DFTV0P` | In stock 10, Reserved 0, Available 10, резервов и связей с вариантами нет | [Карточка склада](oct2-catalog-restored-inventory.txt) |
| Корзина | Cart (0) | [После полной загрузки](oct2-catalog-restored-cart.txt) |
| QA-заказ №15, `order_01M3KVHCAV00GXJB1NNTAZ4PGB` | Приняты 2 повреждённые единицы; Paid Total €25, Outstanding -€20; промежуточный частичный возврат отменён | [Конечная карточка](oct2-order15-final.txt) |
| QA-заказ №19, `order_01M3RP7G2T7B283TB38CED60SB` | Принята 1 обычная единица; Paid Total €20, Outstanding -€10 | [Карточка](oct2-order19-return-current.txt) |
| Новый QA-заказ №21, `order_01M3XYQCAJDRFAZSR5449EJ0B6` | Delivered / Partially refunded; 3 единицы приняты обратно, €40 захвачены, €30 возвращены, остаток оплаты €10 и задолженность €0 | [Конечная карточка после загрузки](oct2-order21-final.txt) |
| SHIRT-L-BLACK | In stock 999998, Reserved 0; отражает обычный возврат 1 единицы №19 и восстановление 2 единиц, купленных в №21 | [После полной загрузки](oct2-catalog-final-stock.txt) |

Принятые возвраты и их история сохранены на наших QA-заказах для дальнейшей проверки. Два новых подтверждённых дефекта этого прохода — ISSUE-031 и ISSUE-032.
