# read-text-file_save-into-Template

![image](https://user-images.githubusercontent.com/61898376/155881358-8b292a2a-2faf-4250-a3e2-2f6824cd65fe.png)



> 1. txt file
> 2. create struct 
> 3. read txt file-> count line
> 4. make template Array , setSize
> 5. read file 
>>     convert string > CString 
>>     save into struct -> save that struct into templArray
> 6. readBack from templArray
 


---
```
// 220227 readTxtfile save_into templateArr.cpp : 이 파일에는 'main' 함수가 포함됩니다. 거기서 프로그램 실행이 시작되고 종료됩니다.
//

#include "pch.h"
#include "framework.h"
#include "220227 readTxtfile save_into templateArr.h"
#include <fstream>
#include <string>

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// 유일한 애플리케이션 개체입니다.

CWinApp theApp;

using namespace std;


struct POINTstruct
{
    int index;
    CString color;    // not string
    float x;
    float y;
    float z;

    POINTstruct() {};
    POINTstruct(int ind, CString col, float x0, float y0, float z0) 
    {
        index = ind; 
        color = col;
        x = x0;
        y = y0;
        z = z0;
    };
};

int main()
{
    int nRetCode = 0;

    HMODULE hModule = ::GetModuleHandle(nullptr);

    if (hModule != nullptr)
    {
        // MFC를 초기화합니다. 초기화하지 못한 경우 오류를 인쇄합니다.
        if (!AfxWinInit(hModule, nullptr, ::GetCommandLine(), 0))
        {
            // TODO: 여기에 애플리케이션 동작을 코딩합니다.
            wprintf(L"심각한 오류: MFC 초기화 실패\n");
            nRetCode = 1;
        }
        else
        {

           
           
            
            //////    read file, cntLine    ////////////



            ifstream file("point-inputData.txt");
            string line;
            int lineCnt = 0;
            if (file.is_open())
            {
                while (getline(file, line))
                {
                    lineCnt++;
                };
                
                file.close();
            }
            else {
                printf("text file reading error 1 ");
            }
            
            printf("line count :  %d", lineCnt);   




            
            /////    Template Array 준비    ////////////



            CArray <POINTstruct, POINTstruct&> PntTempl;
            PntTempl.SetSize(lineCnt);





            /////   READ FILE , save into variables  /////



            ifstream file_("point-inputData.txt");
            int id_file;
            string color_file;
            float x_file;
            float y_file;
            float z_file;
            int cnt = 0;

            if (file_.is_open())
            {
                
                while (file_ >> id_file >> color_file >> x_file >> y_file >> z_file)
                {
                    //printf("file %d, index = %d, color = %s, x = %f, y= %f, z = %f \n ", cnt, id_file, (color_file).c_str(), x_file, y_file, z_file);
                 


                    // string >> char* >> CString 변환



                    const char* colChar = color_file.c_str();          // string to char*

                    size_t convlen;
                    wchar_t colorCStr[100] = L"";
                    mbstowcs_s(&convlen, colorCStr, 100, colChar, 100);   // char* to CString
                    
                    //_tprintf(colorCStr);
                  






                    // 구조체에 저장



                    POINTstruct pntStruct(id_file, colorCStr, x_file, y_file, z_file);
                    printf("\n %d \n", cnt);
                    printf("[SAVING into STRUCT]       ");

                    printf("index = %d  color = ", pntStruct.index);
                    _tprintf(pntStruct.color);
                    printf("  x = %f  y = %f  z = %f \n", pntStruct.x, pntStruct.y, pntStruct.z);






                    // template 배열에 저장



                    PntTempl[cnt] = pntStruct;
                    printf("[SAVED into TEMPLATE]\n \n");

                    
                    cnt++;
                                     
                }

                file_.close();
            }
            else
            {
                printf("file reading error 2 \n");
            }







            ///// 저장된 template 배열로부터 읽고 디스플레이




            printf("\n[READING BACK FROM 저장된 탬플레이트 배열] \n");
            for (int i = 0; i < PntTempl.GetSize(); i++)
            {
                POINTstruct pst = PntTempl[i];
                
                printf("index = %d  color = ", pst.index);
                _tprintf(pst.color);
                printf("  x = %f  y = %f  z = %f \n", pst.x, pst.y, pst.z);

            }

              
        }
    }
    else
    {
        // TODO: 오류 코드를 필요에 따라 수정합니다.
        wprintf(L"심각한 오류: GetModuleHandle 실패\n");
        nRetCode = 1;
    }

    return nRetCode;
}

```
