## User Interface - Status Bar

* MFC�� ���¹� (Status Bar)
	* ������ �ϴܿ� ǥ�õǴ� ���� ����
	* �޴��׸�, ���콺 ��ġ�� ���� �������� ���

* ����ũ�� PANE
	* ����: CAP, NUM, SCRL(Scroll Lock)
	* ������ �ϴ� ������ ǥ�õ�


* ���¹� �ۼ����
	* CStatusBar Ŭ������ ���
	* ����
		1. CStatusBar ��ü �����
		2. CStatusBar::Create() �Լ��� ����ǥ���� ����
		3. Pane�� �����, ũ�⸦ ����
	* Application Wizard�� ���¹�
		* CMainFrame Ŭ���� -> CStatusBar Ŭ���� -> m_wndStatusBar �������
	* MainFrm.cpp�� �Ʒ� �迭�� ������
		* ID_SEPERATOR
		* ID_INDICATIOR_CAPS
		* ID_INDICATIOR_NUM
		* ID_INDICATIOR_SCRL


* Lab
	* Resource View â���� ������Ʈ ��Ŭ�� -> Resource Symbols
	* ID_xxx ���ο� PANE �߰�
	* PANE�� ���ο� �� �����ϱ� ����, 
		* CMainFrame Ŭ������ indicator�� ���ο� ID (ex. ID_POINT) �߰�
		* CMainFrame Ŭ���� ������ OnCreate()�� �ʱ�ȭ �߰�
```c
	static UNIT indicators[] = 
	{
		ID_SEPARATOR,
		ID_POINT,				// <- ���� �߰��� ID
		...
	}

	int CMainFrame::OnCreate(...)
	{
		...	
		m_wndStatusBar.SetPaneInfo(1, ID_POINT, SBPS_NORMAL, 140);
		...
	}
```
	* Message map ����
		* Cxx_View Ŭ���� -> Message -> WM_MOUSEMOVE
	* �޽��� �ڵ鷯 �ҽ� �߰�
```c
	void Cxxx_View::OnMouseMove (UNIT nFlags, CPoint point)
	{
	
		CString str;
		CMainFrame *pF;
		pF = (CMainFrame *)AfxGetApp()->m_pMainWnd;

		CStatusBar *pStatus = &pF->m_wndStatusBar;
		if (pStatus) {
			str.Format(_T("���콺 ��ġ x:%d, y:%d"), point.x, point.y);
			pStatus->SetPaneText(1, str);
		}

		CView::OnMouseMove(nFlags, point);
	}
```
```
	+---------------+--------------------------------------------------------
	|	�Լ�        | ����
	+---------------+--------------------------------------------------------
	| AfxGetApp()   | - ���� ���α׷��� ������ҿ� ���� ������ �� �ִ� �Լ�
	|               | - AfxGetApp()�� CWinApp ��ü�� �����͸� ��ȯ
	+---------------+--------------------------------------------------------
	| SetPaneText() | - CStatusBar Ŭ������ ����Լ�
	|               | - ���¹��� Ư�� Pane�� ���ڿ� ���
	|               | - 1st arg: Pane�� ��ġ, 2nd arg: ����� ���ڿ�
	+---------------+--------------------------------------------------------
```
	* CView Ŭ�������� SetPaneText() �Լ�ȣ�� ���
```c
	ex1) pStatus->SetPaneText(...);
	ex2) CstatusBar *pStatus = & pFrame->m_WndStatusBar;
	ex3) CMainFrame *pFrame = (CMainFrame *) AfxGetApp()->m_pMainWnd;
```

