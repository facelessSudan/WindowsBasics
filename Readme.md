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

`hawkeye.exe <method> <process_name> <dll_path> [ dll_path2 ... ]`

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
**CreateRemoteThread (CRT)**
1. Allocating memory in the target process.
2. Writing the DLL path to the allocated memory
3. Creating a remote thread that calls *LoadLibraryA* with the DLL path.
4. Waiting for the thread to finish execution (with timeout)
5. Verifying that the DLL was loaded succesfully.

**caveat**
-This method is reliable but more detectable by security software.

**QueueUserAPC (APC)**
The QueueUserAPC method works by:
1. Allocating memory in the target process.
2. Writing the DLL path to the allocated memory.
3. Finding all threads in the target process.
4. Queuing  an APC to each thread to call LoadLibraryA
5. Verifying that the DLL was loaded succesfully.

**N/B**
-This method is more stealthy as it doesn't create new threads, but requires the target thread to enter an alertable state to trigger the DLL loading.

# Logging
The injector uses colored console output to provide clear logging:
[DEBUG]: Blue - Detailed debugging information
[INFO]: Green - General information messages
[WARN]: Yellow - warning messsages that don't prevent operation
[ERROR]: Red - Indicate failure.

# Security Considerations
This tool is intended for educational purposes, debugging, and legitimate software development tasks.
Misuse of this tool may violate computer security laws and cybersecurity laws put in place in your counrty.
Always ensure you have proper authorization before injecting DLLs into processes you don't own.

# Extensions
The injector can be extended to support additional injection methods, and therefor its open for extention.
Meethods for extension includes but not limited to:
    - Manual Mapping: Implementing custom PE loader to manually map the DLL.
    - Thread Hijacking: Suspending an existing thread and modifying its context
    - SetWindowsHookEx: Using Windows hook for code injection.
    -RtlCreatUserThread/NtCreateThreadEx: Using undocumented NT APIs.

# License
This project is provides for educational purposes only. Use at your own risk.

# Disclaimer:
The author is not responsible for any misuse or damage caused by the software. Am also a leaner experimenting as I develop. 
Use it on systems that you own
    
