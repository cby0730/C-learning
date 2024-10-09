# C++考題

1. There are 10 random numbers coming to your system, please write down a prototype that these numbers can be sorted in ASC and DESC.  Any language or pseudo code is welcome, chart/graph/state machine or any other ways to show your architecture and method is welcome...everything you wrote on the white board is welcome. 
    - Answer
        
        ```cpp
        void sort(array<int, 10>& numArray, bool isASC){
        	for (int i = 1; i < numArray.length ; ++ i) {
        		int cur = numArray[i];
        		int j = i - 1;
        		bool flag = isASC ? numArray[j] > cur : numArray[j] < cur
        		
        		while (j >= 0 && flag) {
        			numArray[j+1] = numArray[j];
        			-- j;
        		}
        		
        		numArray[++j] = cur;
        	}
        }
        int main() {
        	array<int, 10> numArray;
        	Cin >> isASC ;
        	sort( numArray, isASC);
        	printArray(numArray);
        	return ;
        }
        ```
        
2. Please write down the output.
    
    ```cpp
    int main(){
            char *title = "QSAN";
            int i = 3;
            printf("%s produces Trio", title);
            printf("%c", title[i--]);
            printf("%c", title[i]);
            printf("%c\n", title[--i]);
    }
    ```
    
    - Answer
        
        QSAN produces TrioNAS
        
3. Please point out of potential risks in this program.
    
    ```cpp
    struct card {
        int id;
        char *name;
    };
    void set_card(struct card *_card, int _id, int _name) { 
        _card->id   = _id;
        _card->name = _name;
    }
    int main(void) {
        struct card *Qsan = NULL;
        Qsan = malloc(sizeof(struct card));
        set_card(Qsan, 163, "Winnie");
        printf("(%d, %s)\n", Qsan->id, Qsan->name);
        strcpy(Qsan->name, "Cindy");
        printf("Assign this id to %s)\n", Qsan->name);
    }
    ```
    
    - Answer
        
        ```cpp
        struct card {
            int id;
            char *name;
        };
        void set_card(struct card *_card, int _id, int _name) { // _name型別錯誤
            _card->id   = _id;
            _card->name = _name;
        }
        int main(void) {
            struct card *Qsan = NULL;
            Qsan = malloc(sizeof(struct card)); // 記憶體未被初始化，可能包含奇怪的東西
            set_card(Qsan, 163, "Winnie"); // 傳遞的型別錯誤
            printf("(%d, %s)\n", Qsan->id, Qsan->name);
            strcpy(Qsan->name, "Cindy"); 
            printf("Assign this id to %s)\n", Qsan->name);
            // 沒有free()
        }
        ```
        
4. What's the output of the following program running on a 64-bit system?
    
    ```cpp
    int main() {
        char str[] = {'a', 'b', 'c', 'd', 'e', 'f'};
        char *strP = str;
        printf("sizeof(strP) = %u\n", sizeof(strP));
    }
    ```
    
    - Answer
        
        **關鍵在於：**
        
        - 32-bit system的pointer是4bytes
        - 64-bit system的pointer是8bytes
        
        所以strP的大小會是8
        
5. Below output is 4, can you explain the reason? How could we save more space in this case?
    
    ```cpp
    struct t {
         char version
         short value;
     };
     int main() {
         printf("The size of the struct t is %d\n", sizeof(struct t));
    }
    ```
    
    - Answer
        
        `struct`會以空間最大的成員為基準，做記憶體對齊，`char`是1 byte，`short`是2 bytes，對齊後會是4 bytes。
        
        ```
        1 byte for char version
        1 byte of padding (to align short on a 2-byte boundary)
        2 bytes for short value
        ```
        
        要節省記憶體可以改成：
        
        ```cpp
        struct t {
        	short value; // 
        	char version;
        };
        ```
        
        範例：
        
        ```cpp
        struct t {
            char version;   // 1 byte
            short value;    // 2 bytes
            float f;        // 4 bytes
            double d;       // 8 bytes
        };
        ```
        
        ```
        1 byte for char version
        1 byte of padding
        2 bytes for short value
        4 bytes for float
        8 bytes for double
        
        total : 16 bytes
        ```
        
6. Write down a function to sort sequence number. Then we could get result as {1, 3, 5, 8}.
    - Answer
        
        ```cpp
        void sort(int numArray[4]) {
        	int len = numArray.length();
        	
        	for (int i = 1; i < len; ++i) {
        		int cur = numArray[i];
        		int j = i - 1;
        		
        		while (j >= 0 && numArray[j] > cur) {
        			numArray[j+1] = numArray[j]
        			--j;
        		} // while
        		
        		numArray[j+1] = cur;
        	} // for
        } // sort
        ```
        
7. Please write down the output.
    
    ```cpp
    typedef enum {S0=3, S1=0, S2, S3} State;
    State assign_ment = S2;
    switch (assign_ment){
    	case 1:
    		printf("SA");
    		assign_ment = 3;
    	case 2:
    		printf("N");
    		break;
    	case 3:
    		printf("D");
    	default:
    		break;
    }
    ```
    
    - Answer
        
        **關鍵：**
        
        enum的使用：沒有給數值，就是從1開始給，所以S2 = 1, S3 = 2
        
        case的順序：進去case 1之後，也會繼續檢查其他case，所以值有變動，有可能滿足多個case。
        
        ```cpp
        assign_ment = S2 // 1
        case 1 --> assign_ment = 3 --> assign_ment = S3 // 2
        case 2
        
        output: "SAN"
        ```
        
