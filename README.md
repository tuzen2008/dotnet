# 使用 Visual Studio Code 创建 .NET 类库


在本教程中，将创建包含一个字符串处理方法的简单实用工具库。

类库定义的是可以由应用程序调用的类型和方法。 如果库以 .NET Standard 2.0 为目标，则支持 .NET Standard 2.0 的任何 .NET 实现（包括 .NET Framework）均可调用该库。 如果库以 .NET 5 为目标，则以 .NET 5 为目标的任何应用程序均可调用该库。 本教程演示如何以 .NET 5 为目标。

创建类库后，可将其作为第三方组件进行分发，也可将其作为与一个或多个应用程序捆绑在一起的组件进行分发。

## 先决条件

1. 已安装 [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) 的 [Visual Studio Code](https://code.visualstudio.com/)。 有关如何在 Visual Studio Code 上安装扩展的信息，请访问 [VS Code 扩展市场](https://code.visualstudio.com/docs/editor/extension-gallery)。
2. [.NET 5.0 SDK 或更高版本](https://dotnet.microsoft.com/download)

## 创建解决方案

首先，创建一个空白解决方案来放置类库项目。 解决方案用作一个或多个项目的容器。 将其他相关项目添加到同一个解决方案中。

1. 启动 Visual Studio Code。

2. 从主菜单中选择“文件” > “打开文件夹”（在 macOS 上为“打开...”）

3. 在“打开文件夹”对话框中，创建“ClassLibraryProjects”文件夹，然后单击“选择文件夹”（在 macOS 上为“打开”）。

4. 在主菜单中选择“视图” > “终端”，从 Visual Studio Code 中打开“终端” 。

   “终端”在“ClassLibraryProjects”文件夹中连同命令提示符一起打开。

5. 在“终端”中输入以下命令：

   .NET CLI复制

   ```dotnetcli
   dotnet new sln
   ```

   终端输出如以下示例所示：

   输出复制

   ```output
   The template "Solution File" was created successfully.
   ```

## 创建类库项目

将名为“StringLibrary”的新 .NET 类库项目添加到解决方案。

1. 在终端中，运行以下命令创建库项目：

   .NET CLI复制

   ```dotnetcli
   dotnet new classlib -o StringLibrary
   ```

   `-o` 或 `--output` 命令指定用于放置生成的输出的位置。

   终端输出如以下示例所示：

   输出复制

   ```output
   The template "Class library" was created successfully.
   Processing post-creation actions...
   Running 'dotnet restore' on StringLibrary\StringLibrary.csproj...
     Determining projects to restore...
     Restored C:\Projects\ClassLibraryProjects\StringLibrary\StringLibrary.csproj (in 328 ms).
   Restore succeeded.
   ```

2. 运行以下命令，向解决方案添加库项目：

   .NET CLI复制

   ```dotnetcli
   dotnet sln add StringLibrary/StringLibrary.csproj
   ```

   终端输出如以下示例所示：

   输出复制

   ```output
   Project `StringLibrary\StringLibrary.csproj` added to the solution.
   ```

3. 检查以确保该库以 .NET 5 为目标。 在资源管理器中，打开 StringLibrary/StringLibrary.csproj。

   `TargetFramework` 元素表明项目以 .NET 5.0 为目标。

   XML复制

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">
   
     <PropertyGroup>
       <TargetFramework>net5.0</TargetFramework>
     </PropertyGroup>
   
   </Project>
   ```

4. 打开 Class1.cs 并将代码替换为以下代码。

   C#复制

   ```csharp
   using System;
   
   namespace UtilityLibraries
   {
       public static class StringLibrary
       {
           public static bool StartsWithUpper(this string str)
           {
               if (string.IsNullOrWhiteSpace(str))
                   return false;
   
               char ch = str[0];
               return char.IsUpper(ch);
           }
       }
   }
   ```

   类库 `UtilityLibraries.StringLibrary`包含一个名为 `StartsWithUpper` 的方法。 此方法会返回 [Boolean](https://docs.microsoft.com/zh-cn/dotnet/api/system.boolean) 值，以指明当前字符串实例是否以大写字符开头。 Unicode 标准会区分大小写字符。 如果为大写字符，[Char.IsUpper(Char)](https://docs.microsoft.com/zh-cn/dotnet/api/system.char.isupper#System_Char_IsUpper_System_Char_) 方法返回 `true`。

   `StartsWithUpper` 以[扩展方法](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)的形式进行实现，这样就可以将其作为 [String](https://docs.microsoft.com/zh-cn/dotnet/api/system.string) 类成员进行调用。

5. 保存该文件。

6. 运行以下命令以生成解决方案，并验证项目是否正确编译。

   .NET CLI复制

   ```dotnetcli
   dotnet build
   ```

   终端输出如以下示例所示：

   输出复制

   ```output
   Microsoft (R) Build Engine version 16.7.0+b89cb5fde for .NET
   Copyright (C) Microsoft Corporation. All rights reserved.
     Determining projects to restore...
     All projects are up-to-date for restore.
     StringLibrary -> C:\Projects\ClassLibraryProjects\StringLibrary\bin\Debug\net5.0\StringLibrary.dll
   Build succeeded.
       0 Warning(s)
       0 Error(s)
   Time Elapsed 00:00:02.78
   ```

## 向解决方案添加控制台应用

添加使用类库的控制台应用程序。 应用将提示用户输入字符串，并报告字符串是否以大写字符开头。

1. 在终端中，运行以下命令创建控制台应用项目：

   .NET CLI复制

   ```dotnetcli
   dotnet new console -o ShowCase
   ```

   终端输出如以下示例所示：

   输出复制

   ```output
   The template "Console Application" was created successfully.
   Processing post-creation actions...
   Running 'dotnet restore' on ShowCase\ShowCase.csproj...  
     Determining projects to restore...
     Restored C:\Projects\ClassLibraryProjects\ShowCase\ShowCase.csproj (in 210 ms).
   Restore succeeded.
   ```

2. 运行以下命令，向解决方案添加控制台应用项目：

   .NET CLI复制

   ```dotnetcli
   dotnet sln add ShowCase/ShowCase.csproj
   ```

   终端输出如以下示例所示：

   输出复制

   ```output
   Project `ShowCase\ShowCase.csproj` added to the solution.
   ```

3. 打开 ShowCase/Program.cs 并将所有代码替换为以下代码。

   C#复制

   ```csharp
   using System;
   using UtilityLibraries;
   
   class Program
   {
       static void Main(string[] args)
       {
           int row = 0;
   
           do
           {
               if (row == 0 || row >= 25)
                   ResetConsole();
   
               string input = Console.ReadLine();
               if (string.IsNullOrEmpty(input)) break;
               Console.WriteLine($"Input: {input} {"Begins with uppercase? ",30}: " +
                                 $"{(input.StartsWithUpper() ? "Yes" : "No")}{Environment.NewLine}");
               row += 3;
           } while (true);
           return;
   
           // Declare a ResetConsole local method
           void ResetConsole()
           {
               if (row > 0)
               {
                   Console.WriteLine("Press any key to continue...");
                   Console.ReadKey();
               }
               Console.Clear();
               Console.WriteLine($"{Environment.NewLine}Press <Enter> only to exit; otherwise, enter a string and press <Enter>:{Environment.NewLine}");
               row = 3;
           }
       }
   }
   ```

   该代码使用 `row` 变量来维护写入到控制台窗口的数据行数计数。 如果大于或等于 25，该代码将清除控制台窗口，并向用户显示一条消息。

   该程序会提示用户输入字符串。 它会指明字符串是否以大写字符开头。 如果用户没有输入字符串就按 Enter 键，那么应用程序会终止，控制台窗口会关闭。

4. 保存更改。

## 添加项目引用

最初，新的控制台应用项目无权访问类库。 若要允许该项目调用类库中的方法，可以创建对类库项目的项目引用。

1. 运行下面的命令：

   .NET CLI复制

   ```dotnetcli
   dotnet add ShowCase/ShowCase.csproj reference StringLibrary/StringLibrary.csproj
   ```

   终端输出如以下示例所示：

   输出复制

   ```output
   Reference `..\StringLibrary\StringLibrary.csproj` added to the project.
   ```

## 运行应用

1. 在终端中运行以下命令：

   .NET CLI复制

   ```dotnetcli
   dotnet run --project ShowCase/ShowCase.csproj
   ```

2. 输入字符串并按 Enter 以试用程序，然后按 Enter 退出。

   终端输出如以下示例所示：

   输出复制

   ```output
   Press <Enter> only to exit; otherwise, enter a string and press <Enter>:
   
   A string that starts with an uppercase letter
   Input: A string that starts with an uppercase letter
   Begins with uppercase? : Yes
   
   a string that starts with a lowercase letter
   Input: a string that starts with a lowercase letter
   Begins with uppercase? : No
   ```
## 发布应用
   运行下面的命令：

   ```dotnetcli
   dotnet publish --configuration Release
   ```

   默认生成配置为“调试”，因此此命令指定“版本”生成配置 。 版本生成配置的输出进行了完全优化，且具有最低限度的符号调试信息。

   该命令的输出类似于以下示例：


   ```output
   Microsoft (R) Build Engine version 16.7.0+b89cb5fde for .NET
   Copyright (C) Microsoft Corporation. All rights reserved.
     Determining projects to restore...
     All projects are up-to-date for restore.
     HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\HelloWorld.dll
     HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\publish\
   ```
    输出目录：ClassLibraryProjects\ShowCase\bin\Release\net5.0\publish

## 创建单元测试项目

单元测试在开发和发布期间提供自动化的软件测试。 本教程中使用的测试框架是 MSTest。 [MSTest](https://github.com/Microsoft/testfx-docs) 是可供选择的三个测试框架之一。 其他两个是 [xUnit](https://xunit.net/) 和 [nUnit](https://nunit.org/)。

1. 启动 Visual Studio Code。

2. 打开在[使用 Visual Studio Code 创建 .NET 类库](https://docs.microsoft.com/zh-cn/dotnet/core/tutorials/library-with-visual-studio-code)中创建的 `ClassLibraryProjects` 解决方案。

3. 创建名为“StringLibraryTest”的单元测试项目。

   .NET CLI复制

   ```dotnetcli
   dotnet new mstest -o StringLibraryTest
   ```

   项目模板创建包含以下代码的 UnitTest1.cs 文件：

   C#复制

   ```csharp
   using Microsoft.VisualStudio.TestTools.UnitTesting;
   
   namespace StringLibraryTest
   {
       [TestClass]
       public class UnitTest1
       {
           [TestMethod]
           public void TestMethod1()
           {
           }
       }
   }
   ```

   单元测试模板创建的源代码负责执行以下操作：

   - 它会导入 [Microsoft.VisualStudio.TestTools.UnitTesting](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting) 命名空间，其中包含用于单元测试的类型。
   - 向 `UnitTest1` 类应用 [TestClassAttribute](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.testclassattribute) 特性。
   - 它应用 [TestMethodAttribute](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.testmethodattribute) 特性来定义 `TestMethod1`。

   使用 [[TestClass\]](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.testclassattribute) 标记的测试类中标记有 [[TestMethod\]](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.testmethodattribute) 的所有测试方法都会在单元测试运行时自动执行。

4. 向解决方案添加测试项目。

   .NET CLI复制

   ```dotnetcli
   dotnet sln add StringLibraryTest/StringLibraryTest.csproj
   ```

## 添加项目引用

对于要使用 `StringLibrary` 类的测试库，请在 `StringLibraryTest` 项目中添加对 `StringLibrary` 项目的引用。

1. 运行下面的命令：

   .NET CLI复制

   ```dotnetcli
   dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj
   ```

## 添加并运行单元测试方法

运行单元测试时，Visual Studio 执行使用 [TestClassAttribute](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.testclassattribute) 特性标记的类中标记有 [TestMethodAttribute](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.testmethodattribute) 特性的所有方法。 当第一次遇到测试不通过或测试方法中的所有测试均已成功通过时，测试方法终止。

最常见的测试调用 [Assert](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.assert) 类的成员。 许多断言方法至少包含两个参数，其中一个是预期的测试结果，另一个是实际的测试结果。 下表显示了 `Assert` 类最常调用的一些方法：

| 断言方法           | 函数                                                         |
| :----------------- | :----------------------------------------------------------- |
| `Assert.AreEqual`  | 验证两个值或对象是否相等。 如果值或对象不相等，则断言失败。  |
| `Assert.AreSame`   | 验证两个对象变量引用的是否是同一个对象。 如果这些变量引用不同的对象，则断言失败。 |
| `Assert.IsFalse`   | 验证条件是否为 `false`。 如果条件为 `true`，则断言失败。     |
| `Assert.IsNotNull` | 验证对象是否不为 `null`。 如果对象为 `null`，则断言失败。    |

还可以在测试方法中使用 [Assert.ThrowsException](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.assert.throwsexception) 方法来指示它应引发的异常的类型。 如果未引发指定异常，则测试不通过。

测试 `StringLibrary.StartsWithUpper` 方法时，需要提供许多以大写字符开头的字符串。 在这种情况下，此方法应返回 `true`，以便可以调用 [Assert.IsTrue](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.assert.istrue) 方法。 同样，需要提供许多以非大写字符开头的字符串。 在这种情况下，此方法应返回 `false`，以便可以调用 [Assert.IsFalse](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.assert.isfalse) 方法。

由于库方法可以处理字符串，因此还需要确保它能够成功处理[空字符串 (`String.Empty`)](https://docs.microsoft.com/zh-cn/dotnet/api/system.string.empty) 和 `null` 字符串。 空字符串不包含任何字符，且 [Length](https://docs.microsoft.com/zh-cn/dotnet/api/system.string.length#System_String_Length) 为 0。 `null` 字符串是尚未初始化的字符串。 可以直接将 `StartsWithUpper` 作为静态方法进行调用，并向其传递一个 [String](https://docs.microsoft.com/zh-cn/dotnet/api/system.string) 自变量。 或者，可以对分配给 `null` 的 `string` 变量将 `StartsWithUpper` 作为扩展方法进行调用。

将定义三个方法，每个方法都会对字符串数组中的各个元素调用它的 [Assert](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.visualstudio.testtools.unittesting.assert) 方法。 你将调用方法重载，以便指定在测试失败时要显示的错误消息。 消息标识导致失败的字符串。

创建测试方法：

1. 打开 StringLibraryTest/UnitTest1.cs 并将所有代码替换为以下代码。

   C#复制

   ```csharp
   using System;
   using Microsoft.VisualStudio.TestTools.UnitTesting;
   using UtilityLibraries;
   
   namespace StringLibraryTest
   {
       [TestClass]
       public class UnitTest1
       {
           [TestMethod]
           public void TestStartsWithUpper()
           {
               // Tests that we expect to return true.
               string[] words = { "Alphabet", "Zebra", "ABC", "Αθήνα", "Москва" };
               foreach (var word in words)
               {
                   bool result = word.StartsWithUpper();
                   Assert.IsTrue(result,
                          String.Format("Expected for '{0}': true; Actual: {1}",
                                        word, result));
               }
           }
   
           [TestMethod]
           public void TestDoesNotStartWithUpper()
           {
               // Tests that we expect to return false.
               string[] words = { "alphabet", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                                  "1234", ".", ";", " " };
               foreach (var word in words)
               {
                   bool result = word.StartsWithUpper();
                   Assert.IsFalse(result,
                          String.Format("Expected for '{0}': false; Actual: {1}",
                                        word, result));
               }
           }
   
           [TestMethod]
           public void DirectCallWithNullOrEmpty()
           {
               // Tests that we expect to return false.
               string[] words = { string.Empty, null };
               foreach (var word in words)
               {
                   bool result = StringLibrary.StartsWithUpper(word);
                   Assert.IsFalse(result,
                          String.Format("Expected for '{0}': false; Actual: {1}",
                                        word == null ? "<null>" : word, result));
               }
           }
       }
   }
   ```

   `TestStartsWithUpper` 方法中的大写字符的测试包括希腊文大写字母 alpha (U+0391) 和西里尔文大写字母 EM (U+041C)。 `TestDoesNotStartWithUpper` 方法中的小写字符的测试包括希腊文小写字母 alpha (U+03B1) 和西里尔文小写字母 Ghe (U+0433)。

2. 保存更改。

3. 运行测试：

   .NET CLI复制

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   终端输出显示所有测试都已通过。

   输出复制

   ```output
   Starting test execution, please wait...
   A total of 1 test files matched the specified pattern.
   
   Passed!  - Failed:     0, Passed:     3, Skipped:     0, Total:     3, Duration: 3 ms - StringLibraryTest.dll (net5.0)
   ```

## 处理测试失败

如果进行的是测试驱动开发 (TDD)，请先编写测试，然后测试会在第一次运行时失败。 接着将可以使测试成功的代码添加到应用。 在本教程中，先编写了测试要验证的应用代码然后才创建测试，所以没有看到测试失败。 若要验证测试是否在预期失败时失败，请在测试输入中添加无效值。

1. 通过修改 `TestDoesNotStartWithUpper` 方法中的 `words` 数组来包含字符串“Error”。

   C#复制

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

2. 运行测试：

   .NET CLI复制

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   终端输出显示一个测试失败，并提供关于失败测试的错误消息：“Assert.IsFalse 失败。 “Error”应返回 false；实际返回 True”。 由于此次失败，数组中“Error”之后的所有字符串都未进行测试。

   输出复制

   ```output
   Starting test execution, please wait...
   A total of 1 test files matched the specified pattern.
     Failed TestDoesNotStartWithUpper [28 ms]
     Error Message:
      Assert.IsFalse failed. Expected for 'Error': false; Actual: True
     Stack Trace:
        at StringLibraryTest.UnitTest1.TestDoesNotStartWithUpper() in C:\ClassLibraryProjects\StringLibraryTest\UnitTest1.cs:line 33
   
   Failed!  - Failed:     1, Passed:     2, Skipped:     0, Total:     3, Duration: 31 ms - StringLibraryTest.dll (net5.0)
   ```

3. 删除在步骤 1 中添加的字符串“Error”。 重新运行测试，测试将通过。

## 测试库的发行版本

至此，在运行库的调试版本时，测试已全部通过，接下来应对库的发行版本再运行一次这些测试。 许多因素（包括编译器优化）有时可能会导致调试版本和发行版本出现行为差异。

1. 使用版本生成配置运行测试：

   .NET CLI复制

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj --configuration Release
   ```

   测试通过。

## 调试测试

如果使用 Visual Studio 作为 IDE，则可以使用与[使用 Visual Studio Code 调试 .NET Core 控制台应用程序](https://docs.microsoft.com/zh-cn/dotnet/core/tutorials/debugging-with-visual-studio-code)中所示的相同过程，来通过使用单元测试项目调试代码。 打开 StringLibraryTest/UnitTest1.cs，然后在第 7 行和第 8 行之间选择“调试所有测试”，而不是启动 ShowCase 应用项目。 如果找不到该位置，请按下 Ctrl+Shift+P 打开命令面板，然后输入“重载窗口”。

Visual Studio Code 启动附有调试器的测试项目。 执行将在添加到测试项目的任何断点或基础库代码处停止。
