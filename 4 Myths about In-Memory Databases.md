http://www.slideshare.net/imcsummit/imcs2015-2-bus4-myth-about-inmemory-databases-busted?qid=234683ec-1e5a-41af-a7b4-3e5fe956ba16&v=default&b=&from_search=1

# What affects in-memory DB performance

1. Complexity of processing commands
 * in Redis most are O(1)
1. Query efficiency
 * Is it limited to blob queries?
 * Can you query a discrete value?
1. Pipelining supported?
 * to get lower latency and less context switches
1. Protocol efficiency
 * request parsing
 * response serialization
1. Connection pool vs. short-lived connections
1. Single-threaded vs. multi-threaded
 * **lock-free** vs. parallel computing
1. Amount of shared data
 * **shared-nothing** vs. shared-something vs. shared-everything
1. Built-in acceleration components

