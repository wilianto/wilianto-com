--- 
title: "[TIL] Bash Get Unique Values of a CSV Column Using Bash Command"
date: 2018-08-10T09:52:00
categories:
- bash
- til
layout: post
meta_keywords: bash sorting, bash awk csv, bash unique value
meta_description: Learn how to use bash command awk and sort to print unique values of a CSV column
comments: true
---
Example input CSV file. The file is named **transaction.csv**
```
invoice_number,item_id,qty
#001,1,2
#001,2,2
#002,2,2
#003,1,2
#003,5,3
#004,2,2
#003,5,3
#005,2,2
```

The goal is to get the list of unique invoice_number
```
#001
#002
#003
#004
#005
```

**Step:**
1. Run awk command split by comma (-F), then print the first column. <br>
`awk -F ',' '{print $1}' transaction.csv`
2. Add sorting with unique params `sort -u`. Use `-r` to get reverse order
3. Final command is <br>
`awk -F ',' '{print $1}' transaction.csv | sort -u`

**Referece**
- More detail about `awk` command [http://tldp.org/LDP/abs/html/awk.html](http://tldp.org/LDP/abs/html/awk.html)
- More detail about `sort -u` vs `sort | uniq` performance discussion [https://unix.stackexchange.com/questions/76049/what-is-the-difference-between-sort-u-and-sort-uniq](https://unix.stackexchange.com/questions/76049/what-is-the-difference-between-sort-u-and-sort-uniq)