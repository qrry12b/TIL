# 10진수 문자열을 2진수 문자열로 변환하기 (양수)

```
public static void main(String[] args) {
    
    String str = "999999999999";
    StringBuffer buff = new StringBuffer();
    while(str.length() > 1 || str.charAt(0) != '0' && str.charAt(0) != '1') {
        str = to(str,buff);
    }
    buff.insert(0, str);
    System.out.println(buff.toString());
}

public static String to(String str, StringBuffer resultBuff) {
    byte[] strByte = new byte[str.length()];
    
    StringBuffer buff = new StringBuffer();
    int other = 0;
    for(int i=0; i<str.length(); i++) {
        strByte[i] = (byte)(strByte[i] + (str.charAt(i) - '0'));
        
        if(strByte[i] % 2 == 1) {
            if(i+1 < str.length()) {
                strByte[i+1] = (byte)(strByte[i+1] + 10);		
            }else {
                other = 1;
            }
        }
        
        if(buff.length() == 0 && (strByte[i] / 2) == 0) { continue; }
        buff.append(strByte[i] / 2);
        
    }
    resultBuff.insert(0, other);
    return buff.toString();
}
```

음수를 표현하려면 표시할 비트 길이를 정하고 앞에 빈 비트를 0으로 채운 다음   
2의 보수를 구해면 됩니다.