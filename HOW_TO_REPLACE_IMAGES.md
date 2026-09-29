# 🖼️ Как заменить фоновые изображения на локальные

## Проблема

Сейчас фоновые изображения загружаются по URL из интернета. Если интернет недоступен, изображения не загрузятся (но приложение продолжит работать).

## Решение: Скачать изображения и использовать локально

### Шаг 1: Скачай изображения

Открой эти ссылки в браузере и сохрани изображения:

1. **Девушка со штангой** (становая тяга):
   ```
   https://image.qwenlm.ai/generated-images/59452699-96d6-4a40-9846-5cb2f05ee55f/_result.png
   ```
   Сохрани как: `bg-girl-1.png`

2. **Девушка с гантелями** (вид сзади):
   ```
   https://image.qwenlm.ai/generated-images/f40798f4-d624-4e09-a702-4529f0eda293/_result.png
   ```
   Сохрани как: `bg-girl-2.png`

3. **Девушка на турнике** (подтягивания):
   ```
   https://image.qwenlm.ai/generated-images/19c1a568-3de7-4f66-9286-3bbfa6ccd463/_result.png
   ```
   Сохрани как: `bg-girl-3.png`

### Шаг 2: Положи файлы в папку

**Для веб-версии (GitHub Pages / Netlify):**
```
твой-проект/
├── index.html
├── manifest.json
├── sw.js
├── icon.svg
├── bg-girl-1.png  ← сюда
├── bg-girl-2.png  ← сюда
└── bg-girl-3.png  ← сюда
```

**Для автономного HTML файла:**
```
папка/
├── training-diary-standalone.html
├── bg-girl-1.png  ← сюда
├── bg-girl-2.png  ← сюда
└── bg-girl-3.png  ← сюда
```

### Шаг 3: Обнови CSS в index.html

Найди в `<style>` секцию `body` и замени URL:

**Было:**
```css
background-image: 
  url('https://image.qwenlm.ai/generated-images/59452699-96d6-4a40-9846-5cb2f05ee55f/_result.png'),
  url('https://image.qwenlm.ai/generated-images/f40798f4-d624-4e09-a702-4529f0eda293/_result.png'),
  url('https://image.qwenlm.ai/generated-images/19c1a568-3de7-4f66-9286-3bbfa6ccd463/_result.png'),
  ...
```

**Стало:**
```css
background-image: 
  url('bg-girl-1.png'),
  url('bg-girl-2.png'),
  url('bg-girl-3.png'),
  ...
```

### Шаг 4: Обнови Service Worker (для веб-версии)

В файле `sw.js` найди массив `urlsToCache` и замени URL:

**Было:**
```javascript
const urlsToCache = [
  '/',
  '/index.html',
  '/icon.svg',
  '/manifest.json',
  'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap',
  'https://image.qwenlm.ai/generated-images/59452699-96d6-4a40-9846-5cb2f05ee55f/_result.png',
  'https://image.qwenlm.ai/generated-images/f40798f4-d624-4e09-a702-4529f0eda293/_result.png',
  'https://image.qwenlm.ai/generated-images/19c1a568-3de7-4f66-9286-3bbfa6ccd463/_result.png'
];
```

**Стало:**
```javascript
const urlsToCache = [
  '/',
  '/index.html',
  '/icon.svg',
  '/manifest.json',
  'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap',
  '/bg-girl-1.png',
  '/bg-girl-2.png',
  '/bg-girl-3.png'
];
```

**Важно:** Не забудь изменить версию кэша:
```javascript
const CACHE_NAME = 'training-diary-v4';  // было v3
```

### Шаг 5: Закоммить изменения

```bash
git add .
git commit -m "Заменил URL изображений на локальные файлы"
git push
```

---

## 🎨 Альтернатива: Свои изображения

Если хочешь использовать другие фото:

1. Найди подходящие изображения атлетичных девушек
2. Рекомендуется размер: **600×900 px** (вертикальные)
3. Формат: **PNG** (лучше качество) или **JPG** (меньше размер)
4. Сохрани как `bg-girl-1.png`, `bg-girl-2.png`, `bg-girl-3.png`
5. Обнови CSS и Service Worker как описано выше

---

## 🖼️ Альтернатива: Убрать изображения совсем

Если не хочешь использовать фото девушек, можно убрать их из фона:

Найди в CSS секцию `body` и замени на:

```css
body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  background: var(--bg-primary);
  background-image: 
    radial-gradient(at 20% 30%, rgba(99, 102, 241, 0.15) 0px, transparent 50%),
    radial-gradient(at 80% 70%, rgba(168, 85, 247, 0.15) 0px, transparent 50%),
    radial-gradient(at 50% 100%, rgba(236, 72, 153, 0.1) 0px, transparent 50%);
  background-size: 100% 100%, 100% 100%, 100% 100%;
  background-position: center, center, center;
  background-repeat: no-repeat;
  background-attachment: scroll;
  color: var(--text-primary);
  min-height: 100vh;
  padding: 0.75rem;
  line-height: 1.6;
}
```

И удали секцию `body::before`.

---

## 💡 Советы по изображениям

### Размер файлов
- **PNG**: лучше качество, но больше размер (~200-500 KB)
- **JPG**: меньше размер (~50-150 KB), но может быть артефакты
- **WebP**: оптимальный баланс (~30-100 KB), но не все браузеры поддерживают

### Оптимизация
Используй онлайн-сервисы для сжатия:
- https://tinypng.com/ — сжатие PNG/JPG
- https://squoosh.app/ — конвертация в WebP
- https://compressor.io/ — универсальное сжатие

### Прозрачность
Если хочешь чтобы изображения лучше вписывались в дизайн:
1. Открой в графическом редакторе (Photoshop, GIMP, Figma)
2. Уменьши непрозрачность до 30-50%
3. Или добавь градиентный оверлей

---

## ✅ Проверка

После замены:

1. Открой сайт в браузере
2. Нажми **Ctrl + Shift + R** (жёсткая перезагрузка)
3. Проверь что изображения загрузились
4. Проверь что приложение работает офлайн

---

**Готово!** Теперь изображения хранятся локально и работают без интернета 💪
