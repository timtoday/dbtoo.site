---
title : ORACLE prucedure写法与调用
layout :  post
tags : 
- Linux
- Oracle
---
<div> <br/>create or replace procedure test_procedure (a in number,b out varchar2)<br/>as<br/>    v_a number(20);<br/>  v_b varchar2(20);<br/>begin<br/>  v_a  : = a;<br/>     select C1 into v_b from test where id=v_a ;<br/>  b : =v_b;<br/>end;<br/>-----------------<br/>调用 : <br/>-------------------<br/>declare<br/>     v_result varchar(20);<br/>begin<br/> <br/>     test_procedure(a =&gt; 5,b =&gt; v_result);<br/>     dbms_output.put_line(v_result);<br/> <br/>end;<br/><br/><br/>==============================================================<br/>create or replace procedure test_procedure2(a varchar2)<br/>as<br/>begin <br/> dbms_output.put_line(a);<br/>end;<br/>----------<br/>调用<br/>---------------<br/>begin<br/> test_procedure2(a =&gt;'111111');<br/>end;<br/><br/> </div>