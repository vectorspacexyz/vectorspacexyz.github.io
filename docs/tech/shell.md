---
title: Shell Scripting
date: 2024-02-21
---
## tee -a
Don't forget the -a flag to append entries to the log file rather than rewrite
it each time.

```bash
echo "Invoice ID      : $INVOICE_ID
Party           : $PARTY_NAME, $CITY_NAME
Item            : "ID: $ITEM_ID, DESC: $ITEM_DESC, QUA: $ITEM_QUA $ITEM_QUA_UNIT"
Net Amount      : $NET_AMOUNT
Tax Amount      : $TAX_AMOUNT
Gross Amount    : $GROSS_AMOUNT
Phone Number    : $PHONE_NUMBER
Employee Resp   : $EMPLOYEE_RESP
Request         : ${1%/*}/.${1##*/}
Request Payload : $1
"  | tee -a /home/vector/byd_sms_server/requests.log
```

## cut
cut -d*delimiter* -f*index* is pretty useful in shell scripting.

```bash
for i in *;
do
    VAL=$(du -sh "${i}" | cut -d'   ' -f1)
    if [[ $VAL ==  0 ]]
    then
        rm "$i"
        echo "$i removed"
    fi
done
```

To paste the delimiter in the terminal I used Ctrl-v. Ctrl-v + Tab to insert
tab character. Ctrl-v is used in vim in terminal to insert characters by their
unicode.
