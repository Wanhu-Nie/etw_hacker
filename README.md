# etw-hacker

我所参考的 [InfinityHook](https://github.com/FiYHer/InfinityHookPro/tree/main) 项目中，ckcl logger context 的定位依赖于 EtwpDebuggerData 特征码扫描定位，函数 `PerfInfoLogSysCallEntry` 的 magic 定位是通过栈帧扫描。
在 **etw-hacker** 项目中，ckcl logger context 的定位通过 Windows 新引入的 silo 机制，即
> (_ESERVERSILO_GLOBALS)PspHostSiloGlobals::EtwSiloState -> (_ETW_SILODRIVERSTATE)EtwpHostSiloState::EtwpLoggerContext[LoggerId] -> (_WMI_LOGGER_CONTEXT)ckcl

其次，由于 system_call 触发 ETW 的路径是固定的，因此可通过 TrapFame 进行快速定位。通过识别栈帧中的 magic 来区分 PerfInfoLogSysCallEntry/PerfInfoLogSysCallExit。

详见博文：https://bbs.kanxue.com/thread-289632.htm
