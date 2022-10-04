---
title: "看穿你的逻辑 理解你表达的真谛"
date: 2022-02-26T11:36:23+08:00
draft: false
math: true
tags: ["Reverse","Note"]
categories: ["reverse"]
toc: true
---

## 逻辑语句逆向分析 总结

### if分支

#### if

Debug
```x86asm
0x00411a30      cmp     dword [var_ch], 0x41
0x00411a34      jne     0x411a47
0x00411a36      mov  eax, dword [var_ch]
0x00411a39      push  eax 
0x00411a3a      push  str.This_is_current_alpha:_c
0x00411a3f      call    fcn.004110d7
0x00411a44      add  esp, 8
0x00411a47      mov  esi, esp
```

Release
```x86asm
0x004010a1      cmp     dword [var_8h], 0x41
0x004010a5      jne     0x4010b6
0x004010a7      push    0x41
0x004010a9      push    str.This_is_current_alpha:_c
0x004010ae      call    fcn.00401020
0x004010b3      add     esp, 8
0x004010b6      push    str.pause 
```

#### if_else

Debug
```x86asm
0x00411a30      cmp     dword [var_ch], 0x41
0x00411a34      jne     0x411a49
0x00411a36      mov     eax, dword [var_ch]
0x00411a39      push    eax 
0x00411a3a      push    str.This_is_current_alpha:_c
0x00411a3f      call    fcn.004110d7
0x00411a44      add     esp, 8
0x00411a47      jmp     0x411a56
0x00411a49      push    str.error 
0x00411a4e      call    fcn.004110d7
0x00411a53      add     esp, 4
0x00411a56      mov     esi, esp
```

Release
```x86asm
0x004010a1      cmp     dword [var_8h], 0x41
0x004010a5      jne     0x4010b8
0x004010a7      push    0x41
0x004010a9      push    str.This_is_current_alpha:_c
0x004010ae      call    fcn.00401020
0x004010b3      add     esp, 8
0x004010b6      jmp     0x4010c5
0x004010b8      push    str.error
0x004010bd      call    fcn.00401020
0x004010c2      add     esp, 4
0x004010c5      push    str.pause
```

#### if_else if_else 3语句

Debug
```x86asm
0x00411a30      cmp     dword [var_ch], 0x41
0x00411a34      jne     0x411a49
0x00411a36      mov     eax, dword [var_ch]
0x00411a39      push    eax
0x00411a3a      push    str.This_is_current_alpha:_c
0x00411a3f      call    fcn.004110d7
0x00411a44      add     esp, 8
0x00411a47      jmp     0x411a6f
0x00411a49      cmp     dword [var_ch], 0x42
0x00411a4d      jne     0x411a62
0x00411a4f      mov     eax, dword [var_ch]
0x00411a52      push    eax 
0x00411a53      push    str.This_is_error_alpha:_c
0x00411a58      call    fcn.004110d7
0x00411a5d      add     esp, 8
0x00411a60      jmp     0x411a6f
0x00411a62      push    str.error
0x00411a67      call    fcn.004110d7
0x00411a6c      add     esp, 4
0x00411a6f      mov     esi, esp
```

Release
```x86asm
0x004010a4      cmp     eax, 0x41
0x004010a7      jne     0x4010b9
0x004010a9      push    eax
0x004010aa      push    str.This_is_current_alpha:_c
0x004010af      call    fcn.00401020
0x004010b4      add     esp, 8
0x004010b7      jmp     0x4010db
0x004010b9      cmp     eax, 0x42
0x004010bc      jne     0x4010ce
0x004010be      push    eax
0x004010bf      push    str.This_is_error_alpha:_c
0x004010c4      call    fcn.00401020
0x004010c9      add     esp, 8
0x004010cc      jmp     0x4010db
0x004010ce      push    str.error
0x004010d3      call    fcn.00401020
0x004010d8      add     esp, 4
0x004010db      push    str.pause
```

#### if_else if_else 6语句

Debug
```x86asm
0x00411a30      cmp     dword [var_ch], 0x41
0x00411a34      jne     0x411a49
0x00411a36      mov     eax, dword [var_ch]
0x00411a39      push    eax
0x00411a3a      push    str.This_is_current_alpha:_c
0x00411a3f      call    fcn.004110d7
0x00411a44      add     esp, 8
0x00411a47      jmp     0x411aba
0x00411a49      cmp     dword [var_ch], 0x42
0x00411a4d      jne     0x411a62
0x00411a4f      mov     eax, dword [var_ch]
0x00411a52      push    eax
0x00411a53      push    str.This_is_error_alpha:_c
0x00411a58      call    fcn.004110d7
0x00411a5d      add     esp, 8
0x00411a60      jmp     0x411aba
0x00411a62      cmp     dword [var_ch], 0x43
0x00411a66      jne     0x411a7b
0x00411a68      mov     eax, dword [var_ch]
0x00411a6b      push    eax 
0x00411a6c      push    str.This_is_other_alpha:_c
0x00411a71      call    fcn.004110d7
0x00411a76      add     esp, 8
0x00411a79      jmp     0x411aba
0x00411a7b      cmp     dword [var_ch], 0x44
0x00411a7f      jne     0x411a94
0x00411a81      mov     eax, dword [var_ch]
0x00411a84      push    eax
0x00411a85      push    str.This_is_specify_alpha:_c 
0x00411a8a      call    fcn.004110d7
0x00411a8f      add     esp, 8
0x00411a92      jmp     0x411aba
0x00411a94      cmp     dword [var_ch], 0x45
0x00411a98      jne     0x411aad
0x00411a9a      mov     eax, dword [var_ch]
0x00411a9d      push    eax   
0x00411a9e      push    str.This_is_different_alpha:_c
0x00411aa3      call    fcn.004110d7
0x00411aa8      add     esp, 8
0x00411aab      jmp     0x411aba
0x00411aad      push    str.error
0x00411ab2      call    fcn.004110d7
0x00411ab7      add     esp, 4
0x00411aba      mov     esi, esp
```

