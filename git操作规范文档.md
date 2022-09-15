# git操作规范文档

## 前置条件

```javascript
# 清除掉缓存在git中的用户名和密码  ( 可能之前有人用过这台电脑 )
git credential-manager uninstall

# 记住自己的用户名和密码
git config --global credential.helper store

# 代码仓库地址
https://gitee.com/xxx/xxx.git      // 仓库地址

# 仓库账号和密码   公司给你的or你自己的
账号: 18181358***
密码: 123456
```



## 生成公钥 【推荐使用】

- 打开控制台

```js
window + r 打开控制台
```

- 测试是否有公钥

```js
在git命令行窗口中输入
cd ~/.ssh/

如果提示没有.ssh文件夹或目录，就创建该目录
mkdir .ssh
```

- 配置公钥配置

```js
git config --global user.name "用户名"  
回车
git config --global user.email "邮箱" 
回车

注意：分别配置用户名和邮箱，其中“用户名”可任意起，“邮箱”需为可用邮箱

再次输入命令：


然后停顿处直接回车（总共3次）
```

![image-20211128121952553](img/image-20211128121952553.png)

- 在Git中查看生成的公钥

```js
cat ~/.ssh/id_rsa.pub
```

![image-20211128122230231](img/image-20211128122230231.png)





## 第一次下载(克隆)代码

```javascript
git clone 仓库地址 // 例如: git clone https://gitee.com/xxx/xxx.git
```



## 第二次以后下载(拉取)代码

```javascript
# 每天进公司第一件事情
git pull 仓库地址 分支名 // 例如: git pull origin develop
```



## 提交代码步骤【重点】

```javascript
git add .           // 纳入到版本控制
git commit -m"描述信息"   // 暂存到本地 

# commit一般是一个功能一次 push是一天一次. 一般是下班之前push
# push之前先pull一次

git push 仓库地址 分支名  // 推送到远程服务器仓库 例如: git push origin develop 
```



## commit 规范【重点】

```javascript
feat：新功能（feature）  // git commit -m "feat: 注册功能完成" 

fix：修补bug   // git commit -m "fix: 修复登录表单验证bug"

docs：文档（documentation）  // git commit -m "docs: 新增功能文档"
```



## git 步骤

- 上班 git pull 拉代码
- 下班前 

```js
git pull 拉一次代码
git push 把自己代码提交上去
```



## 其他命令

```javascript
git status  // 查看仓库状态

git branch // 查看当前有哪些分支
git branch 分支名  // 创建分支
git checkout 分支名 // 切换分支
git checkout -b 分支名 // 创建并切换分支

git merge 分支名  // 合并分支到当前分支

git tag -a v1.1 -m "test_tag"； // 打标签
git push origin --tags // 把tag标签(版本) 推送到服务器
```

- 查看日志

  ```js
  git log
  
  #日志太多 结束  :q
  ```

- 解决冲突

  ```js
  在vscode中查看冲突代码
  
  #用自己or用同事or两个都用
  
  重新 add commit push
  ```

- 合并代码

  ```js
  # 切换到到一个分支 把你想要合并的分支 合并过来
  git merge 分支名
  ```

- 回滚代码 回退代码： 回到过去  （ 切换到将来 ）

  ```js
  git log
  git reflog
  
  git reset --hard hash值  （sha值）
  ```

- 紧急修复bug

  - 创建一个紧急修复bug的分支：hotfix
  - 在这个分支上写修复bug的代码
  - 写好以后，测试通过，合并到dev分支。





















