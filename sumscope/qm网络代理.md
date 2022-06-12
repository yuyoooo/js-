# qm 网络代理

## 核心实现

### 思路

1. 全部使用第三方转发
   - 原因：由于electron(不支持/没找到)socks5的账号密码验证
   - 原理：项目启动代理转发服务，qm设置代理到本地转发服务端口，目前：6121
   - 评价：简单-便捷
2. http/https本地代理+验证，socks5使用转发
   - 原因：之前因为socks5不支持就没有考虑，现在因为转发工具的限制，选择了结合实现代理
   - 原理：http/https直接设置代理到代理服务，验证本地实现，socks5走代理转发服务
   - 评价：较麻烦-上线快
   - 当前选择
3. IE代理
   - 描述：IE代理即为全局代理，本项目需要去注册表中获取全局代理信息进行设置
   - 原理：获取注册表进行设置
   - 评价：使用第三方包实现

### elecreon项目添加代理：

1. `session.defaultSession.setProxy`
   - 为当前session添加代理设置，即设即用
   - 对main线程无效
   - 不支持直接设置账号密码
   - 目前选择
2. `app.commandLine.appendArgument('proxy-server'，address:port);`
   - 对整个项目实现代理设置
   - 需要重启才能生效
   - 对应的 `app.commandLine.removeSwitch`16版本才有当前版本不可使用
   - 不支持直接设置账号密码

### 项目添加auth信息

1. 添加 —— 直接添加 `Proxy-Authorization` 无效

   ```js
   app.once('login', (event, webContents, details, authInfo, callback) => {
      if (authInfo.isProxy) {
        event.preventDefault()
        callback(setting.username, setting.password)
      }
   })
   ```

2. 清理——当信息变化时需要先清理验证缓存否则修改不生效

   ```js
   session.defaultSession.clearAuthCache()
   ```

### 第三方实现代理转发

1. 工具 bridge.exe ，socks2http.exe —— 由后端提供，其中beridge.exe弃用，socks2http.exe手动实现

2. 工具需要按照不同的环境进行不同的调用 —— 暂未实现

3. 获取项目exe路径

   ```js
   const cwdPath = path.parse(process.cwd());
   const gridgePath = path.resolve(cwdPath.dir, cwdPath.base, 'src', 'app', 'plugins', 'bridge', 'socks2http.exe');
   ```

4. spawn 启动项目

   - 没有设置 { stdout：true } ，我只需启动服务即可，项目退出即可退出，不做其他监听
   - `process.kill(pid)`无法杀掉 stdout：true 的 spwan 进程
   - spwan 参数需要 arr分割，会自动拼接，否则无法启动

   ```js
   const bridgeProcess = ChildProcess.spawn(gridgePath, ['-local','127.0.0.1:6121','-remote',`${setting.host}:${setting.port}`,'-user', setting.username, '-pwd', setting.password]);
   ```

5. 保存pid,启动本地代理到服务端口

   ```js
   const { pid } = bridgeProcess;
   this.bridgePid = pid
   // 设置项目代理
   this.sessionSetProxy('http://127.0.0.1:6121')
   ```

6. kill服务

   ```js
   process.kill(this.bridgePid, 'SIGKILL')
   ```

### IE代理获取

1. 使用execFile和reg.exe获取注册表中的数据

   ```js
   command = "REG" / "reg.exe"
   execFile(command, ["query", "Hive.HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Internet Settings"], (err, stdout, stderr) => {
       if (err) {
            reject(err);
       } else {
            resolve({ stdout, stderr });
       }
   });
   
   ```

2. 需要判断系统和是否启动IE代理

3. 使用第三方包 `get-proxy-settings` 实现

### 数据存储

1. 存储于store中，settings.prxy
2. store.setValue 默认会存储一个 {}

### 获取MAC地址

1. `os.networkInterfaces()`

## 其他

1. 无法使用Google可以使用必应，百度作用较小
2. 文档描述不清晰，很不友好
3. 可以在github的issues进行查找信息
4. 可在npm包中查找实现原理













