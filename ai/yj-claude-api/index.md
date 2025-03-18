## 云加Claude AI Chat API


> api host: http://ai-chat-control.yh-s4.icepoint.io

### 1. AI对话接口: /claude/chat-process

- 请求方式：POST
- Body体参考：
  ```json
  {
    model: "claude-3-5-sonnet-20241022",
    messages: [
      {
        role: "user",
        content: `你好,你可以做什么？`,
      },
      {
        role: "assistant",
        content: `你好！\n\n作为一名高级程序员助手，我可以帮你完成多种编程相关的任务，....`,
      },
      ......  
    ],
    system: "你是一个高级程序员，可以开发java、python、nodejs等多种语言的程序。",
  }
  ```
  > 说明：发送单条对话时只需要发送  role 为 "user"的消息体,<br /> 需要关联上下文时，把历史对话消息都带入到messages列表中，列表最后的一个user消息体为最新的提问

- 示例代码（JavaScript）

  ```
  axios
    .post("http://xxx/claude/chat-process", {
      model: "claude-3-5-sonnet-20241022",
      messages: [
        {
          role: "user",
          content: `你好,你可以做什么？`,
        },
      ],
      system:
        "你是一个高级程序员，可以开发java、python、nodejs等多种语言的程序。",
    })
    .then((response) => console.log(`Response Data: ${response.data}`));
  ```
  
- 响应示例:
  ```json
  {
    "content": [{
    	"type": "text",
    	"text": "#你好！\n\n作为一名高级程序员助手，我可以帮你完成多种编程相关的任务..."
    }],
    "id": "msg_01LPjgC3rz29oBQLDiLM94XP",
    "model": "claude-3-5-sonnet-20241022",
    "role": "assistant",
    "stop_reason": "end_turn",
    "stop_sequence": null,
    "type": "message",
    "usage": {
    	"input_tokens": 46,
    	"output_tokens": 232,
    	"cache_creation_input_tokens": 0,
    	"cache_read_input_tokens": 0
    }
  }
  ```
  
### 2. AI对话接口-流式输出: /claude/chat-stream

- 请求方式：POST
- Body体参考：(同上)
- 示例代码（JavaScript）

  ```
   axios
    .post(
      "http://xxx/claude/chat-stream",
      {
        model: "claude-3-5-sonnet-20241022",
        messages: [
          {
            role: "user",
            content: `你好,你可以做什么？`,
          },
        ],
        system:
          "你是一个高级程序员，可以开发java、python、nodejs等多种语言的程序。",
      },
      {
        responseType: "stream",
      }
    )
    .then((response) => {
      response.data.on("data", (chunk) => {
        console.log(`Chunk received: ${chunk}`);
      });

      response.data.on("end", () => {
        console.log("Response complete.");
      });

      response.data.on("error", (error) => {
        console.error(`Stream error: ${error}`);
      });
    });
  ```


- 响应示例: 流式输出text内容
