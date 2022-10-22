# 进一步发展的准则

由于这个项目的开发时间仓促，很多地方没有很好的评论解释这个代码在做什么。这篇文章作为一个补丁，可以帮助那些愿意做进一步开发的人理解这个项目的逻辑。

## 基本架构

虚拟目录结构是一个树状结构，由`generate.py`在谁扫描文件目录后生成。每当`/desktop`路由被触发时，它将在DOM被渲染之前被首先请求。它由vuex的一个全局状态管理（`{{$store.state.filemap}}）。基本上，在我们的需求中，每次打开后它都会保持不变，尽管在设计上它可以被修改。

在这个项目中，大部分操作都是通过遍历完成的，这显然对效率不友好，这是因为在最初设计树状结构时，是按照后端的返回值设计的，没有考虑到前端的进一步控制需求。但好在GUI通常不会占用巨大的计算资源，JS引擎在小数据量（这里的小可能指的是几万个，远高于日常使用的可能）下速度足够快。

屏幕中的大部分左键操作都留给了全局状态管理器，但并没有在组件本身中进行内部消化。这是因为大多数点击都伴随着许多组件的协同动作。全局管理器通常需要处理以下部分的逻辑。
- 关于组件本身的逻辑，比如最小化，或者关闭这个窗口。这与我们的虚拟-虚拟-数字对象管理有关。
- 处理全局窗口排序，按照点击的顺序，应该保持一个队列，以确保哪些在上面，哪些在下面。
- 处理焦点问题，最近被左键选择的窗口应该被清晰地突出显示，而其他窗口、图标等应该被清晰地模糊。
- 处理右键上下文菜单，它有自己的一些逻辑，一般来说，上下文菜单在你点击不同的位置时总是会消失。

窗口自身的调整大小&移动事件由自己管理，因为它们通常不会与其他逻辑冲突。

例外的是，窗口的关闭动画是由自己处理的，这是一个设计上的错误，因为关闭事件可能来自于窗口本身之外，所以最初没有考虑。

### 就这样了

以上几乎是全部的逻辑。因为我们不是在写一个真正的GUI，所以大部分的逻辑都保持简单。如果你想做类似的开发，希望这个项目能减少你的工作量。基本上，如果你只是想在这个框架的基础上添加你自己的窗口，那么你唯一需要做的就是创建新的`WindowChildren`组件并设计它的触发按钮。而如果你希望改变整个主题，你基本上需要重新设计任务栏组件（包括一些按钮和一些窗口样式），毕竟桌面背景总是容易改变的。

感谢您的观看。