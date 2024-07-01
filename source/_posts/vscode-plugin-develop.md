---
title: 如何開發 VS Code 套件
date: 2024-07-01 11:11:02
categories: VSCode
tags: VSCode Plugin
---

<br/>

要在你的專案中使用自己開發的 VS Code 擴展，通常需要先完成本地開發、測試和打包，然後在你的 VS Code 中安裝該擴展。以下是詳細步驟：

## 1. 創建自定義擴展
要完全自定義懸停提示，最好的方法是創建自己的 VS Code 擴展。以下是如何從頭開始創建一個簡單的擴展：

1. 安裝必要的工具
首先，確保你已經安裝了 Node.js 和 npm（Node.js 的包管理工具）。你可以從 Node.js 官方網站 安裝它們。

接下來，安裝 yo 和 generator-code，這些工具將幫助你生成 VS Code 擴展模板：
``` cmd
npm install -g yo generator-code
```

2. 生成擴展模板
使用 yo 生成一個新的 VS Code 擴展：
``` cmd
yo code
```
按照提示進行操作，選擇 "New Extension (TypeScript)"，並填寫相關信息。

3. 編輯擴展代碼
生成的擴展模板會在 src/extension.ts 文件中包含一些基本代碼。打開這個文件，並添加以下代碼來實現懸停提示功能：
``` typescript
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {

    const hoverProvider = vscode.languages.registerHoverProvider('plaintext', {
        provideHover(document, position, token) {
            const range = document.getWordRangeAtPosition(position);
            const word = document.getText(range);

            if (word === '你的自定義關鍵字') {
                return new vscode.Hover('這是你的自定義懸停提示訊息');
            }

            return null;
        }
    });

    context.subscriptions.push(hoverProvider);
}

export function deactivate() {}
```
在這個示例中，我們註冊了一個懸停提供者 hoverProvider，當懸停在 你的自定義關鍵字 上時，它會顯示自定義的提示訊息。

4. 編譯擴展
在項目根目錄中運行以下命令來編譯擴展：
``` cmd
npm install
npm run compile
```

## 5. 開發和測試你的擴展
按 F5 啟動擴展開發主機（Extension Development Host）。這將打開一個新的 VS Code 窗口，其中將加載和運行你的擴展。在這個窗口中進行測試，確保擴展正常工作。

## 6. 打包擴展
在你確定擴展可以正常工作後，需要將其打包為 .vsix 文件，以便在其他項目中安裝和使用。

安裝 vsce 工具：
``` cmd
npm install -g vsce
```


在你的擴展項目目錄中運行以下命令來打包擴展：
``` cmd
vsce package
```
這將生成一個 .vsix 文件，例如 my-extension-0.0.1.vsix。

## 7. 安裝擴展
在你的 VS Code 中安裝打包好的 .vsix 文件：

1. 打開 VS Code。
2. 打開命令面板 (Ctrl+Shift+P 或 Cmd+Shift+P)。
3. 輸入並選擇 Extensions: Install from VSIX...。
4. 選擇你剛才生成的 .vsix 文件進行安裝。

## 8. 在專案中使用擴展
安裝完擴展後，你可以在你的項目中使用它。例如，如果你的擴展提供了懸停提示，打開相關文件並將滑鼠懸停在特定的代碼上，即可看到提示訊息。

## 9. 發佈擴展（可選）
如果你希望將擴展發佈到 Visual Studio Code Marketplace，讓其他用戶也能使用，可以按照以下步驟進行：

1. 登錄 Visual Studio Marketplace
    1.1. 登錄到 Visual Studio Marketplace。
    1.2. 創建或獲取一個 Personal Access Token。
2. 發佈擴展
在你的擴展項目目錄中運行以下命令來發佈擴展：

``` cmd
vsce publish
```
你需要使用 Personal Access Token 來進行身份驗證。
