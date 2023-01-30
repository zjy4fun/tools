## node

### nvm mirror

linux:
```bash
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
export NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
```

win: 
```bash
nvm npm_mirror https://npmmirror.com/mirrors/npm/  
nvm node_mirror https://npmmirror.com/mirrors/node/ 
```

### npm mirror

```bash
```
npm config set registry http://mirrors.cloud.tencent.com/npm/

npm config set registry https://mirrors.huaweicloud.com/repository/npm/

npm config set registry https://registry.npmmirror.com
```
```

### windows powershell can't run pnpm/yarn

解决 Windows Powershell 无法执行 yarn/pnpm 的问题

打开 powershell 管理员模式，执行下面的命令
```
set-ExecutionPolicy RemoteSigned
```
