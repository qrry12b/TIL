# JSON 파일 열기 및 저장하기

웹페이지에서 JSON 파일을 읽거나 JSON 파일로 저장합니다

```
<style>
    html,body {margin: 0; padding: 0; border: 0;}

    input {float: left;}
    input[type=file] {display: none;}
    
    td:before, td:after {display: block; content: '';line-height: 0;}
    td:after {clear:both;}
</style>
```

```
<body>
    <table id="table">
        <thead>
            <tr>
                <td>
                    <input type="file">
                    <input type="button" value="Open">
                    <input type="button" value="Save">
                </td>
            </tr>
        </thead>
        <tbody>
        </tbody>
        <tfoot>
            <tr>
                <td>
                    <input type="text" name="key">
                    <input type="text" name="value">
                    <input type="button" value="Put" name="put">
                </td>
            </tr>
        </tfoot>
    </table>
    <script>
        var table, thead, tbody, tfoot;
        var inputFile, inputKey, inputValue;
        document.addEventListener("DOMContentLoaded", () => {
            table = document.getElementById('table');
            thead = table.getElementsByTagName('thead')[0];
            tbody = table.getElementsByTagName('tbody')[0];
            tfoot = table.getElementsByTagName('tfoot')[0];

            Array.from(tfoot.children[0].children[0].children).forEach((val,idx)=>{
                if(val.getAttribute('name') === 'key') {
                    inputKey = val;
                }
                else if(val.getAttribute('name') === 'value') {
                    inputValue = val;
                }
                else if(val.getAttribute('name') === 'put') {
                    val.addEventListener('click',(e)=>{
                        if(tbody) {
                            let tr = document.createElement('tr');
                            let td = document.createElement('td');

                            let input1 = document.createElement('input');
                            input1.setAttribute('type','text');
                            input1.value = inputKey.value;

                            let input2 = document.createElement('input');
                            input2.setAttribute('type','text');
                            input2.value = inputValue.value;

                            td.append(input1);
                            td.append(input2);
                            tr.append(td);
                            tbody.append(tr);

                            inputKey.value = inputValue.value = '';
                        }
                    });
                }
            });

            Array.from(thead.children[0].children[0].children).forEach((val,idx)=>{
                if(val.type === 'button') {
                    if(val.value === 'Save') {
                        val.addEventListener('click',(e)=>{
                            let json = { };

                            Array.from(tbody.children).forEach((tr)=>{
                                let children = tr.children[0].children;
                                let key = children[0].value;
                                let value = children[1].value;
                                json[key] = value;
                            });

                            let downloadLink = document.createElement('a');
                            var jsonStr = JSON.stringify( json , null , '\t');
                            var blob = new Blob([jsonStr], {type: "application/json"});
                            var url  = URL.createObjectURL(blob);
                            
                            let d = new Date();
                            let fileName = d.getFullYear() +'_'+ (d.getMonth()+1) +'_'+ d.getDate() +'_'+ d.getHours()+'_'+d.getMinutes()+'_'+d.getSeconds()+'_'+(Math.floor(100 + Math.random()*100));

                            downloadLink.href = url;
                            downloadLink.download = fileName+'.json';
                            downloadLink.click();
                        });
                    } else if(val.value === 'Open') {
                        val.addEventListener('click',(e)=>{
                            inputFile && inputFile.click();
                        });
                    }
                } else if(val.type === 'file') { 
                    inputFile = val;
                    inputFile.addEventListener('change',(e)=>{
                        let length = inputFile.files.length;
                        if(length !== void 0 && length !== null &&  typeof length === 'number' && length > 0) {
                            let file = inputFile.files[0];
                            let reader = new FileReader();
                            reader.onload = function(event) {
                                try{
                                    let result = JSON.parse(event.target.result);
                                    while(tbody && tbody.lastChild) {
                                        tbody.removeChild(tbody.lastChild);
                                    }
                                    let keys = Object.keys(result);
                                    for(let i=0 ;i<keys.length; i++) {
                                        let key = keys[i], value = result[key];

                                        let tr = document.createElement('tr');
                                        let td = document.createElement('td');

                                        let input1 = document.createElement('input');
                                        input1.setAttribute('type','text');
                                        input1.value = key;

                                        let input2 = document.createElement('input');
                                        input2.setAttribute('type','text');
                                        input2.value = value;

                                        td.append(input1);
                                        td.append(input2);
                                        tr.append(td);
                                        tbody.append(tr);
                                    }

                                }catch(e) {
                                    if(e instanceof SyntaxError) {
                                        alert('JSON Parse SyntaxError');
                                    }
                                }
                            };
                            reader.readAsText(file);
                        }
                    });
                }
            });
        });
    </script>
</body>
```