8. Define macro which would return the maximum values of two variables respectively. Note that you must use the ternary conditional operator (? :) in these two macros. Any potential risk by using this MACRO?
    - Answer
        - 型別問題
            
            ```cpp
            #define MAX(a, b) ((a > b) ? (a) : (b))
            ```
            
            如果型別不同，會造成錯誤。
            
        - 語意問題
            
            ```cpp
            #include <iostream>
            
            #define MAX(a, b) ((a > b) ? (a) : (b))
            
            int main() {
                int a = 5;
                int b = 5;
                int result = MAX(a++, ++b); // (a++ > ++b) ? (a++) : (++b)
                std::cout << "a: " << a << ", b: " << b << ", MAX(a++, ++b): " << result << std::endl;
                return 0;
            }
            
            output:
            a: 6, b: 7, MAX(a++, ++b): 7
            ```
            
9. Assume we are using little endian system, write down the output.
    
    ```cpp
    #define DWORD unsigned int
    #define BYTE  unsigned char
    typedef union _DWORD_PART_ {
    	DWORD dwWord;
    	struct {
    		BYTE byte3;
    		BYTE byte2;
    		BYTE byte1;
    		BYTE byte0;
    	} bybyte;
    } DWORD_PART;
    int main()
    {
    	DWORD_PART payload;
    	payload.dwWord = 0x0;
    	payload.bybyte.byte2 = 0x1;
    	printf("%x\n", payload.dwWord);
    }
    ```
    
    - Answer
        
        **關鍵：**
        
        - union的成員是共享記憶體的
        - dwWord的高位byte會對應到bybyte.byte3，依此類推
        
        ```cpp
        payload.dwWord = 0x0;
        //低                            高
        | byte0 | byte1 | byte2 | byte3 |
        | 0x00  | 0x00  | 0x00  | 0x00  |
        
        payload.bybyte.byte2 = 0x1;
        //低                            高 bybyte
        | byte3 | byte2 | byte1 | byte0 |
        | 0x00  | 0x01  | 0x00  | 0x00  |
        //低                            高 dwWord
        | byte0 | byte1 | byte2 | byte3 |
        | 0x00  | 0x01  | 0x00  | 0x00  |
        
        payload.dwWord = 0x00010000
        
        printf("%x\n", payload.dwWord); // 10000
        ```
        
10. Complete the following function, which would convert a null-terminated string into uppercases. Here is prototype of the function.
    
    ```cpp
    char* to_upper(char *str);
    ```
    
    - Answer
        
        ```cpp
        char* to_upper(char *str){
        	while(*str) {
        		if (*str >= 'a' &&  *str <= 'z'){
        			*str = toUpper(*str)
        			str++;
        		} // for
        	} // while
        } // to_upper
        ```
        
11. Look at the following snippet of code, what is the final value of variable a?
    
    ```cpp
    int less_equal(int v1, int v2)
    {
    	if (v1 <= v2)
    		if (v1 == v2)
    			return 0;
    	else
    		return -1;
    	return 1;
    }
    int	a = 5, b = 4;
    a = less_equal(a, b);
    ```
    
    - Answer
        
        ```cpp
        // 可以寫成這樣
        int less_equal(int v1, int v2)
        {
        	if (v1 <= v2) // 5, 4
        		if (v1 == v2)
        			return 0;
        		else // else會去跟最近的一個if
        			return -1;
        	return 1; // return
        }
        int	a = 5, b = 4;
        a = less_equal(a, b);
        
        output: 1
        ```
        
12. a += b would add variable **a** and variable **b** and save the addition result in variable **a**. Please write a function ***add_complex***, which adds two complex variables and saves the result in the first parameter of ***add_complex*** The data format of the complex is defined below.
    
    ```cpp
    struct complex	{
    	int	real;
    	int	img;
    };
    ```
    
    - Answer
        
        ```cpp
        // C++
        void add_complex(complex& a, const complex& b) {
        	a.real += b.real;
        	a.img += b.img;
        }
        
        // C
        void add_complex(struct complex *a, const struct complex *b) {
            if (a == NULL || b == NULL) {
                // 如果指標為 NULL，則不進行任何操作
                return;
            }
            a->real += b->real; 
            a->img += b->img;  
        }
        ```
        
13. Write the C code to present figure’s condition. 
    
    ![截圖 2024-09-26 中午12.15.37.jpg](C++%E8%80%83%E9%A1%8C%2010dfdac53b4c802bb8c1d7b3a2c16654/%25E6%2588%25AA%25E5%259C%2596_2024-09-26_%25E4%25B8%25AD%25E5%258D%258812.15.37.jpg)
    
    - Answer
        
        ```cpp
        struct Node {
        	Node *prev;
        	Node *next;
        };
        int main() {
        	Node NodeA, NodeB;
        	NodeA.prev = &NodeB;
        	NodeA.next = &NodeB;
        	
        	NodeB.prev = &NodeA;
        	NodeB.next = &NodeA;
        	
        	return 0;
        }
        ```
        
