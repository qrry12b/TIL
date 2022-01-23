# 마이크를 통해 음성 녹음 후 wav파일로 저장하기

```
<body>
    <button id="btnStart">Start</button>
    <button id="btnStop">Stop</button>
    <script type="text/javascript">

    let btnStart;
    let btnStop;

    let stream;
    let mediaRecorder;

    document.addEventListener("DOMContentLoaded", () => {
        btnStart = document.getElementById('btnStart');
        btnStop = document.getElementById('btnStop');
        if(btnStart == null || btnStop == null) return;

        btnStart.addEventListener('click',()=>{
            (async function(){
                if(mediaRecorder) {
                    mediaRecorder.stop();
                    mediaRecorder = stream = null;
                }

                if(navigator === void 0 || navigator.mediaDevices === void 0) {
                    alert('브라우저가 지원하지 않거나 보안 연결이 아닙니다');
                    return;
                }

                try{
                    let result = await navigator.permissions.query({name:'microphone'});
                    if(permissionResult.state === 'denied') {
                        alert('시스템 또는 사용자가 마이크에 대한 액세스를 명시적으로 차단했습니다');
                        return;
                    }
                }catch(error) { }

                try{
                    stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: false });
                }catch(error) {}

                if(stream === void 0 || stream === null) {
                    alert('요청한 기기를 찾을 수 없습니다');
                    return;
                }
                
                const recordedChunks = [];
                mediaRecorder = new MediaRecorder(stream, {mimeType: 'audio/webm'});

                mediaRecorder.addEventListener('dataavailable', function(e) {
                    if (e.data.size > 0) recordedChunks.push(e.data);
                });

                mediaRecorder.addEventListener('stop', function() {
                    let downloadLink = document.createElement('a');
                    downloadLink.href = URL.createObjectURL(new Blob(recordedChunks));
                    downloadLink.download = 'acetest.wav';
                    downloadLink.click();
                });

                mediaRecorder.start();

            })();
        });

        btnStop.addEventListener('click',()=>{
            if(mediaRecorder) {
                mediaRecorder.stop();
                mediaRecorder = stream = null;
            }
        });

    });
    </script>
</body>
```

//TODO//

### REF
* [recording-audio](https://developers.google.com/web/fundamentals/media/recording-audio)