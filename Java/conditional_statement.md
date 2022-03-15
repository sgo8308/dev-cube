### switch문은 왜 한 번 조건에 걸리면 break을 만날 때까지 모두 실행되게 만들어 졌을까?
    
    break를 넣고 안 넣고를 선택하게 만듬으로써 많은 조건 분기들을 간편하게 구현할 수 있기 때문이다.
    
    대신에 break를 써넣지 않아서 발생하는 실수도 많아졋다.
    
### 왜 switch의 비교대상변수는 long이 아닌 정수와 enum만 될까?
    
    switch같은 경우 컴파일 된 바이트 코드에서 jvm instruction set 중 tableswitch와 lookupswitch라는 opcode를 사용한다.
    이들의 피연산자는 int값만 가능하다. byte, short, enum같은 경우 모두 int로 형변환되지만,
    long이나 float,double같은 경우 int로 형변환할 경우 원치않은 값이 나올 수 있기 때문에 형변환하지 못한다. 
    
    따라서 long,float,double 타입을 위한 instuction을 새로 만들어야 하는데,
    long, float, double을 스위치의 값으로 사용할 일이 거의 없기 때문에,
    long, float, double값도 처리할 수 있는 instruction을 instruction set에 넣을 필요성을 느끼지 못해서 switch에서는 지원하지 않는 것 같다.
    
### switch는 내부적으로 어떻게 동작할까?
    - switch의 인자로 정수가 왔을 때
        
        자바 컴파일러는 switch가 바이트 코드로 변환될 때
        jvm instruction set 중에서 tableswitch와 lookupswitch 중 하나를 사용한다.
        
        - tableswitch
            
            모든 case의 값이 뭉쳐 있는 경우
            
            (ex case 1: , case 2: , case 3: ...  or case 1:, case 3:, case 5 ...)
            
            swtich는 tableswtich instruction을 사용한다.
            
            다음과 같은 코드가 있다고 가정하자.
            
            public int simpleSwitch(int intOne) {
                switch (intOne) {
                    case 0:
                        return 3;
                    case 1:
                        return 2;
                    case 4:
                        return 1;
                    default:
                        return -1;
                }
            }
            
            이렇게 case가 연속하지 않고 떨어진 경우
            
            다음과 같은 바이트 코드를 만들어 낸다.
            
            0: iload_1
             1: tableswitch   {
                     default: 42
                         min: 0
                         max: 4
                           0: 36
                           1: 38
                           2: 42
                           3: 42
                           4: 40
                }
            36: iconst_3
            37: ireturn
            38: iconst_2
            39: ireturn
            40: iconst_1
            41: ireturn
            42: iconst_m1
            43: ireturn
            
            compiler는 가짜 case(2,3)들을 만들고,
            이 case에 해당하면 default 조건(42번 줄의 코드)으로 이동하게 한다.
            (만약 default가 없다면 switch문 바깥으로 나간다.)
            
            이렇게 함으로써 단지 switch의 인자가 min값과 max값 사이에 있는지 판단하고
            만약 있다면 이 인자를 key로 사용하여 어떤 줄로 점프해서 명령을 수행할지 바로 알 수 있기 때문에 속도가 빠르다.
            이동해야 할 명령줄을 찾는데 O(1)의 시간이 걸린다.
            
        - lookupswitch
            
            만약 case들이 상당히 퍼져있는 경우,
            컴파일러는 가짜 case들을 많이 만들어야 하기 때문에 메모리 효율적이지 않다.
            
            예를 들어 case 1: , case 100: 이렇게 있다면 가짜 케이스를 99개를 만들어야 한다.
            
            이 때는 lookupswitch를 사용한다.
            
            다음과 같은 코드의 경우 case들이 퍼져 잇다.(tableswitch를 슨다면 가짜 case를 20개는 만들어야 한다.)
            
            public int simpleSwitch(int intOne) {
                switch (intOne) {
                    case 10:
                        return 1;
                    case 20:
                        return 2;
                    case 30:
                        return 3;
                    default:
                        return -1;
                }
            }
            
            이 때는 이러한 바이트 코드를 만들어낸다.
            
            0: iload_1
             1: lookupswitch  {
                     default: 42
                       count: 3
                          10: 36
                          20: 38
                          30: 40
                }
            36: iconst_1
            37: ireturn
            38: iconst_2
            39: ireturn
            40: iconst_3
            41: ireturn
            42: iconst_m1
            43: ireturn
            
            가짜 case들이 없다.
            
            이 때는 switch문의 인자를 key로 사용할 수 없다.
            왜냐하면 존재하지 않는 case라면 점프해야 할 위치를 특정할 수 없기 때문이다.
            
            따라서 이 때는 모든 case에 대해 하나하나 비교해가며 맞는 케이스를 찾으면 그 케이스의 코드 블록이 위치한 곳으로 점프한다.
            
            lookupswitch의 테이블은 정렬되어 있고, 
            lookupswitch는 맞는 값을 찾기 위해 binarysearch를 사용하므로,
            시간 복잡도는 O(logN)이다.
            
            따라서 tableswitch에 비해 수행 시간이 오래 걸린다.
            
    - switch의 인자로 문자열이 왔을 때
        
        
### 왜 switch문에서 case에는 상수만 올 수 있을까?