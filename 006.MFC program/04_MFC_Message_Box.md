## Message Box

* Message Box
	* 간단한 메시지를 출력할때 사용
	* AfxMessageBox()를 이용
	* MB_OK : 확인 버튼만 나옴
	* MB_OKCANCEL : 확인/취소 버튼이 나옴
	* MB_IOCNASTERISK : (I) 메시지까지 메시지 창에 나옴
	* MB_ICONEXCLAMATION

* Message Box 버튼 스타일

```
	+---------------------+-------------------------------------------------
	|        type         |    Description
	+---------------------+-------------------------------------------------
	| MB_OK               |	확인 버튼 (Default)
	| MB_OKCANCEL         |	확인/취소
	| MB_YESNO            | 예/아니오
	| MB_YESNOCANCEL      | 예/아니오/취소
	| MB_RETRYCANCEL      | 재시도/취소
	| MB_ABORTRETRYIGNORE | 취소/재시도/무시
	+---------------------+-------------------------------------------------
```


* AfxMessageBox()의 Return Value

```
	+----------+-------------------------------------------------
	| type     | Description
	+----------+-------------------------------------------------
	| IDABORT  |
	| IDCANCEL |
	| IDIGNORE | 
	| IDNO     | 
	| IDOK     | 
	| IDRETRY  |
	+----------+-------------------------------------------------
```

* Message Box 메시지 아이콘 스타일

```
	+-------+-------------------------------------------------
	| style | Description
	+-------+-------------------------------------------------
	|  (x)  |  MB_ICONHAND, MB_ICONSTOP, MB_ICONERROR
	|  (?)  |  MB_ICONQUESTION
	|  (i)  |  MB_ICONASTERISK, MB_ICONINFORMATION
	|  /!\  |  MB_ICONEXCLAMATION, MB_ICONWARNING
	+-------+-------------------------------------------------
```

* Class Wisard
	* 예제는 윈도우 생성/종료시에 Message Box 출력
	* CXXView클래스에서 Project -> Class Wisard -> Message[Tap]
		- WM_CREATE에 OnCreate 핸들러 추가
		- WM_DESTROY에 OnDestroy 핸들러 추가

* Lab1: MessageBox 만들기
	* CXXView 클래스

```c
int Cex41View::OnCreate(LPCREATESTRUCT lpCreateStruct)
{
	if (CView::OnCreate(lpCreateStruct) == -1)
		return -1;

	// TODO:  여기에 특수화된 작성 코드를 추가합니다.
	AfxMessageBox(_T("윈도우가 만들어짐^^"), MB_ICONINFORMATION);
	return 0;
}


void Cex41View::OnDestroy()
{
	CView::OnDestroy();

	// TODO: 여기에 메시지 처리기 코드를 추가합니다.
	AfxMessageBox(_T("윈도우가 종료됨^^"), MB_ICONINFORMATION);
}
```


