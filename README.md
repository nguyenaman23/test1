<?php
error_reporting(100000);
system('clear');
include("setting.php");
$resss="\033[0m";
$ress = "\033[0;32m";
$res = "\033[0;33m";
$red = "\033[0;31m";
$green = "\033[0;37m";
$yellow = "\033[0;33m";
$white = "\033[0;33m";
$xnhac = "\033[1;96m";
$den = "\033[1;90m";
$do = "\033[1;91m";
$luc = "\033[1;92m";
$vang = "\033[1;93m";
$xduong = "\033[1;94m";
$hong = "\033[1;95m";
$trang = "\033[1;97m";
$tbar = $trang."~ ".$do."[".$luc."✔".$do."]".$trang." => ".$luc;



BANNER("litecoinmania",1);

print $tbar."Wallet : ".$hong.$WALLET."\n";

for($v=1;$v<=30;$v++)
{
  print $trang."= ";
} print "\n";
$stt = 0;
$HEADER_A = [
  "Host:litecoinmania.com",
  "user-agent:".$USERAGENT,
  "cookie:".$COOKIE
];
while(true)
{
$RES = CURL("https://litecoinmania.com/?cc=reCaptcha",2,"",$HEADER_A);
if(count(explode('<input type="text" name="',$RES)) == 2){
$NAME_W = explode('"',explode('<input type="text" name="',$RES)[1])[0];


print $tbar."Bypassing captcha!!!\n"; 

$CAP = RECPV2("https://litecoinmania.com/?cc=reCaptcha","6LcwJsocAAAAAHPRnyPMnjH22QNieflwfMI5TQNS");

if($CAP == 0)
{
  print $do."\rBypass captch failed!!\r";
} else {
  $DATA = array(
    $NAME_W => $WALLET,
    "g-recaptcha-response" => $CAP
  );
  print $tbar."Bypass success!!\n";
  $CLAIM = CURL("https://litecoinmania.com/?cc=reCaptcha",1,$DATA,$HEADER_A);
  if(strpos($CLAIM,"8 satoshi was sent to you"))
  
  {
    $stt++;
    print $tbar."Success claim 8 satoshi!! ".$do."|".$luc." Count: ".$hong.$stt."\n";
    
for($v=1;$v<=30;$v++)
{
  print $trang."= ";
} print "\n";
  } else {
    print $tbar.$do."Claim Failed!! \n";
    
for($v=1;$v<=30;$v++)
{
  print $trang."= ";
} print "\n";
  }

}
}
}
function CURL ($URL,$POST,$ARRAY,$HEADER)
{
  $INIT = curl_init();
  curl_setopt($INIT, CURLOPT_URL, $URL);
  curl_setopt($INIT, CURLOPT_RETURNTRANSFER, TRUE);
  curl_setopt($INIT,CURLOPT_HTTPHEADER, $HEADER);
  curl_setopt($INIT,CURLOPT_TIMEOUT, 10);
  curl_setopt($INIT,CURLOPT_FOLLOWLOCATION, TRUE);
  curl_setopt($INIT,CURLOPT_CONNECTTIMEOUT, 30);
  curl_setopt($INIT,CURLOPT_SSL_VERIFYHOST, FALSE);
  curl_setopt($INIT,CURLOPT_SSL_VERIFYPEER, FALSE);
  if($POST == 1)
  {
    curl_setopt($INIT, CURLOPT_POSTFIELDS, $ARRAY);
    curl_setopt($INIT, CURLOPT_CUSTOMREQUEST, "POST");
  } else {
    curl_setopt($INIT, CURLOPT_CUSTOMREQUEST, "GET");
  }
  $EXEC = curl_exec($INIT);
  $ERROR = curl_error($INIT);
  if($ERROR)
  {
    return false;
  } else {
    return $EXEC;
  }
}




function HCAPTCHA ($WEBSTIE,$KEY)
{
  $JSON = json_encode(array(
    "clientKey" => "pkg.5f273ad813994170a0156e70bd6e66af",
    "task" => array(
      "type" => "HCaptchaTaskProxyless",
      "websiteURL" => $WEBSTIE,
      "websiteKey" => $KEY
    )
  ));
  $HEADER = array(
    "Host: api.anycaptcha.com",
    "Content-Type: application/json"
  );
  $S1 = CURL("https://api.anycaptcha.com/createTask",1,$JSON,$HEADER); 
  $JS = json_decode($S1,TRUE);
  if($JS["errorId"] == 0)
  {
  $DATA = json_encode(array(
    "clientKey" => "pkg.5f273ad813994170a0156e70bd6e66af",
    "taskId"    => $JS["taskId"]
  ));
  while(true)
  {
  $S2 = CURL("https://api.anycaptcha.com/getTaskResult",1,$DATA,$HEADER);
  $JSON = json_decode($S2,TRUE);

  if($JSON["errorCode"] == "SUCCESS"){
    if($JSON["status"] == "ready")
    {
      return $JSON["solution"]["gRecaptchaResponse"];
    }
  } else {
    return 0;
    break;
  }
  }
  }else{
    return 0;
  }
}
function RECPV2 ($WEBSTIE,$KEY)
{
  $JSON = json_encode(array(
    "clientKey" => "pkg.5f273ad813994170a0156e70bd6e66af",
    "task" => array(
      "type" => "RecaptchaV2TaskProxyless",
      "websiteURL" => $WEBSTIE,
      "websiteKey" => $KEY,
      "isInvisible" => "false"
    )
  ));
  $HEADER = array(
    "Host: api.anycaptcha.com",
    "Content-Type: application/json"
  );
  $S1 = CURL("https://api.anycaptcha.com/createTask",1,$JSON,$HEADER); 
  $JS = json_decode($S1,TRUE);
  if($JS["errorId"] == 0)
  {
  $DATA = json_encode(array(
    "clientKey" => "pkg.5f273ad813994170a0156e70bd6e66af",
    "taskId"    => $JS["taskId"]
  ));
  while(true)
  {
  $S2 = CURL("https://api.anycaptcha.com/getTaskResult",1,$DATA,$HEADER);
  $JSON = json_decode($S2,TRUE);
//  DELAY(30);
  if($JSON["errorCode"] == "SUCCESS"){
    if($JSON["status"] == "ready")
    {
      return $JSON["solution"]["gRecaptchaResponse"];
    }
    DELAY(25);
  } else {
    return 0;
    break;
  }
  }
  }else{
    return 0;
  }
}
function DELAY ($count)
{
  for($i=$count; $i>=0; $i--){
    print "\033[1;93m\rWait.... $i sec!!!   \r";
    sleep(1);
}
}

