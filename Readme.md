# Renew IP on TP-Link TD8811B
This small php script diconnect the WAN interface to the ISP (Internet Service Provider) gateway and reconnect again after 10 seconds. Normally you get new IP from ISP.



![Title](https://raw.githubusercontent.com/bohnelang/TPLinkTD881Brenewip/main/td8811b2.jpg)

```

#!/usr/bin/php -q
<?php

$adminname      = "admin";
$adminpasswd    = "*your*router*password*"; 	
$iprouter       = "192.168.1.1"; 

$opts = array(
        'http' => array(
                'method'=>"GET",
                'header' => array( 'host: $iprouter',"Authorization: Basic ".base64_encode("$adminname:$adminpasswd"))
        )
);

$context = stream_context_create($opts);

printf("Old IP: %s\n", trim(file_get_contents("https://checkip.amazonaws.com/"))                                                          );
$s = file_get_contents("http://$iprouter/wancfg.cmd?action=view&linkCtrl=0&vpi=1&vci=32&conId=1",false,$context);
sleep(10);

$s = file_get_contents("http://$iprouter/wancfg.cmd?action=view&linkCtrl=1&vpi=1&vci=32&conId=1",false,$context);
sleep(10);

printf("New IP: %s\n", trim(file_get_contents("https://checkip.amazonaws.com/")));
exit(0);


?>
```
![Title](https://raw.githubusercontent.com/bohnelang/TPLinkTD881Brenewip/main/td8811b1.jpg)