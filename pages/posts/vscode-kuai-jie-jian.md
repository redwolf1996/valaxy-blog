---
title: 'vscode 快捷键'
date: 2023-02-24 09:32:55
tags: [vscode]
published: true
hideInList: false
feature: 
isTop: false
---
> windows和mac分别是两套不同步，但是相同平台之间是自动同步的，选项都一样
```
1. View Toggle Primary Side Bar Visibility  // 切换侧边文件目录显示和隐藏
{
  "key": "alt/cmd+1",
  "command": "workbench.action.toggleSidebarVisibility"
}

2. File Reveal Active File in Explorer View // 聚焦当前文件
{
  "key": "alt/cmd+2",
  "command": "workbench.files.action.showActiveFileInExplorer"
}

3. Delete Line // 删除光标所处的行
{
  "key": "alt/cmd+d",
  "command": "editor.action.deleteLines",
  "when": "textInputFocus && !editorReadonly"
}

4. Move Line Up // 向上移动行
{
  "key": "alt/cmd+up",
  "command": "editor.action.moveLinesUpAction",
  "when": "editorTextFocus && !editorReadonly"
}

5. Move Line Down // 向下移动行
{
  "key": "alt/cmd+down",
  "command": "editor.action.moveLinesDownAction",
  "when": "editorTextFocus && !editorReadonly"
}

6. View Toggle Terminal // 切换终端命令窗口的显示与隐藏
{
  "key": "ctrl+`",
  "command": "workbench.action.terminal.toggleTerminal",
  "when": "terminal.active"
}

7. Git Commit // git 提交
{
  "key": "shift+alt+up",
  "command": "git.commit"
}
{
  "key": "shift+cmd+up",
  "command": "git.commit"
}

8. File Save // 保存文件
{
  "key": "alt/ctrl(win)/cmd(mac)+s",
  "command": "workbench.action.files.save"
}

9. Undo // 更改撤销
{
  "key": "alt/ctrl(win)/cmd(mac)+z",
  "command": "undo"
}

10. Redo //更改前进
{
  "key": "alt/ctrl(win)/cmd(mac)+y",
  "command": "redo"
}

11. Copy // 复制
{
  "key": "cmd/ctrl/alt+c",
  "command": "editor.action.clipboardCopyAction"
}

12. Paste // 粘贴
{
  "key": "cmd/ctrl/alt+v",
  "command": "editor.action.clipboardPasteAction"
}

13. Find // 查找
{
  "key": "cmd/ctrl/alt+f",
  "command": "actions.find",
  "when": "editorFocus || editorIsOpen"
}

14. Replace // 查找并替换
{
  "key": "cmd/ctrl/alt+r",
  "command": "editor.action.startFindReplaceAction",
  "when": "editorFocus || editorIsOpen"
}

15. Close Window/Editor // 关闭窗口、标签页等
{
  "key": "cmd/ctrl/alt+w",
  "command": "workbench.action.closeWindow/closeWindow",
  "when": "!editorIsOpen && !multipleEditorGroups"
}

16. File Open Recent...  // 打开最近文件
{
  "key": "cmd/ctrl/alt+e",
  "command": "workbench.action.openRecent"
}

17. Go to File... // 快速打开文件
{
  "key": "cmd/ctrl/alt+p",
  "command": "workbench.action.quickOpen"
}

18. Show All Commands // 打开vscode命令
{
  "key": "cmd/ctrl/alt+shift+p",
  "command": "workbench.action.showCommands"
}

19. Go to Symbol in Workspace... // 全局搜索
{
  "key": "cmd/ctrl/alt+t",
  "command": "workbench.action.showAllSymbols"
}

20. Cut // 剪切
{
  "key": "cmd/ctrl/alt+x",
  "command": "editor.action.clipboardCutAction"
}

```