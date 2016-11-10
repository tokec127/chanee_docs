## User Interface - Tool Bar

* MFC�� Tool Bar
	* ���� ���Ǵ� �޴� �׸� -> ���� �������� bitmap �׸���ư
	* ������, ���� �������̽�
	* �ָ޴� �ؿ� Tool bar ��ġ


* �⺻������ �����ϴ� Tool bar
	* [NEW][Open][Save] [Cut][Copy][Paste]
	* Resource View -> [Toolbar] ���� -> IDR_MAINFRAME ����


* Lab
	* m_color @ View Class �߰��ϱ�
	* initialize m_color @ OnDraw()
```c
	void Cxxx_View::OnDraw(CDC * pDC)
	{
		...
		CRect r;
		GetClientRect(r);
		pDC->SetTextColor(m_color);
		pDC->DrawText(_T("����������?"), -1, r, DT_SINGLELINE | DT_CENTER | DT_VCENTER);
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
		
