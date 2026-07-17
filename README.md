# 🎮 QQQ-CRAFT Launcher (Phone)

> Український лаунчер Minecraft для сервера **QQQ-CRAFT** — на базі MojoLauncher/PojavLauncher

![QQQ-CRAFT](https://qqq-craft.top/assets/qqq.png)

## 🇺🇦 Про сервер QQQ-CRAFT

**QQQ-CRAFT** — український SMP сервер із акцентом на живу взаємодію та спільний розвиток у світі Minecraft.

- 🌍 **IP:** `play.qqq-craft.top`
- 🎯 **Версія:** 1.21.1 (Java + Bedrock)
- 👥 **Онлайн:** до 50 гравців
- 🌐 **Сайт:** [qqq-craft.top](https://qqq-craft.top)
- 💬 **Discord:** [discord.gg/WWYcBQ2Jhr](https://discord.gg/WWYcBQ2Jhr)
- 📱 **Telegram:** [t.me/qqqua_tg](https://t.me/qqqua_tg)

### ⚔️ Що на сервері?
- 🏰 Виживання з покращеними механіками
- 👹 Унікальні боси з власними умовами виклику
- 💰 Економіка на головах та покращена торгівля
- 🎭 Плащі, перки та кастомні предмети
- 🏛️ Гільдії та спільнота
- 📰 Щотижнева газета The QQQ Times
- 🎵 Фонова музика для подорожей
- 🎣 Покращена риболовля та квести

## 📱 Про лаунчер

Цей лаунчер дозволяє грати в Minecraft: Java Edition на Android, з попередньо налаштованим підключенням до сервера QQQ-CRAFT.

### Функції лаунчера:
- ✅ Автоматичне налаштування підключення до QQQ-CRAFT
- ✅ Українська мова інтерфейсу
- ✅ Швидкий доступ до Discord та Telegram спільноти
- ✅ Вбудований модпак сервера
- ✅ Усі функції MojoLauncher (керування, рендерери, тощо)

## 🔨 Збірка

### Вимоги:
- Android Studio (остання версія)
- Android SDK 36
- NDK 29.0.14206865
- JDK 8+

### Кроки:
```bash
git clone https://github.com/zheniasvinar-dev/QQQ-CRAFT-launcher-phone.git
cd QQQ-CRAFT-launcher-phone
git submodule update --init --recursive
./gradlew assembleFullDebug
```

## 📋 Структура проєкту

```
QQQ-CRAFT-launcher-phone/
├── app_pojavlauncher/     # Основний модуль лаунчера
│   ├── src/main/
│   │   ├── java/          # Java код
│   │   ├── res/           # Ресурси UI, іконки, кольори
│   │   └── assets/        # Конфігурації, компоненти
│   └── build.gradle       # Налаштування збірки
├── forge_installer/       # Встановлювач Forge/NeoForge
├── glfw/                  # GLFW субмодуль
├── gradle/                # Gradle wrapper
└── scripts/               # Допоміжні скрипти
```

## 🎨 Брендинг

Кольорова палітра QQQ-CRAFT:
- **Основний:** `#4CAF50` (зелений)
- **Акцент:** `#8BC34A` (світло-зелений)
- **Фон:** `#1B1B1F` (темний)
- **Текст:** `#FFFFFF` (білий)
- **Кнопки:** `#66BB6A` (зелена кнопка Play)

## 📜 Ліцензія

Цей проєкт базується на [MojoLauncher](https://github.com/mojolauncher/mojolauncher), який базується на [PojavLauncher](https://github.com/PojavLauncherTeam/PojavLauncher).

Ліцензія: GPL-3.0 (див. [LICENSE](LICENSE))

---

<p align="center">
  Зроблено з 💚 для спільноти QQQ-CRAFT 🇺🇦
</p>
