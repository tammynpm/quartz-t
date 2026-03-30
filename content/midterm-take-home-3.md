at t1, A has finished transmitting DATA(A) but B did not receive the AP's TS earlier, B may have transmitted RTS(B) during A's DATA transmission. Since A and B are hidden from each other, and the AP in the middle did not successfully handle both trnasmissions. As a result, A does not receive an ACK and B does not receive a CTS so both eventually time out. 

After those timeouts, both nodes retry using RTS/CTS again. If A wins the next backoff, it sends RTS(A), the AP replies with CTS(A), and now B hears that CTS and defers. A then successfully sends DATA(A) and receives ACK(A). After A finishes, B contends again, sends RTS(B), receives CTS(B), transmits DATA(B), and receives ACK(B).

![[IMG_0438.jpg]]