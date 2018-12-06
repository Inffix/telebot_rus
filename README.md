![](http://i.imgur.com/eELz6Aw.jpg)

Простейший способ писать Telegram ботов.

[![Build Status](https://travis-ci.org/mullwar/telebot.svg)](https://travis-ci.org/mullwar/telebot) [![Dependency Status](https://david-dm.org/mullwar/telebot.svg)](https://david-dm.org/mullwar/telebot) ![Node.js Version](http://img.shields.io/node/v/telebot.svg)

[![TeleBot 2.0](https://img.shields.io/badge/dev-TeleBot%202%2e0-ff0061.svg)](https://github.com/mullwar/telebot/tree/v2.0) [![TeleBot Examples](https://img.shields.io/badge/telebot-examples-blue.svg)](https://github.com/mullwar/telebot/tree/master/examples) [![TeleBot Bot](https://img.shields.io/badge/telebot-community%20bot-blue.svg)](https://github.com/mullwar/telebot-bot) [![TeleBot Group](https://img.shields.io/badge/telebot-community%20group-blue.svg)](https://goo.gl/gXvm12)


**Функции библиотеки:**

- 🍎 Простая в использовании
- 🏰 Полная поддержка [Telegram Bot API](https://core.telegram.org/bots/API) support.
- 💰 Поддерживает [платежи](https://core.telegram.org/bots/payments).
- 🔌 Поддерживает [плагины](https://github.com/mullwar/telebot-plugins)!
- 📡 Встроенная система модификации и событий.
- 🛠 Расширяемая.
- 🔮 Без колбэков, полько промисы.
- 🤓 [Сhangelog](https://github.com/mullwar/telebot/releases).
- ☺️ Дружелюбное [комьюнити](https://goo.gl/gXvm12).

## 🔨 Установка

Скачать и установить через [npm](https://www.npmjs.com/package/telebot) (stable):

```
npm install telebot --save
```

Или клонировать свежий код прямо из git:

```
git clone https://github.com/mullwar/telebot.git
cd telebot
npm install
```

## 🕹 Использование

Импорт модуля telebot и создание нового объекта бота:

```js
const TeleBot = require('telebot');

const bot = new TeleBot({
    token: 'TELEGRAM_BOT_TOKEN', // Обязателное поле. Telegram Bot API токен.
    polling: { // Необязателное поле. Использование поллинга.
        interval: 1000, // Необязателное поле. Как часто проверяются обновления в мс.
        timeout: 0, // Необязателное поле. Таймаут поллинга (0 - короткий поллинг).
        limit: 100, // Необязателное поле. Лимит количества обновлений за запрос.
        retryTimeout: 5000, // Необязателное поле. Таймаут подключения в мс).
        proxy: 'http://username:password@yourproxy.com:8080' // Необязателное поле. HTTP прокси.
    },
    webhook: { // Необязателное поле. Использование вебхука вместо поллинга.
        key: 'key.pem', // Необязателное поле. Приватный ключ сервера.
        cert: 'cert.pem', // Необязателное поле. Публичный ключ.
        url: 'https://....', // HTTPS url для отправки обновлений.
        host: '0.0.0.0', // Хост сервера вебхука.
        port: 443, // Порт сервера.
        maxConnections: 40 // Необязателное поле. Максимальное разрешённое кол-во одновременных HTTPS подключений вебхука
    },
    allowedUpdates: [], // Необязателное поле. Перечислите типы обновлений, которые должен получать ваш бот. Укажите пустой масив для получения всех обновлений.
    usePlugins: ['askUser'], // Необязателное поле. Использовать плагины из pluginFolder.
    pluginFolder: '../plugins/', // Необязателное поле. Расположение папки с плагинами.
    pluginConfig: { // Необязателное поле. Конфигурации плагинов.
        // myPluginName: {
        //   data: 'my custom value'
        // }
    }
});
```

Или просто:

```js
const TeleBot = require('telebot');
const bot = new TeleBot('TELEGRAM_BOT_TOKEN');
```

*Не забудьте вставить [Telegram Bot API](https://core.telegram.org/bots#create-a-new-bot) токен.*

Для запуска поллинга, используйте ```bot.start()```.

```js
bot.on('text', (msg) => msg.reply.text(msg.text));

bot.start();
```

Мы создали бота-повторюшку!

## 🌱 Быстрые примеры

Отправить текст на команды `/start` или `/hello`:

```js
bot.on(['/start', '/hello'], (msg) => msg.reply.text('Welcome!'));
```

Когда получен стикер, ответить:

```js
bot.on('sticker', (msg) => {
    return msg.reply.sticker('http://i.imgur.com/VRYdhuD.png', { asReply: true });
});
```

Отправлять фото на сообщения "show kitty" или "kitty" (используя регулярные выражения):

```js
bot.on(/(show\s)?kitty*/, (msg) => {
    return msg.reply.photo('http://thecatapi.com/api/images/get');
});
```

Комманды с аргументами `/say <ваше сообщение>`:

```js
bot.on(/^\/say (.+)$/, (msg, props) => {
    const text = props.match[1];
    return bot.sendMessage(msg.from.id, text, { replyToMessage: msg.message_id });
});
```

Когда сообщение было отредактировано:

```js
bot.on('edit', (msg) => {
    return msg.reply.text('Вы отредактировали сообщение!', { asReply: true });
});
```

*Примечание: `msg.reply` это не метод бота, это метод плагина [shortReply](/plugins/shortReply.js).*

***[Посмотреть больше примеров!](/examples)***

## ⏰ События

Используйте ```bot.on(<event>, <function>)``` для обработки всех событий TeleBot.

Для примера, добавим слеш, чтобы ответить на команду:

```js
bot.on('/hello', (msg) => {
  return bot.sendMessage(msg.from.id, `Hello, ${ msg.from.first_name }!`);
});
```

Таже, можно добавлять массив событий:

```js
bot.on(['/start', 'audio', 'sticker'], msg => {
  return bot.sendMessage(msg.from.id, 'Bam!');
});
```

### События TeleBot:

- **/&#42;** – любая команда
- **/\<cmd\>** – требуемая команда
- **start** – бот запущен
- **stop** – бот остановлен
- **reconnecting** – бот переподкоючается
- **reconnected** – бот успешно подключён
- **update** - обновление бота
- **tick** – тик бота
- **error** – ошибка бота
- **inlineQuery** - инлайн очередь
- **chosenInlineResult** - выбор инлайн результата
- **callbackQuery** - колбэк данные кнопки
- **shippingQuery** - входящий запрос на покупку
- **preShippingQuery** - входящий запрос на пре-оплату

#### События:

*keyboard*, *button*, *inlineKeyboard*, *inlineQueryKeyboard*, *inlineButton*, *answerList*, *getMe*, *sendMessage*, *deleteMessage*, *forwardMessage*, *sendPhoto*, *sendAudio*, *sendDocument*, *sendSticker*, *sendVideo*, *sendVideoNote*, *sendVoice*, *sendLocation*, *sendVenue*, *sendContact*, *sendChatAction*, *getUserProfilePhotos*, *getFile*, *kickChatMember*, *unbanChatMember*, *answerInlineQuery*, *answerCallbackQuery*, *answerShippingQuery*, *answerPreCheckoutQuery*, *editMessageText*, *editMessageCaption*, *editMessageReplyMarkup*, *setWebhook*

### События Telegram сообщений:

- **&#42;** - любой тип сообщения
- **text** – текстовое сообщщение
- **audio** – аудио файл
- **voice** – голосовое сообщение
- **document** – документ (любого формата)
- **photo** – фото
- **sticker** – стикер
- **video** – видео файл
- **videoNote** - видео-заментка
- **contact** – контакт
- **location** – геолокация
- **venue** – локация
- **game** - игра
- **invoice** - запрос платежа
- **edit** – редактирование сообщения
- **forward** – пересланное сообщение
- **pinnedMessage** – закрепление сообщения
- **newChatMembers** - новый пользователь был добавлен в группу или супергруппу
- **leftChatMember** – пользоватль был удалён
- **newChatTitle** – новое имя чата/канала
- **newChatPhoto** – новый логотип чата/канала
- **deleteChatPhoto** – логотип чата/канала был удалён
- **groupChatCreated** – группа была созданиа
- **channelChatCreated** – канал был создан
- **supergroupChatCreated** – супергруппа была создана
- **migrateToChat** – группа мигрировалла в супергруппу
- **migrateFromChat** –  супергруппа мигрировала из группы

*Читайте большо о типах ответов Telegram Bot API: https://core.telegram.org/bots/api#available-types*

## 🚜 Модификаторы

Вы можете добавить модификатор для обработки данных, прежде чем передавать его в событие.

```js
bot.mod('text', (data) => {
  let msg = data.message;
  msg.text = `📢 ${ msg.text }`;
  return data;
});
```

Этот код бобавляет эможджи к каждому `text` сообщению.

### Модификаторы TeleBot:

- **property** - модификация свойств формы
- **updateList** - список обновлений за тик
- **update** - список обновлений
- **message** - обработка любого типа сообщения
- **\<type\>** - особый тип сообщения

## 🔌 Плагины

Используйте опцию конфигурации `usePlugins` для загрузки плагинов из директории `pluginFolder`:

```js
const bot = new TeleBot({
    token: 'TELEGRAM_BOT_TOKEN',
    usePlugins: ['askUser', 'commandButtons'],
    pluginFolder: '../plugins/',
    pluginConfig: {
        // Конфигурации плагинов
    }
});
```

Или используйте ```plug(require(<plugin_path>))``` для загрузки стороннего плагина.

***[Проверьте нашу папку с плагинами!](/plugins)***

### Структура плагина

```js
module.exports = {
    id: 'myPlugin', // Уникальное имя плагина
    defaultConfig: {
        // Стандартная конфигурация плагина
        key: 'value'
    },
    plugin(bot, pluginConfig) {
        // Код плагина
    }
};
```

## ⚙️ Методы

### Методы TeleBot:

##### `on(<events>, <function>)`

Обработка событий.

##### `event(<event>, <data>)`

Вызов обработчика событий.

##### `mod(<name>, <fn>)`

Добавление модификатора данных.

##### `modRun(<names>, <data>)`

Запуск модификатора данных.

##### `plug(<plugin function>)`

Использоване функции плагина.

##### `keyboard([array of arrays], {resize, once, remove, selective})`

Создания объекта `ReplyKeyboardMarkup` клавиатуры `replyMarkup`.

##### `button(<location | contact>, <text>)`

Создание `KeyboardButton` кнопки.

##### `inlineButton(<text>, {url | callback | game | inline | inlineCurrent | pay})`

Создание объкта кнопок `InlineKeyboardButton`.

##### `inlineKeyboard([array of arrays])`

Создание инлайн клавиатуры для обычных сообщений.

##### `answerList(<inline_query_id>, {nextOffset, cacheTime, personal, pmText, pmParameter})`

Создание объекта очереди ответов `answerInlineQuery`.

##### `inlineQueryKeyboard([array of arrays])`

Создание объекта инлайн клавиатуры для списка ответов.

##### `start()`

Запуск поллинга.

##### `stop(<message>)`

Остановка поллинга.

### Telegram methods:

TeleBot использует стандартные имена методов [Telegram Bot API](https://core.telegram.org/bots/api#available-methods)s.

##### `getMe()`

Простой метод проверки токена бота.

##### `answerQuery(<answerList>)`

Метод для отправки списка ответов инлайн очереди.

##### `getFile(<file_id>)`

Метод для получения основной информаци о файле и подготовка к загрузкеg.

##### `sendMessage(<chat_id>, <text>, {parseMode, replyToMessage, replyMarkup, notification, webPreview})`

Метод для отправки текстовых сообщений.

##### `forwardMessage(<chat_id>, <from_chat_id>, <message_id>, {notification})`

UМетод для пересылки сообщений любого типа.

##### `deleteMessage(<chat_id>, <from_message_id>)`

Метод для удаления собщений. Сообщение может быть удалено, если оно отправлено ранее 48 часов. Любой тип исходящих сообщений можит быть удалён. Также, если бот - администратор чата, он может удалять любые сообщения. Если бот администратор супергруппы или канала, он может удаляьб сообщения люого другого пользователя, включая сервисные сообщения о вступлении или выходе из группы. Возвращает *True* при успешном удалении.

##### `sendPhoto(<chat_id>, <file_id | path | url | buffer | stream>, {caption, fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Метод для отправки фото.

##### `sendAudio(<chat_id>, <file_id | path | url | buffer | stream>, {title, performer, duration, caption, fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Метод для отправки аудио файлов, как голосовых сообщений.

##### `sendDocument(<chat_id>, <file_id | path | url | buffer | stream>, {caption, fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Метод для отправки документа.

##### `sendSticker(<chat_id>, <file_id | path | url | buffer | stream>, {fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Метод для отправки `.webp` стикеров.

##### `sendVideo(<chat_id>, <file_id | path | url | buffer | stream>, {duration, width, height, caption, fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Метод для отправки видеофайлов, клиент Telegram поддерживает `mp4` формат (другие форматы могут бвть отправлены на документ).

##### `sendVideoNote(<chat_id>, <file_id | path | url | buffer | stream>, {duration, length, fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Мето для отправки видеосообщений.

##### `sendVoice(<chat_id>, <file_id | path | url | buffer | stream>, {duration, caption, fileName, serverDownload, replyToMessage, replyMarkup, notification})`

Метод для отправки аудиофайлов, отображается как голосовое сообщение.

##### `sendLocation(<chat_id>, [<latitude>, <longitude>], {replyToMessage, replyMarkup, notification})`

Метод для отправки геоданных.

##### `editMessageLiveLocation({chatId + messageId | inlineMessageId, latitude, longitude}, {replyMarkup})`

Метод для редактирования локации сообщений, отправленных через бота (лдя инлайн ботов). Сообщение может быть отредактировано, пока его live_period не истечёт или трансляции геолокации не будет отклчена с помощью stopMessageLiveLocation.

##### `stopMessageLiveLocation({chatId + messageId | inlineMessageId}, {replyMarkup})`

Метод для остановки трансляции геолокации.

##### `sendVenue(<chat_id>, [<latitude>, <longitude>], <title>, <address>, {foursquareId, replyToMessage, replyMarkup, notification})`

Метод для отправки адреса.

##### `getStickerSet(<name>)`

Метод для получения пака стикеров.

##### `uploadStickerFile(<user_id>, <file_id | path | url | buffer | stream>)`

Метод для выгрузки .png файла со стикером для дальнейшего использования в методах createNewStickerSet и addStickerToSet.

##### `createNewStickerSet(<user_id>, <name>, <file_id | path | url | buffer | stream>, <emojis>, {containsMasks, maskPosition})`

Метод для создания пака стикеров, которыми будет владеть пользователь. Бот сможет изменять созданный пак стикеров.

##### `setChatStickerSet(<chat_id>, <sticker_set_name>)`

Метод для изменения пака стрикеров супергрцппы. Для работы боту необходимы права администратора чата.

##### `deleteChatStickerSet(<chat_id>)`

Метод для удаления пака стикеров из супергруппы. Для работы боту необходимы права администратора чата.

##### `addStickerToSet(<user_id>, <name>, <file_id | path | url | buffer | stream>, <emojis>, {maskPosition})`

Метод для добавления стикера в пак, созданный ботом.

##### `setStickerPositionInSet(<sticker>, <position>)`

Метод для изменения позиции стикера в паке.

##### `deleteStickerFromSet(<sticker>)`

Метод для удаления стикера из пака, созданного ботом.

##### `sendContact(<chat_id>, <number>, <firstName>, <lastName>, { replyToMessage, replyMarkup, notification})`

Метод для отправки контакта.

##### `sendAction(<chat_id>, <action>)`

Метод для отправки процесса действия бота. Выберете, каккоедействие должен видель пользователь: *typing* для показа статуса "печатает", *upload_photo* для фото, *record_video* или *upload_video* для видео, *record_audio* или *upload_audio* для аудиофайлов, *upload_document* для документов, *find_location* для данных геолокации, *record_video_note* или *upload_video_note* для видео-сообщений.

##### `sendGame(<chat_id>, <game_short_name>, {notification, replyToMessage, replyMarkup})`

Метод для отправки игры.

##### `setGameScore(<user_id>, <score>, {force, disableEditMessage, chatId, messageId, inlineMessageId})`

=Мктод для изменения рекорда пользователя в игре. В случае успеха, если собщение было отправлено ботом, вощвращает отредактированное сообщение, иначе возвращает *True*. Возвращает ошибку, если новый рекорд не больше предыдущего в чате и возвращает *False*.

##### `getGameHighScores(<user_id>, {chatId, messageId, inlineMessageId})`

Метод для получения данных рекордов пользователя. Возвращает рекорды пользователя в игре. В случае успеха, возвращает массив объектов *GameHighScore*.

##### `getUserProfilePhotos(<user_id>, {offset, limit})`

Метод для получения списка аватарок профиля пользователя.

##### `getFile(<file_id>)`

Метод для получения основной информации о файле и подготовки его к загрузке.

##### `sendInvoice(<chat_id>, {title, description, payload, providerToken, startParameter, currency, prices, photo: {url, width, height}, need: {name, phoneNumber, email, shippingAddress}, isFlexible, notification, replyToMessage, replyMarkup})`

Метод для отправки запрJса платежа.

##### `getChat(<chat_id>)`

Метод для получения информации и чате/группе/канале.

##### `leaveChat(<chat_id>)`

Метод для выхода бота из чата/группы/канала.

##### `getChatAdministrators(<chat_id>)`

UМетод для получения администраторов чата.

##### `getChatMembersCount(<chat_id>)`

Метод для получения количества пользователей чата.

##### `getChatMember(<chat_id>, <user_id>)`

Метод для получении информации об участнике чата.

##### `kickChatMember(<chat_id>, <user_id>, {untilDate})`

Метод для исключения пользователя из группы или супергруппы.

##### `unbanChatMember(<chat_id>, <user_id>)`

Метод для разблокировки пользователя в группе/супергруппе.

##### `restrictChatMember(<chat_id>, <user_id>, {untilDate, canSendMessages, canSendMediaMessages, canSendOtherMessages, canAddWebPagePreviews})`

Метод для ограничения прав пользователя группы/супергруппы. Бот должен обладать правами администратора.

##### `promoteChatMember(<chat_id>, <user_id>, {canChangeInfo, canPostMessages, canEditMessages, canDeleteMessages, canInviteUsers, canRestrictMembers, canPinMessages, canPromoteMembers})`

Метод для увеличения прав пользовател группы/супергруппы. Бот должен обладать правами администратора.

##### `exportChatInviteLink(<chat_id>)`

Метод для получения ссылки для приглашения поьзователей в супергруппу или канал. Бот должен обладать правами администратора.

##### `setChatPhoto(<chat_id>, <file_id | path | url | buffer | stream>)`

Метод для изменения логотипа чата/группы/канала. Логитип не может быть изменён в приватном чате. Бот должен обладать правами администратора.

##### `deleteChatPhoto(<chat_id>)`

Метод для удаления логотипа чата/группы/канала. Логитип не может быть удалён в приватном чате. Бот должен обладать правами администратора.

##### `setChatTitle(<chat_id>, <title>)`

Метод для изменения названия чата/группы/канала. Название не может быть изменено в приватном чате. Бот должен обладать правами администратора.

##### `setChatDescription(<chat_id>, <description>)`

Метод для смени описания супергруппы или канала. Название не может быть изменено в приватном чате. Бот должен обладать правами администратора.

##### `pinChatMessage(<chat_id>, <message_id>)`

Метод для закрепления сообщения в супергруппе. Название не может быть изменено в приватном чате. Бот должен обладать правами администратора.

##### `editMessageText({chatId & messageId | inlineMsgId}, <text>)`

Метод для редактирования текстовых сообщений, отправленных через бот.

##### `editMessageCaption({chatId & messageId | inlineMsgId}, <caption>)`

Метод для редактирования заголовка сообщений, отправленных через бот.

##### `editMessageReplyMarkup({chatId & messageId | inlineMsgId}, <replyMarkup>)`

Метод для смены клавиатуры сообщений, отправленных через бот.

##### `answerCallbackQuery(<callback_query_id>, {text, url, showAlert, cacheTime})`

Метод для ответа на инлайн очереди, отправленные с инлайн клавиатур

##### `answerShippingQuery(<shipping_query_id>, <ok> {shippingOptions, errorMessage})`

Метод для ответа на платёжные запросы.

##### `answerPreCheckoutQuery(<pre_checkout_query_id>, <ok> {errorMessage})`

Метод для отпета на предплатёжные запросы.

##### `setWebhook(<url>, <certificate>, <allowed_updates>, <max_connections>)`

Метод для задания URL и получения обновлений через вебхук

##### `getWebhookInfo()`

Метод для получения текущего статуса вебхука

##### `deleteWebhook()`

Метод для удаления вебхука, если хотите переключиться на поллин. Возвращает `True` в случае успеха.
