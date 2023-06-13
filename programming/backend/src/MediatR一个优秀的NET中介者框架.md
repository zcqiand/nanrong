# MediatR一个优秀的 .NET 中介者框架
## 什么是MediatR？
MediatR是一个在C#/.NET开发中广泛使用的中介者模式库。它提供了一种简单而强大的方式来实现应用程序中的消息和请求处理。通过将消息的发布者与消息的处理者解耦，MediatR可以使代码更加清晰、可维护和可扩展。

## 为什么选择MediatR？
选择MediatR的原因有很多。首先，MediatR促进了应用程序的解耦。它允许开发人员将业务逻辑分解为小而独立的处理器，这些处理器专注于处理特定类型的消息。这种解耦有助于提高代码的可读性和可测试性，同时也使得代码更容易进行扩展和修改。

其次，MediatR提供了一个简洁的API来管理消息的发布和处理。通过使用MediatR，您可以轻松地定义和发送消息，而无需显式地编写大量的代码来协调消息和处理程序之间的交互。这样可以减少样板代码，使开发过程更加高效。

另外，MediatR还支持管道和中间件机制，可以在消息处理过程中插入自定义的逻辑。这使得您可以在处理消息之前或之后执行其他操作，例如身份验证、缓存、异常处理等。这种灵活性可以根据应用程序的需求来扩展和自定义消息处理过程。

## 如何使用MediatR？
下面是使用MediatR的一般步骤：

### 步骤 1: 安装 MediatR NuGet 包
在您的项目中，通过NuGet包管理器或命令行安装MediatR包。您需要安装两个主要的包：MediatR和MediatR.Extensions.Microsoft.DependencyInjection。

### 步骤 2: 定义消息和处理器
首先，定义您的消息类。消息类是一个简单的POCO类，用于在应用程序中传递数据。例如，假设我们有一个发送电子邮件的功能，可以定义一个名为"SendEmailCommand"的消息类，它包含发送电子邮件所需的所有信息。

```
public class SendEmailCommand : IRequest
{
    public string Recipient { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

接下来，定义一个处理器来处理该消息。处理器是一个实现了IRequestHandler<TRequest\>接口的类，其中TRequest是您定义的消息类型。在这个例子中，我们创建一个名为SendEmailCommandHandler的处理器类，它实现了IRequestHandler<SendEmailCommand\>接口。

```
public class SendEmailCommandHandler : IRequestHandler<SendEmailCommand>
{
    public Task<Unit> Handle(SendEmailCommand request, CancellationToken cancellationToken)
    {
        // 处理发送电子邮件的逻辑
        // ...
        return Task.FromResult(Unit.Value);
    }
}
```

### 步骤 3: 配置依赖注入
打开您的Startup.cs文件，将MediatR服务添加到依赖注入容器中。

```
public void ConfigureServices(IServiceCollection services)
{
    // 添加MediatR服务
    services.AddMediatR(typeof(Startup));
    
    // 其他服务配置...
}
```

### 步骤 4: 发布和处理消息
现在，您可以在应用程序中的任何地方发布和处理消息。假设您在一个控制器中处理发送电子邮件的请求，可以像这样发布消息：

```
public class EmailController : ControllerBase
{
    private readonly IMediator _mediator;

    public EmailController(IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpPost("send")]
    public async Task<IActionResult> SendEmail(SendEmailCommand command)
    {
        await _mediator.Send(command);
        return Ok("Email sent successfully!");
    }
}
```

在这个例子中，当调用_mediator.Send(command)时，MediatR会根据消息类型找到相应的处理器，并将消息传递给处理器的Handle方法进行处理。

以上是使用MediatR的基本步骤和示例代码。当使用MediatR时，一些常见的坑包括：

1. 忘记在依赖注入容器中配置MediatR服务。
2. 消息处理器的命名错误或不一致，导致无法正确匹配处理器。
3. 消息处理器的逻辑错误，导致处理失败或产生意外结果。
4. 没有正确处理异常情况，例如在处理器中捕获和处理异常。
5. 使用MediatR可以极大地简化应用程序中的消息和请求处理，并提高代码的可读性和可维护性。它的强大功能和灵活性使得它成为许多C#开发人员的首选库之一。

希望这篇文章对您有所帮助，祝您在使用MediatR时取得成功！