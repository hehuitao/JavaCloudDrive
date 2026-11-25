

\# 公共字段与协作规范



\## 一、基础约定



\- 编程语言：Java。

\- Java 字段采用小驼峰命名，如 userId。

\- 数据库字段采用下划线命名，如 user\_id。

\- ID 统一使用 Long 类型。

\- 文件大小统一使用 long 类型，单位为字节。

\- 时间统一使用 LocalDateTime，传输格式为 yyyy-MM-dd HH:mm:ss。

\- 根目录 ID 为 0。

\- 公共实体及通信协议放在 common 模块。

\- Java 版本、数据库、通信协议及 GUI 框架由全组统一确定。



\## 二、公共字段



\### 1. User（用户信息）



负责人：A



| 字段 | 类型 | 含义 |

| --- | --- | --- |

| userId | Long | 用户唯一ID |

| username | String | 登录账号，如学号 |

| passwordHash | String | 密码哈希，仅服务端使用 |

| createdAt | LocalDateTime | 注册时间 |



\### 2. FileInfo（文件信息）



负责人：B、C



| 字段 | 类型 | 含义 |

| --- | --- | --- |

| fileId | Long | 文件唯一ID |

| userId | Long | 文件所属用户ID |

| folderId | Long | 所属文件夹ID |

| fileName | String | 文件名称 |

| fileSize | long | 文件大小（字节） |

| fileType | String | 文件后缀，如 pdf |

| storagePath | String | 服务端实际存储路径 |

| fileHash | String | 文件哈希，用于查重 |

| uploadTime | LocalDateTime | 上传时间 |

| deleted | boolean | 是否已删除 |

| deletedAt | LocalDateTime | 删除时间，未删除为 null |



\### 3. FolderInfo（文件夹信息）



负责人：C



| 字段 | 类型 | 含义 |

| --- | --- | --- |

| folderId | Long | 文件夹唯一ID |

| userId | Long | 所属用户ID |

| parentId | Long | 父文件夹ID |

| folderName | String | 文件夹名称 |

| createdAt | LocalDateTime | 创建时间 |

| deleted | boolean | 是否已删除 |

| deletedAt | LocalDateTime | 删除时间，未删除为 null |



\### 4. ShareInfo（分享信息）



负责人：D



| 字段 | 类型 | 含义 |

| --- | --- | --- |

| shareId | Long | 分享唯一ID |

| userId | Long | 分享创建者ID |

| targetId | Long | 被分享文件或文件夹ID |

| targetType | String | FILE 或 FOLDER |

| shareToken | String | 分享链接唯一标识 |

| extractCode | String | 4位数字提取码 |

| createdAt | LocalDateTime | 创建时间 |

| expireAt | LocalDateTime | 过期时间，永久为 null |

| accessCount | long | 成功提取次数 |

| cancelled | boolean | 是否取消分享 |



\## 三、统一通信格式



\### 普通业务请求



| 字段 | 类型 | 含义 |

| --- | --- | --- |

| action | String | 操作类型 |

| token | String | 登录令牌（采用令牌方案时） |

| data | Object | 请求参数 |



\### 普通业务响应



| 字段 | 类型 | 含义 |

| --- | --- | --- |

| success | boolean | 是否成功 |

| code | int | 状态码 |

| message | String | 提示信息 |

| data | Object | 返回数据 |



\### 状态码



| 状态码 | 含义 |

| --- | --- |

| 200 | 成功 |

| 400 | 参数错误 |

| 401 | 未登录或身份验证失败 |

| 403 | 无权限 |

| 404 | 资源不存在 |

| 409 | 资源冲突 |

| 410 | 分享已失效 |

| 500 | 服务端异常 |



具体通信协议及文件二进制传输格式由 A、B、E 共同确定。



\## 四、成员之间需要协调的内容



| 成员 | 协调事项 |

| --- | --- |

| A + 全员 | 用户身份、权限验证、公共接口、数据库连接 |

| A + B + E | 文件传输协议、多客户端并发通信 |

| B + C | 文件元数据、文件夹ID、重名处理 |

| B + E | 文件实际存储路径、文件读取方式 |

| C + D | 文件夹分享、删除后分享的处理 |

| D + E | 提取码验证、游客下载授权 |

| 全员 | 客户端界面布局、模块入口及操作风格 |



\## 五、统一业务规则



1\. 文件实际存储路径：storage/用户ID/年/月/日/文件名。

2\. 重名文件自动添加后缀，如 file(1).pdf。

3\. 网盘逻辑目录与实际磁盘存储路径分离。

4\. 重命名和移动优先修改数据库中的逻辑信息。

5\. 普通删除移入回收站，不立即删除磁盘文件。

6\. 回收站保留7天，永久删除时清理实际文件。

7\. 分享有效期支持1天、7天、30天及永久。

8\. 提取码使用 String 类型，保留前导零。

9\. 分享过期、取消或资源进入回收站后禁止访问。

10\. 服务端必须验证用户权限，不直接信任客户端传入的 userId。

11\. 客户端通过 fileId 请求文件，不直接指定服务端磁盘路径。

12\. 密码哈希、实际存储路径等内部信息不得返回客户端。

13\. 游客仅能通过有效分享及提取码获取授权文件。

14\. 客户端不得直接连接数据库，所有数据操作通过服务端完成。



\## 六、修改要求



\- 以上字段为初始约定，由组长统一维护。

\- 修改公共字段、数据库结构及接口前，必须通知相关成员及组长。

\- 协商确认后统一更新本文档及公共代码。

\- 各成员不得自行定义与公共字段冲突的实体类。

\- 具体通信协议及接口参数应在正式开发前确定。