Release
```x86asm
0x004010a4      cmp     eax, 0x41 
0x004010a7      jne     0x4010b9
0x004010a9      push    eax 
0x004010aa      push    str.This_is_current_alpha:_c
0x004010af      call    fcn.00401020
0x004010b4      add     esp, 8
0x004010b7      jmp     0x40111a
0x004010b9      cmp     eax, 0x42
0x004010bc      jne     0x4010ce
0x004010be      push    eax 
0x004010bf      push    str.This_is_error_alpha:_c
0x004010c4      call    fcn.00401020
0x004010c9      add     esp, 8
0x004010cc      jmp     0x40111a
0x004010ce      cmp     eax, 0x43
0x004010d1      jne     0x4010e3
0x004010d3      push    eax
0x004010d4      push    str.This_is_other_alpha:_c
0x004010d9      call    fcn.00401020
0x004010de      add     esp, 8
0x004010e1      jmp     0x40111a
0x004010e3      cmp     eax, 0x44 
0x004010e6      jne     0x4010f8
0x004010e8      push    eax 
0x004010e9      push    str.This_is_specify_alpha:_c
0x004010ee      call    fcn.00401020
0x004010f3      add     esp, 8
0x004010f6      jmp     0x40111a
0x004010f8      cmp     eax, 0x45
0x004010fb      jne     0x40110d
0x004010fd      push    eax 
0x004010fe      push    str.This_is_different_alpha:_c
0x00401103      call    fcn.00401020
0x00401108      add     esp, 8
0x0040110b      jmp     0x40111a
0x0040110d      push    str.error
0x00401112      call    fcn.00401020
0x00401117      add     esp, 4
0x0040111a      push    str.pause
```

### switch分支

规律：
1.  小于6条分支语句（不包括default），汇编语句不使用跳转表进行优化
2. 大于等于6条分支语句（不包括default），汇编语句使用跳转表进行优化
3. 最小case和最大case差值在255以内使用跳转表优化
4. 最小case和最大case差值在255以外使用判定树优化

#### switch三语句

Debug
```x86asm
0x00411a39      cmp     dword [var_d4h], 0
0x00411a40      je      0x411a56
0x00411a42      cmp     dword [var_d4h], 1
0x00411a49      je      0x411a65
0x00411a4b      cmp     dword [var_d4h], 2
0x00411a52      je      0x411a74
0x00411a54      jmp     0x411a81
0x00411a56      push  str.input_number_is_0 ; 0x417b34 ; int32_t arg_ch
0x00411a5b      call    fcn.004110d7
0x00411a60      add  esp, 4
0x00411a63      jmp     0x411a81
0x00411a65      push  str.input_number_is_1 ; 0x417b4c ; int32_t arg_ch
0x00411a6a      call    fcn.004110d7
0x00411a6f      add  esp, 4
0x00411a72      jmp     0x411a81
0x00411a74      push  str.input_number_is_2 ; 0x417b64 ; int32_t arg_ch
0x00411a79      call    fcn.004110d7
0x00411a7e      add  esp, 4
0x00411a81      mov  esi, esp
```

Release
```x86asm
0x004010a4      sub     eax, 0
0x004010a7      je      0x4010c1
0x004010a9      sub     eax, 1
0x004010ac      je      0x4010ba
0x004010ae      sub     eax, 1
0x004010b1      jne     0x4010ce
0x004010b3      push    str.input_number_is_2
0x004010b8      jmp     0x4010c6
0x004010ba      push    str.input_number_is_1
0x004010bf      jmp     0x4010c6
0x004010c1      push    str.input_number_is_0
0x004010c6      call    fcn.00401020
0x004010cb      add     esp, 4
0x004010ce      push    str.pause
```

#### switch六语句

##### Debug
```x86asm
0x00411a39      cmp     dword [var_d4h], 5
0x00411a40      ja      case.default.0x411a48
0x00411a42      mov     ecx, dword [var_d4h]
0x00411a48      jmp     dword [ecx*4 + 0x411b0c] ; switch table (6 cases) at 0x411b0c
0x00411a4f      push    str.input_number_is_0
0x00411a54      call    fcn.004110d7
0x00411a59      add     esp, 4
0x00411a5c      jmp     case.default.0x411a48
0x00411a5e      push    str.input_number_is_1
0x00411a63      call    fcn.004110d7
0x00411a68      add     esp, 4
0x00411a6b      jmp     case.default.0x411a48
0x00411a6d      push    str.input_number_is_2
0x00411a72      call    fcn.004110d7
0x00411a77      add     esp, 4
0x00411a7a      jmp     case.default.0x411a48
0x00411a7c      push    str.input_number_is_3
0x00411a81      call    fcn.004110d7
0x00411a86      add     esp, 4
0x00411a89      jmp     case.default.0x411a48
0x00411a8b      push    str.input_number_is_4
0x00411a90      call    fcn.004110d7
0x00411a95      add     esp, 4
0x00411a98      jmp     case.default.0x411a48
0x00411a9a      push    str.input_number_is_5
0x00411a9f      call    fcn.004110d7
0x00411aa4      add     esp, 4
;-- default:                       ; from 0x411a48
0x00411aa7      mov     esi, esp


;switch 跳转表
0x00411b0e      .dword 0x1a5e0041
0x00411b10      .int32 4266590 ; case.0x411a48.1
0x00411b14      .int32 4266605 ; case.0x411a48.2
0x00411b18      .int32 4266620 ; case.0x411a48.3
0x00411b1c      .int32 4266635 ; case.0x411a48.4
0x00411b20      .int32 4266650 ; case.0x411a48.5

```

