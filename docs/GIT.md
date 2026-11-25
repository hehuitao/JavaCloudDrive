\# Git 协作规范



\## 一、分支说明



| 分支 | 用途 |

|---|---|

| main | 正式版本 |

| develop | 开发整合与联调 |

| feature/user | A：用户管理 |

| feature/upload | B：文件上传 |

| feature/manage | C：文件管理 |

| feature/share | D：分享与提取 |

| feature/download | E：文件下载 |



\## 二、首次下载项目



接受组长的GitHub仓库邀请后，在终端执行：



```bash

git clone 仓库地址

cd JavaCloudDrive

```



切换至自己负责的分支，例如成员B：



```bash

git switch feature/upload

```



\## 三、每天开发前



确保当前没有未提交的修改，然后执行：



```bash

git pull

git fetch origin

git merge origin/develop

```



同步远程代码，避免使用过期的公共接口。



\## 四、开发完成后



```bash

git status

git add .

git commit -m "本次修改说明"

git push

```



提交信息示例：



\- feat: 实现用户登录

\- feat: 实现批量上传

\- fix: 修复文件下载错误

\- docs: 更新接口文档

\- refactor: 优化文件管理代码



\## 五、代码合并



模块完成后，在GitHub创建Pull Request。



\- base：develop

\- compare：自己的功能分支



由组长审核、测试后合并。



阶段验收通过后，由组长将develop合并到main。



\## 六、注意事项



1\. 不直接向main或develop提交业务代码。

2\. 不擅自修改其他成员负责的代码。

3\. 修改公共字段和接口前必须提前沟通。

4\. 不上传数据库密码、个人配置及实际用户文件。

5\. 不上传target、.idea等生成目录。

6\. 不使用git push --force。

7\. 出现代码冲突及时联系相关成员及组长。

8\. 每完成一个小功能及时提交，不要等到截止日期才上传代码。

