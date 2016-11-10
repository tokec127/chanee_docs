## User Interface - Menu Bar

* Menu
	* ������ ��� �������� ��ɵ��� ���� �޴�
	* �ָ޴�(main)�� �θ޴�(sub)�� ������
	* ��)

```
	+------+------+------+-------+------+-------+------+-----+------+
	| FILE | EDIT | VIEW | DEBUG | TEAM | TOOLS | TEST | ... | HELP |
	+------+------+------+-------+------+-------+------+-----+------+
```

* �޴��� ����
	* Pull-down Menu : ����ڰ� �޴��� �����ϸ� ���� �޴��� drop down�Ǵ� �޴�
	* Cascading Menu : �޴��� ������(�Ǵ� ����)���� �޴��� ������ ���
	* Pop-up Menu (or Context Menu) : ��Ŭ�� �Ǵ� �̺�Ʈ�� ���� �߻��ϴ� �޴�


* MFC�� �޴��ۼ�
	* Application Wisard�� �̿��� ������ ����
	* MFC�� �����ϴ� �⺻�޴� ���� ���α׷��Ӱ� ���ϴ� �޴� ���
	* Resource View�� [Menu] ������ IDR_MAINFRAME�� ����
	* SDI/MDI ������Ʈ�� ���� �ٸ�

* �޴��� �Ӽ�
	* ID : �޴��׸��� �ĺ���
	* Prompt : �޴��� ���¹ٿ� ��Ÿ���� ���ڿ�
	* Seperator : �޴� �׸���� �и��ϴ� ���м�
	* Break
		* None : �и����� ǥ�õ��� ����
		* Column : �� �޴� Į������ ǥ��
		* Bar : Į���� ����, �θ޴��� ��� �÷� ���̿� �и����� ǥ��
	* Caption : �޴��� ǥ�õǴ� ���ڿ�
	* Checked : �޴� �տ� üũ ǥ�ø� ��
	* Enabled : �޴� �׸��� ���ð����ϵ��� ����
	* Grayed  : �޴� �׸��� ȸ������ ǥ��. �޴��� ��Ȱ��ȭ
	* Pop-up  : �޴��� ���� �׸��� ���Եǵ��� ����

* Lab
	* �ָ޴��� [����] �޴��� Pull-down �޴� ����ϱ�
	* Pull-down�� ������ �ΰ� ���ý� ������ ���� �����ϱ�
	* (1) View Ŭ������ ������� �߰� (m_color, COLORREF)
	* (2) View �����ڿ� ������� �ʱ�ȭ �߰�
	* (3) View Ŭ������ OnDraw()�Լ��� ��� �߰�
		* CRect Ŭ���� : �簢���� ��Ÿ���� Ŭ����. 
		* CRect�� RECTŬ������ ���
		* Ŭ���� ��ü�� ���� left, top, bottom, right ��� ������ ���� ����
	* DrawText() :
		* -1 : ���ڿ��� null�� ������ ��
		* r : ����ȭ�Ǵ� �ؽ�Ʈ ��ǥ


```c
	CxxView::CxxView()
	{
		m_color = RGB(0, 0, 0);		// default BLACK
	}

	CxxView::OnDraw()
	{
		...
		CRect r;
		GetClientRect(r);
		pDC->SetTextColor(m_color);
		pDC->DrawText(_T("������"), -1, r, DT_SINGLELINE | DT_CENTER | DT_VCENTER);
	}


	typedef struct tagRECT {
		LONG left;
		LONG top;
		LONG right;
		LONG bottom;
	} RECT;
```

* �޴� �߰��ϱ�
	1. Open [Resource View]		// VIEW -> Resource View (Ctrl+Shift+E)
	2. Select [Menu] @ Resource View
	3. IDR_MAINFRAME <- Menu
	4. "����(&C)" : ���ο� �޴� �߰��ϱ�
	5. "������(&C)" : ���ο� �����޴� �߰��ϱ�
		* ������ �޴��� �Ӽ��� �����Ѵ�. (ID, Prompt, Caption)
		* �������� ���� �����޴��� �߰��Ѵ�.
	6. �޴� ���ÿ� ���� �ڵ鷯�� ����Ѵ�.
		* Project -> Class Wisard
			* Class�̸��� CxxxView ����
			* ���(Command)���� ID_xxx �� ����Ŭ���Ͽ� �޽����� OnBlack�߰�

```c
	void CxxView::OnBlack()
	{
		m_color = RGB(0, 0, 0);
		Invalidate();
	}

	void CxxView::OnRed() { ... }
	void CxxView::OnBlue() { ... }
	void CxxView::OnGreen() { ... }
```
