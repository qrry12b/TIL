# Java - Properties 파일 읽기

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

```
public static void main(String[] args) throws Exception{
    Properties properties = new Properties();
    properties.load(new FileInputStream("./test.properties"));

    for(String str : properties.stringPropertyNames()) {
        if(str.equals("version")) {continue;}
        System.out.println(str+" : "+properties.getProperty(str));
    }
    System.out.println("version : "+properties.getProperty("version"));
}
```