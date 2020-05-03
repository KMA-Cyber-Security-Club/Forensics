# Task 3: Love cat vl :(

[File](https://drive.google.com/file/d/1aUzjLB34NmNccRjjJbib9UnPUsoXW-p0/view?usp=sharing)

Với dạng bài này, có thể là Image Forensics hoặc Memory Forensic, chúng ta bắt đầu với file:

```
vm@ubuntu:~/Downloads$ file love_Cat
love_Cat: data
```

Có vẻ là memory dump, chúng ta sẽ xác định thông tin về HĐH với volatility:
```
vm@ubuntu:~/Downloads$ volatility -f love_Cat imageinfo
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/vm/Downloads/love_Cat)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800027f90a0L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff800027fad00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-03-20 14:27:22 UTC+0000
     Image local date and time : 2019-03-20 15:27:22 +0100
```
Có vẻ là Win7SP1x64...

Sau đó chúng ta xem các các tiến trình với pstree:

```
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat pstree
Volatility Foundation Volatility Framework 2.6
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa80024e6300:explorer.exe                     1896   1848     35    992 2019-03-20 14:14:50 UTC+0000
. 0xfffffa8001ae4060:firefox.exe                     1088   1896      0 ------ 2019-03-20 14:15:16 UTC+0000
. 0xfffffa8000e79060:DumpIt.exe                      2964   1896      2     45 2019-03-20 14:27:20 UTC+0000
. 0xfffffa80025797d0:VBoxTray.exe                    1296   1896     13    140 2019-03-20 14:14:51 UTC+0000
 0xfffffa8001f4c910:wininit.exe                       388    332      3     74 2019-03-20 14:14:42 UTC+0000
. 0xfffffa800175a700:services.exe                     484    388      9    194 2019-03-20 14:14:42 UTC+0000
.. 0xfffffa80021a3b30:svchost.exe                     780    484     20    441 2019-03-20 14:14:43 UTC+0000
... 0xfffffa8002228060:audiodg.exe                   1008    780      6    129 2019-03-20 14:14:43 UTC+0000
.. 0xfffffa800227eab0:mscorsvw.exe                   2840    484      6     82 2019-03-20 14:16:50 UTC+0000
.. 0xfffffa80022db060:spoolsv.exe                    1180    484     12    277 2019-03-20 14:14:44 UTC+0000
.. 0xfffffa800216c060:VBoxService.ex                  672    484     13    136 2019-03-20 14:14:43 UTC+0000
.. 0xfffffa80022765f0:svchost.exe                     552    484     10    256 2019-03-20 14:14:43 UTC+0000
.. 0xfffffa800253d360:svchost.exe                    2992    484     14    319 2019-03-20 14:16:53 UTC+0000
.. 0xfffffa800220b660:svchost.exe                     948    484     32    924 2019-03-20 14:14:43 UTC+0000
.. 0xfffffa8001ec2630:sppsvc.exe                      852    484      4    141 2019-03-20 14:16:52 UTC+0000
.. 0xfffffa8002327290:svchost.exe                    1216    484     17    295 2019-03-20 14:14:44 UTC+0000
.. 0xfffffa800238d260:svchost.exe                    1312    484     16    236 2019-03-20 14:14:44 UTC+0000
.. 0xfffffa8000e92060:svchost.exe                    2820    484      8    108 2019-03-20 14:26:37 UTC+0000
.. 0xfffffa8002290b30:svchost.exe                    1064    484     14    450 2019-03-20 14:14:43 UTC+0000
.. 0xfffffa8002139b30:svchost.exe                     728    484      8    260 2019-03-20 14:14:43 UTC+0000
.. 0xfffffa8002150b30:svchost.exe                     612    484     10    346 2019-03-20 14:14:43 UTC+0000
... 0xfffffa80007ffb30:WmiPrvSE.exe                  2036    612      6    110 2019-03-20 14:18:46 UTC+0000
... 0xfffffa80007e3060:dllhost.exe                   3024    612     22    541 2019-03-20 14:17:04 UTC+0000
.. 0xfffffa800219c310:mscorsvw.exe                   1576    484      6     73 2019-03-20 14:16:52 UTC+0000
.. 0xfffffa80024bf3a0:taskhost.exe                   1832    484      9    173 2019-03-20 14:14:50 UTC+0000
.. 0xfffffa80021e96d0:SearchIndexer.                 1912    484     12    587 2019-03-20 14:14:54 UTC+0000
... 0xfffffa800262eb30:SearchProtocol                2340   1912      8    280 2019-03-20 14:27:07 UTC+0000
... 0xfffffa800076bb30:SearchFilterHo                2940   1912      5    107 2019-03-20 14:27:07 UTC+0000
.. 0xfffffa80021d7b30:svchost.exe                     892    484     19    434 2019-03-20 14:14:43 UTC+0000
... 0xfffffa80024deb30:dwm.exe                       1876    892      3     70 2019-03-20 14:14:50 UTC+0000
. 0xfffffa8002010700:lsm.exe                          508    388     10    146 2019-03-20 14:14:42 UTC+0000
. 0xfffffa8002020740:lsass.exe                        500    388      7    546 2019-03-20 14:14:42 UTC+0000
 0xfffffa8001f6cb30:csrss.exe                         340    332     10    390 2019-03-20 14:14:42 UTC+0000
 0xfffffa8001f50b30:csrss.exe                         400    380      8    233 2019-03-20 14:14:42 UTC+0000
. 0xfffffa8000e2f490:conhost.exe                     2556    400      2     53 2019-03-20 14:27:20 UTC+0000
 0xfffffa8001eef680:winlogon.exe                      440    380      5    113 2019-03-20 14:14:42 UTC+0000
 0xfffffa800069b040:System                              4      0     81    495 2019-03-20 14:14:39 UTC+0000
. 0xfffffa800183e040:smss.exe                         268      4      2     29 2019-03-20 14:14:39 UTC+0000
```

Chúng ta thấy có process Firefox.exe cần được chú ý. Đây là trình duyệt Mozilla Firefox, được mở từ PID 1896 - explorer.exe.
Tiếp theo chúng ta sẽ quét các thành phần mạng với netscan:

```
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat netscan
Volatility Foundation Volatility Framework 2.6
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created
0x1e1967f0         UDPv4    0.0.0.0:3702                   *:*                                   1312     svchost.exe    2019-03-20 14:14:55 UTC+0000
0x1e1f09c0         UDPv4    0.0.0.0:3702                   *:*                                   1312     svchost.exe    2019-03-20 14:14:55 UTC+0000
0x1e1f09c0         UDPv6    :::3702                        *:*                                   1312     svchost.exe    2019-03-20 14:14:55 UTC+0000
0x1e1f0ec0         UDPv4    0.0.0.0:3702                   *:*                                   1312     svchost.exe    2019-03-20 14:14:55 UTC+0000
0x1e1f0ec0         UDPv6    :::3702                        *:*                                   1312     svchost.exe    2019-03-20 14:14:55 UTC+0000
0x1e2a8a80         UDPv4    0.0.0.0:5355                   *:*                                   1064     svchost.exe    2019-03-20 14:14:53 UTC+0000
0x1e2a8a80         UDPv6    :::5355                        *:*                                   1064     svchost.exe    2019-03-20 14:14:53 UTC+0000
0x1e2bf010         UDPv4    0.0.0.0:3702                   *:*                                   1312     svchost.exe    2019-03-20 14:14:55 UTC+0000
0x1e2cd3e0         UDPv4    0.0.0.0:5355                   *:*                                   1064     svchost.exe    2019-03-20 14:14:53 UTC+0000
0x1e323700         UDPv4    0.0.0.0:0                      *:*                                   1064     svchost.exe    2019-03-20 14:14:49 UTC+0000
0x1e323700         UDPv6    :::0                           *:*                                   1064     svchost.exe    2019-03-20 14:14:49 UTC+0000
0x1e393820         UDPv4    10.0.2.15:137                  *:*                                   4        System         2019-03-20 14:14:49 UTC+0000
0x1e3f8600         UDPv4    0.0.0.0:50779                  *:*                                   1312     svchost.exe    2019-03-20 14:14:44 UTC+0000
0x1e3fabb0         UDPv4    0.0.0.0:50780                  *:*                                   1312     svchost.exe    2019-03-20 14:14:44 UTC+0000
0x1e3fabb0         UDPv6    :::50780                       *:*                                   1312     svchost.exe    2019-03-20 14:14:44 UTC+0000
0x1e52bec0         UDPv4    10.0.2.15:138                  *:*                                   4        System         2019-03-20 14:14:49 UTC+0000
0x1e719ec0         UDPv6    ::1:1900                       *:*                                   1312     svchost.exe    2019-03-20 14:16:51 UTC+0000
0x1e725ec0         UDPv4    127.0.0.1:49318                *:*                                   1312     svchost.exe    2019-03-20 14:16:51 UTC+0000
0x1e7299c0         UDPv4    10.0.2.15:1900                 *:*                                   1312     svchost.exe    2019-03-20 14:16:51 UTC+0000
0x1e7349c0         UDPv6    fe80::7552:153e:65ac:6478:1900 *:*                                   1312     svchost.exe    2019-03-20 14:16:51 UTC+0000
0x1e743ec0         UDPv6    ::1:49317                      *:*                                   1312     svchost.exe    2019-03-20 14:16:51 UTC+0000
0x1e7e3b10         UDPv4    127.0.0.1:1900                 *:*                                   1312     svchost.exe    2019-03-20 14:16:51 UTC+0000
0x1e05d330         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        484      services.exe   
0x1e05d330         TCPv6    :::49155                       :::0                 LISTENING        484      services.exe   
0x1e273ef0         TCPv4    10.0.2.15:139                  0.0.0.0:0            LISTENING        4        System         
0x1e2da7a0         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        948      svchost.exe    
0x1e2da7a0         TCPv6    :::49154                       :::0                 LISTENING        948      svchost.exe    
0x1e2fa2c0         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x1e2fa2c0         TCPv6    :::49156                       :::0                 LISTENING        500      lsass.exe      
0x1e334830         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x1e3dc930         TCPv4    0.0.0.0:5357                   0.0.0.0:0            LISTENING        4        System         
0x1e3dc930         TCPv6    :::5357                        :::0                 LISTENING        4        System         
0x1e58ba70         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        728      svchost.exe    
0x1e596d10         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        728      svchost.exe    
0x1e596d10         TCPv6    :::135                         :::0                 LISTENING        728      svchost.exe    
0x1e5a07c0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        388      wininit.exe    
0x1e5cdd90         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        780      svchost.exe    
0x1e5d0940         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        780      svchost.exe    
0x1e5d0940         TCPv6    :::49153                       :::0                 LISTENING        780      svchost.exe    
0x1e6f6540         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        948      svchost.exe    
0x1e712ef0         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        484      services.exe   
0x1e7140f0         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x1e7140f0         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x1e131cf0         TCPv4    -:49176                        99.80.68.141:80      CLOSED           1088     firefox.exe    
0x1e5ce450         TCPv6    -:0                            383b:1a02:80fa:ffff:984c:2202:80fa:ffff:0 CLOSED           1        pU????       
0x1e6f5cf0         TCPv6    -:0                            48b0:6900:80fa:ffff:48b0:6900:80fa:ffff:0 CLOSED           1        pU????       
0x1ee45990         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        388      wininit.exe    
0x1ee45990         TCPv6    :::49152                       :::0                 LISTENING        388      wininit.exe    
```
Đúng như dự đoán, chúng ta có 1 kết nối đến địa chỉ IP: 99.80.68.141:80 từ process firefox.exe.
Tiếp theo, chúng ta tiến hành tìm kiếm file với từ khóa "cat"
```
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat filescan | grep -i "cat"
Volatility Foundation Volatility Framework 2.6
0x00000000047f6b70     16      0 R--rwd \Device\HarddiskVolume2\Windows\System32\clbcatq.dll
0x000000001de06850     15      1 RW-rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010001.wsb
0x000000001de07070     15      1 RW-rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010001.wid
0x000000001de07750     12      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-Client-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
0x000000001de08130      1      1 RW---- \Device\HarddiskVolume2\Windows\System32\catroot2\edb.log
0x000000001dec9430     16      0 -W-rw- \Device\HarddiskVolume2\Users\Noxious\Desktop\cat (8).jpgcat (8).jpg
0x000000001e10a340     16      0 R--r-- \Device\HarddiskVolume2\Users\Noxious\Desktop\cat (9).jpg
0x000000001e151800     13      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot2\{127D0A1D-4EF2-11D1-8608-00C04FC295EE}\catdb
0x000000001e19c070     32      1 -W-r-- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\GatherLogs\SystemIndex\SystemIndex.2.gthr
0x000000001e19d3f0      1      1 RW---- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\MSS.log
0x000000001e19d5c0      1      1 RW---- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Windows.edb
0x000000001e19dc90     17      0 R--r-- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\MSS.log
0x000000001e1a03e0     10      0 R--r-- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Windows.edb
0x000000001e1aaf20     18      1 RW-rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\SecStore\CiST0000.000
0x000000001e1e3e90     16      0 RW-r-- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010002.dir
0x000000001e207cd0     33      1 RWDr-d \Device\HarddiskVolume2\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-Application.etl
0x000000001e21f200      2      0 RW-rw- \Device\HarddiskVolume2\Users\Noxious\AppData\Roaming\Microsoft\Windows\Recent\cat (8).lnk
0x000000001e220bc0      3      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\ntpe.cat
0x000000001e2da6b0      2      1 ------ \Device\NamedPipe\Winsock2\CatalogChangeListener-3b4-0
0x000000001e2f3ab0      9      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-Media-Format-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
0x000000001e305bb0      3      1 R--rwd \Device\HarddiskVolume2\Windows\System32\config\systemprofile\AppData\Roaming\Microsoft\SystemCertificates\My
0x000000001e30df20      1      1 RW---- \Device\HarddiskVolume2\Windows\System32\catroot2\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\catdb
0x000000001e312890      1      1 RW---- \Device\HarddiskVolume2\Windows\System32\catroot2\{127D0A1D-4EF2-11D1-8608-00C04FC295EE}\catdb
0x000000001e31ec70      1      1 R--rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010001.dir
0x000000001e38af20     18      1 RW-r-- \Device\HarddiskVolume2\Windows\System32\winevt\Logs\Microsoft-Windows-Application-Experience%4Program-Telemetry.evtx
0x000000001e423750     16      0 -W---- \Device\HarddiskVolume2\Windows\System32\CodeIntegrity\bootcat.cache
0x000000001e518760     16      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-OfflineFiles-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
0x000000001e592320      2      1 ------ \Device\NamedPipe\Winsock2\CatalogChangeListener-2d8-0
0x000000001e5a35d0      2      1 ------ \Device\NamedPipe\Winsock2\CatalogChangeListener-184-0
0x000000001e5cebc0      2      1 ------ \Device\NamedPipe\Winsock2\CatalogChangeListener-30c-0
0x000000001e5cf5a0      2      0 -W-r-- \Device\HarddiskVolume2\Users\Noxious\Desktop\cat (8).jpgcat (8).jpg
0x000000001e5ecca0     17      1 RW-rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\INDEX.000
0x000000001e686a30      4      1 RW-rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\PropMap\CiPT0000.000
0x000000001e6a8c10     15      0 R--r-d \Device\HarddiskVolume2\Windows\System32\clbcatq.dll
0x000000001e6bdd70      9      0 R--r-d \Device\HarddiskVolume2\Windows\SysWOW64\clbcatq.dll
0x000000001e6cba20     28      1 RW-r-- \Device\HarddiskVolume2\Windows\System32\winevt\Logs\Application.evtx
0x000000001e70f910      2      1 ------ \Device\NamedPipe\Winsock2\CatalogChangeListener-1e4-0
0x000000001eae4f20     16      0 RW-r-- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010001.dir
0x000000001ec28ea0     32      1 -W-r-- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\GatherLogs\SystemIndex\SystemIndex.2.Crwl
0x000000001ec87330     18      1 RW-r-- \Device\HarddiskVolume2\Windows\System32\winevt\Logs\Microsoft-Windows-Application-Experience%4Problem-Steps-Recorder.evtx
0x000000001eee5c70     17      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot2\edb.log
0x000000001ef76c50     18      1 RW-r-- \Device\HarddiskVolume2\Windows\System32\winevt\Logs\Microsoft-Windows-Application-Experience%4Program-Compatibility-Troubleshooter.evtx
0x000000001efbd070     16      1 RW-rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010002.wid
0x000000001efbe2d0      6      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot2\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\catdb
0x000000001f1d1a20      2      1 ------ \Device\NamedPipe\Winsock2\CatalogChangeListener-1f4-0
0x000000001f1d37f0     13      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-Common-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
0x000000001f47cd10     19      1 RWD--- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\tmp.edb
0x000000001f47d6d0      3      1 R--rwd \Device\HarddiskVolume2\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SystemCertificates\My
0x000000001f47dc80      1      1 R--rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010002.dir
0x000000001f62c420     13      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-Client-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
0x000000001fe29d50     12      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-Common-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
0x000000001fe70450      1      1 R--rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010001.ci
0x000000001fe8c050      9      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\nt5.cat
0x000000001fea25a0      7      0 R--r-- \Device\HarddiskVolume2\Windows\System32\catroot\{F750E6C3-38EE-11D1-85E5-00C04FC295EE}\Microsoft-Windows-Foundation-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
0x000000001feb59b0      1      1 R--rw- \Device\HarddiskVolume2\ProgramData\Microsoft\Search\Data\Applications\Windows\Projects\SystemIndex\Indexer\CiFiles\00010002.ci
```
Chúng ta thấy có 2 file cat (8).jpg và cat (9).jpg, chúng ta sẽ dump 2 file đó để khai thác thông tin:
```
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat dumpfiles -Q 0x000000001e10a340 -n --dump-dir=dump/
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x1e10a340   None   \Device\HarddiskVolume2\Users\Noxious\Desktop\cat (9).jpg
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat dumpfiles -Q 0x000000001e5cf5a0 -n --dump-dir=dump/
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x1e5cf5a0   None   \Device\HarddiskVolume2\Users\Noxious\Desktop\cat (8).jpgcat (8).jpg
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat dumpfiles -Q 0x000000001dec9430  -n --dump-dir=dump/
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x1dec9430   None   \Device\HarddiskVolume2\Users\Noxious\Desktop\cat (8).jpgcat (8).jpg
```
Sau một hồi thử với file, strings, binwalk, stegsolve,... chúng ta đã có thêm thông tin với exiftool:
```vm@ubuntu:~/Downloads/dump$ exiftool cat8.jpg
ExifTool Version Number         : 10.80
File Name                       : cat8.jpg
Directory                       : .
File Size                       : 44 kB
File Modification Date/Time     : 2020:04:29 14:48:47+07:00
File Access Date/Time           : 2020:05:03 15:02:21+07:00
File Inode Change Date/Time     : 2020:05:03 15:01:41+07:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 96
Y Resolution                    : 96
XMP Toolkit                     : Image::ExifTool 11.16
Creator                         : 99.80.68.141
Image Width                     : 582
Image Height                    : 328
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 582x328
Megapixels                      : 0.191
```
Vậy chúng ta sẽ bắt đầu nghiên cứu về địa chỉ IP: 99.80.68.141
Chúng ta bắt đầu tiến hành dump tất cả bộ nhớ của process firefox và tiến hành nghiên cứu:
```
vm@ubuntu:~/Downloads$ volatility --profile=Win7SP1x64 -f love_Cat memdump -p 1088 -D dump/
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing firefox.exe [  1088] to 1088.dmp
```
```
vm@ubuntu:~/Downloads/dump$ strings 1088.dmp | grep -w "cat\|jpg"
cat+H
cat-H
.jpg.edr
.jpg
cat (6).lnk
cat (6).lnk
.jpg
.jpg
cat (8).lnk
cat (2).lnk
cat (2).lnk
cat (0).lnk
cat (0).lnk
cat (3).lnk
cat (3).lnk
cat (11).lnk
cat (11).lnk
cat (8).lnk
cat (9).lnk
cat (9).lnk
cat (1).lnk
cat (1).lnk
cat (10).lnk
cat (10).lnk
cat (7).lnk
cat (7).lnk
.jpg
.jpg
cat (10).lnk
.jpg
.jpg
.jpg
.cat
.jpg
.jpg
.jpg
.jpg
.jpg
Microsoft-Windows-VirtualPC-USB-RPM-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-GPUPipeline-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Gadget-Platform-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-ICM-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Gadget-Platform-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-GPUPipeline-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-GroupPolicy-ClientExtensions-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Client-Wired-Network-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Client-Wired-Network-Drivers-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-ClipsInTheLibrary-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Client-LanguagePack-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Client-Features-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
ofMicrosoft-Windows-Client-Features-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Common-Modem-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Disk-Diagnosis-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Disk-Diagnosis-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Editions-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-UltimateEdition~31bf3856ad364e35~amd64~~6.1.7600.16385.cat\l#m`
Microsoft-Windows-WindowsFoundation-LanguagePack-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Display-ChangeDesktopBackground-Disabled-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat|%
Microsoft-Windows-Display-ChangeDesktopBackground-Disabled-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-CodecPack-Basic-Encoder-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Common-Drivers-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-CodecPack-Basic-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Printing-Foundation-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Printing-Foundation-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
76Microsoft-Windows-SnippingTool-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat'
Microsoft-Windows-Printing-Foundation-Starter-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-Printing-Foundation-Starter-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Printing-Foundation-Starter-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-ClipsInTheLibrary-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat%
Microsoft-Windows-CodecPack-Basic-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Common-Modem-Drivers-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat%
Microsoft-Windows-SampleContent-Ringtones-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-SearchEngine-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-SearchEngine-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-Customization-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-HomeBasicEdition~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
owMicrosoft-Windows-TerminalServices-CommandLineTools-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-SnippingTool-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Server-Help-Package.ClientProfessional~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat|P
Microsoft-Windows-Starter-Features-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-StarterEdition~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Starter-Features-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-WMPNetworkSharingService-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Xps-Foundation-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
lLMicrosoft-Windows-Xps-Foundation-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Networking-MPSSVC-Rules-BusinessEdition-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-WMPNetworkSharingService-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-WMI-SNMP-Provider-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Shell-InboxGames-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat(
Microsoft-Windows-Shell-InboxGames-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-ShareMedia-ControlPanel-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Security-SPP-Component-SKU-Ultimate-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-ShareMedia-ControlPanel-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat-Window
Microsoft-Windows-Personalization-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-PhotoPremiumPackage~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Printer-Drivers-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
tiMicrosoft-Windows-PhotoBasicPackage~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Shell-PremiumInboxGames-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-CommandLineTools-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-WinOcr-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-Publishing-WMIProvider-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-Publishing-WMIProvider-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Telnet-Server-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-MiscRedirection-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-MiscRedirection-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-RemoteApplications-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat\l#m
Microsoft-Windows-BusinessScanning-Feature-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Common-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Client-Drivers-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Branding-Ultimate-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-BusinessScanning-Feature-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAUE-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-Customization-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Server-Help-Package.ClientUltimate~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-PeerDist-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-PeerDist-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-ParentalControls-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-PeerToPeer-Full-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-GroupPolicy-ClientTools-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAHB-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-GroupPolicy-ClientTools-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAHB-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-StarterEdition~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-Sidebar-Killbits-SDP-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat~31bf38
Microsoft-Windows-StickyNotes-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-StickyNotes-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Shell-HomeGroup-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-SystemRestore-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Indexing-Service-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~en-US~8.0.7600.16385.cat
Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~8.0.7600.16385.cat
Microsoft-Windows-InternetExplorer-Package~31bf3856ad364e35~amd64~en-US~8.0.7600.16385.cat(
Microsoft-Windows-IIS-WebServer-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Indexing-Service-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-IIS-WebServer-AddOn-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-InternetExplorer-Package~31bf3856ad364e35~amd64~~8.0.7600.16385.cat
Microsoft-Windows-MediaPlayer-DVDRegistration-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Security-SPP-Component-SKU-Enterprise-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-EnterpriseEdition~31bf3856ad364e35~amd64~~6.1.7600.16385.cat%
Microsoft-Windows-Security-SPP-Component-SKU-HomeBasic-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-MediaCenter-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-MobilePC-Client-SideShow-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
owMicrosoft-Windows-MSMQ-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Anytime-Upgrade-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Anytime-Upgrade-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Foundation-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAHB-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAHP-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAHP-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Help-CoreClientUAHP-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAHB-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Networking-MPSSVC-Rules-EnterpriseEdition-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Networking-MPSSVC-Rules-HomeBasicEdition-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Networking-MPSSVC-Rules-HomeBasicEdition-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Networking-MPSSVC-Rules-HomePremiumEdition-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAPE-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAPE-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Help-CoreClientUAPE-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAPS-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Help-CoreClientUAPS-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Printing-Foundation-Starter-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Branding-Enterprise-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Branding-Enterprise-Client-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-RemoteAssistance-Package-Client~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Branding-HomeBasic-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-UsbRedirector-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat(
Microsoft-Windows-TerminalServices-UsbRedirector-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-TerminalServices-WMIProvider-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-RemoteAssistance-Package-Client~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-RecDisc-SDP-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Printing-LocalPrinting-Enterprise-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat$
Microsoft-Windows-Security-SPP-Component-SKU-HomePremium-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-Security-SPP-Component-SKU-Professional-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-TFTP-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-RDC-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Printing-PremiumTools-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat(
PaMicrosoft-Windows-Printing-XPSServices-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat$
Microsoft-Windows-Tuner-Drivers-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Server-Help-Package.ClientEnterprise~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat`
Server-Help-Package.ClientHomeBasic~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Server-Help-Package.ClientHomeBasic~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Server-Help-Package.ClientHomeBasic~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Server-Help-Package.ClientEnterprise~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Server-Help-Package.ClientHomeBasic~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Server-Help-Package.ClientHomePremium~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Printing-XPSServices-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-WindowsMediaPlayer-Troubleshooters-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Server-Help-Package.ClientStarter~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Server-Help-Package.ClientStarter~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Server-Help-Package.ClientStarter~31bf3856ad364e35~amd64~~6.1.7600.16385.cat\l#m`
Server-Help-Package.ClientStarter~31bf3856ad364e35~amd64~~6.1.7601.17514.cat\l#m`
Networking-MPSSVC-Rules-StarterEdition-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Networking-MPSSVC-Rules-UltimateEdition-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat(
Microsoft-Windows-Anytime-Upgrade-Results-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUASE-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUAUE-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Help-CoreClientUASE-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Help-CoreClientUASE-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Branding-HomeBasic-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Branding-HomeBasic-Client-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat\
Microsoft-Windows-Branding-HomePremium-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Branding-HomePremium-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
Microsoft-Windows-Branding-HomePremium-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Branding-Starter-Client-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-Branding-Ultimate-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat.
Microsoft-Windows-Branding-HomePremium-Client-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-Branding-Professional-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7601.17514.cat
Microsoft-Windows-Branding-Professional-Client-Package~31bf3856ad364e35~amd64~~6.1.7600.16385.cat
Microsoft-Windows-Branding-Professional-Client-Package~31bf3856ad364e35~amd64~~6.1.7601.17514.cat
Microsoft-Windows-Branding-Starter-Client-Package~31bf3856ad364e35~amd64~en-US~6.1.7600.16385.cat
.jpg
.jpg
C:\Users\Noxious\Desktop\cat (8).jpg
C:\Users\Noxious\Desktop\cat (7).jpg
C:\Users\Noxious\Desktop\cat (9).jpg
C:\Users\Noxious\Desktop\cat (2).jpg
C:\Users\Noxious\Desktop\cat (11).jpg
C:\Users\Noxious\Desktop\cat (3).jpg
C:\Users\Noxious\Desktop\cat (1).jpg
C:\Users\Noxious\Desktop\cat (10).jpg
C:\Users\Noxious\Desktop\cat (0).jpg
<HTML><HEAD><STYLE>BODY {font-family: Tahoma;font-size: 10pt;color: 415432;margin-left: 25 px;margin-top: 25 px;background-position: top left;background-repeat: repeat;}</STYLE></HEAD> <BODY background="greenbubbles.jpg"></BODY> </HTML> 
<HTML><HEAD><STYLE>BODY {font-family: Tahoma;font-size: 10pt;color: 415432;margin-left: 25 px;margin-top: 25 px;background-position: top left;background-repeat: repeat;}</STYLE></HEAD> <BODY background="greenbubbles.jpg"></BODY> </HTML> 
<HTML><HEAD><STYLE>BODY {font-family: Tahoma;font-size: 10pt;color: ffffff;margin-left: 20 px;margin-top: 20 px;background-position: top left;background-repeat: repeat;}</STYLE></HEAD> <BODY background="Peacock.jpg"></BODY> </HTML> 
cat (10)
```
Ta thấy có thêm rất nhiều file cat.jpg đã đượC tải về, và đã bị xóa, có lẽ chúng vẫn còn ở server với địa chỉ IP: 99.80.68.141. Do server đã bị tắt nên chúng ta không thể tiếp tục, tuy nhiên em có tìm đưỢc writeup nên chúng ta sẽ tiếp tục phàn còn lại ở: 

> https://ptr-yudai.hatenablog.com/entry/2019/03/25/152043#Foren-994pts-Cat-Hunting

Khi truy cập địa chỉ IP: 99.80.68.141, ta có trang web sau:

![](3.1.png)

Sử dụng dirsearch, chúng ta có:
```
$ ./dirsearch.py -u http://99.80.68.141/ -e ''
...
[14:37:38] 403 -  295B  - /.htusers
[14:38:43] 301 -  310B  - /img  ->  http://99.80.68.141/img/
[14:38:45] 200 -    2KB - /index.php
[14:38:45] 200 -    2KB - /index.php/login/
[14:39:16] 403 -  300B  - /server-status
...
```
File chúng ta tìm được ở thư mục http://99.80.68.141/img/, sau khi thử truy cập vào tất cả các file cat (x).jpg, chúng ta có file cat (11).jpg thực ra là một mã hóa base64. Decode nó và chúng ta có flag:
```
$ cat "cat (11).jpg" 
c2VjdXJpbmV0c3tkMjU3MzZmZWJmZDgwOWVjNGViYTc2YjBhYWU5ZWFiMH0K
$ cat "cat (11).jpg" | base64 -d
securinets{d25736febfd809ec4eba76b0aae9eab0}
```

>Flag: securinets{d25736febfd809ec4eba76b0aae9eab0}