# Advanced DLL Injector

This is a **powerfull and versatile DLL injector** written in C, designed for injecting dynamic link libraries
(DLLs) into running Windows processes. This tool supports multiple injection methods and process targeting by name, 
making it suitable for advanced  users and exploring process manipulation.

# Features
- **Multiple DLL Support**: Injecting one or more DLLs into a target process in a single execution.
- **Flexible Injection Methods:**
    -*crt: CreateRemoteThread* - Uses **CreateRemoteThread** to load the DLL via *LoadLibraryA*
    -*apc: QueueUserAPC* - Queues asynchronous procedure call to all threads of the target process for stealthier injection.
- **Process Name Targeting:** Specify the target process by name(eg. Notepad.exe) instead of PID, withautomatic PID resolution.
- **DLL Load Verification:** Check if the injected DLL is successfully loaded into the target process's module list.
- **Robust Error Handling:** Provides detailed error messages with Windows error codes for troubleshooting.
- **Coloured Output:** Uses ANSI color codes for clear, readable logging.
- **Timeout Protection:** Implements a 30-second timeout for CRT injectionto prevent hangs.
- **Resource Management** Ensures proper cleanup of handles and allocated memory.

# Compilation
To compile the injector, you will need a C compiler like minGW GCC or Microsoft Visual C++.

# Using GCC (MinGW):

`gcc -o hawkeye.exe hawkeye.c -lpsapi`

# Using Visual Studio:

`cl hawkeye.c /link psapi.lib`

# Usage

`hawkeye.exe <method> <process_name> <dll_path> [ dll_path2 ... ]

# Arguments:
- method: Injection method to use
    - *crt: CreateRemoteThread*
    - *apc: QueueUserAPC*
- process_name: Name of the target process (notepad.exe)
- dll_path: Full path to the DLL(s) to inject.

# Example
  -  inject a single DLL using *CreateRemoteThread*  hawkeye.exe crt notepad.exe C:\path\to\mydll.dll

  - Inject multiple DLLs using QueuesUseAPC hawkeye.exe apc explorer.exe C:\path\to\dll1.dll C:\path\to\dll2.dll

# Injection Method Details
**CreateRemoteThread (**
