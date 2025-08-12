Readkidz API

# Readkidz Open API （0.1.0）

> API Host: https://xxx.com
- 说明：以下接口示例使用 JavaScript 语法
- 请求头（Default）:
    - 'Content-Type': 'application/json'
    - 'Authorization': 'Bearer {api-key}'


## 图像克隆

> Readkidz基于用户需求，构建的克隆功能，参考用户上传图像以及用户选择   的参考风格，最终克隆出AI数字形象。

### 1.风格列表
- 接口: /v1/image/clone_styles
- 请求方式：GET
- 请求头:
    - 'Content-Type': 'application/json'
    - 'Authorization': 'Bearer {api-key}'
- Url query 参数：
    - pageSize: 20  //每页数量  默认：20  最大值:200
    - page: 1  //当前页号  默认：1
- 响应体：
    ```json
    {
    	"code": 200,
    	"success": true,
    	"data": {
    	    count:99, //总数据行数
    	    row:[{
                    "id":"xxxx",             // 风格ID
                    "nameEn":"Crayon Style", // 风格英文名
                    "nameCn":"蜡笔风格",      // 中文名
        	        "exampleUrls":["http://xxx.com/1.png",...], // 风示例图（一张或多张）
        	        "cloneType":["humans","animals","landscapes","objects"] // 支持克隆的目标对象类型：humans:人类,animals:动物,landscapes:风景,objects:物品
        	    },
        	    ...
        	]
    	},
    	"msg": "success"
    }
    ```
    >  说明：code值非200时，有对应的msg说明
- 请求失败： 参考消息体msg说明

### 2.克隆图像
- 接口: /v1/image/clone
- 请求方式：POST
- 请求头:
    - 'Content-Type': 'application/json'
    - 'Authorization': 'Bearer {api-key}'
- 请求体：
    ```json
    {
        "styleId":"xxxx",                        //必填，风格ID 
    	"referImageUrl": "http://xxx.com/1.png", //必填；参考图像url
    	"aspectRatio": "1:1",                    //生成图片的比例 默认1:1  取值[1:1,2:1,4:3,16:9]
    	"callbackUrl": "http://xxx.com/callback-url"  //回调接口，业务方接收任务完成数据（也可以通过查询任务信息接口获取结果），
    }
    ```
- 响应体：
    ```json
    {
    	"code": 200,
    	"success": true,
    	"data": {
    	    "jobId":"xxx"  //任务ID，可用于追溯任务结果
    	},
    	"msg": "success"
    }
    ```
    > 说明：code值非200时，有对应的msg说明
- 请求失败： 参考消息体msg说明
- 示例代码（JavaScript）
    ```javascript
    async function getCloneTask() {
      const postData = JSON.stringify({
        styleId: "xxx",
        referImageUrl: "http://xxx.com/1.png",
        callbackUrl: "http://xxx.com/job-result",
      });
      const data = await axios.post("{host}/v1/image/clone", postData, {
        headers: {
          "Content-Type": "application/json",
          Authorization: "{apiKey}",
        },
      });
      console.log(data);
    }
    
    getCloneTask();
    ```

### 3.查询任务信息  

>*说明：仅限查询提交任务成功后24小时内的任务，所以请求任务后请尽快取回*<br /> <span style="color:red;">*注意：图片地址可能会失效，建议转存*</span>

- 接口: /v1/image/clone/{jobId}
- 请求方式：GET
- 请求头:
    - 'Content-Type': 'application/json'
    - 'Authorization': 'Bearer {api-key}'
- 响应体：
    ```json
    {
    	"code": 200,
    	"success": true,
    	"data": {
    		"jobId": "xxx",               //任务ID
    		"finishTime": 1751429190000,  //任务完成时间戳
    		"imageUrls": ["http://xxx.com/1.png", ...], //多张图的url 正常为4张图，**注意：图片地址可能会失效，建议转存**
    		"status": "string",          //任务状态 任务状态取值  任务状态 ( CREATED：创建,SUBMITTED：任务提交,PREPARING 准备中, IN_PROGRESS：进行中, SUCCESS：完成, FAILURE：失败 ) 
    		"failReason": "string",      //失败信息
    		"progress": 50.2,            //图片进度（预测值，不一定准确，判断完成用任务状态值）
    	},
    	"msg": "success"
    }
    ```
    >  说明：code值非200时，有对应的msg说明
- 请求失败： 参考消息体msg说明

### 4. 克隆场景
- 接口: /v1/image/clone/scene
- 请求方式：POST
- 请求头:
    - 'Content-Type': 'application/json'
    - 'Authorization': 'Bearer {api-key}'
- 请求体：
    ```json
    {
        "styleId":"xxxx",                        //必填，风格ID 
    	"referImageUrl": "http://xxx.com/1.png", //必填；参考图像url
    	"prompt": "xxx",                         //必填；场景提示词文本
    	"ow":100,                                //图片参考权重 取值范围内1 - 1000   默认 100
    	"aspectRatio": "1:1",                    //生成图片的比例 默认1:1  取值[1:1,2:1,4:3,16:9]
    	"callbackUrl": "http://xxx.com/callback-url"  //回调接口，业务方接收任务完成数据（也可以通过查询任务信息接口获取结果），
    }
    ```
- 响应体：
    ```json
    {
    	"code": 200,
    	"success": true,
    	"data": {
    	    "jobId":"xxx"  //任务ID，可用于追溯任务结果
    	},
    	"msg": "success"
    }
    ```
    > 说明：code值非200时，有对应的msg说明
- 请求失败： 参考消息体msg说明
