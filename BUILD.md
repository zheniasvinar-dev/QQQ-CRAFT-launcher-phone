# 🔨 Інструкція зі збірки QQQ-CRAFT Launcher

## Вимоги

1. **Android Studio** (остання стабільна версія)
2. **JDK 8+**
3. **Android SDK:**
   - Platform 36 (Android 16)
   - Build Tools 36.x.x
   - NDK 29.0.14206865
   - CMake (остання версія)

## Кроки збірки

### 1. Клонування
```bash
git clone https://github.com/zheniasvinar-dev/QQQ-CRAFT-launcher-phone.git
cd QQQ-CRAFT-launcher-phone
git submodule update --init --recursive
```

### 2. Встановлення GLFW субмодулю
```bash
git submodule add https://github.com/MojoLauncher/glfw.git glfw
```

### 3. Збірка через Android Studio
1. Відкрийте проєкт в Android Studio
2. Дочекайтеся синхронізації Gradle
3. Виберіть `fullDebug` build variant
4. Натисніть **Build > Make Project** (Ctrl+F9)

### 4. Збірка через командний рядок
```bash
# Debug версія
./gradlew assembleFullDebug

# Release версія (потребує ключ підпису)
./gradlew assembleFullRelease
```

## Створення ключа підпису (для release)

```bash
keytool -genkey -v -keystore qqqcraft_release.jks \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias key0 \
  -dname "CN=QQQ-CRAFT, OU=Development, O=QQQ-CRAFT, L=Chernivtsi, ST=Chernivtsi, C=UA"
```

## Варіанти збірки

| Variant | Опис |
|---------|------|
| `fullDebug` | З JRE runtime, debug режим |
| `fullRelease` | З JRE runtime, release |
| `noruntimeDebug` | Без JRE runtime, debug |
| `noruntimeRelease` | Без JRE runtime, release |

## Рішення проблем

### Gradle не синхронізується
- Перевірте підключення до Інтернету
- Видаліть `.gradle` та `build` директорії і спробуйте знову

### NDK не знайдено
- Встановіть NDK через SDK Manager: `ndk;29.0.14206865`

### GLFW submodule відсутній
```bash
git submodule update --init --recursive
```

### Помилка "Cannot find git.artdeell.mojo.R"
- Зробіть Clean Project, потім Rebuild Project
- Це через зміну namespace на `ua.qqqcraft.launcher`

## Налаштування для розробників

### Зміна IP сервера
Відредагуйте `Tools.java`:
```java
public static final String SERVER_IP = "play.qqq-craft.top";
```

### Зміна посилань на спільноту
```java
public static final String URL_DISCORD = "https://discord.gg/WWYcBQ2Jhr";
public static final String URL_TELEGRAM = "https://t.me/qqqua_tg";
```

### Зміна кольорів
Відредагуйте `res/values/colors.xml` для зміни кольорів інтерфейсу.