14. Please write a function to detect a value which is power of 2. 
    - Answer
        
        ```cpp
        bool isPow2(int num) {
        	if (num <= 0)
        		return false;
        	else 
        		return (n & (n-1)) == 0
        } // isPow2
        ```
        
        1. **基本概念**：
            - 2 的次方數在二進位表示中，只有一個位元是 `1`，其餘位元都是 `0`。
            - 例如：
                - 1 (2^0) → `0001`
                - 2 (2^1) → `0010`
                - 4 (2^2) → `0100`
                - 8 (2^3) → `1000`
        2. **位元運算原理**：
            - 當 `n` 是 2 的次方時，`n` 的二進位表示只有一個 `1`，而 `n-1` 則是在這個 `1` 之後的所有位元都是 `1`。
            - 例如：
                - 如果 `n = 4` (`0100`)，則 `n - 1 = 3` (`0011`)。
                - `0100 & 0011 = 0000`，結果為 `0`。
            - 這意味著，對於 2 的次方數，`n & (n - 1)` 的結果總是 `0`。
        3. **函式邏輯**：
            - 首先檢查 `n` 是否為 `0`。因為 `0` 不是 2 的次方。
            - 然後使用位元運算 `(n & (n - 1))` 來判斷。如果結果為 `0`，則 `n` 是 2 的次方；否則，則不是。
15. Please write the output
    
    ```cpp
    #define DEBUG 1
    #ifdef DEBUG
    printk("Sentence 1\n");
    #if defined(NO_THIS_CONFIG_1)
    printk("Sentence 2\n");
    #elif defined(NO_THIS_CONFIG_2)
    printk("Sentence 3\n");
    #endif
    printk("Sentence 4\n");
    #else
    printk("Sentence 5\n");
    #endif
    ```
    
16. If we define a structure as below.
    
    ```cpp
    struct Node
    {
        int Data;
        Node *Next;
    };
    Node N1;
    Node *pN = &N1;
    Select below options which could save data correctly.
    a) (*N1).Data 
    b) N1.Data
    c) (*pN).Data
    d) N1->Data
    e) pN.Data
    f) pN->Data
    ```
    
    - Answer
        
        b, c, f
        
        關鍵：N1是Node實例，pN是指向N1的指標。
        
        - N1使用dot operator是合法的。
        - `(*pN)`等於N1，也可以使用dot operator。
        - 使用arrow operator於指標上，是可以合法存取其中的成員資料。
17. Please write a function (get_Fibonacci_num(int n)) to get Fibonacci number, the n could be any number.(p.s.) Fibonacci is F0 = 0, F1 = 1, Fn = Fn-1 + Fn-2, if n>1
    - Answer
        
        ```cpp
        int get_Fibonacci_num (int n){
        		if (n==0) return 0;
        		if (n==1) return 1;
        		
        		return get_Fibonacci_num(n-1) + get_Fibonacci_num(n-2);
        } // get_Fibonacci_num
        ```
        
18. 執行以下的程式後，螢幕的輸出是？
    
    ```cpp
    #define SQR（x）x * x
    void main()
    {
    	int a = 5；
    	int b = 3；
    	int c = 1；
    	a *= SQR(b + c);
    	cout << a;
    }
    ```
    
    - Answer
        
        ```cpp
        // 展開後會變成以下
        void main()
        {
        	int a = 5；
        	int b = 3；
        	int c = 1；
        	a *= b + c * b + c;
        	cout << a;
        }
        
        output: 35
        ```
        
19. 執行程式後，以下的輸出結果是？
    
    ```cpp
    class Base
    {
    	char m_cData;
    public:
    	Base (char c) :m_cData(c) {}
    	virtual void Output() { cout << "Data From Base!!\n"; }
    	void Output2() { cout << "Data2 From Base!!\n"; };
    	virtual ~Base() { cout << m_cData; }
    };
    
    class MyClass public Base
    {
    	char m_cData;
    public:
    	MyClass (char c) :Base(c + 1), m_cData (c) {}
    	virtual void Output() { cout << "Data From MyClass!!\n"; }
    	void Output2() { cout << "Data2 From MyClass!!\n"; };
    	~MyClass() { cout << m_cData; }
    };
    
    void main ()
    {
    	MyClass* pData;
    	pData = new MyClass('B');
    	Base* pPtrData = (Base*)pData;
    	pPtrData-›Output();
    	pPtrData-›Output2();
    	delete pData;
    }
    ```
    
    - Answer
        
        
20. 執行以下的程式碼，輸出的結果是？
    
    ```cpp
    void main()
    {
    	int num = 1;
    	int& ref = num;
    	ref = ref + 2;
    	cout << num;
    	num = num + 3;
    	cout << ref << endl;
    }
    ```
    
21. 請寫出以下程式碼的輸出結果
    
    ```cpp
    #include <cstdio>
    int fun (int x, int y)
    {
    	if (y == 0) return 7; 
    	return (x + fun(x, y - 1));
    }
    void main()
    {
    	printf("%d %d", fun(5, 9), fun(11, 3));
    }
    ```
    
    - Answer
        
        fun(x, y)的意思就是→ x * y + 7
        
        ```cpp
        output:
        52 40
        ```
        
