# 🛠️ Збірка QQQ-CRAFT Launcher на macOS Intel (i7)

## Специфіка Intel Mac
> На Intel Mac (x86_64) все працює "з коробки" — не потрібні ніякі Rosetta чи ARM емуляції, як на Apple Silicon.

---

## Крок 1: Встанови Homebrew

Відкрий **Terminal** і виконай:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

## Крок 2: Встанови JDK 8

```bash
brew install --cask temurin@8
```

Або якщо хочеш через сайт:
- Завантаж з [adoptium.net](https://adoptium.net/) — JDK 8 для macOS x64

Перевір:
```bash
java -version
# Має бути: openjdk version "1.8.x" або "8.x"
```

Якщо java показує іншу версію, встанови `JAVA_HOME`:
```bash
# Знайди шлях
/usr/libexec/java_home -V

# Додай в ~/.zshrc або ~/.bash_profile
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)
```

---

## Крок 3: Встанови Android Studio

1. Завантаж **Android Studio** з [developer.android.com](https://developer.android.com/studio)
2. Перетягни в Applications
3. Запусти і пройди початкове налаштування

### Через Android Studio встанови SDK компоненти:

Відкрий **Android Studio → Settings → SDK Manager** і встанови:

| Компонент | Версія |
|-----------|--------|
| Android SDK Platform | **36** (Android 16) |
| Android SDK Build-Tools | **36.x.x** |
| Android SDK Platform-Tools | остання |
| NDK (Side by side) | **29.0.14206865** |
| CMake | остання версія |

### Або через командний рядок:
```bash
# Встанови cmdline-tools
brew install --cask android-commandlinetools

# Прийми ліцензії
sdkmanager --licenses

# Встанови необхідне
sdkmanager "platforms;android-36"
sdkmanager "build-tools;36.0.0"
sdkmanager "ndk;29.0.14206865"
sdkmanager "cmake;3.22.1"
```

---

## Крок 4: Клонуй репозиторій

```bash
cd ~/Projects  # або будь-яка папка
git clone https://github.com/zheniasvinar-dev/QQQ-CRAFT-launcher-phone.git
cd QQQ-CRAFT-launcher-phone
```

### Додай GLFW субмодуль (ОБОВ'ЯЗКОВО!)

```bash
git submodule add https://github.com/MojoLauncher/glfw.git glfw
git submodule update --init --recursive
```

---

## Крок 5: Збірка

### Варіант А: Через Android Studio (рекомендовано)

1. Відкрий Android Studio
2. **File → Open** → вибери папку `QQQ-CRAFT-launcher-phone`
3. Дочекайся синхронізації Gradle (може зайняти 2-5 хвилин перший раз)
4. Вибери **Build Variant**: `fullDebug`
   - Зліва внизу → **Build Variants** → `app_pojavlauncher` → `fullDebug`
5. Натисни **Build → Make Project** (⌘F9)
6. APK буде в: `app_pojavlauncher/build/outputs/apk/full/debug/`

### Варіант Б: Через Terminal

```bash
cd ~/Projects/QQQ-CRAFT-launcher-phone

# Зроби gradlew виконуваним
chmod +x gradlew

# Debug збірка
./gradlew assembleFullDebug

# Результат буде тут:
# app_pojavlauncher/build/outputs/apk/full/debug/app_pojavlauncher-full-debug.apk
```

---

## ⚡ Оптимізація для Intel i7

### Паралельна збірка (використовує всі ядра)

```bash
# Подивись скільки ядер
sysctl -n hw.ncpu
# На i7 зазвичай 4-8

# Збілдь з усіма ядрами
./gradlew assembleFullDebug --parallel --max-workers=8
```

### Прискорення Gradle

Додай в `~/.gradle/gradle.properties`:
```properties
# Використовувати більше RAM (i7 має 16-32GB)
org.gradle.jvmargs=-Xmx4096m -XX:+UseParallelGC
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.daemon=true
```

### Прискорення нативної збірки (CMake/NDK)

У файлі `gradle.properties` проєкту додай:
```properties
# Кількість паралельних NDK білдів
android.builder.sdkDownload=true
```

---

## 🐛 Можливі проблеми та рішення

### ❌ "NDK not configured"
```bash
sdkmanager "ndk;29.0.14206865"
```

### ❌ "CMake not found"
```bash
sdkmanager "cmake;3.22.1"
# або
brew install cmake
```

### ❌ "GLFW submodule missing"
```bash
git submodule update --init --recursive
```
Якщо не допомагає:
```bash
rm -rf glfw
git clone https://github.com/MojoLauncher/glfw.git glfw
```

### ❌ "SDK location not found"
Створи файл `local.properties`:
```bash
echo "sdk.dir=/Users/$(whoami)/Library/Android/sdk" > local.properties
```

### ❌ "Execution failed for task ':app_pojavlauncher:externalNativeBuildFullDebug'"
Перевір що NDK і CMake встановлені:
```bash
sdkmanager --list | grep -E "ndk|cmake"
```

### ❌ "A problem occurred configuring project ':glfw:android-gradle'"
```bash
# Переконайся що glfw папка існує і має файли
ls glfw/
# Якщо порожня:
git submodule update --init --recursive
```

### ❌ Gradle синхронізація зависає
```bash
# Очисти кеш
rm -rf ~/.gradle/caches/
rm -rf .gradle/
rm -rf build/
./gradlew --refresh-dependencies
```

### ❌ "Could not resolve git.artdeell.dnbootstrap"
Це бібліотека GLFW. Переконайся що `settings.gradle` включає `glfw:android-gradle` і субмодуль клоновано.

---

## 📱 Встановлення на телефон

### Через USB (adb)
```bash
# Підключи телефон з увімкненим USB Debugging
adb install app_pojavlauncher/build/outputs/apk/full/debug/app_pojavlauncher-full-debug.apk
```

### Через Android Studio
1. Підключи телефон по USB
2. Натисни **Run ▶** (⌃R)
3. Вибери свій телефон

### Перекинь APK на телефон
```bash
# Знайди APK
open app_pojavlauncher/build/outputs/apk/full/debug/
# Перекинь файл .apk на телефон і встанови
```

---

## ⏱️ Очікуваний час збірки на Intel i7

| Етап | Час |
|------|-----|
| Перший білд (з завантаженням) | 10-20 хвилин |
| Нативна збірка (CMake/NDK) | 3-7 хвилин |
| Повторний білд (без змін) | 30-60 секунд |
| Clean rebuild | 5-10 хвилин |

> 💡 Перший білд завжди найдовший — Gradle завантажує всі залежності, NDK компілює нативний код. Наступні білди будуть набагато швидші завдяки кешу.

---

## 🎯 Швидкий старт (копі-паст все)

```bash
# 1. Клонування
git clone https://github.com/zheniasvinar-dev/QQQ-CRAFT-launcher-phone.git
cd QQQ-CRAFT-launcher-phone

# 2. GLFW субмодуль
git submodule add https://github.com/MojoLauncher/glfw.git glfw
git submodule update --init --recursive

# 3. SDK шлях
echo "sdk.dir=$HOME/Library/Android/sdk" > local.properties

# 4. Збірка
chmod +x gradlew
./gradlew assembleFullDebug --parallel

# 5. Результат
open app_pojavlauncher/build/outputs/apk/full/debug/
```

Зроблено з 💚 для QQQ-CRAFT 🇺🇦
