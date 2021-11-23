
# A Tor config for specific country.

### For education purpose only.

การติดตั้ง Tor แบบเบื้องต้นบน Kali Linux และ config torrc ในการกำหนดประเทศ

*การที่เราเปิด Web หรือที่เห็นต่างๆ ทุกวันนี้ให้มองว่ามี  Clearnet , Deepnet , Darknet.
(Surface Web , Deep Web , Dark Web)  .

![enter image description here](https://edukasinewss.com/wp-content/uploads/2021/08/the-deep-web.jpg)
  cr. image edukasinewss.com
  
 อย่างที่ทราบกันว่า Tor นั้นจุดประสงค์หลักของมันเลยก็คือเพื่อความ  Anonimity / Privacy
และตัวของโครงการ Tor  (Torproject.org) เองเขาก็จะเน้นรณรงค์ในเรื่อง Human Right และสิทธิในความ Privacy ฯลฯ    
โดยตัวเครือข่ายของ Tor  นั้นทำงานโดยมีอาสาสมัครทั่วโลกช่วยรันเครือข่ายไม่ว่าจะเป็นการวาง  Relay หรือ Bridge server (แนวๆ P2P หรือ Decentralize) เพื่อเชื่อมต่อกันและส่งต่อๆกันแบบหลายๆชั้น หรือเรียกว่าเครือข่ายหัวหอม (The Onion Router) เพื่อให้ระบุตัวคนใช้นั้นทำได้ยากมากขึ้น
และจากที่การที่มีความ Anonimity / Privacy สูงจึงมีผู้เอาไปใช้ปกปิดตัวตนเพื่อไปใช้ในทางไม่ดีมีอยู่มาก
แต่ทั้งนี้เองตามก็สามารถนำไปใช้ในทางที่ดีได้เช่นกัน ยกตัวอย่างในด้านดี เช่น ระบบ Deepnet ต่างๆ , การศึกษา Network ในอีกรูปแบบ , ระบบ Whistleblower , 
ข่าวอิสระ Freedom Press , การสื่อสารด้วย Cryptography ในอีกรูปแบบ , 
หรือระบบที่ต้องการความ Privacy  และ อื่นๆ ฯลฯ
ซึ่งในบทความนี้ผมจะไม่พูดในส่วนนั้นแล้วกัน

ในบทความนี้ผมจะขอ share การติดตั้งค่า Tor config ในส่วนของการเปลี่ยนประเทศเพื่อเชื่อมต่อออกประเทศที่เราต้องการ หรือ StrictNode ให้ไป ExitNode ยังประเทศที่เราต้องการ
   
   เบื้องต้นก่อนเลยเราต้องติดตั้ง Tor Service (ยกตัวอย่างของผมติดตั้งบน Kali Linux )
   โดยใช้คำสั่งใน terminal

    sudo apt-get install tor -y
![enter image description here](https://raw.githubusercontent.com/deen666/markdown_store_files/main/tor-install.png)

หลังจากที่ติดตั้งเรียบร้อยให้ตรวจสอบ status ของ Tor service โดยใช้คำสั่ง

    sudo systemctl status tor
 
หากตัว service ไม่ได้ทำงานหรือยังไม่ได้รันอยู่ให้ start ด้วยคำสั่ง

    sudo systemctl start tor

หลังจากที่ตรวจสอบอีกครั้งว่า Tor service ทำงานแล้ว ตัว  service เองจะเปิด
  port  9050  (default) ในการเชื่อมต่อ
สามารถทดสอบโดยใช้  SOCKS5  ( locahost:9050 หรือ 127.0.0.1:9050) ในการเชื่อมต่อ
โดยใช้คำสั่ง

    curl -sSL --socks5 127.0.0.1:9050 https://check.torproject.org/api/ip

หากเชื่อมต่อสู่ Tor แล้วจะมี response IsTor เป็น true

    {"IsTor":true,"IP":"xx.xx.xx.xx"} 

และจะเห็นว่า IP ของเรานั้นเปลี่ยนไปไม่ใช่ public IP เดิมของเราและไม่ใช่ประเทศเดิมของเรา
ในตอนนี้ตัว IP เองจะสุ่มจาก Relay ที่อาสาสมัครได้วางเป็น Relay server ไว้ในทั่วโลกตามที่เคยกล่าวไว้ในตอนแรก
เราสามารถนำ IP ไปค้นหาได้ที่
https://metrics.torproject.org/rs.html#search

![enter image description here](https://raw.githubusercontent.com/deen666/markdown_store_files/main/tor-ip-change.png)

และเข้าเรื่องว่าเราจะเปลี่ยน IP จากการสุ่มนี้ให้เป็น IP หรือประเทศที่เราต้องการแบบเจาะจง
ให้ทำการแก้ไขไฟล์ torrc ซึ่งเป็นไฟล์ config สำหรับแก้ค่าต่างๆ ของ Tor service บนเครื่องเราเอง
(โดย default จะติดตั้งอยู่ใน path  /etc/tor/ ให้ใช้ TextEditor แก้ไขตัวไฟล์ torrc ที่อยู่ใน path นั้นได้เลย)
หรือเราสามารถใช้คำสั่ง  nano หรือ vi (vim)  ได้ เช่น

    sudo nano /etc/tor/torrc

เมื่อเปิดไฟล์แล้วให้เพิ่มเติมดังนี้

    ExitNodes {ตัวย่อประเทศที่เราต้องการ}    * ให้ดู  List of country codes for Tor ในรายการท้ายล่างบทความ
    StrictNodes 1 

และให้ตรวจสอบใน search relay https://metrics.torproject.org/rs.html#search/country:ตัวย่อประเทศ

เช่น https://metrics.torproject.org/rs.html#search/country:jp
ว่ามี Relay ประเทศนั้นๆ ได้ทำงานอยู่และมีปริมาณ Exit Address มากน้อยแค่ไหน 
(หากมีน้อยจะทำให้เชื่อมต่อช้า หรือไม่สำเร็จ)
    

โดยตัวอย่างผมจะทดสอบเปลี่ยนโดยเจาะจงว่าให้เป็นประเทศญี่ปุ่น  (Japan)

    ExitNodes {jp}
    StrictNodes 1

*หากต้องการใส่หลายๆ ประเทศให้ใส่  ,  ขั้นตามตัวอย่าง (มันจะสุ่มมา 1 ประเทศจากใน list ที่เรากำหนดไว้เท่านั้น)* 

    ExitNodes {th},{jp},{hk},{sg}
    StrictNodes 1
  
![enter image description here](https://raw.githubusercontent.com/deen666/markdown_store_files/main/torrc-specific.png)

หลังจากแก้และ Save ไฟล์ torrc แล้ว  ให้ทำการ restart Tor service อีกครั้งโดยใช้คำสั่ง

    systemctl restart tor

รอสักพัก และทดสอบตรวจสอบ IP อีกครั้งจะพบว่า IP ได้ถูกเปลี่ยนไปตามประเทศที่เราได้กำหนดไว้จริงๆ แล้ว

![enter image description here](https://raw.githubusercontent.com/deen666/markdown_store_files/main/tor-ip-change-specific.png)




   >  *หมายเหตุ* 
   >  คำสั่งอื่นๆ ให้ศึกษา Command line ของ Linux เพิ่มเติม
   >  และวิธีติดตั้งฉบับเต็มของ Tor และการตรวจสอบ Integrety ของตัวไฟล์เองที่ https://community.torproject.org/


   >   **Disclaimer** บทความ/ข้อความมีวัตถุประสงค์เพื่อใช้เป็นข้อมูลเพื่อการศึกษาเท่านั้น ความคิดเห็นที่ปรากฏอยู่ในบทความ/ข้อความนี้เป็นเพียงการนำเสนอในมุมมอง/ความคิดเห็นบางส่วนของผู้เขียน และมิได้ยืนยันหรือรับรองถึงความถูกต้อง หรือสมบูรณ์ของข้อมูลดังกล่าวแต่อย่างใด หากนำไปใช้ในทางไม่ถูกต้อง หรือเกิดความเสียหายทางตรง ความเสียหายทางอ้อม หรือความเสียหายอันสืบเนื่องอันเป็นผลมาจากการใช้ข้อมูลในบทความนี้ ผู้เขียนย่อมไม่ต้องรับผิดต่อความเสียหายใดๆ 

   


## List of country codes for Tor

| Country | Abbrev |
|--|--|
|ASCENSION ISLAND|  {ac} |
|AFGHANISTAN |{af}|
|ALAND |{ax}|
|ALBANIA |{al}|
|ALGERIA |{dz}|
|ANDORRA |{ad}|
|ANGOLA |{ao}|
|ANGUILLA |{ai}|
|ANTARCTICA |{aq}|
|ANTIGUA AND BARBUDA |{ag}|
|ARGENTINA REPUBLIC |{ar}|
|ARMENIA |{am}|
|ARUBA |{aw}|
|AUSTRALIA |{au}|
|AUSTRIA |{at}|
|AZERBAIJAN |{az}|
|BAHAMAS |{bs}|
|BAHRAIN |{bh}|
|BANGLADESH |{bd}|
|BARBADOS |{bb}|
|BELARUS |{by}|
|BELGIUM |{be}|
|BELIZE |{bz}|
|BENIN |{bj}|
|BERMUDA |{bm}|
|BHUTAN |{bt}|
|BOLIVIA |{bo}|
|BOSNIA AND HERZEGOVINA |{ba}|
|BOTSWANA |{bw}|
|BOUVET ISLAND |{bv}|
|BRAZIL |{br}|
|BRITISH INDIAN OCEAN TERR |{io}|
|BRITISH VIRGIN ISLANDS |{vg}|
|BRUNEI DARUSSALAM |{bn}|
|BULGARIA |{bg}|
|BURKINA FASO |{bf}|
|BURUNDI |{bi}|
|CAMBODIA |{kh}|
|CAMEROON |{cm}|
|CANADA |{ca}|
|CAPE VERDE |{cv}|
|CAYMAN ISLANDS |{ky}|
|CENTRAL AFRICAN REPUBLIC |{cf}|
|CHAD |{td}|
|CHILE |{cl}|
|PEOPLE’S REPUBLIC OF CHINA |{cn}|
|CHRISTMAS ISLANDS |{cx}|
|COCOS ISLANDS |{cc}|
|COLOMBIA |{co}|
|COMORAS |{km}|
|CONGO |{cg}|
|CONGO (DEMOCRATIC REPUBLIC) |{cd}|
|COOK ISLANDS |{ck}|
|COSTA RICA |{cr}|
|COTE D IVOIRE |{ci}|
|CROATIA |{hr}|
|CUBA |{cu}|
|CYPRUS |{cy}|
|CZECH REPUBLIC |{cz}|
|DENMARK |{dk}|
|DJIBOUTI |{dj}|
|DOMINICA |{dm}|
|DOMINICAN REPUBLIC |{do}|
|EAST TIMOR |{tp}|
|ECUADOR |{ec}|
|EGYPT |{eg}|
|EL SALVADOR |{sv}|
|EQUATORIAL GUINEA |{gq}|
|ESTONIA |{ee}|
|ETHIOPIA |{et}|
|FALKLAND ISLANDS |{fk}|
|FAROE ISLANDS |{fo}|
|FIJI |{fj}|
|FINLAND |{fi}|
|FRANCE |{fr}|
|FRANCE METROPOLITAN |{fx}|
|FRENCH GUIANA |{gf}|
|FRENCH POLYNESIA |{pf}|
|FRENCH SOUTHERN TERRITORIES |{tf}|
|GABON |{ga}|
|GAMBIA |{gm}|
|GEORGIA |{ge}|
|GERMANY |{de}|
|GHANA |{gh}|
|GIBRALTER |{gi}|
|GREECE |{gr}|
|GREENLAND |{gl}|
|GRENADA |{gd}|
|GUADELOUPE |{gp}|
|GUAM |{gu}|
|GUATEMALA |{gt}|
|GUINEA |{gn}|
|GUINEA-BISSAU |{gw}|
|GUYANA |{gy}|
|HAITI |{ht}|
|HEARD & MCDONALD ISLAND |{hm}|
|HONDURAS |{hn}|
|HONG KONG |{hk}|
|HUNGARY |{hu}|
|ICELAND |{is}|
|INDIA |{in}|
|INDONESIA |{id}|
|IRAN, ISLAMIC REPUBLIC OF |{ir}|
|IRAQ |{iq}|
|IRELAND |{ie}|
|ISLE OF MAN |{im}|
|ISRAEL |{il}|
|ITALY |{it}|
|JAMAICA |{jm}|
|JAPAN |{jp}|
|JORDAN |{jo}|
|KAZAKHSTAN |{kz}|
|KENYA |{ke}|
|KIRIBATI |{ki}|
|KOREA, DEM. PEOPLES REP OF |{kp}|
|KOREA, REPUBLIC OF |{kr}|
|KUWAIT |{kw}|
|KYRGYZSTAN |{kg}|
|LAO PEOPLE’S DEM. REPUBLIC |{la}|
|LATVIA |{lv}|
|LEBANON |{lb}|
|LESOTHO |{ls}|
|LIBERIA |{lr}|
|LIBYAN ARAB JAMAHIRIYA |{ly}|
|LIECHTENSTEIN |{li}|
|LITHUANIA |{lt}|
|LUXEMBOURG |{lu}|
|MACAO |{mo}|
|MACEDONIA |{mk}|
|MADAGASCAR |{mg}|
|MALAWI |{mw}|
|MALAYSIA |{my}|
|MALDIVES |{mv}|
|MALI |{ml}|
|MALTA |{mt}|
|MARSHALL ISLANDS |{mh}|
|MARTINIQUE |{mq}|
|MAURITANIA |{mr}|
|MAURITIUS |{mu}|
|MAYOTTE |{yt}|
|MEXICO |{mx}|
|MICRONESIA |{fm}|
|MOLDAVA REPUBLIC OF |{md}|
|MONACO |{mc}|
|MONGOLIA |{mn}|
|MONTENEGRO |{me}|
|MONTSERRAT |{ms}|
|MOROCCO |{ma}|
|MOZAMBIQUE |{mz}|
|MYANMAR |{mm}|
|NAMIBIA |{na}|
|NAURU |{nr}|
|NEPAL |{np}|
|NETHERLANDS ANTILLES |{an}|
|NETHERLANDS, THE |{nl}|
|NEW CALEDONIA |{nc}|
|NEW ZEALAND |{nz}|
|NICARAGUA |{ni}|
|NIGER |{ne}|
|NIGERIA |{ng}|
|NIUE |{nu}|
|NORFOLK ISLAND |{nf}|
|NORTHERN MARIANA ISLANDS |{mp}|
|NORWAY |{no}|
|OMAN |{om}|
|PAKISTAN |{pk}|
|PALAU |{pw}|
|PALESTINE |{ps}|
|PANAMA |{pa}|
|PAPUA NEW GUINEA |{pg}|
|PARAGUAY |{py}|
|PERU |{pe}|
|PHILIPPINES (REPUBLIC OF THE) |{ph}|
|PITCAIRN |{pn}|
|POLAND |{pl}|
|PORTUGAL |{pt}|
|PUERTO RICO |{pr}|
|QATAR |{qa}|
|REUNION |{re}|
|ROMANIA |{ro}|
|RUSSIAN FEDERATION |{ru}|
|RWANDA |{rw}|
|SAMOA |{ws}|
|SAN MARINO |{sm}|
|SAO TOME/PRINCIPE |{st}|
|SAUDI ARABIA |{sa}|
|SCOTLAND |{uk}|
|SENEGAL |{sn}|
|SERBIA |{rs}|
|SEYCHELLES |{sc}|
|SIERRA LEONE |{sl}|
|SINGAPORE |{sg}|
|SLOVAKIA |{sk}|
|SLOVENIA |{si}|
|SOLOMON ISLANDS |{sb}|
|SOMALIA |{so}|
|SOMOA,GILBERT,ELLICE ISLANDS |{as}|
|SOUTH AFRICA |{za}|
|SOUTH GEORGIA, SOUTH SANDWICH ISLANDS |{gs}|
|SOVIET UNION |{su}|
|SPAIN |{es}|
|SRI LANKA |{lk}|
|ST. HELENA |{sh}|
|ST. KITTS AND NEVIS |{kn}|
|ST. LUCIA |{lc}|
|ST. PIERRE AND MIQUELON |{pm}|
|ST. VINCENT & THE GRENADINES |{vc}|
|SUDAN |{sd}|
|SURINAME |{sr}|
|SVALBARD AND JAN MAYEN |{sj}|
|SWAZILAND |{sz}|
|SWEDEN |{se}|
|SWITZERLAND |{ch}|
|SYRIAN ARAB REPUBLIC |{sy}|
|TAIWAN |{tw}|
|TAJIKISTAN |{tj}|
|TANZANIA, UNITED REPUBLIC OF |{tz}|
|THAILAND |{th}|
|TOGO |{tg}|
|TOKELAU |{tk}|
|TONGA |{to}|
|TRINIDAD AND TOBAGO |{tt}|
|TUNISIA |{tn}|
|TURKEY |{tr}|
|TURKMENISTAN |{tm}|
|TURKS AND CALCOS ISLANDS |{tc}|
|TUVALU |{tv}|
|UGANDA |{ug}|
|UKRAINE |{ua}|
|UNITED ARAB EMIRATES |{ae}|
|UNITED KINGDOM (no new registrations) |{gb}|
|UNITED KINGDOM |{uk}|
|UNITED STATES |{us}|
|URUGUAY |{uy}|
|UZBEKISTAN |{uz}|
|VANUATU |{vu}|
|VATICAN CITY STATE |{va}|
|VENEZUELA |{ve}|
|VIET NAM |{vn}|
|VIRGIN ISLANDS (USA) |{vi}|
|WALLIS AND FUTUNA ISLANDS |{wf}|
|WESTERN SAHARA |{eh}|
|YEMEN |{ye}|
|ZAMBIA |{zm}|
|ZIMBABWE |{zw}|
