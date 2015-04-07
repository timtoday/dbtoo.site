---
title : mantis bug
layout :  post
tags : 
- Php
---
<div> SYSTEM WARNING :  require_once(\mantis-1.1.0a3-cvs\lang\strings_auto.txt) [function.require-once] :  failed to open stream :  No such file or directory<br/><br/>Fatal error :  require_once() [function.require] :  Failed opening required '\mantis-1.1.0a3-cvs\lang\strings_auto.txt' (include_path='.') in \mantis-1.1.0a3-cvs\core\lang_api.php on line 37<br/><br/><br/><br/>modify this line in email_api.php : <br/>$t_mail-&gt;SetLanguage( lang_get( 'phpmailer_language'), PHPMAILER_PATH . 'language' . DIRECTORY_SEPARATOR );<br/><br/>this bug should be in the mantis-1.1.0a3-cvs. </div>