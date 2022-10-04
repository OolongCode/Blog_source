---
title: "MFC基础 笔记"
date: 2022-02-22T18:30:22+08:00
draft: false
math: true
tags: ["Windows",“Note"]
categories: ["develop"]
toc: true
---

# MFC
默认UNICODE编码

## CTIME类
获取时间的类对象，可以通过类方法获取时间

使用方法
```cpp
CTime m_Time = CTime::GetCurrentTime();
int nYear = m_Time.GetYear();
int nMonth = m_Time.GetMonth();
int nDay = m_Time.GetDay();
int nHour = m_Time.GetHour();
int nMinute = m_Time.GetMinute();
int nSecond = m_Time.GetSecond();

```

## MFC三种开发模式
1. SDK --> Win32
2. MFC --> MFC
3. 托管 --> CLR

## 字符串
ASCII `char CHAR`
UNICODE `wchar_t WCHAR`
T `TCHAR`

### CString类
定义与初始化
```cpp
CString str(L"大大大");

CString str1;
str1 = L"小小小";

CString str2 = str1;

CString str3 = L'A';

CString str4(L"A",66);
```

格式化字符串
```cpp
CString str;
int nFlag = 100;
char strz[] = "Hello World!";
str.Format(L"char = %S int = %d", strz, nFlag);

```

字符串长度获取
```cpp
int count = str.GetLength();
```

判断字符串为空
```cpp
bool flag = str.IsEmpty();
```

字符串拼接
```cpp
CString A(L"A");
CString B(L"B");
A = A + B;
```

切片操作
```cpp
CString A(L"ABCDEFGH");
CString B = A.Left(2);
CString C = A.Mid(2, 2);
CString D = A.Right(2);

```

删除操作
```cpp
CString A(L"ABCDEFGH");
A.Remove(L"A");
```

获取字符串
```cpp
CString A(L"ABCDEFGH");
LPTSTR TSTR = A.GetBuffer();
```
## Windows消息基础

消息的来源
- 由操作系统产生
- 用户触发事件转换
- 由另一个消息产生

消息的定义

| 前缀 | 说明         |
| ---- | ------------ |
| WM   | 普通窗口     |
| BM   | 按钮         |
| CB   | 组合框       |
| CDM  | 通用对话框   |
| DBT  | 设备信息     |
| DL   | 下拉列表     |
| EM   | 编辑框       |
| HKM  | 热键         |
| IPM  | IP控件       |
| LB   | 列表框       |
| LVM  | 列表视图     |
| MCM  | 日历控件     |
| PBM  | 进度条       |
| PSM  | 属性         |
| RB   | 伸缩条       |
| SB   | 状态栏       |
| STM  | 静态栏       |
| TB   | 工具条       |
| TBM  | 跟踪条       |
| TCM  | 标签控件     |
| TVM  | 树视图       |
| UDM  | 微调按钮控件 |

MSG结构体
```c
typedef struct tagMSG{
    HWND hwnd;
    UINT message;
    WPARAM wParam;
    LPARAM lParam;
    DWORD time;
    POINT pt;
#ifdef _MAC
	DWORD lPrivate;
#endif
} 	MSG, *PMSG, NEAR *NPMSG, FAR *LPMSG;
```

### 预定义消息
#### 窗口消息
`WM_CREATE` 创建窗口
`WM_PAINT` 绘制窗口
`WM_DESTROY` 销毁窗口
`WM_KILLFOCUS` 失去焦点
`WM_SETFOCUS` 获得焦点
`WM_MEASUREITEM` 
`WM_DRAWTEM`

命令消息
`WM_COMMAND`

#### 控件通知消息
`WM_xxxx`
- `WM_PARENTNOTIFY`
- `WM_CTLCOLOR` ...
- `WM_VSCROLL/WM_HSCROLL`

按钮控件
组合框控件
编辑框控件
列表框控件

`WM_NOTIFY`
控件绘制结构体

```c
typedef struct tagDRAWITEMSTRUCT {
    UINT        CtlType;
    UINT        CtlID;
    UINT        itemID;
    UINT        itemAction;
    UINT        itemState;
    HWND        hwndItem;
    HDC         hDC;
    RECT        rcItem;
    ULONG_PTR   itemData;
} DRAWITEMSTRUCT, NEAR *PDRAWITEMSTRUCT, FAR *LPDRAWITEMSTRUCT;
```

```c
typedef struct tagNMHDR
{
    HWND      hwndFrom;
    UINT_PTR  idFrom;
    UINT      code;         // NM_ code
}   NMHDR;
```

```c
typedef struct tagLVKEYDOWN
{
    NMHDR hdr;
    WORD wVKey;
    UINT flags;
} NMLVKEYDOWN, *LPNMLVKEYDOWN;
```

### 自定义消息
```c
#define MY_MSG WM_USER+1
```

使用方法
`SendMessage` 同步方式
`PostMessage` 异步方式


## Win32(SDK)

1. WinMain
2. MSG结构体
3. 注册窗口
4. 创建窗口
5. 显示窗口
6. 刷新窗口
7. 消息循环
8. WindowProc

```cpp
// WindowsSDK.cpp : 定义应用程序的入口点。
//

#include "framework.h"
#include "WindowsSDK.h"

#define MAX_LOADSTRING 100

// 全局变量:
HINSTANCE hInst;                                // 当前实例
WCHAR szTitle[MAX_LOADSTRING];                  // 标题栏文本
WCHAR szWindowClass[MAX_LOADSTRING];            // 主窗口类名

// 此代码模块中包含的函数的前向声明:
ATOM                MyRegisterClass(HINSTANCE hInstance);
BOOL                InitInstance(HINSTANCE, int);
LRESULT CALLBACK    WndProc(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    About(HWND, UINT, WPARAM, LPARAM);

int APIENTRY wWinMain(_In_ HINSTANCE hInstance,
                     _In_opt_ HINSTANCE hPrevInstance,
                     _In_ LPWSTR    lpCmdLine,
                     _In_ int       nCmdShow)
{
    UNREFERENCED_PARAMETER(hPrevInstance);
    UNREFERENCED_PARAMETER(lpCmdLine);

    // TODO: 在此处放置代码。

    // 初始化全局字符串
    LoadStringW(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
    LoadStringW(hInstance, IDC_WINDOWSSDK, szWindowClass, MAX_LOADSTRING);
    MyRegisterClass(hInstance);

    // 执行应用程序初始化:
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_WINDOWSSDK));

    MSG msg;

    // 主消息循环:
    while (GetMessage(&msg, nullptr, 0, 0))
    {
        if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }
    }

    return (int) msg.wParam;
}



//
//  函数: MyRegisterClass()
//
//  目标: 注册窗口类。
//
ATOM MyRegisterClass(HINSTANCE hInstance)
{
    WNDCLASSEXW wcex;

    wcex.cbSize = sizeof(WNDCLASSEX);

    wcex.style          = CS_HREDRAW | CS_VREDRAW;
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_WINDOWSSDK));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_WINDOWSSDK);
    wcex.lpszClassName  = szWindowClass;
    wcex.hIconSm        = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

    return RegisterClassExW(&wcex);
}

//
//   函数: InitInstance(HINSTANCE, int)
//
//   目标: 保存实例句柄并创建主窗口
//
//   注释:
//
//        在此函数中，我们在全局变量中保存实例句柄并
//        创建和显示主程序窗口。
//
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // 将实例句柄存储在全局变量中

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}

//
//  函数: WndProc(HWND, UINT, WPARAM, LPARAM)
//
//  目标: 处理主窗口的消息。
//
//  WM_COMMAND  - 处理应用程序菜单
//  WM_PAINT    - 绘制主窗口
//  WM_DESTROY  - 发送退出消息并返回
//
//
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
    case WM_COMMAND:
        {
            int wmId = LOWORD(wParam);
            // 分析菜单选择:
            switch (wmId)
            {
            case IDM_ABOUT:
                DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
                break;
            case IDM_EXIT:
                DestroyWindow(hWnd);
                break;
            default:
                return DefWindowProc(hWnd, message, wParam, lParam);
            }
        }
        break;
    case WM_PAINT:
        {
            PAINTSTRUCT ps;
            HDC hdc = BeginPaint(hWnd, &ps);
            // TODO: 在此处添加使用 hdc 的任何绘图代码...
            EndPaint(hWnd, &ps);
        }
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// “关于”框的消息处理程序。
INT_PTR CALLBACK About(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}

```

## 键盘消息

与key有关的

相关代码
```cpp
BOOL CMFCKeyDlg::PreTranslateMessage(MSG* pMsg)
{
	// TODO: 在此添加专用代码和/或调用基类
	SHORT nShift = GetKeyState(VK_SHIFT);

	switch (pMsg->message)
	{
	case WM_KEYDOWN:
	{

		switch (pMsg->wParam)
		{
		case VK_F2:
		{
			if (nShift < 0)
			{
				AfxMessageBox(L"F2 + shift Message!");
				break;
			}
			AfxMessageBox(L"F2 Message!");
			break;
		}
		case 0x41:
		{
			AfxMessageBox(L"A Message!");
			break;
		}
		case VK_ESCAPE:
			return 0;
		case VK_RETURN:
			return 0;
		default:
			break;
		}
		break;
	}
			
	default:
		break;
	}
	return CDialogEx::PreTranslateMessage(pMsg);
}
```

虚拟键码
- `VK_xxx`
- ASCII码

虚拟键码表

| 虚拟键码               | 对应值 | 对应键       |
| ---------------------- | ------ | ------------ |
| VK_LBUTTON             | 1      | 鼠标左键     |
| VK_RBUTTON             | 2      | 鼠标右键     |
| VK_CANCEL              | 3      | Cancel       |
| VK_MBUTTON             | 4      | 鼠标中键     |
| VK_XBUTTON1            | 5      |              |
| VK_XBUTTON2            | 6      |              |
| VK_BACK                | 8      | Backspace    |
| VK_TAB                 | 9      | Tab          |
| VK_CLEAR               | 12     | Clear        |
| VK_RETURN              | 13     | Enter        |
| VK_SHIFT               | 16     | Shift        |
| VK_CONTROL             | 17     | Ctrl         |
| VK_MENU                | 18     | Alt          |
| VK_PAUSE               | 19     | Pause        |
| VK_CAPITAL             | 20     | Caps Lock    |
| VK_KANA                | 21     |              |
| VK_HANGUL              | 21     |              |
| VK_JUNJA               | 23     |              |
| VK_FINAL               | 24     |              |
| VK_HANJA               | 25     |              |
| VK_KANJI               | 25*    |              |
| VK_ESCAPE              | 27     | Esc          |
| VK_CONVERT             | 28     |              |
| VK_NONCONVERT          | 29     |              |
| VK_ACCEPT              | 30     |              |
| VK_MODECHANGE          | 31     |              |
| VK_SPACE               | 32     | Space        |
| VK_PRIOR               | 33     | Page Up      |
| VK_NEXT                | 34     | Page Down    |
| VK_END                 | 35     | End          |
| VK_HOME                | 36     | Home         |
| VK_LEFT                | 37     | Left Arrow   |
| VK_UP                  | 38     | Up Arrow     |
| VK_RIGHT               | 39     | Right Arrow  |
| VK_DOWN                | 40     | Down Arrow   |
| VK_SELECT              | 41     | Select       |
| VK_PRINT               | 42     | Print        |
| VK_EXECUTE             | 43     | Execute      |
| VK_SNAPSHOT            | 44     | Snapshot     |
| VK_INSERT              | 45     | Insert       |
| VK_DELETE              | 46     | Delete       |
| VK_HELP                | 47     | Help         |
|                        | 48     | 0            |
|                        | 49     | 1            |
|                        | 50     | 2            |
|                        | 51     | 3            |
|                        | 52     | 4            |
|                        | 53     | 5            |
|                        | 54     | 6            |
|                        | 55     | 7            |
|                        | 56     | 8            |
|                        | 57     | 9            |
|                        | 65     | A            |
|                        | 66     | B            |
|                        | 67     | C            |
|                        | 68     | D            |
|                        | 69     | E            |
|                        | 70     | F            |
|                        | 71     | G            |
|                        | 72     | H            |
|                        | 73     | I            |
|                        | 74     | J            |
|                        | 75     | K            |
|                        | 76     | L            |
|                        | 77     | M            |
|                        | 78     | N            |
|                        | 79     | O            |
|                        | 80     | P            |
|                        | 81     | Q            |
|                        | 82     | R            |
|                        | 83     | S            |
|                        | 84     | T            |
|                        | 85     | U            |
|                        | 86     | V            |
|                        | 87     | W            |
|                        | 88     | X            |
|                        | 89     | Y            |
|                        | 90     | Z            |
| VK_LWIN                | 91     |              |
| VK_RWIN                | 92     |              |
| VK_APPS                | 93     |              |
| VK_SLEEP               | 95     |              |
| VK_NUMPAD0             | 96     | 小键盘 0     |
| VK_NUMPAD1             | 97     | 小键盘 1     |
| VK_NUMPAD2             | 98     | 小键盘 2     |
| VK_NUMPAD3             | 99     | 小键盘 3     |
| VK_NUMPAD4             | 100    | 小键盘 4     |
| VK_NUMPAD5             | 101    | 小键盘 5     |
| VK_NUMPAD6             | 102    | 小键盘 6     |
| VK_NUMPAD7             | 103    | 小键盘 7     |
| VK_NUMPAD8             | 104    | 小键盘 8     |
| VK_NUMPAD9             | 105    | 小键盘 9     |
| VK_MULTIPLY            | 106    | 小键盘 *     |
| VK_ADD                 | 107    | 小键盘 +     |
| VK_SEPARATOR           | 108    | 小键盘 Enter |
| VK_SUBTRACT            | 109    | 小键盘 -     |
| VK_DECIMAL             | 110    | 小键盘 .     |
| VK_DIVIDE              | 111    | 小键盘 /     |
| VK_F1                  | 112    | F1           |
| VK_F2                  | 113    | F2           |
| VK_F3                  | 114    | F3           |
| VK_F4                  | 115    | F4           |
| VK_F5                  | 116    | F5           |
| VK_F6                  | 117    | F6           |
| VK_F7                  | 118    | F7           |
| VK_F8                  | 119    | F8           |
| VK_F9                  | 120    | F9           |
| VK_F10                 | 121    | F10          |
| VK_F11                 | 122    | F11          |
| VK_F12                 | 123    | F12          |
| VK_F13                 | 124    |              |
| VK_F14                 | 125    |              |
| VK_F15                 | 126    |              |
| VK_F16                 | 127    |              |
| VK_F17                 | 128    |              |
| VK_F18                 | 129    |              |
| VK_F19                 | 130    |              |
| VK_F20                 | 131    |              |
| VK_F21                 | 132    |              |
| VK_F22                 | 133    |              |
| VK_F23                 | 134    |              |
| VK_F24                 | 135    |              |
| VK_NUMLOCK             | 144    | Num Lock     |
| VK_SCROLL              | 145    | Scroll       |
| VK_LSHIFT              | 160    |              |
| VK_RSHIFT              | 161    |              |
| VK_LCONTROL            | 162    |              |
| VK_RCONTROL            | 163    |              |
| VK_LMENU               | 164    |              |
| VK_RMENU               | 165    |              |
| VK_BROWSER_BACK        | 166    |              |
| VK_BROWSER_FORWARD     | 167    |              |
| VK_BROWSER_REFRESH     | 168    |              |
| VK_BROWSER_STOP        | 169    |              |
| VK_BROWSER_SEARCH      | 170    |              |
| VK_BROWSER_FAVORITES   | 171    |              |
| VK_BROWSER_HOME        | 172    |              |
| VK_VOLUME_MUTE         | 173    | VolumeMute   |
| VK_VOLUME_DOWN         | 174    | VolumeDown   |
| VK_VOLUME_UP           | 175    | VolumeUp     |
| VK_MEDIA_NEXT_TRACK    | 176    |              |
| VK_MEDIA_PREV_TRACK    | 177    |              |
| VK_MEDIA_STOP          | 178    |              |
| VK_MEDIA_PLAY_PAUSE    | 179    |              |
| VK_LAUNCH_MAIL         | 180    |              |
| VK_LAUNCH_MEDIA_SELECT | 181    |              |
| VK_LAUNCH_APP1         | 182    |              |
| VK_LAUNCH_APP2         | 183    |              |
| VK_OEM_1               | 186    | ; :          |
| VK_OEM_PLUS            | 187    | = +          |
| VK_OEM_COMMA           | 188    |              |
| VK_OEM_MINUS           | 189    | - _          |
| VK_OEM_PERIOD          | 190    |              |
| VK_OEM_2               | 191    | / ?          |
| VK_OEM_3               | 192    | \` ~         |
| VK_OEM_4               | 219    | \[ \{        |
| VK_OEM_5               | 220    | \\ \|        |
| VK_OEM_6               | 221    | ] }          |
| VK_OEM_7               | 222    | ' "          |
| VK_OEM_8               | 223    |              |
| VK_OEM_102             | 226    |              |
| VK_PACKET              | 231    |              |
| VK_PROCESSKEY          | 229    |              |
| VK_ATTN                | 246    |              |
| VK_CRSEL               | 247    |              |
| VK_EXSEL               | 248    |              |
| VK_EREOF               | 249    |              |
| VK_PLAY                | 250    |              |
| VK_ZOOM                | 251    |              |
| VK_NONAME              | 252    |              |
| VK_PA1                 | 253    |              |
| VK_OEM_CLEAR           | 254    |              |


## 鼠标消息
与BUTTON有关
L左键
M中键
R右键

相关代码
```cpp
switch (pMsg->message)
	{
	case WM_LBUTTONDOWN:
	{
		AfxMessageBox(L"客户区鼠标左键点击");
		break;
	}
	case WM_NCLBUTTONDOWN:
	{
		AfxMessageBox(L"非客户区鼠标左键点击");
		break;
	}
	case WM_LBUTTONDBLCLK:
	{
		AfxMessageBox(L"客户区鼠标左键双击");
		break;
	}
	case WM_NCLBUTTONDBLCLK:
	{
		AfxMessageBox(L"非客户区鼠标左键双击");
		break;
	}
	case WM_LBUTTONUP:
	{
		AfxMessageBox(L"鼠标左键被释放");
		break;
	}
	default:
		break;
	}

```

## Windows控制台

```cpp
#include <Windows.h>

int main()
{
	DWORD dwCount;
	HANDLE hStd = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(hStd, FOREGROUND_RED | FOREGROUND_BLUE | BACKGROUND_BLUE);
	WriteConsole(hStd, L"Hello World!", wcslen(L"Hello World!"), &dwCount, NULL);
	SetConsoleTitle(L"Title");
	return 0;
}
```

## 内存申请与释放

`HeapAlloc` 内存申请
`HeapFree` 内存释放

相关代码
```cpp
HANDLE hHeap = GetProcessHeap();
char * str = (char*)HeapAlloc(hHeap, HEAP_ZERO_MEMORY, 0x100);
HeapFree(hHeap, 0, str);
```

## 文件操作

### Win32
查看官方文档学习
```cpp
#include <Windows.h>

int main()
{
	HANDLE hFile = CreateFile(L"C:\\Users\\15890\\Desktop\\Test.txt",GENERIC_ALL,NULL,NULL,OPEN_EXISTING,FILE_ATTRIBUTE_NORMAL,NULL);
	if (INVALID_HANDLE_VALUE == hFile)
	{
		MessageBox(0, L"CreateFile Failed!", L"Error", MB_OK);
		ExitProcess(0);
		system("pause");
	}
	DWORD dwFileLen = GetFileSize(hFile, NULL);
	PTCHAR szBuffer;
	HANDLE hHeap = GetProcessHeap();
	szBuffer = (PTCHAR)HeapAlloc(hHeap, HEAP_ZERO_MEMORY, dwFileLen);
	DWORD dwReadSize;
	ReadFile(hFile, szBuffer, dwFileLen,&dwReadSize, NULL);
	CHAR szWriteBuffer[] = "Hello Windows API";
	DWORD dwWriteSize;
	WriteFile(hFile, szWriteBuffer,strlen(szWriteBuffer), &dwWriteSize, NULL);
	HeapFree(hHeap, 0, szBuffer);
	CloseHandle(hFile);
	return 0;
}
```

### CFile类
官方文档非常详细
```cpp
CFile cFile;
cFile.Open(L"C:\\Users\\15890\\Desktop\\Test.txt", CFile::modeReadWrite);
DWORD dwFileLen = cFile.GetLength();
PTCHAR szBuffer;
HANDLE hHeap = GetProcessHeap();
szBuffer = (PTCHAR)HeapAlloc(hHeap, HEAP_ZERO_MEMORY, dwFileLen);
cFile.Read(szBuffer, dwFileLen);
CHAR pbufWrite[] = "Hello CFile";
cFile.Write(pbufWrite, strlen(pbufWrite));
cFile.Flush();
```

## 消息框

`MessageBox` Win32
```cpp
DWORD ret = MessageBox(L"Hello World!", L"Title", MB_YESNO);
if (ret == IDYES)
{
	MessageBox(L"Yes", L"Tip", MB_OK);
}
else if(ret == IDNO)
{
	MessageBox(L"No", L"Tip", MB_OK);
}

```

`AfxMessageBox` MFC
```cpp
DWORD ret = AfxMessageBox(L"Hello World!", MB_YESNO);
if (ret == IDYES)
{
	AfxMessageBox(L"Yes", MB_OK);
}
else if (ret == IDNO)
{
	AfxMessageBox(L"No", MB_OK);
}
```

## 对话框
### 模态对话框
会占用主对话框

相关代码
```cpp
void CMFCAppModDlg::OnBnClickedButton1()
{
	// TODO: 在此添加控件通知处理程序代码
	CDialogMod dlg;
	dlg.DoModal();
}

```

### 非模态对话框
不会占用主对话框

相关代码
```cpp
void CMFCAppModDlg::OnBnClickedButton2()
{
	// TODO: 在此添加控件通知处理程序代码
	CDialogModLess* m_Dlg = NULL;
	if (!m_Dlg)
	{
		m_Dlg = new CDialogModLess;
		m_Dlg->Create(IDD_DIALOG2,this);
		m_Dlg->ShowWindow(SW_SHOW);
	}
}

```

### 通用对话框-文件选择

相关代码
```cpp
CFileDialog m_dlg(TRUE, NULL, NULL, NULL, L"文本文件|*.txt|可执行文件|*.exe|所有文件|*.*||", this);
m_dlg.DoModal();
CString szPath = m_dlg.GetPathName();
if(!szPath.IsEmpty())
	AfxMessageBox(szPath);
```

## 状态栏

相关代码
```cpp
m_StatusBar.Create(this);
UINT arr[] = { 1,2,3 };
m_StatusBar.SetIndicators(arr, 3);
m_StatusBar.SetPaneInfo(0, arr[0], SBPS_NORMAL, 100);
m_StatusBar.SetPaneInfo(1, arr[1], SBPS_NORMAL, 100);
m_StatusBar.SetPaneInfo(2, arr[2], SBPS_NORMAL, 100);
m_StatusBar.SetPaneText(0, L"Text1");
m_StatusBar.SetPaneText(1, L"Text2");
m_StatusBar.SetPaneText(2, L"Text3");
RepositionBars(AFX_IDW_CONTROLBAR_FIRST, AFX_IDW_CONTROLBAR_LAST, 0);

```

## 控件

常用控件
- 单选控件
- 复选控件
- 按钮控件
- 静态文本控件
- 编辑控件
- 列表控件
- 下拉框控件
- 进度条控件
- 滑块控件
- IP控件
- 树状控件
- __tab控件__ 

(可参考微软官方文档进行学习)

### tab控件

初始化
```cpp
m_Tab.InsertItem(0, L"Text");
m_Tab.InsertItem(1, L"List");
m_Tab.InsertItem(2, L"Tree");
m_Tab.InsertItem(3, L"Other");

m_dlgText.Create(IDD_DIALOG1, &m_Tab);
m_dlgList.Create(IDD_DIALOG2, &m_Tab);
m_dlgTree.Create(IDD_DIALOG3, &m_Tab);
m_dlgOther.Create(IDD_DIALOG4, &m_Tab);

CRect rs;
m_Tab.GetClientRect(rs);
rs.top += 20;
m_dlgText.MoveWindow(rs);
m_dlgList.MoveWindow(rs);
m_dlgTree.MoveWindow(rs);
m_dlgOther.MoveWindow(rs);
m_dlgText.ShowWindow(SW_SHOW);

```

使用
```cpp
int nCurSel;
nCurSel = m_Tab.GetCurSel();
switch (nCurSel)
{
case 0:
{
	m_dlgText.ShowWindow(SW_SHOW);
	m_dlgList.ShowWindow(SW_HIDE);
	m_dlgTree.ShowWindow(SW_HIDE);
	m_dlgOther.ShowWindow(SW_HIDE);
	break;
}
case 1:
{
	m_dlgText.ShowWindow(SW_HIDE);
	m_dlgList.ShowWindow(SW_SHOW);
	m_dlgTree.ShowWindow(SW_HIDE);
	m_dlgOther.ShowWindow(SW_HIDE);
	break;
}
case 2:
{
	m_dlgText.ShowWindow(SW_HIDE);
	m_dlgList.ShowWindow(SW_HIDE);
	m_dlgTree.ShowWindow(SW_SHOW);
	m_dlgOther.ShowWindow(SW_HIDE);
	break;
}
case 3:
{
	m_dlgText.ShowWindow(SW_HIDE);
	m_dlgList.ShowWindow(SW_HIDE);
	m_dlgTree.ShowWindow(SW_HIDE);
	m_dlgOther.ShowWindow(SW_SHOW);
	break;
}
default:
	break;
}
```

### 热键

相关代码

注册
```cpp
RegisterHotKey(m_hWnd, HOT_KEY_MESSAGEBOX, MOD_WIN, VK_F2);
```

使用
```cpp
void CMFCAppUseDlg::OnHotKey(UINT nHotKeyId, UINT nKey1, UINT nKey2)
{
	// TODO: 在此添加消息处理程序代码和/或调用默认值
	switch (nHotKeyId)
	{
	case HOT_KEY_MESSAGEBOX:
	{
		AfxMessageBox(
			L"\u2764\u2721\u262a\u2600\u2602\u2622\u2648\u2649\u264a\u264b\u264c\u264d"
			L"\u264e\u264f\u2650\u2651\u2652\u2653\u2690\u2691\u26c4\u2620\u27bc\u2200"
		);
		break;
	}
	default:
		break;
	}
	CDialogEx::OnHotKey(nHotKeyId, nKey1, nKey2);
}
```
