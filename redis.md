<!--- START OF PLANTUML 18 --> 

![](http://www.plantuml.com/plantuml/svg/YovAJSairajMq5N8JSpCqz2CLT3LjLE8pipBB0bEBIfBBNBEpqlBJ0TAS64JXAWko2yepKaiINNEpyrDp4i9IKpAIGLA0W00)

file:///C:/Users/francesco/Documents/projects/javascript/plantuml.html#%5Bredis.c%20-%20main()%5D%20--%3E%20%5BinitServerConfig()%5D%0A%5BinitServerConfig()%5D%20--%3E%20%5BpopulateCommandTable()%5D

---

<!--- END OF PLANTUML 18 --> 

```c
struct redisCommand {
    char *name;
    redisCommandProc *proc;
    int arity;
    char *sflags; /* Flags as string representation, one char per flag. */
    int flags;    /* The actual flags, obtained from the 'sflags' field. */
    /* Use a function to determine keys arguments in a command line. */
    redisGetKeysProc *getkeys_proc;
    /* What keys should be loaded in background when calling this command? */
    int firstkey; /* The first argument that's a key (0 = no keys) */
    int lastkey;  /* The last argument that's a key */
    int keystep;  /* The step between first and last key */
    long long microseconds, calls;
};

typedef int *redisGetKeysProc(struct redisCommand *cmd, robj **argv, int argc, int *numkeys, int flags);
```
