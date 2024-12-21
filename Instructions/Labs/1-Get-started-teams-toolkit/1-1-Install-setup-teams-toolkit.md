# 练习 1：为 Visual Studio Code 安装并设置 Teams Toolkit

在本练习中，你将为 Visual Studio Code 安装 Teams Toolkit 并设置你的环境。

## 任务 1：为 Visual Studio Code 安装 Teams Toolkit

1. 打开 **Visual Studio Code**。
2. 从边栏中选择“扩展”图标。****
3. 在“扩展”部分中使用搜索栏搜索“Teams Toolkit”。**** 然后选择“安装”。

![在 Visual Studio Code 上安装 Teams Toolkit 的屏幕截图。](../../media/teams-toolkit-install.png)

**备注**：本模块中的练习使用 Teams Toolkit v5.8.1。 请注意，次要版本可能有所不同。

还可以从 Visual Studio Marketplace 中安装 Teams Toolkit。[](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)

## 任务 2：准备你的 Microsoft 365 工作或学校帐户

如果你已对某个适用于开发和测试的 Microsoft 365 工作或学校帐户具有管理员访问权限，则可以使用该帐户来运行和调试应用。 务必使用一个可以安全地执行操作且不会影响实际用户的租户。

如果没有，可以使用 Microsoft 365 开发人员计划创建一个免费测试帐户。[](https://aka.ms/m365developers)  设置完成后，Microsoft 365 开发人员计划将为你提供对可用于构建 Teams 应用的租户的管理员访问权限。

## 任务 3：配置 Microsoft 365 租户以上传 Teams 的应用

按照以下步骤为租户启用自定义应用上传：

1. 使用 Microsoft 365 管理员凭据登录到 Microsoft Teams 管理中心。[](https://admin.teams.microsoft.com)****

2. 在边栏中，选择“Teams 应用”，然后选择“设置策略”。********

3. 选择“全局(组织级默认)策略”，然后打开“上传自定义应用”开关。********

   ![配置自定义应用上传的屏幕截图。](../../media/configure-upload-apps.png)

4. 选择**保存**按钮以保存更改。 你的租户现已配置为允许自定义应用旁加载。

在下一单元中，你将了解如何创建 Teams 应用并在 Teams 本地运行它。
