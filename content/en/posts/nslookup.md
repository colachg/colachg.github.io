---
title: "Nslookup not working inside container"
date: 2021-12-10T15:57:06+08:00
draft: false
---
# 

Today when I debug a CI pipeline error, I found something strange. Here was the first error:
![img.png](/images/nslookup_error.png)

So I sshed to the host and used `nslookup` to query the domain name but everything went well.
![img.png](/images/nslookup_2.png)

Then I ran a docker container to simulate the runner's original environment.  
It happened again.  
![img.png](/images/nslookup_3.png)

I was suspicious of that there was something wrong in the network between the nameserver and the container. So I used
`tcpdump` to catch the dns packets between them but found nothing wrong. I had no ideas and wanted to dig it more. I installed
`dig` to find more info. 

When I used dig to query the name, it worked well even the result of `nslookup`. But there was something different with
the output of the last output of `nslookup`.
```shell
/ # nslookup mirrors.aliyun.com
;; Truncated, retrying in TCP mode.
```
I thought that maybe after I had installed `bind-tools` (dig) with some any libs so that `nslookup` support TCP mode.
After I googled this I found that:  
> In DNS Protocol design, UDP transport Block size (payload size) has been limited to 512-Bytes to optimize performance whilst generating minimal network traffic.

Maybe I should add the `bind-tools` to my base image or inject it into the building runtime to avoid this situation. 

# Reference:  
- https://serverfault.com/questions/587625/why-dns-through-udp-has-a-512-bytes-limit