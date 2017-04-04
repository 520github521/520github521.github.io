---
category: PHP
path: '/PHP'
title: '页面下载'
type: 'http download'

layout: nil
---

### 远程下载方法：

``` public function software_download($file){ 
            $file_name = $file;     //下载文件名    
            $file_dir = "./res/uploads/software/";        //下载文件存放目录    
            //检查文件是否存在    
            if (! file_exists ( $file_dir . $file_name )) {    
                echo "文件找不到";    
                exit ();    
            } else {    
                //打开文件    
                $file = fopen ( $file_dir . $file_name, "r" );    
                //输入文件标签     
                Header ( "Content-type: application/octet-stream" );    
                Header ( "Accept-Ranges: bytes" );    
                Header ( "Accept-Length: " . filesize ( $file_dir . $file_name ) );    
                Header ( "Content-Disposition: attachment; filename=" . $file_name );    
                //输出文件内容     
                //读取文件内容并直接输出到浏览器    
                echo fread ( $file, filesize ( $file_dir . $file_name ) );    
                fclose ( $file );    
                exit ();    
            }     ```  


```public function software_download($file){                
        $url="http://192.168.2.22/res/uploads/".$file;     
        $ch=curl_init();  
        $timeout=3;  
        curl_setopt($ch,CURLOPT_URL,$url);  
        curl_setopt($ch,CURLOPT_FOLLOWLOCATION,1);  
        curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);  
	curl_setopt($ch,CURLOPT_CONNECTTIMEOUT,$timeout);  
        $img=curl_exec($ch);  
        curl_close($ch); 
        $fp2=@fopen("C:/Users/WBF/fp1.exe",'w');  
	fwrite($fp2,$img);  
	fclose($fp2);  
	unset($img,$url);
   }```
