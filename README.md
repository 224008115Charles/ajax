# ԭ��js��ajax��Ʒ���
####����ajax��Ʒ���ԭ������
  1. ����Դ�������õĽǶ��Լ���վ�Ż��Ƕ�ȥ�룬ÿ��Ϊ���Ǽ������ܣ�ȥ����һ����ܣ�������   
  2. �ݶ���w3c��ajax����Ʒ���������level1��level2�Ĺ淶�����ֻ�Ȼ���ʵĸо�    
  3. ����������ajax�Ŀ��򷽰������־���������������Ĳ����泩    
  4. �Լ��Ŀ�ܵײ�ҲҪ��Ҫ�õ�ajax�Ļ������ܣ���get post���󣬶���level2���ϴ���ʱû�õ���   
  5. ��ؼ���Ҳ��֮ǰ��������ʮ��ģ�������Կ�ʼ����ajax������Ʒ���    
  
####һЩ����
  * �������ͬԴ���ԣ������������İ�ȫ���ܣ�ͬԴ��ָ��������Э�飬�˿���ͬ��������д�Ľӿڲ���˿ڷֱ�Ϊ1122��2211������ͬԴ�����ڿ���
  * ajax����һ�ּ�����������������CSS/HTML/Javascript�������������������ṩ��XMLHttpRequest�����������ʹ����������Է���HTTP���������HTTP��Ӧ��
  * nginx����һ�������ܵ�HTTP�ͷ�����������
  * IIS:΢�����ĵķ�������windowϵͳ�Դ�
  * XMLHttpRequest ���������£�
 ? ?![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129224459115-1023971996.png)
  * XMLHttpRequest Level 1��Ҫ��������ȱ��:
    1. ��ͬԴ���Ե����ƣ����ܷ��Ϳ�������
    2. ���ܷ��Ͷ������ļ�����ͼƬ����Ƶ����Ƶ�ȣ���ֻ�ܷ��ʹ��ı����ݣ�
    3. ���ͺͻ�ȡ���ݵĹ����У��޷�ʵʱ��ȡ������Ϣ��ֻ���ж��Ƿ���ɣ�
  * XMLHttpRequest Level 2�����������¹���:
    1. ���Է��Ϳ��������ڷ�������������£�   
    2. ֧�ַ��ͺͽ��ն��������ݣ�    
    3. ����formData����֧�ַ��ͱ����ݣ�   
    4. ���ͺͻ�ȡ����ʱ�����Ի�ȡ������Ϣ��   
    5. ������������ĳ�ʱʱ�䣻   
    
####��ʼ׼��
  * ��ǰ�˴���
  * nginx��������������ǰ��˷����ã�
  * ��̨2�׽ӿڣ��˿ڣ�1122���˿ڣ�2211��  PS��һ�ݱ���֧�ֿ�������
  * IIS�������������̨�ӿڣ�
  * chrome���postman���ӿڲ��ԣ�
  * IE��chrome��firefox��Opera��safari��edge 6����������������Բ���
  
###XMLHttpRequest����������
  1. ʵ����XMLHttpRequest����IE8-9��΢���װ��ActiveXObject('Microsoft.XMLHTTP')�����һ��ʵ��
  2. ͨ��ʵ��openһ���������÷������ͺͽӿ��Լ�ͬ�첽
  3. ������Ҫ���ñ��ģ��Լ������¼���success��error��timeout�ȣ�
  4. ����ʵ����send����������http/https������
  5. �������ص����ͻ��˽��գ�������Ӧ����
  
####�ؼ�����
            //����xhr����
            var xhr = tool.createXhrObject();

            //���ĳЩ�ض��汾��mozillar�������BUG��������
            xhr.overrideMimeType?(xhr.overrideMimeType("text/javascript")):(null);

            //���IE8��xhr������    PS��ie8�µ�xhr��xhr.onload�¼��������������ж�
            xhr.onload===undefined?(xhr.xhr_ie8=true):(xhr.xhr_ie8=false);

            //��������get��post��,����xhr.open     get:ƴ�Ӻ�url��open   post:��open����������������
            ajaxSetting.data === ""?(null):(xhr = tool.dealWithParam(ajaxSetting,this,xhr));

            //���ó�ʱʱ�䣨ֻ���첽������г�ʱʱ�䣩
            ajaxSetting.async?(xhr.timeout = ajaxSetting.time):(null);

            //����httpЭ���ͷ��
            tool.each(ajaxSetting.requestHeader,function(item,index){xhr.setRequestHeader(index,item)});

            //onload�¼���IE8��û�и��¼���
            xhr.onload = function(e) {
                if(this.status == 200||this.status == 304){
                    ajaxSetting.dataType.toUpperCase() == "JSON"?(ajaxSetting.success(JSON.parse(xhr.responseText))):(ajaxSetting.success(xhr.responseText));
                }else{
                    /*
                     *  ���Ϊ�˼���IE8��9�����⣬�Լ�������ɶ���ɵ��������󣬱���404��
                     *   �������������IE8��9�¿���ʧ�ܲ���onerror����
                     *       ����֧����Level 2 �İ汾 ֱ����onerror
                     * */
                    ajaxSetting.error(e.currentTarget.status, e.currentTarget.statusText);
                }
            };

            //xmlhttprequestÿ�α仯һ��״̬����ص��¼�������չ��
            xhr.onreadystatechange = function(){
                switch(xhr.readyState){
                    case 1://��
                        //do something
                        break;
                    case 2://��ȡheader
                        //do something
                        break;
                    case 3://����
                        //do something
                        break;
                    case 4://���
                        //��ie8���棬��xhr��onload�¼���ֻ�ܷ��ڴ˴�����ص����
                        xhr.xhr_ie8?((xhr.status == 200 || xhr.status == 304)?(ajaxSetting.dataType.toUpperCase() == "JSON"?(ajaxSetting.success(JSON.parse(xhr.responseText))):(ajaxSetting.success(xhr.responseText))):(null)):(null);
                        break;
                };
            };

            //ontimeout��ʱ�¼�
            xhr.ontimeout = function(e){
                ajaxSetting.timeout(999,e?(e.type):("timeout"));   //IE8 û��e����
                xhr.abort();  //�ر�����
            };

            //�����¼���ֱ��ajaxʧ�ܣ�������onload�¼�
            xhr.onerror = function(e){
                ajaxSetting.error();
            };

            //��������
            xhr.send((function(result){result == undefined?(result =null):(null);return result;})(this.postParam));
            
