```ts
    await apiRequest ({
      method: "POST",
      url: "http://3.39.168.74:8080/notice/Insert_Notice",
      data: _values
    })
    .then( response => {
      console.log("success");
    })
    .catch( error => {
      console.log("fail");
    });
```
```ts
    //let accessToken = localStorage.getItem('accessToken');

    try {
      const res = await apiRequest.post(
        'http://3.39.168.74:8080/notice/Insert_Notice',
        _values,
        {
          headers: {
            'Content-Type': 'multipart/form-data'
            //"X-AUTH-TOKEN": accessToken
          }
        }
      );
      console.log(res);
    } catch (e) {
      console.log(e);
    }
```