22. 請寫出以下程式碼的輸出結果
    
    ```cpp
    void main( )
    {
    	int
    	b = 3;
    	int arr[] = ( 6,7,8,9,10 };
    	int* ptr = arr;
    	*(ptr++) += 123;
    	printf("%d, %d\n", *ptr, *(++ptr));
    }
    ```
    
    - Answer
        
        ```cpp
        void main( )
        {
        	int
        	b = 3;
        	int arr[] = ( 6,7,8,9,10 };
        	int* ptr = arr; // *ptr = 6
        	*(ptr++) += 123; // 6 + 123 = 128, *ptr = 7
        	printf("%d, %d\n", *ptr, *(++ptr)); // *(++ptr) = 8
        }
        
        output:
        7, 8
        ```
        
23. 請寫出以下程式碼的輸出結果
    
    ```cpp
    void main()
    {
    	int32_t a = ('A' << 24) | ('C' << 16) | ('B' << 8) | ('w' << 0);
    	const char* c = (const char*)(&a);
    	vector<char> v(c, c + 3);
    	for (vector<char>::reverse_iterator it = v.rbegin(); it != v.rend(); ++it)
    		cout << *it;
    }
    ```
    
    - Answer
        - 注意little endian
        - v(c, c + 3)只會取前三個元素
        
        ```cpp
        int32_t a = ('A' << 24) | ('C' << 16) | ('B' << 8) | ('w' << 0);
        Become:
        •	(0x41 << 24) | (0x43 << 16) | (0x42 << 8) | 0x77
        •	Which equals 0x41'43'42'77
        
        The memory layout of a would be (little endian):
        •	Address | Value | Character
        •	c[0]    | 0x77  | 'w'
        •	c[1]    | 0x42  | 'B'
        •	c[2]    | 0x43  | 'C'
        •	c[3]    | 0x41  | 'A'
        ```
        
        ```cpp
        vector<char> v(c, c + 3); // v -> wBC
        
        reverse_output:
        CBw
        ```
        
        ```cpp
        // If include all 4 bytes
        vector<char> v(c, c + 4);
        
        reverse_output:
        ACBw
        ```
        
24. 以下的程式碼有錯誤，如果要讓最後的輸出為”AABC!”，且要在getMem的function中建立memory，請進行適當的修改。
    
    ```cpp
    #include <cstring> 
    #include <cstdio>
    void getMem(char* ptr, int size)
    {
    	if (size > 0) {
    		ptr = new char[size];
    	}
    }
    void main()
    {
    	char* str = NULL;
    	getMem(str, 50);
    	*str = 'A';
    	printf("%s", str);
    	strpy (str, "ABC!");
    	printf("%s", str);
    }
    ```
    
    - Answer
        
        ```cpp
        void getMem(char*& ptr, int size) // 使用&才會改動到main的str
        {
        	if (size > 0) {
        		ptr = new char[size];
        	}
        }
        void main()
        {
        	char* str = NULL;
        	getMem(str, 50);
        	*str = 'A';
        	printf("%s", str);
        	strpy (str, "ABC!");
        	printf("%s", str);
        	delete str[]; // 使用new要搭配delete來釋放記憶體，以面造成memory leak
        }
        ```
        
