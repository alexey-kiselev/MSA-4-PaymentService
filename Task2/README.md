# Задание 2. Применяем State Machine на практике

## Таблица переходов состояний платежа

| Исходное состояние       | Переходное состояние       | Событие                                     |
| ------------------------ | -------------------------- | ------------------------------------------- |
| `START`                  | `CREATED`                  | Платёж создан (`CREATE_PAYMENT`)            |
| `CREATED`                | `FUNDS_HOLDED`             | Средства заблокированы (`HOLD_FUNDS`)       |
| `CREATED`                | `CANCELED_CREATE_FAILED`   | Ошибка создания                             |
| `FUNDS_HOLDED`           | `FRAUD_PENDING`            | Запущена проверка антифрода (`CHECK_FRAUD`) |
| `FUNDS_HOLDED`           | `CANCELED_HOLD_FAILED`     | Ошибка блокировки после создания платежа    |
| `FRAUD_PENDING`          | `FRAUD_APPROVED_AUTO`      | Антифрод вернул разрешение                  |
| `FRAUD_PENDING`          | `FRAUD_MANUAL_REVIEW`      | Антифрод вернул "ручная проверка"           |
| `FRAUD_PENDING`          | `CANCELED_FRAUD_DENIED`    | Антифрод вернул запрет                      |
| `FRAUD_MANUAL_REVIEW`    | `FRAUD_APPROVED_MANUAL`    | Оператор разрешил операцию                  |
| `FRAUD_MANUAL_REVIEW`    | `CANCELED_OPERATOR_DENIED` | Оператор запретил операцию                  |
| `FRAUD_MANUAL_REVIEW`    | `FRAUD_APPROVED_CUT_OFF`   | Истёк timeout без решения оператора         |
| `FRAUD_APPROVED_AUTO`    | `TRANSFERRED`              | Перевод выполнен (`TRANSFER_FUNDS`)         |
| `FRAUD_APPROVED_AUTO`    | `CANCELED_TRANSFER_FAILED` | Ошибка перевода                             |
| `FRAUD_APPROVED_MANUAL`  | `TRANSFERRED`              | Перевод выполнен (`TRANSFER_FUNDS`)         |
| `FRAUD_APPROVED_MANUAL`  | `CANCELED_TRANSFER_FAILED` | Ошибка перевода                             |
| `FRAUD_APPROVED_CUT_OFF` | `TRANSFERRED`              | Перевод выполнен (`TRANSFER_FUNDS`)         |
| `FRAUD_APPROVED_CUT_OFF` | `CANCELED_TRANSFER_FAILED` | Ошибка перевода                             |
| `TRANSFERRED`            | `COMPLETED`                | Клиент уведомлён (`NOTIFY_CLIENT`)          |

Когда `CANCELED_*` - `RELEASE_HOLD`, `CANCEL_PAYMENT` и отправляем уведомления.