整十连续
```x86asm
0x140011a26      mov     eax, dword [var_d4h]
0x140011a2c      sub     eax, 0x21c ; 540
0x140011a31      mov     dword [var_d4h], eax
0x140011a37      cmp     dword [var_d4h], 0x3c
0x140011a3e      ja      case.0x140011a64.1
0x140011a44      movsxd  rax, dword [var_d4h]
0x140011a4b      lea     rcx, [0x140000000]
0x140011a52      movzx   eax, byte [rcx + rax + 0x11b24]
0x140011a5a      mov     eax, dword [rcx + rax*4 + 0x11b04]
0x140011a61      add     rax, rcx
0x140011a64      jmp     rax 
0x140011a66      lea     rcx, str.input_number_is_540
0x140011a6d      call    fcn.14001119f
0x140011a72      jmp     case.0x140011a64.1
0x140011a74      lea     rcx, str.input_number_is_550
0x140011a7b      call    fcn.14001119f
0x140011a80      jmp     case.0x140011a64.1
0x140011a82      lea     rcx, str.input_number_is_560
0x140011a89      call    fcn.14001119f
0x140011a8e      jmp     case.0x140011a64.1
0x140011a90      lea     rcx, str.input_number_is_570
0x140011a97      call    fcn.14001119f
0x140011a9c      jmp     case.0x140011a64.1
0x140011a9e      lea     rcx, str.input_number_is_580
0x140011aa5      call    fcn.14001119f
0x140011aaa      jmp     case.0x140011a64.1
0x140011aac      lea     rcx, str.input_number_is_590
0x140011ab3      call    fcn.14001119f
0x140011ab8      jmp     case.0x140011a64.1
0x140011aba      lea     rcx, str.input_number_is_600
0x140011ac1      call    fcn.14001119f
0x140011ac6      lea     rcx, str.pause
0x140011acd      call    qword [system]


;分支跳转表
0x140011b04      .int32 72294
0x140011b08      .int32 72308
0x140011b0c      .int32 72322
0x140011b10      .int32 72336
0x140011b14      .int32 72350
0x140011b18      .int32 72364
0x140011b1c      .int32 72378
0x140011b20      .int32 72390


;数据处理表
0x140011b24      .char 0
0x140011b25      .char 7
0x140011b26      .char 7
0x140011b27      .char 7
0x140011b28      .char 7
0x140011b29      .char 7
0x140011b2a      .char 7
0x140011b2b      .char 7
0x140011b2c      .char 7
0x140011b2d      .char 7
0x140011b2e      .char 1
0x140011b2f      .char 7
0x140011b30      .char 7
0x140011b31      .char 7
0x140011b32      .char 7
0x140011b33      .char 7
0x140011b34      .char 7
0x140011b35      .char 7
0x140011b36      .char 7
0x140011b37      .char 7
0x140011b38      .char 2
0x140011b39      .char 7
0x140011b3a      .char 7
0x140011b3b      .char 7
0x140011b3c      .char 7
0x140011b3d      .char 7
0x140011b3e      .char 7
0x140011b3f      .char 7
0x140011b40      .char 7
0x140011b41      .char 7
0x140011b42      .char 3
0x140011b43      .char 7
0x140011b44      .char 7
0x140011b45      .char 7
0x140011b46      .char 7
0x140011b47      .char 7
0x140011b48      .char 7
0x140011b49      .char 7
0x140011b4a      .char 7
0x140011b4b      .char 7
0x140011b4c      .char 4
0x140011b4d      .char 7
0x140011b4e      .char 7
0x140011b4f      .char 7
0x140011b50      .char 7
0x140011b51      .char 7
0x140011b52      .char 7
0x140011b53      .char 7
0x140011b54      .char 7
0x140011b55      .char 7
0x140011b56      .char 5
0x140011b57      .char 7
0x140011b58      .char 7
0x140011b59      .char 7
0x140011b5a      .char 7
0x140011b5b      .char 7
0x140011b5c      .char 7
0x140011b5d      .char 7
0x140011b5e      .char 7
0x140011b5f      .char 7
0x140011b60      .char 6

```

##### Release
```x86asm
0x004010a4      cmp     eax, 5     ; 5
0x004010a7      ja      case.default.0x4010a9
0x004010a9      jmp     dword [eax*4 + 0x401100] ; switch table (6 cases) at 0x401100
0x004010b0      push    str.input_number_is_0 ; 0x40210c
0x004010b5      jmp     0x4010d8
0x004010b7      push    str.input_number_is_1 ; 0x402120
0x004010bc      jmp     0x4010d8
0x004010be      push    str.input_number_is_2 ; 0x402134
0x004010c3      jmp     0x4010d8
0x004010c5      push    str.input_number_is_3 ; 0x402148
0x004010ca      jmp     0x4010d8
0x004010cc      push    str.input_number_is_4 ; 0x40215c
0x004010d1      jmp     0x4010d8
0x004010d3      push    str.input_number_is_5 ; 0x402170
0x004010d8      call    fcn.00401020
0x004010dd      add     esp, 4
;-- default:                       ; from 0x4010a9
0x004010e0      push  str.pause

;switch 跳转表
0x00401100      .int32 4198576 ; case.0x4010a9.0
0x00401104      .int32 4198583 ; case.0x4010a9.1
0x00401108      .int32 4198590 ; case.0x4010a9.2
0x0040110c      .int32 4198597 ; case.0x4010a9.3
0x00401110      .int32 4198604 ; case.0x4010a9.4
0x00401114      .int32 4198611 ; case.0x4010a9.5
```

整十连续
```x86asm
0x1400010f4      mov     eax, dword [var_20h]
0x1400010f8      add     eax, 0xfffffde4
0x1400010fd      cmp     eax, 0x3c
0x140001100      ja      case.0x14000111d.541
0x140001102      lea     rdx, [0x140000000]
0x140001109      cdqe
0x14000110b      movzx   eax, byte [rdx + rax + 0x11a4]
0x140001113      mov     ecx, dword [rdx + rax*4 + 0x1184]
0x14000111a      add     rcx, rdx
0x14000111d      jmp     rcx       ; switch table (61 cases) at 0x140001184
0x14000111f      lea     rcx, str.input_number_is_540 
0x140001126      jmp     0x14000115c
0x140001128      lea     rcx, str.input_number_is_550 
0x14000112f      jmp     0x14000115c
0x140001131      lea     rcx, str.input_number_is_560 
0x140001138      jmp     0x14000115c
0x14000113a      lea     rcx, str.input_number_is_570 
0x140001141      jmp     0x14000115c
0x140001143      lea     rcx, str.input_number_is_580
0x14000114a      jmp     0x14000115c
0x14000114c      lea     rcx, str.input_number_is_590
0x140001153      jmp     0x14000115c
0x140001155      lea     rcx, str.input_number_is_600
0x14000115c      call    fcn.140001020
0x140001161      lea     rcx, str.pause
0x140001168      call    fcn.140013094


;分支跳转表
0x140001184      .int32 4383
0x140001188      .int32 4392
0x14000118c      .int32 4401
0x140001190      .int32 4410
0x140001194      .int32 4419
0x140001198      .int32 4428
0x14000119c      .int32 4437
0x1400011a0      .int32 4449


;数据处理表
0x1400011a4      .char 0
0x1400011a5      .char 7
0x1400011a6      .char 7
0x1400011a7      .char 7
0x1400011a8      .char 7
0x1400011a9      .char 7
0x1400011aa      .char 7
0x1400011ab      .char 7
0x1400011ac      .char 7
0x1400011ad      .char 7
0x1400011ae      .char 1
0x1400011af      .char 7
0x1400011b0      .char 7
0x1400011b1      .char 7
0x1400011b2      .char 7
0x1400011b3      .char 7
0x1400011b4      .char 7
0x1400011b5      .char 7
0x1400011b6      .char 7
0x1400011b7      .char 7
0x1400011b8      .char 2
0x1400011b9      .char 7
0x1400011ba      .char 7
0x1400011bb      .char 7
0x1400011bc      .char 7
0x1400011bd      .char 7
0x1400011be      .char 7
0x1400011bf      .char 7
0x1400011c0      .char 7
0x1400011c1      .char 7
0x1400011c2      .char 3
0x1400011c3      .char 7
0x1400011c4      .char 7
0x1400011c5      .char 7
0x1400011c6      .char 7
0x1400011c7      .char 7
0x1400011c8      .char 7
0x1400011c9      .char 7
0x1400011ca      .char 7
0x1400011cb      .char 7
0x1400011cc      .char 4
0x1400011cd      .char 7
0x1400011ce      .char 7
0x1400011cf      .char 7
0x1400011d0      .char 7
0x1400011d1      .char 7
0x1400011d2      .char 7
0x1400011d3      .char 7
0x1400011d4      .char 7
0x1400011d5      .char 7
0x1400011d6      .char 5
0x1400011d7      .char 7
0x1400011d8      .char 7
0x1400011d9      .char 7
0x1400011da      .char 7
0x1400011db      .char 7
0x1400011dc      .char 7
0x1400011dd      .char 7
0x1400011de      .char 7
0x1400011df      .char 7
0x1400011e0      .char 6

```

#### switch单不连续


##### Debug

少分支语句
```x86asm
0x140011a26      cmp     dword [var_d4h], 1
0x140011a2d      je      0x140011a58
0x140011a2f      cmp     dword [var_d4h], 2
0x140011a36      je      0x140011a66
0x140011a38      cmp     dword [var_d4h], 3
0x140011a3f      je      0x140011a74
0x140011a41      cmp     dword [var_d4h], 4
0x140011a48      je      0x140011a82
0x140011a4a      cmp     dword [var_d4h], 0xc8
0x140011a54      je      0x140011a90
0x140011a56      jmp     0x140011a9c
0x140011a58      lea     rcx, str.input_number_is_1
0x140011a5f      call    fcn.14001119f
0x140011a64      jmp     0x140011a9c
0x140011a66      lea     rcx, str.input_number_is_2
0x140011a6d      call    fcn.14001119f
0x140011a72      jmp     0x140011a9c
0x140011a74      lea     rcx, str.input_number_is_3
0x140011a7b      call    fcn.14001119f
0x140011a80      jmp     0x140011a9c
0x140011a82      lea     rcx, str.input_number_is_4
0x140011a89      call    fcn.14001119f
0x140011a8e      jmp     0x140011a9c
0x140011a90      lea     rcx, str.input_number_is_5
0x140011a97      call    fcn.14001119f
0x140011a9c      lea     rcx, str.pause
```

多分支语句
```x86asm

0x140011a26      cmp     dword [var_d4h], 0xc8
0x140011a30      ja      case.0x140011a56.6
0x140011a36      movsxd  rax, dword [var_d4h]
0x140011a3d      lea     rcx, [0x140000000]
0x140011a44      movzx   eax, byte [rcx + rax + 0x11b14]
0x140011a4c      mov     eax, dword [rcx + rax*4 + 0x11af4]
0x140011a53      add     rax, rcx
0x140011a56      jmp     rax       ; switch table (201 cases) at 0x140011af4
0x140011a58      lea     rcx, str.input_number_is_0
0x140011a5f      call    fcn.14001119f
0x140011a64      jmp     case.0x140011a56.6
0x140011a66      lea     rcx, str.input_number_is_1
0x140011a6d      call    fcn.14001119f
0x140011a72      jmp     case.0x140011a56.6
0x140011a74      lea     rcx, str.input_number_is_2
0x140011a7b      call    fcn.14001119f
0x140011a80      jmp     case.0x140011a56.6
0x140011a82      lea     rcx, str.input_number_is_3
0x140011a89      call    fcn.14001119f
0x140011a8e      jmp     case.0x140011a56.6
0x140011a90      lea     rcx, str.input_number_is_4
0x140011a97      call    fcn.14001119f
0x140011a9c      jmp     case.0x140011a56.6
0x140011a9e      lea     rcx, str.input_number_is_5
0x140011aa5      call    fcn.14001119f
0x140011aaa      jmp     case.0x140011a56.6
0x140011aac      lea     rcx, str.input_number_is_200
0x140011ab3      call    fcn.14001119f
0x140011ab8      lea     rcx, str.pause
0x140011abf      call    qword [system]

; 分支跳转表
0x140011af4      .int32 72280
0x140011af8      .int32 72294
0x140011afc      .int32 72308
0x140011b00      .int32 72322
0x140011b04      .int32 72336
0x140011b08      .int32 72350
0x140011b0c      .int32 72364
0x140011b10      .int32 72376

```

