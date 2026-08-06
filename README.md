# Legacy Notepad（正體中文 fork）

這是 [ForLoopCodes/legacy-notepad](https://github.com/ForLoopCodes/legacy-notepad) 的個人 fork。

Legacy Notepad 是一個使用 C++17 與 Win32 API 製作的輕量 Windows 記事本替代方案。原專案目標是提供比新版 Windows 記事本更單純、啟動更快、依賴更少的文字編輯器。

本 fork 在保留原專案架構與主要功能的基礎上，加入正體中文介面，並修正部分使用上會影響體驗的小問題。

## 本 fork 的主要變更

- 新增 **正體中文（zh-TW）** 作為第三語言選項，與英文、日文並存。
- 語言選單支援執行期間切換，不需要重新啟動程式。
- 「自動換行」設定會寫入使用者設定，下次啟動時會保留。
- 修正 `Redo` 指令原本誤觸發 `Undo` 的問題。
- 調整部分本地化後容易被裁切的對話框版面。
- 補齊部分選單與對話框的正體中文翻譯。
- 調整 `↑` / `↓` 在單行文件與文件首尾的邊界導覽行為。
- 修正尋找功能的選取位置錯亂問題，改用 RichEdit 原生搜尋定位。
- 保留原專案版本號；若未來本 fork 發布獨立版本，會另外使用 fork 專用 release tag 標示。

## 原專案簡介

A lightweight, 25x fast, Windows notepad alternative built with C++17 and Win32 API which I made because microsoft wont stop adding AI bloatware to notepad.exe.

<img width="752" height="242" alt="image" src="https://github.com/user-attachments/assets/bca18796-b088-4488-a445-649a549ddace" />
<img width="738" height="243" alt="image" src="https://github.com/user-attachments/assets/e9ba9361-d1ea-41ee-a18d-f38fb9bdb546" />

## 截圖

<img width="986" height="735" alt="image" src="https://github.com/user-attachments/assets/939cbb8b-a770-449c-9d9d-e93e076787d3" />

## 功能

- **多語言介面**：支援英文、日文、正體中文，並可在執行期間切換。
- **多種文字編碼**：支援 UTF-8、UTF-8 BOM、UTF-16 LE/BE、ANSI，以及換行格式選擇。
- **基本文字編輯**：自動換行、字型選擇、縮放、時間／日期插入、尋找、取代、到指定行與鍵盤導覽。
- **背景圖片**：支援圖片背景、平鋪、延展、符合、填滿、定位與不透明度控制。此功能仍有已知限制。
- **最上層顯示**：可將視窗固定在其他視窗上方。
- **列印**：支援列印與版面設定對話框。
- **自訂圖示**：可從 `.ico` 檔案，或 `.exe`、`.dll`、`.icl`、`.mun` 等圖示庫更換應用程式圖示。

## 建置需求

建置 Legacy Notepad 需要：

- **作業系統**：Windows 7 SP1 或更新版本。Windows 10/11 可取得較完整的 UI 功能支援。
- **建置系統**：[CMake](https://cmake.org/download/) 3.16 或更新版本。
- **編譯器**：支援 C++17 的工具鏈：
  - **MinGW-w64**：原專案以 GCC 13.2.0 測試。
  - **MSVC**：Visual Studio 2022 或更新版本。
- **SDK**：Windows SDK，可透過 Visual Studio 或 MSYS2 安裝。
- **相依元件**：程式使用標準 Windows 元件，包括 `GDI+`、`RichEdit 4.1`（`Msftedit.dll`）、`Common Controls` 與 `DWM`。

## 建置與執行

### 使用 MinGW

確認 MinGW 的 `bin` 資料夾已加入系統 `PATH`。

```powershell
# 1. Clone repository
git clone https://github.com/ForLoopCodes/legacy-notepad.git
cd legacy-notepad

# 2. Create and enter build directory
mkdir build
cd build

# 3. Configure and build
cmake -G "MinGW Makefiles" ..
mingw32-make -j$(nproc)

# 4. Run the application
.\legacy-notepad.exe
```

### 使用 Visual Studio / MSVC

1. 使用 Visual Studio 2022 開啟專案資料夾。
2. Visual Studio 會自動偵測 `CMakeLists.txt` 並設定專案。
3. 在啟動項目選單中選擇 `legacy-notepad.exe`。
4. 按 `F5` 建置並執行。

也可以使用命令列：

```powershell
cmake -B build -S .
cmake --build build --config Release
```

若使用 Visual Studio Developer Command Prompt，也可以指定 NMake：

```cmd
cmake -S . -B build-msvc-x64 -G "NMake Makefiles" -DCMAKE_BUILD_TYPE=Release
cmake --build build-msvc-x64
build-msvc-x64\legacy-notepad.exe
```

## 架構

- **Entry**：`src/main.cpp`，負責視窗類別、訊息迴圈與模組串接。
- **Core**：`src/core`，共用型別與全域狀態。
- **Language**：`src/lang`，包含英文、日文與正體中文介面字串。
- **Modules**：`src/modules`
  - `theme`：深色模式與視窗外觀。
  - `editor`：RichEdit 控制項、自動換行、縮放與輸入處理。
  - `file`：檔案讀寫、編碼與換行格式。
  - `ui`：標題列、狀態列與版面配置。
  - `background`：GDI+ 背景圖片功能。
  - `dialog`：尋找、取代、到指定行、字型與透明度對話框。
  - `commands`：選單指令處理。
- **Resources**：`src/notepad.rc`、`src/resource.h`，包含選單、快捷鍵、圖示與資源 ID。

## Repository tree

```text
src/
  core/           # types, globals
  lang/           # UI language strings
  modules/        # theme, editor, file, ui, background, dialog, commands
  main.cpp
  notepad.rc
CMakeLists.txt
```

## File quicklook

| File/Folder                        | Purpose |
| ---------------------------------- | ------- |
| `src/main.cpp`                     | Win32 entry point, WndProc, module wiring |
| `src/core/types.h`                 | Enums, structs, app constants |
| `src/core/globals.*`               | Shared handles/state definitions |
| `src/lang/*`                       | English, Japanese, Traditional Chinese UI strings |
| `src/modules/editor.*`             | RichEdit setup, word wrap, zoom, keyboard handling |
| `src/modules/file.*`               | Load/save, encoding and line endings |
| `src/modules/ui.*`                 | Title/status updates, layout sizing |
| `src/modules/theme.*`              | Dark mode title/menu/status, theming |
| `src/modules/background.*`         | GDI+ background image/opacity/position |
| `src/modules/dialog.*`             | Find/replace/goto, font, transparency dialogs |
| `src/modules/commands.*`           | Menu command handlers |
| `src/notepad.rc`, `src/resource.h` | Menus, accelerators, icons |

## 已知問題

- Windows Defender 可能會標記 `legacy-notepad-x86.exe`。請參考原專案 issue #26：https://github.com/ForLoopCodes/legacy-notepad/issues/26
- 背景圖片功能在不同 DPI、縮放比例與 RichEdit 重繪狀態下，可能有顯示差異。
- 未簽章的 Windows 執行檔偶爾可能觸發防毒軟體的啟發式偵測。下載公開二進位檔時請確認 SHA-256。

## License

MIT License - see [LICENSE](LICENSE).

## 原作者資訊

原專案由 ForLoopCodes 維護。  
更多資訊可參考：[x.com/forloopcodes](https://x.com/forloopcodes)