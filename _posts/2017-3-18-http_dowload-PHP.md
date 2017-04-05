---
category: PHP
path: '/PHP'
title: '页面下载'
type: 'http download'

layout: nil
---

### 远程下载方法：

```public function software_download($file){ 
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
            Header ( "Accept-Length: " . filesize ( $file_dir . $file_name));    
            Header ( "Content-Disposition: attachment; filename=".$file_name);    
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
        $fp2=@fopen("C:/fp1.exe",'w');  
	fwrite($fp2,$img);  
	fclose($fp2);  
	unset($img,$url);
   }```
 但是上面两种方法很容易出现php内存溢出，需要修改php.ini：memory_limit ，下面方法可以避免（第一个方法支持断点续传）：  
 
 
```public function software_download($file){ 
        $filePath= './res/uploads/software/'.$file;
        //设置文件最长执行时间和内存
        set_time_limit ( 0 );
        //ini_set ( 'memory_limit', '1024M' );
        //检测文件是否存在
        if (! is_file ( $filePath )) {
        die ( "<b>404 File not found!</b>" );
        }
        $filename = basename ( $filePath ); //获取文件名字
        //开始写输出头信息 
        header ( "Cache-Control: public" );
        //设置输出浏览器格式
        header ( "Content-Type: application/octet-stream" );
        header ( "Content-Disposition: attachment; filename=" . $filename );
        header ( "Content-Transfer-Encoding: binary" );
        header ( "Accept-Ranges: bytes" );
        $size = filesize ( $filePath );
        $range=0;
        //如果有$_SERVER['HTTP_RANGE']参数
        if (isset ( $_SERVER ['HTTP_RANGE'] )) {
        /*Range头域 　　Range头域可以请求实体的一个或者多个子范围。
        例如，
        表示头500个字节：bytes=0-499
        表示第二个500字节：bytes=500-999
        表示最后500个字节：bytes=-500
        表示500字节以后的范围：bytes=500-
        第一个和最后一个字节：bytes=0-0,-1
        同时指定几个范围：bytes=500-600,601-999
        但是服务器可以忽略此请求头，如果无条件GET包含Range请求头，
	响应会以状态码206（PartialContent）返回而不是以200 （OK）.
        */
        // 断点后再次连接 $_SERVER['HTTP_RANGE'] 的值 bytes=4390912-
        list ( $a, $range ) = explode ( "=", $_SERVER ['HTTP_RANGE'] );
        //if yes, download missing part
        $size2 = $size - 1; //文件总字节数
        $new_length = $size2 - $range; //获取下次下载的长度
        header ( "HTTP/1.1 206 Partial Content" );
        header ( "Content-Length: {$new_length}" ); //输入总长
        header ( "Content-Range: bytes {$range}-{$size2}/{$size}" ); 
	//Content-Range: bytes 4908618-4988927/4988928 95%的时候
        } else {
        //第一次连接
        $size2 = $size - 1;
        header ( "Content-Range: bytes 0-{$size2}/{$size}" ); 
	//Content-Range: bytes 0-4988927/4988928
        header ( "Content-Length: " . $size ); //输出总长
        }
        //打开文件
        $fp = fopen ( "{$filePath}", "rb" );
        //设置指针位置
        fseek ( $fp, $range );
        //输出
        while ( ! feof ( $fp ) ) {
        print ( fread ( $fp, 1024 * 8 ) ); //输出文件
        flush (); //输出缓冲
        ob_flush ();
        }
        fclose ( $fp );
        exit ();
 } ```	  
 
```public function software_download($file){    
        set_time_limit(0); // 设置脚本执行时间无限长
        $srcPath = './res/uploads/software/'.$file;
        $dstPath = 'c:/'.$file;
        if (!$fpSrc = fopen($srcPath, "rb"))
        {
            return false;
        }

        $isWriteFileOpen = false; // 写文件 是否已打开？
        do
        {
            $data = fread($fpSrc, 8192); // 每次读取 8*1024 bit =1024 byte=1kb
            if (!$data)
            {
                break;
            }
            else if (!$isWriteFileOpen)
            {
                // 第一次读取文件，并且有内容，才创建文件
                $fpDst = fopen($dstPath, "wb");
                $isWriteFileOpen = true;
                fwrite($fpDst, $data);
            }
            else
            {
                // 写入
                fwrite($fpDst, $data);
            }
        } while (true);

        fclose($fpSrc);
        fclose($fpDst);

        return true;
 } ```	
