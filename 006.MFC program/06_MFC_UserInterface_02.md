## User Interface - Tool Bar

* MFC의 Tool Bar
	* 자주 사용되는 메뉴 항목 -> 작은 아이콘의 bitmap 그림버튼
	* 직관적, 편리한 인터페이스
	* 주메뉴 밑에 Tool bar 위치


* 기본적으로 제공하는 Tool bar
	* [NEW][Open][Save] [Cut][Copy][Paste]
	* Resource View -> [Toolbar] 선택 -> IDR_MAINFRAME 존재


* Lab
	* m_color @ View Class 추가하기
	* initialize m_color @ OnDraw()
```c
	void Cxxx_View::OnDraw(CDC * pDC)
	{
		...
		CRect r;
		GetClientRect(r);
		pDC->SetTextColor(m_color);
		pDC->DrawText(_T("무슨색이지?"), -1, r, DT_SINGLELINE | DT_CENTER | DT_VCENTER);
	}
```
	* set properties of icon by clicking right button
		* ID -> ID_BCOLOR, ID_GCOLOR, ID_RCOLOR
		* Prompt -> "Set Blue", "Set Green", "Set Red"
		* To show Tooltip, set Prompt -> "Set Blue\nBlue", ...
	* set `event handler`
		* [Project] -> [Class Wizard] -> [CxxxView]
		* check `event handler` to each `ID` in [add member function]

```c
	void Cxxx_View::OnBcolor()
	{
		m_color = RGB(0,0,255);
		Invalidate();	
	}

	void Cxxx_View::OnGcolor() { ... }
	void Cxxx_View::OnRcolor() { ... }
```	
		
