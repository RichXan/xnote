wire是 Google 开源的一个依赖注入工具。它是一个代码生成器，并不是一个框架。我们只需要在一个特殊的`go`文件中告诉`wire`类型之间的依赖关系，它会自动帮我们生成代码，帮助我们创建指定类型的对象，并组装它的依赖。

`wire`的要求很简单，新建一个`wire.go`文件（文件名可以随意），创建我们的初始化函数。比如，我们要创建并初始化一个`Mission`对象，我们就可以这样：
```go
type Mission struct {
  Player  Player
  Monster Monster
}

func NewMission(p Player, m Monster) Mission {
  return Mission{p, m}
}

func (m Mission) Start() {
  fmt.Printf("%s defeats %s, world peace!\n", m.Player.Name, m.Monster.Name)
}

//+build wireinject
package main

import "github.com/google/wire"

func InitMission(name string) Mission {
  wire.Build(NewMonster, NewPlayer, NewMission)
  return Mission{}
}
```
首先这个函数的返回值就是我们需要创建的对象类型，`wire`只需要知道类型，`return`后返回什么不重要。然后在函数中，我们调用`wire.Build()`将创建`Mission`所依赖的类型的构造器传进去。例如，需要调用`NewMission()`创建`Mission`类型，`NewMission()`接受两个参数一个`Monster`类型，一个`Player`类型。`Monster`类型对象需要调用`NewMonster()`创建，`Player`类型对象需要调用`NewPlayer()`创建。所以`NewMonster()`和`NewPlayer()`我们也需要传给`wire`。


## 基础概念

`wire`有两个基础概念，`Provider`（构造器）和`Injector`（注入器）。
`Provider`实际上就是创建函数，大家意会一下。我们上面`InitMission`就是`Injector`。每个注入器实际上就是一个对象的创建和初始化函数。在这个函数中，我们只需要告诉`wire`要创建什么类型的对象，这个类型的依赖，`wire`工具会为我们生成一个函数完成对象的创建和初始化工作.

实际上，`wire`在生成代码时，构造器需要的参数（或者叫依赖）会从参数中查找或通过其它构造器生成。决定选择哪个参数或构造器完全根据类型。如果参数或构造器生成的对象有类型相同的情况，运行`wire`工具时会报错。如果我们想要定制创建行为，就需要为不同类型创建不同的参数结构：