```x86asm
0x140011a1d      mov     eax, dword [var_4h]
0x140011a20      mov     dword [var_d4h], eax
0x140011a26      mov     eax, dword [var_d4h]
0x140011a2c      sub     eax, 0xa
0x140011a2f      mov     dword [var_d4h], eax
0x140011a35      cmp     dword [var_d4h], 0xbe
0x140011a3f      ja      case.0x140011a65.6
0x140011a45      movsxd  rax, dword [var_d4h]
0x140011a4c      lea     rcx, [0x140000000]
0x140011a53      movzx   eax, byte [rcx + rax + 0x11b24]
0x140011a5b      mov     eax, dword [rcx + rax*4 + 0x11b04]
0x140011a62      add     rax, rcx
0x140011a65      jmp     rax       ; switch table (191 cases) at 0x140011b04
0x140011a67      lea     rcx, str.input_number_is_0 ; 0x14001ad28
0x140011a6e      call    fcn.14001119f
0x140011a73      jmp     case.0x140011a65.6
0x140011a75      lea     rcx, str.input_number_is_1 ; 0x14001ad40
0x140011a7c      call    fcn.14001119f
0x140011a81      jmp     case.0x140011a65.6
0x140011a83      lea     rcx, str.input_number_is_2 ; 0x14001ad58
0x140011a8a      call    fcn.14001119f
0x140011a8f      jmp     case.0x140011a65.6
0x140011a91      lea     rcx, str.input_number_is_3 ; 0x14001ad70
0x140011a98      call    fcn.14001119f
0x140011a9d      jmp     case.0x140011a65.6
0x140011a9f      lea     rcx, str.input_number_is_4 ; 0x14001ad88
0x140011aa6      call    fcn.14001119f
0x140011aab      jmp     case.0x140011a65.6
0x140011aad      lea     rcx, str.input_number_is_5 ; 0x14001ada0
0x140011ab4      call    fcn.14001119f
0x140011ab9      jmp     case.0x140011a65.6
0x140011abb      lea     rcx, str.input_number_is_200 ; 0x14001adb8
0x140011ac2      call    fcn.14001119f
0x140011ac7      lea     rcx, str.pause ; 0x14001add4 ; const char *string
0x140011ace      call    qword [system] ; 0x140021310 ; int system(const char *string)


;跳转分支表
0x140011b04      .int32 72295
0x140011b08      .int32 72309
0x140011b0c      .int32 72323
0x140011b10      .int32 72337
0x140011b14      .int32 72351
0x140011b18      .int32 72365
0x140011b1c      .int32 72379
0x140011b20      .int32 72391

```

##### Release

少分支语句
```x86asm
0x140001108      sub     ecx, 1
0x14000110b      je      0x140001148
0x14000110d      sub     ecx, 1
0x140001110      je      0x14000113f
0x140001112      sub     ecx, 1
0x140001115      je      0x140001136
0x140001117      sub     ecx, 1
0x14000111a      je      0x14000112d
0x14000111c      cmp     ecx, 0xc4 ; 196
0x140001122      jne     0x140001154
0x140001124      lea     rcx, str.input_number_is_5
0x14000112b      jmp     0x14000114f
0x14000112d      lea     rcx, str.input_number_is_4
0x140001134      jmp     0x14000114f
0x140001136      lea     rcx, str.input_number_is_3
0x14000113d      jmp     0x14000114f
0x14000113f      lea     rcx, str.input_number_is_2
0x140001146      jmp     0x14000114f
0x140001148      lea     rcx, str.input_number_is_1
0x14000114f      call    fcn.140001020
0x140001154      lea     rcx, str.pause
0x14000115b      call    qword [system]
```

多分支语句
```x86asm
0x140001109      cmp     eax, 0xc8 ; 200
0x14000110e      ja      case.0x140001129.6
0x140001110      lea     rdx, [0x140000000]
0x140001117      movzx   eax, byte [rdx + rax + 0x11b0]
0x14000111f      mov     ecx, dword [rdx + rax*4 + 0x1190]
0x140001126      add     rcx, rdx
0x140001129      jmp     rcx       ; switch table (201 cases) at 0x140001190
0x14000112b      lea     rcx, str.input_number_is_0
0x140001132      jmp     0x140001168
0x140001134      lea     rcx, str.input_number_is_1
0x14000113b      jmp     0x140001168
0x14000113d      lea     rcx, str.input_number_is_2
0x140001144      jmp     0x140001168
0x140001146      lea     rcx, str.input_number_is_3
0x14000114d      jmp     0x140001168
0x14000114f      lea     rcx, str.input_number_is_4
0x140001156      jmp     0x140001168
0x140001158      lea     rcx, str.input_number_is_5
0x14000115f      jmp     0x140001168
0x140001161      lea     rcx, str.input_number_is_200
0x140001168      call    fcn.140001020
0x14000116d      lea     rcx, str.pause ; 0x140002300
0x140001174      call    qword [system] ; 0x140002168

; 分支跳转表
0x140001190      .int32 4395
0x140001194      .int32 4404
0x140001198      .int32 4413
0x14000119c      .int32 4422
0x1400011a0      .int32 4431
0x1400011a4      .int32 4440
0x1400011a8      .int32 4449
0x1400011ac      .int32 4461
```

```x86asm
0x140001104      mov     eax, dword [var_20h]
0x140001108      add     eax, 0xfffffff6
0x14000110b      cmp     eax, 0xbe ; 190
0x140001110      ja      case.0x14000112d.16
0x140001112      lea     rdx, [0x140000000]
0x140001119      cdqe
0x14000111b      movzx   eax, byte [rdx + rax + 0x11b4]
0x140001123      mov     ecx, dword [rdx + rax*4 + 0x1194]
0x14000112a      add     rcx, rdx
0x14000112d      jmp     rcx       ; switch table (191 cases) at 0x140001194
0x14000112f      lea     rcx, str.input_number_is_0 ; 0x140002258
0x140001136      jmp     0x14000116c
0x140001138      lea     rcx, str.input_number_is_1 ; 0x140002270
0x14000113f      jmp     0x14000116c
0x140001141      lea     rcx, str.input_number_is_2 ; 0x140002288
0x140001148      jmp     0x14000116c
0x14000114a      lea     rcx, str.input_number_is_3 ; 0x1400022a0
0x140001151      jmp     0x14000116c
0x140001153      lea     rcx, str.input_number_is_4 ; 0x1400022b8
0x14000115a      jmp     0x14000116c
0x14000115c      lea     rcx, str.input_number_is_5 ; 0x1400022d0
0x140001163      jmp     0x14000116c
0x140001165      lea     rcx, str.input_number_is_200 ; 0x1400022e8
0x14000116c      call    fcn.140001020
;-- default:                       ; from 0x14000112d
0x140001171      lea     rcx, str.pause ; 0x140002300 ; const char *string
0x140001178      call    qword [system] ; 0x140002168 ; int system(const char *string)


;跳转分支表
0x140001194      .int32 4399
0x140001198      .int32 4408
0x14000119c      .int32 4417
0x1400011a0      .int32 4426
0x1400011a4      .int32 4435
0x1400011a8      .int32 4444
0x1400011ac      .int32 4453
0x1400011b0      .int32 4465

```


