

#### 	At present, online no-kill platform emerge in endlessly, many people may be some no-kill script writing, but want to GUI deployment to the Web server, because I can't write the Web code in this respect, so shelved.

#### 	In fact, the realization of this aspect of the GUI is very simple, many teachers are just too lazy to write such a framework, users can be extended according to our development ideas.





```
<?php

$shellcode = $_POST['shellcode'] ?? null;

if (empty($shellcode)||$_SERVER['REQUEST_METHOD'] != 'POST') {
    header('HTTP/1.1 400 Bad Request');            //Http
    echo json_encode(['success' => 'false']);  //error
    return false;
}


    // $url = "https://service-5369sd4f-1258472441.sh.apigw.tencentcs.com/bootstrap-2.min.js";

    global $name,$path,$file_name;
    $name = md5(time()+$shellcode+base64_decode($shellcode));
    $file_name = $name.".exe";
    $file_dir = "./build/";        // 
    $path = $file_dir . $file_name;

    $shellcode = sprintf("python ./Bypass/Input.py %s %s",$shellcode,$name);

    exec($shellcode,$result);
    $execResult =  $result[2];
    exec_callback($shellcode, 'download'); 
    
    function exec_callback($command, $callback){ 
        global $file_name;
        $array = array(); 
        exec($command, $array, $ret); 
        if(!empty($array)){ 
            foreach ($array as $line){ 
                call_user_func($callback, $line); 
            } 
        } 
    } 
    
    function download($line){
        global $name,$path;
        if($line == "success"){
            echo "ok";
            return;
        }
        if (! file_exists ( $path )) {    
            header('HTTP/1.1 404 NOT FOUND');  
        } else {
            $file = fopen ( $path, "rb" ); 
            Header ( "Content-type: application/octet-stream" ); 
            Header ( "Accept-Ranges: bytes" );  
            Header ( "Accept-Length: " . filesize ( $path ) );  
            header('Content-Disposition: attachment; filename="'.$name.'.exe"');
            echo fread ( $file, filesize ( $path ) );    
            fclose ( $file );    
            exit ();    

       }  
    }
```
