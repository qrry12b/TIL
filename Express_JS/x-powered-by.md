# Express.js X-Powered-By 헤더 비활성화

검색해보면 X-Powered-By 정보는 서버의 정보를 알 수 있어 더 쉽게 보안상 취약점을 검색하는데 사용될 수 있습니다.   

미들웨어를 사용해 제거하는 경우도 있지만 미들웨어는 지나가는 모든 요청에 대해 처리를 하기 때문에   
미들웨어 사용보다는 Express 설정에서 처음부터 비활성화하는 방법을 권장합니다.   

다음과 같이 X-Powered-By 헤더를 비활성화 할 수 있습니다.
```
app.disable('x-powered-by');
```

### REF
* [docs](http://expressjs.com/en/api.html#app.set)