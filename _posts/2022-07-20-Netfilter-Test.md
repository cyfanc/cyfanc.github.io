---
title: Netfilter Test 
published: true
---

# netfilter demo

## nfqueue_test.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <linux/netfilter.h>
#include <libnetfilter_queue/libnetfilter_queue.h>

static int cb(struct nfq_q_handle *qh, struct nfgenmsg *nfmsg,struct nfq_data *nfa, void *data)
{
    int id = 0;

    struct nfqnl_msg_packet_hdr *ph;

    char *nf_packet;

    int ret;

    ph = nfq_get_msg_packet_hdr(nfa);

    if (ph) id = ntohl(ph->packet_id);

    int len = nfq_get_payload(nfa, (unsigned char**)&nf_packet);

    nfq_set_verdict(qh, id, NF_ACCEPT,(u_int32_t)len, nf_packet);

    printf("forward packet id :%d \n",id);
    
    return ret;
    
}

int main(int argc, char** argv)
{
    struct nfq_handle *h;
    struct nfq_q_handle *qh;
    struct nfnl_handle *nh;
    char buf[65535];
    int rv;
    int fd;

    h = nfq_open();
    if (!h)
    {
        perror("nfq_open");
        exit(-1);
    }

    if (nfq_unbind_pf(h, PF_INET) < 0)
    {
        perror("nfq_unbind_pf");
        exit(-1);
    }

    if (nfq_bind_pf(h, PF_INET) < 0)
    {
        perror("nfq_bind_pf");
        exit(-1);
    }

    qh = nfq_create_queue(h, 0, &cb, NULL);
    if (!qh)
    {
        perror("nfq_create_queue");
        exit(-1);
    }

    if (nfq_set_mode(qh, NFQNL_COPY_PACKET, 0xffff) < 0)
    {
        perror("nfq_set_mode");
        exit(-1);
    }

    if (nfq_set_queue_maxlen(qh,3000) < 0)
    {
        perror("nfq_set_queue_maxlen");
        exit(-1);
    }

    nh = nfq_nfnlh(h);
    fd = nfnl_fd(nh);

    for (;;)
    {
        if ((rv = recv(fd, buf, sizeof(buf), 0)) >= 0) 
        {
            nfq_handle_packet(h, buf, rv); /* send packet to callback */
            continue;
        }
    }

    return 0;
}

```

## Makefile
```
gcc nfqueue_test.c -lnfnetlink -lnetfilter_queue -o nfqueue_test
```

## set iptables
```
#set iptables forward rules
iptables -t mangle -A POSTROUTING -d baidu.com -p tcp --dport 80  -j NFQUEUE --queue-bypass --queue-num 0
```

## run
```
./nfqueue_test 
forward packet id :1 
forward packet id :2 
forward packet id :3 
forward packet id :4 
forward packet id :5 
forward packet id :6 
forward packet id :7 
forward packet id :8 
forward packet id :9 
forward packet id :10 
forward packet id :11
```

## curl test
```
curl baidu.com
```
