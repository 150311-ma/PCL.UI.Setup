# 设置页 适配清单

目标：把 `views/` 里的页面改成只依赖 `PCL.Modular.Abstractions` 的契约，不再直接摸全局状态。

## 全局符号依赖（按文件）

| 文件 | 行数 | 仍在直接引用的全局符号 |
| --- | --- | --- |
| `PageSetupLaunch.xaml.vb` | 515 | `Settings`×9、`Hint`×3、`MyMsgBox`×3、`FrmMain`×2 |
| `PageSetupLeft.xaml.vb` | 175 | `Settings`×12、`FrmMain`×9、`MyMsgBox`×4 |
| `PageSetupLink.xaml.vb` | 34 | `Hint`×1 |
| `PageSetupSystem.xaml.vb` | 133 | `Hint`×4、`MyMsgBox`×3、`Settings`×2 |
| `PageSetupUI.xaml.vb` | 626 | `Settings`×57、`FrmMain`×27、`Hint`×11、`MyMsgBox`×6、`ThemeUnlock`×3、`RunInUi`×1 |
| `SettingService.vb` | 81 | `Settings`×3 |
| `Settings.vb` | 479 | `FrmMain`×2、`ModNet`×2、`Settings`×1 |

## 适配步骤

1. **换数据来源**：把 `ModMain` / `States` 的读取改成 `IModuleContext.Services` 里对应的服务调用。
2. **换宿主窗口引用**：`FrmMain.PanXXX` 这类直接操作宿主控件的代码，改成通过 `IHostShell` 暴露的插槽。
3. **换提示**：`Hint` / `MyMsgBox` 改成 `INotificationService`。
4. **换主题**：`ThemeUnlock` 等主题开关改成 `IThemeService`。
5. 每换完一个文件，就在下表中把该行删掉；表空了才算这个模块真正独立。

> 说明：这一步是**迁移进度的账本**，不是待办愿望单。没做完的行必须留着。