#### switch多不连续


##### Debug
```x86asm
0x140011a1d      mov     eax, dword [var_4h]
0x140011a20      mov     dword [var_d4h], eax
0x140011a26      cmp     dword [var_d4h], 0x71
0x140011a2d      jg      0x140011a5c
0x140011a2f      cmp     dword [var_d4h], 0x71
0x140011a36      je      0x140011aac
0x140011a38      cmp     dword [var_d4h], 0x11
0x140011a3f      je      0x140011ac8
0x140011a45      cmp     dword [var_d4h], 0x22
0x140011a4c      je      0x140011aba
0x140011a4e      cmp     dword [var_d4h], 0x3d
0x140011a55      je      0x140011a90
0x140011a57      jmp     0x140011ae2
0x140011a5c      cmp     dword [var_d4h], 0xc8
0x140011a66      je      0x140011ad6
0x140011a68      cmp     dword [var_d4h], 0x21c
0x140011a72      je      0x140011a82
0x140011a74      cmp     dword [var_d4h], 0x41c
0x140011a7e      je      0x140011a9e
0x140011a80      jmp     0x140011ae2
0x140011a82      lea     rcx, str.input_number_is_540 ; 0x14001ad28
0x140011a89      call    fcn.14001119f
0x140011a8e      jmp     0x140011ae2
0x140011a90      lea     rcx, str.input_number_is_61 ; 0x14001ad48
0x140011a97      call    fcn.14001119f
0x140011a9c      jmp     0x140011ae2
0x140011a9e      lea     rcx, str.input_number_is_1052 ; 0x14001ad60
0x140011aa5      call    fcn.14001119f
0x140011aaa      jmp     0x140011ae2
0x140011aac      lea     rcx, str.input_number_is_113 ; 0x14001ad80
0x140011ab3      call    fcn.14001119f
0x140011ab8      jmp     0x140011ae2
0x140011aba      lea     rcx, str.input_number_is_34 ; 0x14001ada0
0x140011ac1      call    fcn.14001119f
0x140011ac6      jmp     0x140011ae2
0x140011ac8      lea     rcx, str.input_number_is_17 ; 0x14001adb8
0x140011acf      call    fcn.14001119f
0x140011ad4      jmp     0x140011ae2
0x140011ad6      lea     rcx, str.input_number_is_200 ; 0x14001add0
0x140011add      call    fcn.14001119f
0x140011ae2      lea     rcx, str.pause ; 0x14001adec ; const char *string
0x140011ae9      call    qword [system] ; 0x140021310 ; int system(const char *string)
```

##### Release
```x86asm
0x140001104      mov     eax, dword [var_20h]
0x140001108      cmp     eax, 0x71 ; 113
0x14000110b      jg      0x140001142
0x14000110d      je      0x140001139
0x14000110f      cmp     eax, 0x11 ; 17
0x140001112      je      0x140001130
0x140001114      cmp     eax, 0x22 ; 34
0x140001117      je      0x140001127
0x140001119      cmp     eax, 0x3d ; 61
0x14000111c      jne     0x140001175
0x14000111e      lea     rcx, str.input_number_is_61
0x140001125      jmp     0x140001170
0x140001127      lea     rcx, str.input_number_is_34
0x14000112e      jmp     0x140001170
0x140001130      lea     rcx, str.input_number_is_17
0x140001137      jmp     0x140001170
0x140001139      lea     rcx, str.input_number_is_113
0x140001140      jmp     0x140001170
0x140001142      cmp     eax, 0xc8 ; 200
0x140001147      je      0x140001169
0x140001149      cmp     eax, 0x21c ; 540
0x14000114e      je      0x140001160
0x140001150      cmp     eax, 0x41c ; 1052
0x140001155      jne     0x140001175
0x140001157      lea     rcx, str.input_number_is_1052
0x14000115e      jmp     0x140001170
0x140001160      lea     rcx, str.input_number_is_540
0x140001167      jmp     0x140001170
0x140001169      lea     rcx, str.input_number_is_200
0x140001170      call    fcn.140001020
0x140001175      lea     rcx, str.pause
0x14000117c      call    qword [system]
```

#### switch无break
##### Debug
```x86asm
0x140011a26      mov     eax, dword [var_d4h]
0x140011a2c      dec     eax
0x140011a2e      mov     dword [var_d4h], eax
0x140011a34      cmp     dword [var_d4h], 5
0x140011a3b      ja      case.default.0x140011a55
0x140011a3d      movsxd  rax, dword [var_d4h]
0x140011a44      lea     rcx, [0x140000000]
0x140011a4b      mov     eax, dword [rcx + rax*4 + 0x11adc]
0x140011a52      add     rax, rcx
0x140011a55      jmp     rax       ; switch table (6 cases) at 0x140011adc
0x140011a57      lea     rcx, str.input_number_is_1
0x140011a5e      call    fcn.14001119f
0x140011a63      lea     rcx, str.input_number_is_2
0x140011a6a      call    fcn.14001119f
0x140011a6f      lea     rcx, str.input_number_is_3
0x140011a76      call    fcn.14001119f
0x140011a7b      lea     rcx, str.input_number_is_4
0x140011a82      call    fcn.14001119f
0x140011a87      lea     rcx, str.input_number_is_5
0x140011a8e      call    fcn.14001119f
0x140011a93      lea     rcx, str.input_number_is_6
0x140011a9a      call    fcn.14001119f
0x140011a9f      lea     rcx, str.pause
0x140011aa6      call    qword [system]


;分支跳转表
0x140011adc      .int32 72279
0x140011ae0      .int32 72291
0x140011ae4      .int32 72303
0x140011ae8      .int32 72315
0x140011aec      .int32 72327
0x140011af0      .int32 72339
```

