---
lab:
  title: 创建 Teams 选项卡
  module: Exercise 3
---

# 练习 3：创建 Teams 选项卡

## 场景

假设 IT 支持团队想要创建 Teams 选项卡，以帮助用户在提交支持工单时访问必要信息。 例如，团队需要显示用户的区域设置代码以处理和报告工单。 当前任务是创建此选项卡以显示用户的区域设置代码。 未来会将其他信息添加到选项卡中。

## 练习任务

任务是创建 Teams 选项卡应用，该应用使用 Teams 上下文检索用户的区域设置，并将其显示在个人选项卡中。

需要完成以下任务才能完成练习：

1. 使用 Teams Toolkit for Visual Studio Code 创建选项卡应用。
1. 更新应用以检索和显示用户的区域设置。

**预计完成时间：** 10 分钟

## 任务 1：使用 Teams Toolkit for Visual Studio Code 创建选项卡应用

1. 打开 Visual Studio Code。
1. 在边栏上，选择“**Microsoft Teams**”图标以打开“**TEAMS 工具包**”面板。
1. 依次选择“**新建应用**”、“**选项卡**”。
1. 从可用选项列表中选择“**基本选项卡**”。
1. 对于编程语言，选择“**TypeScript**”。
1. 对于“**工作区文件夹**”，选择“**默认文件夹**”。
1. 对于“**应用程序名称**”，输入“**UserInfoApp**”。

成功搭建所有文件夹和文件后，会显示一条通知，Visual Studio Code 的新实例会打开新的项目文件夹。

在“**资源管理器**”面板中，*src*文件夹包含应用的源代码。 *src*文件夹外部的文件与服务器相关，例如机器人。

## 任务 2：更新应用以检索和显示用户的区域设置

现在，可以将所需的功能添加到选项卡应用。

1. 导航到`src` > `views`文件夹，然后打开`hello.html`文件。
1. 找到`<div>`元素并将其更新为在`<div>`和`</div>`标记之间包含以下元素：

    ```html
        <h1>Hello, User</h1>
      <span>
        <h2>Review your locale code:</h2>
        <p id="locale"></p>
      </span>
    ```

1. 导航到`src` > `static` > `scripts`文件夹，然后打开`teamsapp.js`文件。
1. 将  文件的内容替换为以下代码：

    ```typescript
        (function () {
          "use strict";
        
          // Call the initialize API first
          microsoftTeams.app.initialize().then(function () {
            microsoftTeams.app.getContext().then(function (context) {
              if (context?.app?.locale) {
                updateLocale(context.app.locale);
              }
              else{
                updateLocale("unknown");
              }
            });
          });
        
          function updateLocale(locale) {
            if(locale){
              document.getElementById("locale").innerHTML = locale;
            }
          }
        })();
    ```

    此代码使用**上下文**对象检索用户的区域设置，并更新 HTML 以显示区域设置代码。

## 检查你的工作

在调试模式下运行应用以测试功能。

1. 在 Visual Studio Code 中，选择“**Microsoft Teams**”图标以打开“**TEAMS 工具包**”面板。

2. 如果未在 Teams 工具包中登录到 Azure：在“**帐户**”部分中，选择“**登录到 Azure**”。 在打开的对话框中，选择“**登录**”按钮并输入 Microsoft 365 凭据。

   Teams 工具包需要具有全局管理员权限的 Microsoft 365 工作或学校帐户。

3. 使用以下方法之一开始使用附加的调试器运行应用：

   - 选择 F5 键。
   - 在 Visual Studio Code 中，选择“**运行** > ”“**开始调试**”。
   - 在 Teams 工具包的“**环境**”部分中，打开“*本地*”文件夹，然后选择所选的浏览器。

4. Visual Studio Code 执行一些检查后，可在“**控制台**”选项卡上查看操作，此时会打开新的浏览器窗口。 在“**UserInfoApp**”对话框中，选择“**添加**”按钮以在 Teams 中安装应用以预览。

应用现在可在边栏上查看。 应用预配置了两个选项卡：**个人选项卡**和**关于**。 验证区域设置代码是否显示在选项卡中。
