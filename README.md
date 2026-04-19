我讓他用C++的語法解釋Vue的語法
例如 v-model 就類似 cin 等等


| Vue 語法 | C++ 比喻 | 說明 |
|---------|---------|------|
| `ref('')` | `string nameBox` | 一個「盒子」，用來存放資料 |
| `v-model="nameBox"` | `cin >> nameBox` | 輸入框的內容會自動放進盒子裡 |
| `{{ nameBox }}` | `cout << nameBox` | 把盒子裡的內容顯示出來 |
| `v-if="photoBox"` | `if (!photoBox.empty())` | 如果盒子裡有東西才顯示 |
| `@click="searchGame"` | `button.onClick = searchGame` | 按鈕被點擊時執行的動作 |
| `fetch(...)` | `curl` | 去網路要資料 |

### 核心程式碼解釋

```javascript
// 就像 C++ 的變數宣告
const nameBox = ref('')      // string nameBox = "";
const photoBox = ref('')     // string photoBox = "";
const isLoadingBox = ref(false)  // bool isLoadingBox = false;

// 就像 C++ 的函式
function searchGame() {
  if (!nameBox.value) return;  // if (nameBox.empty()) return;
  
  // 就像 C++ 的 fetch/網路請求
  fetch('https://api.github.com/users/' + nameBox.value)
    .then(response => response.json())
    .then(data => {
      photoBox.value = data.avatar_url;  // photoBox = data.avatar_url;
    });
}
```
