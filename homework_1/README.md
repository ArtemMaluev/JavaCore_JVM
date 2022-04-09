# ������ "��������� JVM"

```Java
public class JvmComprehension {

    public static void main(String[] args) {
    	int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```

## ����� ������ JVM:

  ### 1. �������� ������� (ClassLoaders)
  �������������� ������������ �������
  ���������� ��� �������� ���������� ����������:
  * Application class loader. ��������� ������ ����������
  * Platform class loader. ��������� ��������� ������ Java SE � JDK
  * Bootstrap class loader. ��������� �������� ������ Java SE � JDK

  Application class loader ����� �������� ����� JvmComprehension
  Bootstrap class loader ����� ���������� ������ Object, Integer, System

  ### 2. ���������� (Linking)
  ��� ���������� ����������:
  * �����������. ��������� ������������ ����
  * ����������. �������������� ����������� ���������� ������ � �������������� ������ ��� �������� �� ���������
  * ���������� ������. ��������� ������ �� ������ ������

  ### 3. �������������
  �������������� ���������� ������ �� ����������� ���������� ����������

  ### 4. ������� ������
  * ������� ������ Stack ����������
  * ������� ������ Heap ����������
  * ������� ������ Metaspace
  
  ����� JvmComprehension � ��������� ������ ����������� � ������� ������ **Metaspace**

  * � **Stack** ��������� ����� ������ main()
      * ���������� *int i = 1*
      * ������ *o* �� ������ Object
      * ������ *ii* �� ������ Integer
  * � **Heap** ��������� ������ Object � ����������� � ���������� *o* ������ main()
  * � **Heap** ��������� ������ Integer � ����������� � ���������� *ii* ������ main()
  * � **Stack** ��������� ����� ������ printAll()
      * ������ *o* �� ������ Object
      * ���������� *int i = 1*
      * ������ *ii* �� ������ Integer
      * ������ *uselessVar* �� ������ Integer
  * � **Heap** ������ Object ����������� � ���������� *o* ������ printAll()
  * � **Heap** ������ Integer ����������� � ���������� *ii* ������ printAll()
  * � **Heap** ��������� ������ Integer � ����������� � ���������� *uselessVar* ������ printAll()
  * � **Stack** ��������� ����� ������ println()
      * ������ *o* �� ������ Object
      * ���������� *int i = 1*
      * ������ *ii* �� ������ Integer
  * � **Heap** ������ Object ����������� � ���������� *o* ������ println()
  * � **Heap** ������ Integer ����������� � ���������� *ii* ������ println()
  * � **Heap** ��������� ������ String � ����������� � ���������� *o* ������ println()
  * � **Stack** ��������� ����� ������ println() 
  * � **Heap** ��������� ������ String � ����������� � ������� ������ println()

  ### 5. ���������� (Execution Engine)
  ����������� ����-���.
  * Interpreter �������������� ����-��� � ����� ������ �� �������
  * JIT Compiler ������������ ��� ��������� ������������� ��������������
  * Garbage Collector ������������ �������� ������� �� ������ (Heap), ������� ������ �� ������������
    * � Heap ���������� ��������� ����:
      - Eden. ���� �������� ��������
      - Survivor1, Survivor2. ���� �������� �� ������������ ������
      - Tenured. ���� ��������� ������������ ��������, ����� �������� ������� ��������� ���������� �� ��������
    * ��� ������ ������ ���������� ������������ ���������
