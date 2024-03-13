#include <windows.h>
#include <stdio.h>

#define ROTR32(value, shift)	(((DWORD) value >> (BYTE) shift) | ((DWORD) value << (32 - (BYTE) shift)))

int strlen_z(wchar_t* str) {
	int num = 0;
	for (size_t i = 0; str[i] != 0; i++)
	{
		num++;
	}
	//需要读取多个字节来构成宽字节
	return 2 * num + 2;
}

int main() {
	char str[] = "LoadLibraryA";
	//kernel32.dll
	wchar_t str1[] = { 'K','E','R','N','E','L','3','2','.','D','L','L',0 };
	DWORD dwModuleHash = 0;
	for (int i = 0; i < strlen_z(str1); i++)
	{
		PCSTR pTempChar = ((PCSTR)str1 + i);
		dwModuleHash = ROTR32(dwModuleHash, 13);
		if (*pTempChar >= 0x61)
		{
			dwModuleHash += (*pTempChar - 0x20);
		}
		else
		{
			dwModuleHash += *pTempChar;
		}
	}
	DWORD Hash = 0;
	for (int j = 0; j < strlen(str); j++)
	{
		int dwFunctionHash = 0;
		char* pTempChar = str;

		do
		{
			dwFunctionHash = ROTR32(dwFunctionHash, 13);
			dwFunctionHash += *pTempChar;
			pTempChar++;
		} while (*(pTempChar - 1) != 0);
		Hash = dwFunctionHash + dwModuleHash;
		dwFunctionHash += dwModuleHash;
	}
	printf("0x%08X", Hash);
}
