Golang - 使用CSharp调用Go
========================

```C#
using System;
using System.IO;
using System.Windows.Forms;
using System.Drawing;
using System.Threading;
using System.Runtime.InteropServices;

namespace WindowsFormsApp
{

    public class GoUtils
    {
        public const string DLL_NAME = "golib.dll";

        [DllImport(DLL_NAME, EntryPoint = "PrintHelloWorld", CallingConvention = CallingConvention.Cdecl, ExactSpelling = false)]
        public static extern void PrintHelloWorld();

        [DllImport(DLL_NAME, EntryPoint = "PrintMsg", CallingConvention = CallingConvention.Cdecl, ExactSpelling = false)]
        public static extern void PrintMsg(byte[] msg);

        [DllImport(DLL_NAME, EntryPoint = "Sum", CallingConvention = CallingConvention.Cdecl, ExactSpelling = false)]
        public static extern int Sum(int a, int b);

    }

    static class Program
    {


        static EventWaitHandle _waitHandle = new AutoResetEvent(false);

        /// <summary>
        /// 应用程序的主入口点。
        /// </summary>
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);

            var read =  File.OpenRead("icon.ico");
            var image = Image.FromStream(read);
            read.Close();
            var ico = Icon.FromHandle(((Bitmap)image).GetHicon());

            var m =  new ContextMenu();
            m.MenuItems.Add("Test1", MenuTest1_Click);
            m.MenuItems.Add("Test2", MenuTest2_Click);
            m.MenuItems.Add("Test3", MenuTest3_Click);

            var notifyIcon = new NotifyIcon();
            notifyIcon.Icon = ico;
           
            notifyIcon.ContextMenu = m;

            notifyIcon.Visible = true;

            MessageBox.Show(GoUtils.Sum(1, 2).ToString());

            _waitHandle.WaitOne();
        }

        static void MenuTest1_Click(object sender, EventArgs e)
        {
            _waitHandle.Set();
        }

        static void MenuTest2_Click(object sender, EventArgs e)
        {
            _waitHandle.Set();
        }

        static void MenuTest3_Click(object sender, EventArgs e)
        {
            _waitHandle.Set();
        }
    }
}

```



```go
package main
 
import "C"
import (
	"fmt"
)
 
//export PrintHelloWorld
func PrintHelloWorld() {
	fmt.Println("Hello World!")
}
 
//export PrintMsg
func PrintMsg(msg *C.char) {
	// 输出参数
	var str string
	str = C.GoString(msg)
	fmt.Println(str)
}
 
//export Sum
func Sum(a, b int) int {
	return a + b
}
 
func main() {
	// bytes := []rune("124")
	// fmt.Println(string(bytes))
}
```

