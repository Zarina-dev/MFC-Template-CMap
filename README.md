# CArray TEMPLATE
## read text file save into Template 

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
![image](https://user-images.githubusercontent.com/61898376/156090364-fe0450c3-9ffd-46ab-aa90-21296c9b7362.png)


-----
# CMAP = dictionary [간단한 설명: in MFC-C-Cpp proj]
## read txt file -> save it into map(dictionary) -> find values by given key

text file :

![image](https://user-images.githubusercontent.com/61898376/155935453-f3bac098-20d8-4611-a35a-c3de3f443ed6.png)

result :

![image](https://user-images.githubusercontent.com/61898376/155935530-cef61c4d-708b-4d26-988a-8ae0131da2b5.png)



```
// 220228 readTxt_dictionary저장.cpp : 이 파일에는 'main' 함수가 포함됩니다. 거기서 프로그램 실행이 시작되고 종료됩니다.
//

#include "pch.h"
#include "framework.h"
#include "220228 readTxt_dictionary저장.h"
#include <fstream>
#include <afxtempl.h>
#include <string>



#ifdef _DEBUG
#define new DEBUG_NEW
#endif


CWinApp theApp;

using namespace std;

// 3. HashKey => create cmap template [cstring] = uint


template <> UINT AFXAPI HashKey(CString& str)
{
	LPCTSTR key = (LPCTSTR)str;
	return HashKey(key);
}

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
			/////////////////////////////////////////////////////
			// 0. include afxtempl.h(cmap), fstream (file reading)
			// 1. read txt file
			// 2. string >> CString 변환
			// 
			// 3. HashKey => create cmap template [cstring] = uint
			// 4. CMap
			// 5. 읽은 데이터 저장
			//
			// 6. key(name) is given ->  find the value(키) by 
		 // 7. value(키) is given ->  find the key(name)
			///////////////////////////////////////////////////////



		
			// 4. CMap

			CMap <CString, CString&, UINT, UINT&> dict_DB;



			// 1. read txt file


			ifstream file_("person_heigth.txt");
			string name;
			UINT ki, i = 0;
			
			if (file_.is_open())
			{
			
				while (file_ >> name >> ki) 
				{

					// 2. string >> CString 변환


					const char* name_char = name.c_str();
					size_t convByte;
					wchar_t name_cstr[40] = L"";
					mbstowcs_s(&convByte, name_cstr, 40, name_char, 40);

					//_tprintf(_T("%s \n"), name_cstr);



					// 5. 읽은 데이터 저장


					CString name_c = name_cstr;
					dict_DB.SetAt(name_c, ki);
				
				}



				// 6. 저장 데이터 (dictionary == map) 확인
			

				POSITION pos = dict_DB.GetStartPosition();

				printf(" Person      Height\n");


				while (pos != NULL)
				{
					CString person;
					UINT height;
					dict_DB.GetNextAssoc(pos, person, height);
					_tprintf(_T("[%s] : %d\n"), person, height);
				}
				printf("\n==============================================\n");



				// 6. key(name) is given ->  find the value(키) by 


				UINT foundData;
				CString findKey = _T("KimSoon-hee");
				if (dict_DB.Lookup(findKey, foundData))
				{
					
					_tprintf(_T("%s 's found! his height is %d \n\n"), findKey, foundData);
					
				}
				else {
					printf("\nnot found~");
				}



				// 7. value(키) is given ->  find the key(name)

				POSITION pos_find = dict_DB.GetStartPosition();
				UINT  targetKi = 182;  // GuOh-seong

				while (pos_find != NULL)
				{
					CString name;
					UINT ki;
					dict_DB.GetNextAssoc(pos_find, name, ki);
					
					if (ki == targetKi)
					{
						_tprintf(_T("%d is found! it's %s 's height \n\n"), targetKi, name);
						break;
					}
				}


			}
			else {
				cout << "error" << endl;
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
