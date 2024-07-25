在C++ Windows项目中，使用libuv创建线程并发送消息给窗口涉及以下几个步骤：

1. 安装libuv库。你可以从[libuv GitHub仓库](https://github.com/libuv/libuv)下载预编译的二进制文件或者从源代码编译。

2. 包含必要的头文件。在你的C++源文件中添加以下头文件：

```cpp
#include <iostream>
#include <windows.h>
#include <uv.h>
```

3. 创建一个窗口。为了简化，我们将使用Win32 API创建一个简单的窗口。你可以根据需要创建更复杂的窗口。

```cpp
void create_window() {
    WNDCLASSEX wnd_class = {0};
    wnd_class.cbSize = sizeof(WNDCLASSEX);
    wnd_class.style = CS_HREDRAW | CS_VREDRAW;
    wnd_class.lpfnWndProc = DefWindowProc;
    wnd_class.hInstance = GetModuleHandle(NULL);
    wnd_class.lpszClassName = L"MyWindowClass";
    RegisterClassEx(&wnd_class);

    HWND hWnd = CreateWindowEx(0, L"MyWindowClass", L"My Window", WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT, 640, 480, NULL, NULL, wnd_class.hInstance, NULL);
    if (!hWnd) {
        std::cerr << "Failed to create window." << std::endl;
        exit(EXIT_FAILURE);
    }

    ShowWindow(hWnd, SW_SHOW);
}
```

4. 创建一个libuv线程，在线程中执行任务并将消息发送到窗口。

```cpp
void thread_func(void* arg) {
    // 模拟耗时操作
    Sleep(3000);

    // 发送消息给窗口
    PostMessage(reinterpret_cast<HWND>(arg), WM_USER + 1, 0, 0);
}
```

5. 在主函数中初始化libuv，创建窗口，创建线程并运行事件循环。

```cpp
int main() {
    uv_loop_t* loop = uv_default_loop();

    create_window();

    HWND hWnd = GetForegroundWindow(); // 获取当前活动窗口的句柄

    uv_thread_t thread;
    uv_thread_create(&thread, thread_func, reinterpret_cast<void*>(hWnd));

    uv_run(loop, UV_RUN_DEFAULT);

    uv_thread_join(&thread);

    return 0;
}
```

6. 处理窗口消息。添加一个窗口过程函数来处理发送的消息。

```cpp
LRESULT CALLBACK WndProc(HWND hWnd, UINT msg, WPARAM wParam, LPARAM lParam) {
    switch (msg) {
        case WM_CLOSE:
            PostQuitMessage(0);
            break;
        case WM_USER + 1: // 自定义消息
            MessageBox(hWnd, L"Message from libuv thread!", L"Message", MB_OK);
            break;
        default:
            return DefWindowProc(hWnd, msg, wParam, lParam);
    }
    return 0;
}
```

现在，当你运行程序时，libuv线程将在3秒后发送一条消息给窗口，窗口将弹出一个消息框显示“Message from libuv thread!”。