###���Դ���
####ǰ��ͬԴ���Դ���
    ajax.post("/api/ajax1/ajaxT1/",{"name":"�����첽post����","age":"success"},function(data){alert(data)});  //�ýӿ���1122��
####ǰ�˿�����Դ���
    ajax.post_cross("http://192.168.0.3:2211/api/weixin/ajaxT2/",{"name":"���Կ���post����","age":"success"},function(data){alert(data)});
####��˿���ӿڴ���
    /// <summary>
    /// ���Կ�������
    /// </summary>
    /// <param name="module"></param>
    /// <returns></returns>
    [Route("ajaxT2")]
    public String kuaAjaxT2([FromBody]TModule module)
    {
        String result = "����post����ɹ���"+module.name+"-"+module.age;
        return result;
    }
####���ͬԴ���Դ���
    /// <summary>
    /// ����ajaxͬԴ����
    /// </summary>
    /// <param qwer="code"></param>
    /// <returns>result</returns>
    [Route("ajaxT2")]
    public String GetkuaAjaxT1(string name,string age)
    {
        String result = "1J����ɹ�:" + name + "-" + age;
        return result;
    }
###�����Ǹ���������Ĳ��Խ�������ṩͬԴpost����Ϳ���post����
#####ͬԴ����
######chrome
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230238927-2089656702.png)
######IE8-9
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230248709-1923043215.png)
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230306302-1703939611.png)
######IE10+
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230347068-1928619242.png)
######firefox
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230452615-113489743.png)
######opera
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230611365-1169854535.png)
######safari
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230713615-2040676482.png)
######edge
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230814818-1130849183.png)

#####�������
######chrome
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230913599-1189375449.png)
######IE8-9
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230942302-1611540664.png)
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129230956990-1637046338.png)
######IE10+
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129231021209-1271264367.png)
######firefox
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129231044943-1370903842.png)
######opera
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129231104552-1637987456.png)
######safari
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129231127834-855103713.png)
######edge
![](http://images2015.cnblogs.com/blog/801930/201611/801930-20161129231145693-678151401.png)

####��������ѷ�װ��һ��js�⣬����Ҹ�����Ŀ�����Լ��������ƣ��������Ѿ���װ��һЩ��������
  * �첽get����  --  ajax.get
  * �첽post����  --  ajax.post
  * ͬ��get����  --  ajax.getSync
  * ͬ��post����  --  ajax.postSync
  * ����get����  --  ajax.getCross
  * ����post����  --  ajax.postCross
  * ͨ����������  --  ajax.common
PS���÷���Ϊ����ʹ�ã����õĿ���ֱ��ʹ�þ���汾��ֻ��common���� 

####�������˰���µ��о����о�ajax����Ʒ���������˵�������кܴ���ջ�ģ�����������˽⣬js���˽⣬�������������˽⣬��˵���ϰ�����кܴ�Ľ����ģ��ر��ǽ��������������о�������һ��level����Ȼ��ʱ��ûȥ��˾������С��˾�ε������Ǵ�û�з����Լ���ִ�ŵ�׷����һ��Ŀ��bat��ϣ������ͨ���ҵ�Ŭ������ȥ���ٽ���һ��ϴ�񡣲�����ʱ���������ƾͺ��ˣ�����Ϊ��ǰ�˼ܹ�ʦ�����룬Ϊ���Լ���ǰ�˼ܹ�����������Ŭ����ȥ��������δ��������Զ...

###�汾���½���
  1. ������Ҫ��ǰ�����ÿ���������ͷ������ɾ�� ?==>author:keepfool from cnblog
  2. ����toolһЩ������ӵ��es5+�¼��� ? ? ? ? ? ?==>author:	pod4g from github  
  
####���˽���
  * �Ա���
  * ���ã�Ů
  * ����Ŀ�꣺ǰ�˼ܹ�ʦ
  * ְҵĿ�꣺ȫջ�ܹ�ʦ
  * ����:http://www.cnblogs.com/GerryOfZhong