##### Release
```x86asm
0x1400010f4      mov     eax, dword [var_20h]
0x1400010f8      dec     eax
0x1400010fa      cmp     eax, 5    ; 5
0x1400010fd      ja      case.default.0x140001112
0x1400010ff      lea     rdx, [0x140000000]
0x140001106      cdqe
0x140001108      mov     ecx, dword [rdx + rax*4 + 0x117c]
0x14000110f      add     rcx, rdx
;-- switch
0x140001112      jmp     rcx       ; switch table (6 cases) at 0x14000117c
;-- case 1:                        ; from 0x140001112
0x140001114      lea     rcx, str.input_number_is_1 ; 0x14005e548 ; int64_t arg1
0x14000111b      call    fcn.140001020
;-- case 2:                        ; from 0x140001112
0x140001120      lea     rcx, str.input_number_is_2 ; 0x14005e560 ; int64_t arg1
0x140001127      call    fcn.140001020
;-- case 3:                        ; from 0x140001112
0x14000112c      lea     rcx, str.input_number_is_3 ; 0x14005e578 ; int64_t arg1
0x140001133      call    fcn.140001020
;-- case 4:                        ; from 0x140001112
0x140001138      lea     rcx, str.input_number_is_4 ; 0x14005e590 ; int64_t arg1
0x14000113f      call    fcn.140001020
;-- case 5:                        ; from 0x140001112
0x140001144      lea     rcx, str.input_number_is_5 ; 0x14005e5a8 ; int64_t arg1
0x14000114b      call    fcn.140001020
;-- case 6:                        ; from 0x140001112
0x140001150      lea     rcx, str.input_number_is_6 ; 0x14005e5c0 ; int64_t arg1
0x140001157      call    fcn.140001020
;-- default:                       ; from 0x140001112
0x14000115c      lea     rcx, str.pause ; 0x14005e5d4 ; int64_t arg1
0x140001163      call    fcn.140013044


;分支跳转表
0x14000117c      .int32 4372
0x140001180      .int32 4384
0x140001184      .int32 4396
0x140001188      .int32 4408
0x14000118c      .int32 4420
0x140001190      .int32 4432
```

#### switch无default

##### Debug

```x86asm
0x140011a26      mov     eax, dword [var_d4h]
0x140011a2c      dec     eax
0x140011a2e      mov     dword [var_d4h], eax
0x140011a34      cmp     dword [var_d4h], 5
0x140011a3b      ja      case.default.0x140011a55
0x140011a3d      movsxd  rax, dword [var_d4h]
0x140011a44      lea     rcx, [0x140000000]
0x140011a4b      mov     eax, dword [rcx + rax*4 + 0x11ae8]
0x140011a52      add     rax, rcx
0x140011a55      jmp     rax       ; switch table (6 cases) at 0x140011ae8
0x140011a57      lea     rcx, str.input_number_is_1
0x140011a5e      call    fcn.14001119f
0x140011a63      jmp     case.default.0x140011a55
0x140011a65      lea     rcx, str.input_number_is_2
0x140011a6c      call    fcn.14001119f
0x140011a71      jmp     case.default.0x140011a55
0x140011a73      lea     rcx, str.input_number_is_3
0x140011a7a      call    fcn.14001119f
0x140011a7f      jmp     case.default.0x140011a55
0x140011a81      lea     rcx, str.input_number_is_4
0x140011a88      call    fcn.14001119f
0x140011a8d      jmp     case.default.0x140011a55
0x140011a8f      lea     rcx, str.input_number_is_5
0x140011a96      call    fcn.14001119f
0x140011a9b      jmp     case.default.0x140011a55
0x140011a9d      lea     rcx, str.input_number_is_6
0x140011aa4      call    fcn.14001119f
0x140011aa9      lea     rcx, str.pause
0x140011ab0      call    qword [system]


;switch 跳转表
0x140011ae8      .int32 72279
0x140011aec      .int32 72293
0x140011af0      .int32 72307
0x140011af4      .int32 72321
0x140011af8      .int32 72335
0x140011afc      .int32 72349

```

##### Release

```x86asm
0x140001104      mov     eax, dword [var_20h]
0x140001108      dec     eax
0x14000110a      cmp     eax, 5    ; 5
0x14000110d      ja      case.default.0x140001122
0x14000110f      lea     rdx, [0x140000000]
0x140001116      cdqe
0x140001118      mov     ecx, dword [rdx + rax*4 + 0x1180]
0x14000111f      add     rcx, rdx
0x140001122      jmp     rcx       ; switch table (6 cases) at 0x140001180
0x140001124      lea     rcx, str.input_number_is_1
0x14000112b      jmp     0x140001158
0x14000112d      lea     rcx, str.input_number_is_2
0x140001134      jmp     0x140001158
0x140001136      lea     rcx, str.input_number_is_3
0x14000113d      jmp     0x140001158
0x14000113f      lea     rcx, str.input_number_is_4
0x140001146      jmp     0x140001158
0x140001148      lea     rcx, str.input_number_is_5
0x14000114f      jmp     0x140001158
0x140001151      lea     rcx, str.input_number_is_6
0x140001158      call    fcn.140001020
0x14000115d      lea     rcx, str.pause
0x140001164      call    qword [system]


;switch 跳转表
0x140001180      .int32 4388
0x140001184      .int32 4397
0x140001188      .int32 4406
0x14000118c      .int32 4415
0x140001190      .int32 4424
0x140001194      .int32 4433
```

### while循环

Debug
```x86asm
0x14001187b      mov     qword [var_8h], 0
0x140011883      mov     dword [var_24h], 0
0x14001188a      cmp     qword [var_8h], 0x64
0x14001188f      jae     0x1400118a9
0x140011891      movsxd  rax, dword [var_24h]
0x140011895      add     rax, qword [var_8h]
0x140011899      mov     dword [var_24h], eax
0x14001189c      mov     rax, qword [var_8h]
0x1400118a0      inc     rax
0x1400118a3      mov     qword [var_8h], rax
0x1400118a7      jmp     0x14001188a
0x1400118a9      mov     edx, dword [var_24h]
0x1400118ac      lea     rcx, str.result_____d ; 0x140019c28
0x1400118b3      call    fcn.140011190
```

