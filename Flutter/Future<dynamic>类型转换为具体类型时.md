Future<dynamic>类型转换为具体类型时，需要用async await修饰
```
HttpUtil().post(SERVER_API_URL + LOGIN, data: params) ///返回的为Future类型

    Map<String, dynamic> map = await HttpUtil().post(SERVER_API_URL + LOGIN, data: params);

    print(map);
```

