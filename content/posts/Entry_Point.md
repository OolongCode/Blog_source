---
title: "寻找入口点 回归最开始的美好"
date: 2022-02-26T11:12:10+08:00
draft: false
math: true
tags: ["Reverse","Note"]
categories: ["reverse"]
toc: true
---

# 寻找主函数

入口点（主函数）并不确定 --> 编译器版本

寻找编译器特征 来确定 主函数


## VS 2013-2017 特征


### 2015 Debug x86

1. jmp
2. mainCRTStartup里面的call
3. 第二个call
4. movzx
test
jz
mov
mov
push
call
add
push
call
add
call(main)
5. 最后一个call
6. jmp
7. main

### 2015 Release x86

1. jmp
2. call`__p___argv`
mov
call`__p___argc`
mov
call`_get_initial_narrow_environment`
push
push
push
call(main)
3. main

### 2015 Debug x64

1. jmp
2. call
3. 第二个call
4. movzx
test
jz
mov
mov
call
xor
call
call(main)
5. 最后一个call
6. jmp
7. main

### 2015 Release x64

1. jmp
2. call`__p___argv`
movrdi, rax
call`__p___argc`
movrbx, rax
call
mov
mov
mov
call(main)
3. main

### 2017 Debug x86

1. jmp
2. call
3. 第二个call
4. movzx   ecx, al
testecx, ecx
jz short loc_411E82
mov
mov 
push 
call
add
call (main)
5. 最后一个call
6. jmp
7. main

### 2017 Release x86

1. jmp
2. call
mov
call
mov
call
push
push
push
call(main)
3. main

### 2017 Debug x64

1. jmp
2. call
3. 第二个call
4. movzx
test
jz 
mov 
mov
call
call(main)
5. 最后一个call
6. jmp
7. main

### 2017 Release x64

1. jmp
2. call`__p___argv`
mov
call`__p___argc`
mov
call
mov
mov
mov
call(main)
3. main

### 2019 Debug x86

1. jmp
2. call
3. 第二个call
4. movzx
test
jz
mov
mov
push
call
add
call(main)
5. 最后一个call
6. jmp
7. main

### 2019 Release x86

1. jmp
2. call`__p___argv`
mov
call`__p___argc`
push
push
push
call(main)
3. main

### 2019 Debug x64

1. jmp
2. call
3. 第二个call
4. movzx
test
jz
mov
mov
call
call(main)
5. 最后一个call
6. jmp
7. main

### 2019 Release x64

1. jmp
2. call`__p___argv`
mov
call`__p___argc`
push
push 
push
call(main)
3. main


### 2022 Debug x86

1. jmp
2. call
3. 第二个call
4. movzx
test
je
mov
mov
push
call
add
call(main)
5. 最后一个call
6. jmp
7. main

### 2022 Release x86

1. call
2. 第二个call
3. movzx
test
je
mov
mov
push
call
add
call(main)
4. 最后一个call
5. main

### 2022 Debug x64

1. jmp
2. call
3. 第二个call
4. movzx
test
je
mov
mov
call
call(main)
5. 最后一个call
6. jmp
7. main

### 2022 Release x64

1. call`__p___argv`
mov
call`__p___argc`
mov
mov
mov
call(main)
2. main


## MinGW GCC

### x86 Debug

1. jmp
2. call
mov
mov
mov
mov
mov 
mov
mov
mov
mov
call(main)
3. main


### x86 Release

1. jmp

2. call
mov
mov
mov
mov
mov
mov
mov
mov
mov
call(main)

3. main

### x64 Debug

1. 第二个call
2. call
mov
mov
mov
mov
mov
mov
mov
call(main)
3. main

### x64 Release

1. 第二个call
2. call
mov
mov
mov
mov
mov
mov
mov
call(main)
3. main

## Clang

### x86 Debug

1. jmp
2. call
mov
call
mov
call
push
push
push
call
3. main

### x86 Release

1. jmp
2. call
mov
call
mov
call
push
push
push
call
3. main

### x64 Debug

1. jmp
2. call
mov
call
mov
call
mov
mov
mov
call
3. main

### x64 Release

1. jmp
2. call
mov
call
mov
call
mov
mov
mov
call
3. main

## Go Build
1. jmp

2.  jmp

3.  call
mov
mov
mov
mov
call
call
call
learax, mainflag


4. dq offset runtime_main 


5.  lea
call
mov
mov
call
mov
call
cmp
jnz
cmp
jnz
mov
lea
callmain

6. main