function BANNER($NAME_TOOL,$AD)
{
$resss="\033[0m";
$ress = "\033[0;32m";
$res = "\033[0;33m";
$red = "\033[0;31m";
$green = "\033[0;37m";
$yellow = "\033[0;33m";
$white = "\033[0;33m";
$xnhac = "\033[1;96m";
$den = "\033[1;90m";
$do = "\033[1;91m";
$luc = "\033[1;92m";
$vang = "\033[1;93m";
$xduong = "\033[1;94m";
$hong = "\033[1;95m";
$trang = "\033[1;97m";
$BANNER = "
            ███╗   ██╗████████╗ █████╗ 
            ████╗  ██║╚══██╔══╝██╔══██╗
            ██╔██╗ ██║   ██║   ███████║
            ██║╚██╗██║   ██║   ██╔══██║
            ██║ ╚████║   ██║   ██║  ██║
            ╚═╝  ╚═══╝   ╚═╝   ╚═╝  ╚═╝
";
for($v=1;$v<=30;$v++)
{
  print $trang."= ";
} print "\n";
print $do.$BANNER."\n";
$tbar = $trang."~ ".$do."[".$luc."✔".$do."]".$trang." => ".$luc;
for($v=1;$v<=30;$v++)
{
  print $trang."= ";
} print "\n";
print $tbar."Tool: ".$vang.$NAME_TOOL."\n";
print $tbar."Version: ".$vang."0.0.1"."\n";
print $tbar."Team Creator: ".$vang."Tools Miner Coin"."\n";
print $tbar."Telegram Group: ".$vang."@Tools Miner Coin"."\n";
print $tbar."Telegram Support: ".$vang."@MinhTuan_TV"."\n";
print $tbar.$hong."SPONSOR: ".$vang."MR.CHUONG NGUYEN"."\n";
for($v=1;$v<=30;$v++)
{
  print $trang."= ";
} print "\n";
//print "hi";
if($AD == 1)
{
if(file_get_contents("https://google.com")){
$URL_GET = "https://pastebin.com/raw/YuQWkh5U";
    $INIT2 = curl_init($URL_GET);
    curl_setopt($INIT2,CURLOPT_RETURNTRANSFER, 1);
    if(curl_error($INIT2))
    {
      die("Wifi disconnect");
    } else {
$URL_GET = "https://pastebin.com/raw/PNYR8taDt";
    $INIT2 = curl_init($URL_GET);
    curl_setopt($INIT2,CURLOPT_RETURNTRANSFER, 1);
    if(curl_error($INIT2))
    {
      die("Wifi disconnect");
    }
    }
$URL_GETKEY = "https://pastebin.com/raw/hc2WiJJ4";

$INIT = curl_init($URL_GETKEY);
curl_setopt($INIT, CURLOPT_RETURNTRANSFER ,1);
if(curl_error($INIT))
{
  die("Wifi disconnect!");
} else {
  if(curl_exec($INIT) == "false")
  {
    $URL_GET = "https://pastebin.com/raw/YuQWkh5U";
    $INIT2 = curl_init($URL_GET);
    curl_setopt($INIT2,CURLOPT_RETURNTRANSFER, 1);
    if(curl_errno($INIT2))
    {
      die("Wifi disconnect");
    } else {
      $JSON = json_decode(curl_exec($INIT2),TRUE);
      print $tbar."Key date ".$hong.$JSON["date"].$luc." link get key ".$red.$JSON["link1s"].$luc."!\n";
      print $tbar."Input key: ".$hong;
      $key = trim(fgets(STDIN));
      if($key==$JSON["key"])
      {
      $INIT = curl_init($URL_GETKEY);
curl_setopt($INIT, CURLOPT_RETURNTRANSFER ,1);
if(curl_errno($INIT))
{
  die("Wifi disconnect!");
} else {
  if(curl_exec($INIT) == "false")
  {
    die($do."No cheat!");
  } else {
        print $tbar."Key is correct!\n";
  }
}
      } else {
        die($do."Key incorrect!");
      }
    }
  } else {
    print $tbar."Key is correct!\n";
  }
}} else {
  die("wifi disconnect");
}
}
system('xdg-open https://youtube.com/channel/UCTluuRSJFQCJCqQtq5Amj7A');
sleep(30);
}
