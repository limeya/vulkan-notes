
# vulkan执行中涉及的几个组件
## Device
为了渲染图形，计算机中必须有显卡，或者独立显卡或者集成显卡，这些都是`PhysicalDeivce`，而在vulkan的术语环境下，这个`PhysicalDeivce`指的是支持vulkan的具体的物理设备。

同样地，为了在各种应用中表示该`PhysicalDevice`，vulkan中引入`Device`的概念，它是一个抽象的logical representation。

对于在应用中使用时，对于vulkan兼容的`PhysicalDeivce`（指的是通过vulkan的API可以操作该设备），我们要find；对于`Device`，我们要create。

## Queue

`Queue`在数据结构中是先入先出的特性。根据这一点，在很多的消息队列中广泛使用，比如：生产者-消费者模型，尤其是多线程的环境下。如果你了解这一点，对于vulkan中`Queue`发挥的作用，就不会陌生了。

在vulkan中，`Queue`是execution engine和application之间的interface：
- application是command的生产者；
- execution engine是command的消费者；

具体来说，`Queue`的责任在于收集所有来自应用的commands,application将command提交到`Queue`，然后execution engine取走调度执行。

一个`PhysicalDevice`包含很多的`Queue`，每个`Queue`可能有不同的功能，这里的功能不同，我理解是：特定的`Queue`只能接受特定的command，比如：内存操作、并行计算等。

### Family
按照功能不同，这些`Queue`并划分到`Family`，这样一组`Queue`内形成的`Family`内，支持多种功能的`Queue`：
- video decode；
- video encode；
- graphics（重点使用）；
- compute（并行计算，深度学习模型等）；
- transfer；
- sparse memory management；

# vulkan执行模型
