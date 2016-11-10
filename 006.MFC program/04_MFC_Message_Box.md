## Message Box

* Message Box
	* ������ �޽����� ����Ҷ� ���
	* AfxMessageBox()�� �̿�
	* MB_OK : Ȯ�� ��ư�� ����
	* MB_OKCANCEL : Ȯ��/��� ��ư�� ����
	* MB_IOCNASTERISK : (I) �޽������� �޽��� â�� ����
	* MB_ICONEXCLAMATION

* Message Box ��ư ��Ÿ��

```
	+---------------------+-------------------------------------------------
	|        type         |    Description
	+---------------------+-------------------------------------------------
	| MB_OK               |	Ȯ�� ��ư (Default)
	| MB_OKCANCEL         |	Ȯ��/���
	| MB_YESNO            | ��/�ƴϿ�
	| MB_YESNOCANCEL      | ��/�ƴϿ�/���
	| MB_RETRYCANCEL      | ��õ�/���
	| MB_ABORTRETRYIGNORE | ���/��õ�/����
	+---------------------+-------------------------------------------------
```


* AfxMessageBox()�� Return Value

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

* Message Box �޽��� ������ ��Ÿ��

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
	* ������ ������ ����/����ÿ� Message Box ���
	* CXXViewŬ�������� Project -> Class Wisard -> Message[Tap]
		- WM_CREATE�� OnCreate �ڵ鷯 �߰�
		- WM_DESTROY�� OnDestroy �ڵ鷯 �߰�

* Lab1: MessageBox �����
	* CXXView Ŭ����

```c
int Cex41View::OnCreate(LPCREATESTRUCT lpCreateStruct)
{
	if (CView::OnCreate(lpCreateStruct) == -1)
		return -1;

	// TODO:  ���⿡ Ư��ȭ�� �ۼ� �ڵ带 �߰��մϴ�.
	AfxMessageBox(_T("�����찡 �������^^"), MB_ICONINFORMATION);
	return 0;
}


void Cex41View::OnDestroy()
{
	CView::OnDestroy();

	// TODO: ���⿡ �޽��� ó���� �ڵ带 �߰��մϴ�.
	AfxMessageBox(_T("�����찡 �����^^"), MB_ICONINFORMATION);
}
```


