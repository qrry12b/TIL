# 자바 비트 연산자

Bitwise OR (|)
대응되는 비트 중에서 하나라도 1이면 1을 반환합니다
00000011 | 01000000 = 01000011

Bitwise AND (&)
대응되는 비트가 모두 1일때 1을 반환합니다
01100011 | 01000010 = 0100010

Bitwise XOR (^) 
대응되는 비트가 서로 다를경우 1을 반환합니다
00010010 ^ 11110000 = 11100010

Bitwise Complement [NOT] (~)
비트를 0은 1로, 1은 0으로 반전합니다
~01010101 = 10101010

Bit-Shift Operators (Shift Operators)
비트를 이동시킵니다

<< 명시된 수만큼 비트를 왼쪽으로 이동합니다
ex) 00000111 << 2 = 00011100

\>> 부호를 유지하면서 비트를 오른쪽으로 이동합니다
ex ) 10001111 >> 2 = 10000011

\>> 지정한 수만큼 비트를 이동시키며 새로운 비트는 0이 됩니다
ex ) 10001101 >>> 2 = 00100011