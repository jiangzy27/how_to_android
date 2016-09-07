##读取json
我们首先在assets目录下，创建一个json文件

```
{
  "language":[
    {"id":1,"ide":"eclipse","name":"java"},
    {"id":2,"ide":"zend","name":"php"},
    {"id":3,"ide":"android studio","name":"android"}
  ],
  "cat":"computer"
}
```
这里要注意，在创建json的时候，键值对一定都要加双引号，不加或者单引号都将导致解析失败。

```
            try {
                InputStream is = getResources().getAssets().open("a.json");//返回的是字节流InputStram流类型
                //我们要读取字符，就要将字节流包装成字符流
                InputStreamReader isr = new InputStreamReader(is,"UTF-8");
                //进一步将字符流包装成buffer流，buffer会自动提供一个缓冲区
                BufferedReader bfr = new BufferedReader(isr);
                //System.out.println(bfr.readLine());//好处是可以直接读取一行数据

                String line;
                StringBuilder sb = new StringBuilder();
                while((line =bfr.readLine())!=null){
                    sb.append(line);
                }
                //解析json
                try {
                    JSONObject json = new JSONObject(sb.toString());
                    System.out.println(json.getString("cat"));
                    JSONArray array = json.getJSONArray("language");
                    for(int i=0;i<array.length();i++){
                        JSONObject lan = array.getJSONObject(i);
                        System.out.println(lan.getInt("id"));
                    }

                } catch (JSONException e) {
                    e.printStackTrace();
                }

                bfr.close();
                isr.close();
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
```


