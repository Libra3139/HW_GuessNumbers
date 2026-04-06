# 🐱 猜數字遊戲

一個可愛風格的數字猜測遊戲，使用 Vue 3 製作！

## 🎮 遊戲玩法

1. 電腦會在心裡想一個數字（0 到 100）
2. 你來猜測這個數字
3. 電腦會告訴你「太小了 👆」或「太大了 👇」
4. 根據提示不斷縮小範圍，直到猜對！

## ✨ 功能特色

- 🐱 **可愛小貓咪**：會根據遊戲狀態改變表情
  - 等待中：睡覺打哈欠 😴
  - 數字太小：驚訝往上 👆😾
  - 數字太大：無奈往下 👇😿
  - 猜對了：開心跳躍 🎉✨
- 🎨 **粉色點點背景**：少女風格的視覺設計
- 🍬 **圓圓胖胖的按鈕**：糖果風格的互動元素
- ✨ **撒星星特效**：猜對了會有滿天星星
- 🔄 **再玩一次**：遊戲結束可以快速重來
- 📱 **響應式設計**：支援手機和平板

## 🛠️ 技術棧

- HTML5
- CSS3（動畫、漸層、圓角）
- Vue 3（CDN 版本）

## 🚀 如何使用

1. **直接用瀏覽器打開**
   ```
   雙擊 index.html 檔案
   ```

2. **或使用 VS Code**
   - 安裝「Live Server」擴充功能
   - 在 index.html 上按右鍵 →「Open with Live Server」

## 📁 檔案結構

```
├── index.html      # 遊戲主檔案（所有程式碼在這裡）
├── AGENTS.md       # 開發代理指示
└── docs/
    └── PLAN.md     # 實作計畫
```

## 💡 遊戲技巧

這個遊戲可以用**二分搜尋法**快速找到答案！

每次猜**中間的數字**：
- 範圍 0~100 → 先猜 50
- 如果太大 → 變成 0~49
- 如果太小 → 變成 51~100

**100 個數字只需要猜 7 次就能找到答案！**

## 🔄 Vue 語法 vs C++ 語法 對照表

對於只會 C++ 的同學，這個表格幫你理解 Vue 的概念：

### 1. 資料盒子（data）

| Vue | C++ |
|-----|-----|
| `data() { return { answer: 50 } }` | `int answer = 50;` |
| `data() { return { guess: null } }` | `int* guess = nullptr;` |
| `data() { return { attempts: 0 } }` | `int attempts = 0;` |
| `data() { return { status: 'idle' } }` | `string status = "idle";` |

### 2. 顯示資料（模板插值）

| Vue | C++ |
|-----|-----|
| `{{ answer }}` | `cout << answer;` |
| `{{ guess }}` | `cout << guess;` |
| `{{ attempts }}` | `cout << attempts;` |
| `{{ statusMessage }}` | `cout << statusMessage;` |

### 3. 雙向綁定（v-model）

| Vue | C++ |
|-----|-----|
| `v-model="guess"` | `cin >> guess;` |
| 輸入框和盒子是**連線的** | 輸入就像 scanf 一樣寫入變數 |

### 4. 迴圈（v-for）

| Vue | C++ |
|-----|-----|
| `v-for="star in stars"` | `for (Star star : stars)` |
| `v-for="(star, i) in stars"` | `for (int i = 0; i < stars.size(); i++)` |

### 5. 條件顯示（v-if）

| Vue | C++ |
|-----|-----|
| `v-if="status === 'correct'"` | `if (status == "correct") { ... }` |
| 只在條件成立時顯示元素 | 只在條件成立時執行程式碼 |

### 6. 動態樣式（:class）

| Vue | C++ |
|-----|-----|
| `:class="{ celebrating: status === 'correct' }"` | 需要自己判斷後輸出不同 class 名稱 |

### 7. 點擊事件（@click）

| Vue | C++ |
|-----|-----|
| `@click="checkGuess"` | 對應一個 function call |
| 按鈕被點擊時執行函式 | 需要一個按鈕 framework 才能做到 |

### 8. 函式（methods）

| Vue | C++ |
|-----|-----|
| `methods: { checkGuess() { ... } }` | `void checkGuess() { ... }` |
| `methods: { spawnStars() { ... } }` | `void spawnStars() { ... }` |

### 9. 完整範例：checkGuess 函式

**Vue 版本：**
```javascript
checkGuess() {
    const guessNumber = Number(this.guess)
    this.attempts++
    
    if (guessNumber === this.answer) {
        this.status = 'correct'
    } else if (guessNumber < this.answer) {
        this.status = 'too-small'
    } else {
        this.status = 'too-big'
    }
}
```

**C++ 版本：**
```cpp
void checkGuess() {
    int guessNumber = guess;
    attempts++;
    
    if (guessNumber == answer) {
        status = "correct";
    } else if (guessNumber < answer) {
        status = "too-small";
    } else {
        status = "too-big";
    }
}
```

### 10. 完整範例：resetGame 函式

**Vue 版本：**
```javascript
resetGame() {
    this.answer = Math.floor(Math.random() * 101)
    this.minValue = 0
    this.maxValue = 100
    this.guess = null
    this.attempts = 0
    this.status = 'idle'
    this.statusMessage = '試著猜一個數字吧！'
}
```

**C++ 版本：**
```cpp
void resetGame() {
    srand(time(NULL));
    answer = rand() % 101;
    minValue = 0;
    maxValue = 100;
    guess = 0;
    attempts = 0;
    status = "idle";
    statusMessage = "試著猜一個數字吧！";
}
```

### 11. Vue 的特色（vs C++）

| 特色 | 說明 |
|-----|------|
| **自動更新畫面** | 改變資料，畫面自動跟著變（不需要手動 `cout`） |
| **聲明式** | 說「我要什麼」，不用寫「怎麼做」 |
| **響應式** | 資料變了，綁定的地方自動更新 |

## 🎨 截圖

```
┌─────────────────────────┐
│     🐱 猜數字遊戲       │
│                         │
│        /\  /\          │
│       /  \/  \         │
│      │  ●  ●  │        │
│      │   ‿   │        │
│       \  ‿  /         │
│     Z z z              │
│                         │
│      範圍：0 ~ 100     │
│   ┌─────────────────┐  │
│   │    輸入數字...    │  │
│   └─────────────────┘  │
│      ┌───────────┐      │
│      │    猜！    │      │
│      └───────────┘      │
│                         │
│       猜了 0 次         │
└─────────────────────────┘
```

## 📝 License

這個專案僅供學習使用。
