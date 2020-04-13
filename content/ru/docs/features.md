# Особенности блокчейна Minter

## Адреса

**Публичные ключи** – адреса в блокчейне для совершения транзакций (отправка или получение монет, создание монет, делегирование и т.д.), допускается использование термина “кошелек”.

Все адреса состоят из префикса и произвольного набора символов, включающего цифры **от 0 до 9** и латинские буквы **a, b, c, d, e** и **f** – все адреса не чувствительны к регистру.

Пользовательские: **Mx** + набор из **40** символов.\
**Пример:** *Mx04bea23efb744dc93b4fda4c20bf4a21c6e195f1*

Адреса валидаторов: **Mp** + набор из **64** символов.\
**Пример:** *Mp19b3b04b4fda4c2f0b2f3ccc0bf4a21cfea25f3efbd02cf2a72a22fa01010120*

Для транзакций используется префикс **Mt** + набор из **64** символов.\
**Пример:** *Mtb48d99cfb99b7c6626c069f90aba00c40ef9f77ef557c73dd2a876902823335c*

Для чеков используется префикс **Mc** + набор из **364** символов.\
**Пример:** *Mcf8b488313233343132343301843b9ac9ff8a42495000000000000000780de0b 6b3a76400008a11495000000000000000b141d7f9598dd6b2659d632033d9133758afa29c438c7af0eabbe93fafa8054f91346660c58324e00ddff4196f0b7eac0b4c387c27c1990e12f220aa6328420207ff001ba0872305a2142f7b7f1fa390e305320a7e977b4e760c07e68075aad646ebbf3b27a07c0f475631d65189a80c68794d385f50383edb3b0c8cf4978сd6ad3888f39825*

## Блок

**Блок** – ячейка с данными в сети, в них хранятся транзакции и информация, которая в них содержится. Все блоки в блокчейне связаны между собой последовательно и непрерывно. Информация в блоках не может быть изменена после того, как его подписали ⅔+ валидаторов.

Структура блоков:
- Заголовок
- Транзакции
- Подписи не менее ⅔ валидаторов

Алгоритм генерирования адресов и схема подписей были импортированы из Ethereum (Cryptographic Signature Scheme: Elliptic Curve Digital Signature Algorithm, «алгоритм цифровой подписи с эллиптическими кривыми»).

Характеристики блоков Minter:
- Время блока: 5 секунд
- Размер: до 10 000 транзакций ~180 байтов каждая + заголовок + подписи валидаторов
- Стартовая награда за блок 333 BIP линейно уменьшается до 115 BIP в течении 7 лет с момента запуска блокчейна.

В текущем виде сеть может обрабатывать до 10 000 транзакций за 1 блок (2000 транзакций в секунду), однако, с выходом новых обновлений Minter, это значение будет увеличиваться. Скорость блока в среднем составляет 5 секунд и зависит от многих параметров, среди которых: параметры ноды, технические характеристики серверов, скорость взаимодействия валидаторов.

Данные по количеству вознаграждений в блоке можно найти на [explorer.minter.network](https://explorer.minter.network/).

Актуальную информацию о статусе сети и её компонентов, количестве блоков и средней скорости их генерации можно найти на странице [status.minter.network](https://status.minter.network/).

## Вознаграждения

![Rewards](/img/docs/block.jpg)

Кроме стандартной информации, в блоке также присутствуют вознаграждения, которые распределяются между всеми валидаторами пропорцио, подписавшими блок, пропорционально их стейку. Далее, валидаторы распределяют вознаграждения между своими делегаторами, забирая часть наград как комиссию (которая заранее известна и указывается при декларировании новой мастерноды).

С первого блока, награды за каждый блок составляли 333 BIP, однако, они линейно уменьшаются до 68 BIP (к этому моменту вся эмиссия 10 млрд BIP будет выпущена). Вознаграждения уменьшаются на 1 BIP каждые 200000 блоков. Ориентировочно базовое вознаграждение будет начисляться в течении 7 лет после запуска основной сети.

Помимо наград за блок, между валидаторами и их делегаторами распределяется комиссия, которая взимается за каждую транзакцию в сети Minter. [Подробнее о комиссиях](https://docs.minter.network/#section/Commissions).

## Чеки

Чек Minter похож на обычный банковский чек. Каждый пользователь сети может выдать чек на любое количество монет и передать его другому лицу.

Чеки имеют префикс **Mc** и состоят из **364** символов.\
**Пример:** *Mcf8b488313233343132343301843b9ac9ff8a42495000000000000000780de0b6b3a76400008a11495000000000000000b141d7f9598dd6b2659d632033d9133758afa29c438c7af0eabbe93fafa8054f91346660c58324e00ddff4196f0b7eac0b4c387c27c1990e12f220aa6328420207ff001ba0872305a2142f7b7f1fa390e305320a7e977b4e760c07e68075aad646ebbf3b27a07c0f475631d65189a80c68794d385f50383edb3b0c8cf4978сd6ad3888f39825*

Чеки являются одноразовыми, создаются и обналичиваются через [консоль](https://console.minter.network/).

Для того, чтобы создать чек, необходимо перейти в соответствующую вкладку меню консоли.

![Create check](/img/docs/check-create.jpg)

Параметры создания чека:
- **Nonсe** - уникальный идентификатор чека. Его можно использовать, чтобы идентифицировать чеки с одинаковой суммой;
- **Монета** - монета, которая будет использоваться в чеке;
- **Количество** - количество выбранной монеты, которое сможет обналичить получатель;
- **Пароль** - доступ к обналичиванию чека, предотвращающий обналичивание посторонним лицом;
- **Монета для оплаты комиссии** (0.03 BIP);
- **Действителен до блока** - блок, до которого чек будет валидным. После выбранного блока, чек невозможно будет обналичить.

Для того, чтобы обналичить чек, необходимо ввести сам чек (с префиксом “Mc”) в соответствующее поле формы, а также пароль:

![Redeem check](/img/docs/check-redeem.jpg)

Статусы ошибок:
- **Error: Check already redeemed** - чек уже обналичен;
- **Invalid hex string** - некорректный hex чека;
- **Check is invalid** - некорректный чек;
- **Error: Coin not exists** - монета не существует;
- **Error: Invalid proof** - неверный пароль, чек еще не обналичен.