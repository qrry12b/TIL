# Java - SAXParser 로 HTML 문서 구조 출력하기

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

```
/* org.xml.sax.helpers.DefaultHandler 의 구현 */

public static void main(String[] args) throws ParserConfigurationException, SAXException, IOException {
    SAXParserFactory spf = SAXParserFactory.newInstance();
    SAXParser sp = spf.newSAXParser();

    Test1 m = new Test1();
    sp.parse(new File("./test2.xml"), m);
}

private List<String> list = new ArrayList<String>();

@Override
public void startDocument() throws SAXException {
    System.out.println("Start Document");
}

public void endDocument() throws SAXException {
    System.out.println("End Document");
};

@Override
public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
    int index = attributes.getIndex("id");
    String t = qName+(index!=-1?"#"+attributes.getValue(index):"");
    list.add(t);
    StringBuffer buf = new StringBuffer();
    for(String str : list) {buf.append(str+" > ");}
    buf.setLength(buf.length()-3);
    System.out.println(buf.toString());
}

@Override
public void endElement(String uri, String localName, String qName) throws SAXException {
    list.remove(list.size()-1);
}
```

간단하게 사용할때는 DOM 방식보다 자원소모가 적고 속도가 빠른편이다.   
대신 DOM 방식보다 제한되는 부분도 있다.