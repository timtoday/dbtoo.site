---
title : python采集百度博客
layout :  post
tags : 
- Python
---
终于弄到一个好点的模板，一口气把百度的陈年旧文字全采过来了，简单的Py脚本就搞掂啦
```python
import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )

from bs4 import BeautifulSoup
import urllib
import os


def u2md(url):    
    html_doc = urllib.urlopen(url).read()

    soup = BeautifulSoup(html_doc)
    ct = unicode(soup.find(id="content"))
   
    h2 = unicode(soup.h2.string)
    print h2

    ctmk=u'---\n'
    ctmk=ctmk+u'title:'+h2+'\n'
    ctmk=ctmk+u'layout: post\n'
    ctmk=ctmk+u'---\n'
    ctmk=ctmk+ct
     
    

    fileHandle = open ( h2+'.md', 'w' ) 
    fileHandle.write (ctmk) 
    fileHandle.close() 

utls = ['http://hi.baidu.com/tim_today/item/7565d62e954a55fd51fd8776',
    'http://hi.baidu.com/tim_today/item/6fc9733981bca585b611db7a',
    'http://hi.baidu.com/tim_today/item/5a6b94914a6730dd1a49df7a',
    'http://hi.baidu.com/tim_today/item/5bee01d2a448de91270ae77a',
    'http://hi.baidu.com/tim_today/item/7f14fc150569856a70d5e87a',
    'http://hi.baidu.com/tim_today/item/8c904a54645dae908d12ed7a',
    'http://hi.baidu.com/tim_today/item/99493ac35117867188ad9e76',
    'http://hi.baidu.com/tim_today/item/95547e30a7748ff22684f47a',
    'http://hi.baidu.com/tim_today/item/60af2b7b0789b8336f29f67a',
    'http://hi.baidu.com/tim_today/item/7d2fa63d8481628ff4e4ad76',
    'http://hi.baidu.com/tim_today/item/fcbec936e75278bd623aff7a',
    'http://hi.baidu.com/tim_today/item/72948e3265630f352f0f817a',
    'http://hi.baidu.com/tim_today/item/715a1daca88a1313a9cfb776',
    'http://hi.baidu.com/tim_today/item/aa3a43dc92acd43ce2108f7a',
    'http://hi.baidu.com/tim_today/item/4f081dd35f19591a20e25073',
    'http://hi.baidu.com/tim_today/item/55f3ed64f6daf2107cdecc77',
    'http://hi.baidu.com/tim_today/item/e358f81e1ed32a673e87ce77',
    'http://hi.baidu.com/tim_today/item/d58b91a23783d5218819d377',
    'http://hi.baidu.com/tim_today/item/6bfcc616910090ef9913d677',
    'http://hi.baidu.com/tim_today/item/6f5c87653e94132368105b73',
    'http://hi.baidu.com/tim_today/item/f647f7ded214b53a49e1dd77',
    'http://hi.baidu.com/tim_today/item/bcfd4d45e3b65292823ae177',
    'http://hi.baidu.com/tim_today/item/b4549f418fc40136fb896073',
    'http://hi.baidu.com/tim_today/item/49b9ef219370584247996273',
    'http://hi.baidu.com/tim_today/item/aaba5f4b320a0beba4c06673',
    'http://hi.baidu.com/tim_today/item/ca7d6993454f7a30326eeb77',
    'http://hi.baidu.com/tim_today/item/4fa56e45f34a56f0dd0f6c73',
    'http://hi.baidu.com/tim_today/item/4dd0b00a11c3a314acdc7073',
    'http://hi.baidu.com/tim_today/item/59a402e17aaa25adc00d7573',
    'http://hi.baidu.com/tim_today/item/3fc27c0d4d8f32d972e67673',
    'http://hi.baidu.com/tim_today/item/bbbacaf42ee6bb0e85d27873',
    'http://hi.baidu.com/tim_today/item/ae06ceee9d69dbc7baf37d73',
    'http://hi.baidu.com/tim_today/item/344782ed869a8f2b5a7cfb77',
    'http://hi.baidu.com/tim_today/item/2b277c0d216823ef34990273',
    'http://hi.baidu.com/tim_today/item/396cfbecfcc40e0864db0053',
    'http://hi.baidu.com/tim_today/item/e9128959c4575e3194eb0553',
    'http://hi.baidu.com/tim_today/item/6bec4113c128934fe65e0653',
    'http://hi.baidu.com/tim_today/item/0866cd345dab1e80c3cf2951',
    'http://hi.baidu.com/tim_today/item/10039f0e689265e5fe240d53',
    'http://hi.baidu.com/tim_today/item/034fe76a1d07470da0cf0f53',
    'http://hi.baidu.com/tim_today/item/311bd409084b3f69d55a1153',
    'http://hi.baidu.com/tim_today/item/790ba7f4694d6c12cf9f3251',
    'http://hi.baidu.com/tim_today/item/744f84f902515058c8f33751',
    'http://hi.baidu.com/tim_today/item/ad4845089f520f8e03ce1b53',
    'http://hi.baidu.com/tim_today/item/fb991aca2655bcddef183b51',
    'http://hi.baidu.com/tim_today/item/cb064439b819f8fbdf222153',
    'http://hi.baidu.com/tim_today/item/6d4fd87db2029c3a70442353',
    'http://hi.baidu.com/tim_today/item/a56240277b9d988a6f2cc351',
    'http://hi.baidu.com/tim_today/item/66636736e2689824b3c0c551',
    'http://hi.baidu.com/tim_today/item/df7ece366a6e22f2a9842853',
    'http://hi.baidu.com/tim_today/item/f489bfff15b38bc60cd1c851',
    'http://hi.baidu.com/tim_today/item/3b9757cc7e785f0bac092f53',
    'http://hi.baidu.com/tim_today/item/61f290d9aa783f1dd68ed051',
    'http://hi.baidu.com/tim_today/item/744184f902515058c8f33753',
    'http://hi.baidu.com/tim_today/item/ad78c239706c980bcfb9fe27',
    'http://hi.baidu.com/tim_today/item/867e4532e77d04c42f8ec229',
    'http://hi.baidu.com/tim_today/item/7754c82e954a55fd51fd8727',
    'http://hi.baidu.com/tim_today/item/b8c82a13b48367fcddeeca29',
    'http://hi.baidu.com/tim_today/item/1fbc0c3f2a4d61fb97f88d27',
    'http://hi.baidu.com/tim_today/item/e75cc4f129f848da6225d229',
    'http://hi.baidu.com/tim_today/item/cd5e365aefb62e17abf6d729',
    'http://hi.baidu.com/tim_today/item/f775e8ded214b53a49e1dd29',
    'http://hi.baidu.com/tim_today/item/4f0d44ec2dcb353e87d9de29']
for u in utls:
    u2md(u)  
    


```