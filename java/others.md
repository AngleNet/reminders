* Mysql插入datetime类型的数据

    Timestamp 可以直接插入Mysql中。在Mysql中， `date`对应的是"YYYY-MM-DD"类型，而`timestamp`对应的才是"YYYY-MM-DD HH:MM:SS"类型
    ```Java
    Timestamp ts = Timestamp.valueOf("2018-07-01 23:30:20");
    ```
* `Pair`代码
```Java
public class Pair<FIRST, SECOND> implements Comparable<Pair<FIRST, SECOND>> {

    public final FIRST first;
    public final SECOND second;

    private Pair(FIRST first, SECOND second) {
        this.first = first;
        this.second = second;
    }

    // Why declare like this?
    public static <FIRST, SECOND> Pair<FIRST, SECOND> of(FIRST first,
            SECOND second) {
        return new Pair<FIRST, SECOND>(first, second);
}
Pair<Integer, Integer> pair = Pair.of(2, 3);
```

* 