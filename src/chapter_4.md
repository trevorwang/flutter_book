## 状态管理和Riverpod

在前面的代码中我们完成了一个聊天界面，但是用户在输入消息之后并没有更新的界面上。其根本是我们的UI 没有知道messages更新，因此我们需要想办法让UI知道我们有数据更新了。这里就需要状态管理了。

### 什么是状态管理

状态管理是一个关键的概念，用于处理应用程序中数据的流动和更新。在Flutter应用程序中，状态管理是确保应用程序UI和数据保持同步的方法。状态管理的主要目的是在应用程序的不同部分共享和同步数据，同时提供良好的代码结构和可维护性。

在Flutter中，有多种状态管理方法可供选择。以下是一些常见的状态管理方法：

1. **StatefulWidget**：这是Flutter中最基本的状态管理方法。有状态的组件可以存储自己的状态，并在需要时更新。这种方法适用于简单的场景，例如独立的UI元素或局部状态。
2. **InheritedWidget**和**InheritedModel**：这些是允许状态在组件树中向下传递的特殊类型的组件。它们可以帮助您在应用程序的不同层级之间共享状态。这种方法对于较小的应用程序或有限的状态共享需求较为合适。
3. **Provider：Provider**是一个依赖注入和状态管理库，它允许您在组件树中向下传递数据和状态。Provider可以监听状态变化，并在需要时重新构建关联的组件。这种方法适用于各种规模的应用程序，具有良好的可扩展性和灵活性。
4. **Riverpod：Riverpod**是一个相对较新的状态管理库，它类似于Provider，但提供了更多的功能和改进。Riverpod允许您创建不可变的、可组合的和可测试的状态管理解决方案。这种方法适用于需要更高度可控和可测试性的应用程序。
5. **BLoC（Business Logic Component）**：BLoC是一种基于响应式编程的状态管理方法。BLoC将业务逻辑与UI分离，使您可以轻松地测试和重用代码。BLoC通常与RxDart（一种Dart的响应式编程库）一起使用，以提供强大的数据流处理能力。这种方法适用于需要处理复杂业务逻辑和大量数据流的应用程序。
6. **Redux：Redux**是一种集中式状态管理库，它将应用程序的状态存储在一个单一的状态树中。Redux使用纯函数（称为reducers）来处理状态更新，使您可以轻松地跟踪和管理应用程序的状态变化。这种方法适用于需要严格的状态管理和可预测性的应用程序。

选择合适的状态管理方法取决于您的应用程序的需求、复杂性和个人喜好。不同的方法有不同的优缺点，因此在选择状态管理方法时，请务必充分了解每种方法的特点，并权衡其适用性。

在本章节中，我们选择了**Riverpod** 作为状态管理的解决方案

### [Riverpod]([New Tab (riverpod.dev)](https://riverpod.dev/))

Riverpod是一个状态管理库，用于构建Flutter应用程序。它是Provider库的改进版，由同一作者开发。Riverpod解决了Provider的一些局限性，并引入了更多功能和改进。以下是Riverpod的一些主要特点：

1. **不可变性**：Riverpod中的状态是不可变的，这意味着状态在更新时会创建一个新的对象，而不是修改现有对象。这有助于减少错误，并使状态更易于理解和跟踪。
2. **类型安全**：Riverpod在编译时提供了更强的类型安全性，有助于减少类型错误并提高代码质量。
3. **无需BuildContext**：与Provider不同，Riverpod不依赖于BuildContext来访问状态。这使得在组件之外的位置（如函数或类）访问状态变得更加容易，同时提高了可测试性。
4. **可组合**：Riverpod允许您组合不同的Provider以创建更复杂的状态管理解决方案。这有助于保持代码的模块化和可维护性。
5. **易于测试**：由于Riverpod的状态不依赖于BuildContext，您可以更轻松地编写单元测试。此外，Riverpod提供了用于模拟状态和测试的实用工具。
6. **家族功能**：Riverpod具有所谓的“家族”功能，允许您根据参数创建多个相同类型的Provider实例。这使得在使用相同逻辑但参数不同的多个组件时可以更好地管理状态。
7. **非常灵活**：Riverpod具有很高的灵活性，可以很好地适应不同的应用程序结构和需求。您可以使用Riverpod来构建简单的局部状态管理，或者构建复杂的全局状态管理解决方案。

总之，Riverpod是一个强大的状态管理库，适用于各种规模的Flutter应用程序。它提供了不可变性、类型安全性、无需BuildContext的访问、可组合性、易于测试和家族功能等多种优点。如果您正在寻找一个现代、灵活且易于使用的状态管理解决方案，Riverpod是一个值得考虑的选择。[更多相关的内容直接参考相关文章]([Flutter Riverpod 全面深入解析，为什么官方推荐它？ - 掘金 (juejin.cn)](https://juejin.cn/post/7063111063427874847))

### 使用 Riverpod来改造我们的程序

1. `pubspec.yaml`中添加以下依赖，然后执行`flutter pub get`

   ```yaml
   dependencies:
     # 。。。
   	flutter_hooks: ^0.18.0
     hooks_riverpod: ^2.3.2
   ```

2. `main.dart` 中添加 `ProviderScope`

   ```dart
   import 'package:hooks_riverpod/hooks_riverpod.dart';
   
   void main() {
     runApp(const ProviderScope(child: MyApp()));
   }
   
   ```

   

3. 修改 `ChatScreen` ，使其继承自`HookConsumerWidget`,  `build`函数需要对应修改

   ```dart
   import 'package:hooks_riverpod/hooks_riverpod.dart';
   
   class ChatScreen extends HookConsumerWidget {
     ChatScreen({super.key});
   // ....
     @override
     Widget build(BuildContext context, WidgetRef ref) {
   ```

4. 新建`states` 文件夹，然后新建`message_state.dart`, 来管理 message

   ```dart
   import 'package:hooks_riverpod/hooks_riverpod.dart';
   
   import '../models/message.dart';
   
   class MessageList extends StateNotifier<List<Message>> {
     MessageList() : super([]);
   
     void addMessage(Message message) {
       state = [...state, message];
     }
   }
   
   final messageProvider = StateNotifierProvider<MessageList, List<Message>>(
     (ref) => MessageList(),
   );
   
   ```

5. 修改 `ChatScreen`使用`messageProvider` 来管理数据

   ```dart
     Widget build(BuildContext context, WidgetRef ref) {
       final messages = ref.watch(messageProvider);  // 获取数据
       return Scaffold(
         //....
         
   ```

   ```dart
     // 增加WidgetRef
     _sendMessage(WidgetRef ref, String content) {
       final message =
           Message(content: content, isUser: true, timestamp: DateTime.now());
       ref.read(messageProvider.notifier).addMessage(message); // 添加消息
       _textController.clear();
     }
   ```

   ```dart
                         if (_textController.text.isNotEmpty) {
                           _sendMessage(ref, _textController.text); // 增加 ref 参数
                         }
   ```

6. 运行后，可以查看下效果。输入消息后，点击发送，可以直接更新到消息列表了。

   ![Screenshot 2023-04-04 at 13.19.19](assets/Screenshot2023-04-04%20at13.19.19.png)
   到此我们已经完成了一个基本的聊天界面，接下来的章节我们将会对接 ChatGPT API了。