Release
```x86asm
0x140001074      xor     eax, eax
0x140001076      mov     edx, eax
0x140001078      nop     dword [rax + rax]
0x140001080      add     edx, eax
0x140001082      inc     rax
0x140001085      cmp     rax, 0x64 ; 100
0x140001089      jb      0x140001080
0x14000108b      lea     rcx, str.result_____d
0x140001092      call    fcn.140001010

```

### for循环

Debug
```x86asm
0x140011883      mov     dword [var_24h], 0
0x14001188a      mov     qword [var_8h], 0
0x140011892      jmp     0x14001189f
0x140011894      mov     rax, qword [var_8h]
0x140011898      inc     rax
0x14001189b      mov     qword [var_8h], rax
0x14001189f      cmp     qword [var_8h], 0x64
0x1400118a4      jae     0x1400118b3
0x1400118a6      movsxd  rax, dword [var_24h]
0x1400118aa      add     rax, qword [var_8h]
0x1400118ae      mov     dword [var_24h], eax
0x1400118b1      jmp     0x140011894
0x1400118b3      mov     edx, dword [var_24h]
0x1400118b6      lea     rcx, str.result_____d ; 0x140019c28
0x1400118bd      call    fcn.140011190

```

Release
```x86asm
0x140001074      xor     ecx, ecx
0x140001076      mov     eax, ecx
0x140001078      nop     dword [rax + rax]
0x140001080      add     ecx, eax
0x140001082      inc     rax
0x140001085      cmp     rax, 0x64 ; 100
0x140001089      jb      0x140001080
0x14000108b      mov     edx, ecx  ; int64_t arg2
0x14000108d      lea     rcx, str.result_____d
0x140001094      call    fcn.140001010

```

### do while循环

Debug
```x86asm
0x14001187b      mov     qword [var_8h], 0
0x140011883      mov     dword [var_24h], 0
0x14001188a      movsxd  rax, dword [var_24h]
0x14001188e      add     rax, qword [var_8h]
0x140011892      mov     dword [var_24h], eax
0x140011895      mov     rax, qword [var_8h]
0x140011899      inc     rax
0x14001189c      mov     qword [var_8h], rax
0x1400118a0      cmp     qword [var_8h], 0x64
0x1400118a5      jb      0x14001188a
0x1400118a7      mov     edx, dword [var_24h]
0x1400118aa      lea     rcx, str.result_____d
0x1400118b1      call    fcn.140011190
```

Release
```x86asm
0x140001074      xor     eax, eax
0x140001076      mov     edx, eax
0x140001078      nop     dword [rax + rax]
0x140001080      add     edx, eax
0x140001082      inc     rax
0x140001085      cmp     rax, 0x64 ; 100
0x140001089      jb      0x140001080
0x14000108b      lea     rcx, str.result_____d
0x140001092      call    fcn.140001010
```

### 分支嵌套

Debug
```x86asm
x14001197d      cmp     dword [var_4h], 0x41
0x140011981      jl      0x1400119aa
0x140011983      cmp     dword [var_4h], 0x5a
0x140011987      jg      0x1400119aa
0x140011989      cmp     dword [var_4h], 0x58
0x14001198d      jne     0x14001199e
0x14001198f      mov     edx, dword [var_4h]
0x140011992      lea     rcx, str.input_alpha_is:__c
0x140011999      call    fcn.140011195
0x14001199e      lea     rcx, str.You_are_right
0x1400119a5      call    fcn.140011195
0x1400119aa      lea     rcx, str.pause
0x1400119b1      call    qword [system]
```

Release
```x86asm
0x140001104      mov ecx, dword [var_20h]
0x140001108      lea eax, [rcx - 0x41]
0x14000110b      cmp eax, 0x19     ; 25
0x14000110e      ja 0x14000112f
0x140001110      cmp ecx, 0x58     ; 88
0x140001113      jne 0x140001123
0x140001115      mov edx, ecx      ; int64_t arg2
0x140001117      lea rcx, str.input_alpha_is:__c
0x14000111e      call fcn.140001020
0x140001123      lea rcx, str.You_are_right
0x14000112a      call fcn.140001020
0x14000112f      lea rcx, str.pause
0x140001136      call qword [system]
```


### 循环嵌套

Debug
```x86asm
0x14001187b      mov     qword [var_8h], 0
0x140011883      mov     qword [var_28h], 0
0x14001188b      mov     dword [var_44h], 0
0x140011892      cmp     qword [var_8h], 0x64
0x140011897      jae     0x1400118c8
0x140011899      mov     rax, qword [var_8h]
0x14001189d      cmp     qword [var_28h], rax
0x1400118a1      jae     0x1400118bb
0x1400118a3      movsxd  rax, dword [var_44h]
0x1400118a7      add     rax, qword [var_28h]
0x1400118ab      mov     dword [var_44h], eax
0x1400118ae      mov     rax, qword [var_28h]
0x1400118b2      inc     rax
0x1400118b5      mov     qword [var_28h], rax
0x1400118b9      jmp     0x140011899
0x1400118bb      mov     rax, qword [var_8h]
0x1400118bf      inc     rax
0x1400118c2      mov     qword [var_8h], rax
0x1400118c6      jmp     0x140011892
0x1400118c8      mov     edx, dword [var_44h]
0x1400118cb      lea     rcx, str.result_____d ; 0x140019c28
0x1400118d2      call    fcn.140011190

```

Release
```x86asm
0x140001074      xor     eax, eax
0x140001076      mov     edx, eax
0x140001078      mov     ecx, eax
0x14000107a      nop     word [rax + rax]
0x140001080      cmp     rax, rcx
0x140001083      jae     0x14000108f
0x140001085      add     edx, eax
0x140001087      inc     rax
0x14000108a      cmp     rax, rcx
0x14000108d      jb      0x140001085
0x14000108f      inc     rcx
0x140001092      cmp     rcx, 0x64 ; 100
0x140001096      jb      0x140001080
0x140001098      lea     rcx, str.result_____d ; 0x140002240 ; int64_t arg1
0x14000109f      call    fcn.140001010

```
