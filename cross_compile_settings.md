# Build vim for synology DS213j 

### Official guide 
```https://developer.synology.com/developer-guide/compile_applications/download_dsm_tool_chain.html```

### Tool chain for DS213j 
```https://sourceforge.net/projects/dsgpl/files/DSM%205.2%20Tool%20Chains/Marvell%20Armada%20370%20Linux%203.2.40```

### CROSS compile settings 
``` shell
# This is the cross-compile setting for ubuntu16.04 and the synology DS213J
export CLFS_HOST=arm-unknown-linux
export CLFS_TARGET=arm-unknown-linux
export CLFS_BUILD=x86_64-linux-gnu
export CROSS_COMPILE=/usr/local/arm-unknown-linux-gnueabi/bin/arm-unknown-linux-gnueabi
export CROSS=/usr/local/arm-unknown-linux-gnueabi/bin/arm-unknown-linux-gnueabi
# Indicates that we use the variable fields CROSS with arm-none-linux-gnueabi.
export CC=${CROSS_COMPILE}-gcc
# Indicates that we use the GCC compiler for the ARM architecture.
export LD=${CROSS_COMPILE}-ld
# Indicates that we use the dynamic library of the ARM architecture.
export AS=${CROSS_COMPILE}-as
# Indicates that we use the assembler of the ARM architecture.
```
### Build vim
```
git clone https://github.com/vim/vim.git --depth 1
```
As the is cross compile we add  LFS(Linux from scratch) settings 
```
# for vim cross compile
cat > src/auto/config.cache << "EOF"
vim_cv_getcwd_broken=no
vim_cv_memmove_handles_overlap=yes
vim_cv_stat_ignores_slash=no
vim_cv_terminfo=yes
vim_cv_toupper_broken=no
vim_cv_tty_group=world
EOF
```
configure 
```
./configure \
--host=${CLFS_HOST} \
--target=${CLFS_TARGET} \
--build=${CLFS_BUILD} \
-with-tlib=ncurses \
```
then we do make, the bin is in src/vim