版本不对
在Ubuntu下如何更新node版本、更换node版本
```bash
sudo apt-get install npm
sudo npm install n -g
sudo n stable
```

备注  
n是一个[Node](https://so.csdn.net/so/search?q=Node&spm=1001.2101.3001.7020)工具包，它提供了几个升级命令参数：

`n` 显示已安装的Node版本  
`n latest` 安装最新版本Node  
`n stable` 安装最新稳定版Node  
`n lts` 安装最新长期维护版(lts)Node  
`n version` 根据提供的版本号安装Node


下载完成后 如果发现 `node -v` 仍然是之前的版本，
根据不同的 shell 版本执行 `hash -r` 或者 `rehash` 即可，
我用的是 `zsh`，所以执行 `hash -r`（根据上图倒数第三行提示）
![[Pasted image 20220407190959.png]]