25. 請寫出以下程式的輸出結果（編譯為 64bit的執行檔）。
    
    ```cpp
    void main ()
    {
    	char str[] = "TEST!" ;
    	char* p = str;
    	int n = 10;
    	char str2[100];
    	void* pData = malloc(100);
    	
    	cout << sizeof(str) << " ";
    	cout << sizeof(p) << " "；
    	cout << sizeof(n) «< " ";
    	cout << sizeof(str2) << " ";
    	cout << sizeof(pData) << " ";
    }
    ```
    
    - Answer
        
        ```cpp
        char str[] = "TEST!" ; // char array
        char* p = str; // pointer
        int n = 10; // int
        char str2[100]; // char array
        void* pData = malloc(100); // pointer
        
        6, 8, 4, 100, 8
        ```
        
        以下另兩種狀況：
        
        ```cpp
        // Condition 1
        char* str = "TEST!"
        char* str2 = {str};
        // str -> { "TEST!" }
        
        - str2是儲存pointer的陣列。
        - sizeof( str ) = 8 bytes // pointer
        - sizeof( *str ) = 1 bytes // point to char "T" 
        ```
        
        ```cpp
        // Condition 2
        char* str3 = str;
        //  str -> "TEST!"
        
        - str3是指向"TEST!"的pointer
        - sizeof(str3) = 8 bytes // pointer
        - sizeof(*str3) = 1 bytes // char
        ```
        
        額外例子可以參考：[完整sizeof題型筆記](https://www.notion.so/sizeof-111fdac53b4c8052b34fd0f21d863e74?pvs=21) 
        
26. 請寫出以下程式的輸出結果。
    
    ```cpp
    struct C
    {
    	void foo() { cout << "A"; } 
    	void foo() const { cout << "B"; }
    };
    struct S
    {
    	vector<C> v; 
    	const C* o;
    	C const* q; 
    	C *const p;
    	S()：v(1), o(&v[0]), q(&v[0]), p(&v[0]) {}
    };
    void main()
    {
    	S s;
    	const S& b = s;
    	s.v[0].foo();
    	s.o->foo();
    	s.q->foo();
    	s.p->foo();
    	cout << endl; 
    	b.v[0].foo();
    	b.o->foo();
    	b.q->foo();
    	b.p->foo();
    }
    ```
    
    - Answer
        
        ```cpp
        S s; // non-const object
        s.v[0].foo(); // v is non-const, call foo()
        s.o->foo(); // o is a pointer, which point to const C, call foo() const
        s.q->foo(); // q is a pointer, which point to const C, call foo() const
        s.p->foo(); // p is a const pointer, which point to C, call foo()
        
        output: ABBA
        ```
        
        ```cpp
        const S& b = s; // b is a const object
        b.v[0].foo(); // b is a const object, call foo() const
        b.o->foo(); // o pointer to const C as same as above
        b.q->foo(); // q pointer to const C as same as above
        b.p->foo(); // although p points to non-const C, b is const object. Call foo() const 
        
        output: BBBB
        ```
        
        完整解參考：[const & *](https://www.notion.so/const-111fdac53b4c806981cfcd26b4cd7b56?pvs=21) 
        
27. 寫下以下的輸出結果
    
    ```cpp
    template <typename T> void func(T) { cout << "A"; } 
    template <> void func(int *) { cout << "B"; } 
    template <typename T> void func(T*) { cout << "C"; } 
    void main()
    {
    	int a = 1;
    	double b = 0;
    	func(a) ;
    	func (&a) ;
    	cout << endl; 
    	func (b);
    	func(&b);
    }
    ```
    
    - Answer
        
        ```cpp
        int a = 1;
        double b = 0;
        func(a) ; // call void func(T)
        func (&a) ; // call void func(int *), higher priority than void func(T*)
        cout << endl; 
        func (b); // call void func(T)
        func(&b); // call void func(T*)
        
        output:
        AB
        AC
        ```
        
28. What is “const” ? and what is the meaning of:
    1. const int a;
    2. int const a;
    3. const int *a;
    4. int * const a;
    5. int const * const a;
    - Answer
        
        const 代表只可讀不可改。
        
        1. 一個常數型整數
        
        2. 同 1，一個常數型整數
        
        3. 一個指向常數型整數的指標 (整型數不可修改，但指標可以)
        
        4. 一個指向整數的常數型指標 (指標指向的整數可以修改，但指標不可修改)
        
        5. 一個指向常數型整數的常數型指標 (指標指向的整數不可修改，同時指標也不可修改)
        
29. Explain “struct” and “union” ?
    - Answer
        
        **簡答：**
        
        - struct成員使用各自的記憶體，可以隨時取用任何成員。
        - union成員共享記憶體（以最大的為準），一次只能使用一個成員。
        
        **詳細`struct` 和 `union` 的差異：**
        
        - 結構體（`struct`）和聯合體（`union`）都是用於在單一變數中存儲多個不同類型的數據，但它們在內存佈局和使用方式上有明顯的差異。
        - **內存佈局：**
            - **`struct`（結構體）：**
                - 為每個成員分配獨立的內存空間。
                - 結構體的總大小是所有成員大小的總和（加上可能的內存對齊填充）。
            - **`union`（聯合體）：**
                - 所有成員共享同一段內存。
                - 聯合體的總大小等於最大成員的大小。
        - **使用方式：**
            - **`struct`：**
                - 可以同時訪問所有成員。
                - 適用於需要同時存儲和使用多個數據的情況。
            - **`union`：**
                - 任一時刻只能存儲一個成員的值。
                - 常用於需要在同一內存位置存儲不同類型數據的場合，節省內存。
        
        **示例：**
        
        ```c
        struct ExampleStruct {
            int i;
            float f;
            char c;
        };
        
        union ExampleUnion {
            int i;
            float f;
            char c;
        };
        
        ```
        
        - **`ExampleStruct`** 的大小至少是 `int`、`float` 和 `char` 大小的總和。
        - **`ExampleUnion`** 的大小等於 `int`、`float`、`char` 中最大的那個。
        
        **總結：**`struct` 用於組合多個相關的數據，而 `union` 用於在同一內存位置存儲不同的數據，以節省內存。
        
30. What is the difference between “Inline Function” and “Macro” ?
    - Answer
        - Macro是在預處理時直接單純的文字替換，不做型別檢查
        - inline function是在compile階段時決定是否展開，會做型別檢查。
        
        比較下面兩個例子：
        
        inline寫法：
        
        ```cpp
        inline int square(int x) {
            return x * x;
        }
        output: square(3 + 2) → (3 + 2) * (3 + 2) = 25
        ```
        
        Macro寫法：
        
        ```cpp
        #define square(x) (x * x)
        output: square(3 + 2) → 3 + 2 * 3 + 2 = 11
        ```
        
31. Explain “static” ?
    - Answer
        1. static 出現在變數前，且該變數宣告於函式中 (C/C++)：
        局部變數加上 static 修飾後便不會因為function結束而消失。
        2. static 出現在全域變數前(C/C++)：
        讓變數只限定在該檔案內，而不是整個程式中（解決編譯時連結多個檔案造成相同變數名衝突）。
        3. static 出現在函式前(C/C++)：
        該函式只限定在該檔案內，而不是整個程式。
        4. static 出現在類別的成員變數前 (C++ only)：
        表示該變數不屬於某個類別實例，他屬於這個類別，所有此類別的實例都共用這個變數。
        5. static 出現在類別的成員函式之前 (C++ only)：
        表示該函式不屬於某個類別實例，他屬於這個類別，所有此類別的實例都共用這個函式（即便我們沒有產生實例出來，我們也隨時可以取用這個函式）（靜態函數裡的變數也都要是靜態的）。
        
        static在編譯時確定其儲存空間，但是在程序加載到內存時（程式開始跑之前），才會分配內存。 static變量存儲在內存佈局中的**數據段（Data Segment）**。
        
        **內存佈局詳解：**
        
        1.	**程式碼段（Text Segment）：** 存放程式的可執行代碼，通常是只讀的。
        
        2.	**初始化數據段（.data）：** 存放已初始化的全域變量和static變量。
        
        3.	**未初始化數據段（.bss）：** 存放未初始化或初始化為零的全域和static變量。
        
        4.	**只讀數據段（.rodata）：** 存放只讀的常量數據，如字串字面量、const變量。
        
        5.	**堆區（Heap）：** 用於動態內存分配，程式運行時由程式員手動控制（如malloc、new）。
        
        6.	堆疊**區（Stack）：** 存放函數的參數值、局部變量等，由編譯器自動分配和釋放。
        
        ```cpp
        高地址
        ----------
        | stack區 |
        ----------
        | heap區  |
        ----------
        | uninit  |
        ----------
        |  init   |
        ----------
        | .rodata |
        ----------
        | 程式碼段 |
        ----------
        低地址
        ```
        
32. What is stack and heap when talking about memory ?
    - Answer
        - Stack: allocated by compiler, store function parameters and local variables
            1. very fast access
            2. don't have to explicitly de-allocate variables
            3. space is managed efficiently by CPU, memory will not become fragmented
            4. local variables only
            5. limit on stack size (OS-dependent)
            6. variables cannot be resized
            - Heap: allocated by programmer
                1. variables can be accessed globally
                2. no limit on memory size
                3. (relatively) slower access
                4. no guaranteed efficient use of space, memory may become fragmented
                5. you must manage memory (you're in charge of allocating and freeing variables)
                6. leakage will happen
                7. variables can be resized using realloc()
        - Static: store global variable and static variable (initialized when process starts)
        - Literal Constant: store comments (text)
        
        ![image.png](C++%E8%80%83%E9%A1%8C%2010dfdac53b4c802bb8c1d7b3a2c16654/image.png)
        
33. Explain “thread” and “process”, and what is the difference ?
    - Answer
        - Process：
            1. 一個被編譯過並執行的程式就是process
            2. process之間彼此獨立
            3. process有自己的資源，例如記憶體。
            4. 建立和結束的代價會比較高。
        - Thread：
            1. 存在於Process之中
            2. 共享著process的記憶體和資源
            3. 建立和結束的代價會比較低。
        
        最主要的差異就是thread彼此共享process的記憶體，而process彼此之間獨立。
        
34. define a variable “*a”* from the following requests：
    1. An integer
    2. A pointer to an integer
    3. A pointer to a pointer to an integer
    4. An array of 10 integers
    5. An array of 10 pointers to integers
    6. A pointer to an array of 10 integers
    7. A pointer to a function that takes an integer as an argument and returns an integer
    8. An array of ten pointers to functions that take an integer argument and return an integer
    - Answer
        
        1. int a
        
        2. int *a
        
        3. int **a
        
        4. int a[10]
        
        5. int *a[10]
        
        6. int (*a)[10]
        
        7. int (*a)(int)
        
        8. int (*a[10])(int)
        
        **1. 三者的差別：**
        
        - **`int a[10];`**
            - 聲明一個包含10個`int`類型元素的陣列。
            - `a`的類型是`int[10]`。
        - **`int (*a)[10];`**
            - 聲明一個指向包含10個`int`類型元素的陣列的指標。
            - `a`的類型是`int (*)[10]`。
        - **`int *a[10];`**
            - 聲明一個包含10個指向`int`類型的指標的陣列。
            - `a`的類型是`int *[10]`。
        
        **示意圖：**
        
        - **`int a[10];`**
            
            ```
            a: [ int, int, int, int, int, int, int, int, int, int ]
            
            ```
            
        - **`int (*a)[10];`**
            
            ```
            a: [ ptr, ptr, ptr, ptr, ptr, ptr, ptr, ptr, ptr, ptr ]
            
            ```
            
        - **`int *a[10];`**
            
            ```
            a ----> [ int, int, int, int, int, int, int, int, int, int ]
            
            ```
            
        
        **2. 關於`void *a(void);`**
        
        - **`void *a(void);`**
            - 聲明一個函數`a`，它不接受任何參數，返回類型為`void *`（指向任何類型的指標）。
            - 這是合法的函數聲明。
        - **`void *a void;`**
            - 這是不合法的語法，編譯器會報錯。
        
        **總結：**
        
        - 可以寫`void *a(void);`，表示函數聲明。
        - 不能寫`void *a void;`，因為語法錯誤。
35. What is the content of array a ?
    
    ```cpp
    int a[] = {6, 7, 8, 9, 10};
    int *p = a;
    *(p++) += 123;
    *(++p) += 123;
    ```
    
    - Answer
        
        {128, 7, 131, 9, 10}
        
36. Typedef 在 C 語言中頻繁用以宣告一個已經存在的資料型態的同義字。也可以用預處理器做類似的事。例如，思考一下下面的例子︰
    
    ```cpp
    #define dPS struct s*
    typedef struct s* tPS;
    ```
    
    以上兩種情況的意圖都是要定義dPS 和 tPS 作為一個指向結構s指標。哪種方法更好呢 ? (如果有的話)為什麼 ?
    
    - Answer
        
        typedef 更好。
        
        思考下面的例子︰
        
        ```cpp
        dPS p1, p2;
        tPS p3, p4;
        ```
        
        第一個擴展為：
        
        ```cpp
        struct s* p1, p2;
        // 定義p1為一個指向struct的指標
        // p2就是一般的struct實例
        ```
        
        所以沒事不要亂用 macro。
        
37. What is the difference between variable declaration and definition ?
    - Answer
        - **宣告（Declaration）：**
            - 告訴編譯器變數的名稱和類型，但不為其分配內存。
            - 例子：`extern int a;`
            - 用於在多個文件中引用同一個變量。
        - **定義（Definition）：**
            - 分配內存並可能初始化變數。
            - 例子：`int a = 10;`
            - 每個變量只能有一次定義。
        - **總結：**宣告是告知編譯器變量的存在，定義則是真正創建變量並分配內存。
38. What is the difference between function declaration and definition ?
    - Answer
        - **宣告（Declaration）：**
            - 函式宣告告訴編譯器函式的名稱、回傳類型以及參數類型，但不包含函式的具體實現。宣告主要用於讓編譯器在使用函式時知道它的存在和如何調用它。通常，函式宣告會放在標頭檔（`.h` 或 `.hpp`）中，以便其他檔案可以引用。
            - 例子：
                
                ```cpp
                // 函式宣告
                int add(int a, int b);
                ```
                
        - **定義（Definition）：**
            - 函式定義則包含了函式的具體實現，即函式內部的程式碼。這是編譯器在編譯過程中生成可執行碼的部分。
            - 例子：
                
                ```cpp
                // 函式定義
                int add(int a, int b) {
                    return a + b;
                }
                ```
                
            - 每個函式在整個專案中只能有一個定義。
        - **總結：**宣告是告訴編譯器如何調用函式，定義是詳細實作，讓編譯器可以生成程式碼。
39. Is the program below correct ?
    
    ```cpp
    unsigned int zero = 0;
    unsigned int compzero = 0xFFFF; /*1’s complement of zero */
    ```
    
    - Answer
        - **問題分析：**
            - 0xFFFF 表示一個 16 位的全 1 數值。
            - 如果 `unsigned int` 是 32 位，則 0xFFFF 並不是全 1，需要使用 0xFFFFFFFF。
        - **正確的做法：**
            - 使用按位取反運算符來獲取 1 的補數，並確保適用於不同位數。
                
                ```c
                unsigned int compzero = ~0; /* 1's complement of zero */
                ```
                
        - **關於補數：**
            - **1's 補數（1's Complement）：**
                - 對二進制數的每一位取反（0 變 1，1 變 0）。
                - 例如，對於 8 位的 0，1's 補數是 0xFF。
            - **2's 補數（2's Complement）：**
                - 1's 補數加 1。
                - 常用於表示有符號數的負數形式。
        
        **反運算（位取反運算）：**
        
        - 使用`~`運算符，對整數的二進位逐位取反。
        - 即將二進位中的`0`變為`1`，`1`變為`0`。
        
        **示例代碼解析：**
        
        1. **`unsigned int compzero = ~0; /* 1's complement of zero */`**
            - `0`的二進位表示：`000...000`（所有位為`0`）。
            - 取反後：`111...111`（所有位為`1`）。
            - 但因為1’s complement，所以1變0，所以是全0。
        2. **`unsigned int max_uint = ~0u;`**
            - `0u`表示無符號整數`0`。
            - 取反後，同樣得到所有位為`1`的值。
            - 對於`unsigned int`，這表示其能表示的最大值，即`UINT_MAX`。
        
        **總結：**
        
        - 通過`~0`或`~0u`，可以得到對應類型的最大無符號整數值。
        - 這是因為對`0`取反，得到的二進位表示是該類型能表示的最大值。
40. What is the output of the following program ?
    
    ```cpp
    void foo(void) {
    	unsigned int a = 6;
    	int b = -20;
    	(a + b > 6) ? puts(“> 6”) : puts(“<= 6”);
    }
    ```
    
    - Answer
        
        計算無符號int和有符號int時，會全部轉成無符號int，此時-20就會變成超大的正數，因此相加結果一定大於6
        
41. The faster way to an integer multiply by 7 ?
    - Answer
        
        ```cpp
        // Solution 1
        num = (num << 2) + (num << 1) + (num << 0) 
        // *4 + *2 + *1 = *7
        // 左移n就代表乘以2^n
        
        // Solution 2
        int result = (x << 3) - x; // x * 8 - x = x * 7
        ```
        
        - **原因：**
            - 位移運算比乘法運算快，因為它是直接操作二進制位。
            - 減少乘法操作，提高運算效率。
42. Reverse a string
    - Answer
        
        ```cpp
        void reverseString(string& str)
        {
        	int len = str.length();
        	for (int i = 0; i < len / 2; ++i)
        	{
        		swap(str[i], str[len - 1 - i]);
        	}
        }
        ```
        
43. 保證 n 一定是下面五個數字之一，不能用 if 和 switch case，請用你認為最快的方法實作 main
    
    ```cpp
    extern void func1(void);
    extern void func2(void);
    extern void func3(void);
    extern void func4(void);
    extern void func5(void);
    void main(int n)
    {
    	if n==1 execute func1;
    	if n==2 execute func2;
    	if n==3 execute func3;
    	if n==4 execute func4;
    	if n==5 execute func5;
    }
    ```
    
    - Answer
        
        ```cpp
        // Solution 1
        void (*func_array[5])(void) = {func1, func2, func3, func4, func5};
        
        void main(int n)
        {
            if (n >= 1 && n <=5) {
                func_array[n - 1](); // n 從 1 開始，數組索引從 0 開始
            }
        }
        
        // Solution 2
        typedef void (*fp)(void);
        
        fp fpArray[6];
        fpa[1] = func1; // equivalent to fpa[1] = &func1
        fpa[2] = func2;
        fpa[3] = func3;
        fpa[4] = func4;
        fpa[5] = func5;
        
        void main(int n) {
            (*fpa[n])();
        }
        ```
        
44. Find the possible error:
    
    ```cpp
    int ival;
    int **p;
    ival = *p;
    ```
    
    - Answer
        
        **問題分析：**
        
        - `p` 的類型是 `int *`。
        - 將 `int *` 賦值給 `int` 類型的 `ival`，類型不匹配。
        
        **正確的做法：**
        
        - 如果想將 `*p` 賦值給 `ival`：
            
            ```c
            ival = **p;
            
            ```
            
        - 如果 `p` 指向 `int *`，需要解引用兩次。
        - 總結：需要確保左右兩邊的類型匹配，解引用正確的次數。
45. Explain “volatile”. Can we use “const” and “volatile” in the same variable? Can we use “volatile” in a pointer?
    - Answer
        
        **解釋：**
        
        - **`volatile` 關鍵字：**
            - 告訴編譯器該變量可能在程序之外被修改（如硬件、其他線程）。
            - 防止編譯器對該變量進行優化，確保每次訪問都從內存中讀取。
        - **`const` 和 `volatile` 一起使用：**
            - 可以同時使用，表示變量是只讀的，但可能被外部修改。
            - 例子：
                
                ```c
                const volatile int cv_var;
                
                ```
                
        - **在指標中使用 `volatile`：**
            - **指向 volatile 的指標：**
                
                ```c
                int * volatile ptr; // 指標本身是 volatile
                volatile int *ptr;  // 指向 volatile int 的指標
                
                ```
                
            - **解釋：**
                - `int * volatile ptr;`：指標 `ptr` 是易變的，指向的對象是普通的 `int`。
                - `volatile int *ptr;`：指標 `ptr` 指向一個 volatile 的 `int` 變量。
        
        **示例：**
        
        - **硬件寄存器：**
            
            ```c
            volatile unsigned int * const reg_addr = (unsigned int *)0xFFFF0000;
            
            ```
            
            - `reg_addr` 是一個常量指針，指向一個 volatile 的無符號整數，用於硬件寄存器訪問。
        
        **總結：**`volatile` 用於告知編譯器不要對變量進行優化，可與 `const` 結合使用，並可用於指針聲明中。
        
46. What is the possible error of below SQR function:
    
    ```cpp
    int SQR(volatile int *a) { return (*a)*(*a); }
    ```
    
    - Answer
        
        由於*a是volatile，他的值可能隨時被改變，所以第一個(*a)和第二個(*a)可能會不一樣。
        
        如果想要做平方，可以改寫成：
        
        ```cpp
        long SQR(volatile int *a)
        {
        	int b = *a;
        	return b*b;
        }
        // use long to avoid overflow
        ```
        
47. What is wrong with this code?
    
    ```cpp
    // VAR.h
    # ifndef VAR_H
    # define VAR_H
    
    int Var = 0;
    
    # endif
    ```
    
    ```cpp
    // file1.cpp
    #include "Var.h"
    
    void func1() {
        Var = 10;
    }
    ```
    
    ```cpp
    // file2.cpp
    #include "Var.h"
    
    void func2() {
        Var = 20;
    }
    ```
    
    ```cpp
    // main.cpp
    #include <iostream>
    #include "Var.h"
    
    int main() {
        std::cout << "Var = " << Var << std::endl;
        return 0;
    }
    ```
    
    - Answer
        
        會出現重複定義的錯誤。
        
        ```cpp
        duplicate symbol _Var in:
            file1.o
            file2.o
        ld: 1 duplicate symbol for architecture x86_64
        ```
        
        這是因為 `Var.h` 被 `file1.cpp`、`file2.cpp` 和 `main.cpp` 同時包含，導致 `Var` 在每個檔案中都會被定義，但變數不能榮富定義。
        
    

[完整sizeof題型筆記](https://www.notion.so/sizeof-111fdac53b4c8052b34fd0f21d863e74?pvs=21)

[const & *](https://www.notion.so/const-111fdac53b4c806981cfcd26b4cd7b56?pvs=21)