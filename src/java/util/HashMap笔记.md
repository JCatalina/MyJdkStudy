>数据结构： 数组加链表



>影响性能的两个属性： initial capacity 和 load factor-->(影响查找和rehash)



>重点属性：initial capacity 和 load factor, 
>
>?			Entry[] table
>
>?			threshold=capacity *   load factor     --下一个resize操作的size值。---阈值、临界值



>特点：
>
>1、 实际容量必须是2的幂.原因：%的优化，加快对于根据hash值查找链表的位置
>
>`static int indexFor(int h, int length) {    return h & (length-1);}`
>
>2、扩容为原来的2大小的两倍



>重点方法：
>
>1、构造方法
>
>        int capacity = 1;
>        while (capacity < initialCapacity)
>            capacity <<= 1;-----------------找到刚好大于传入capacity的2的幂
>    
>        this.loadFactor = loadFactor;-------------------------------加载因子
>        threshold = (int)Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);-----阈值
>        table = new Entry[capacity];`----------------------数组大小为计算的capacity
>2、扩容方法
>
>`
>
>    public V put(K key, V value) {
>        if (key == null)
>            return putForNullKey(value);
>        int hash = hash(key);
>        int i = indexFor(hash, table.length);
>        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
>            Object k;
>            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
>                V oldValue = e.value;
>                e.value = value;
>                e.recordAccess(this);
>                return oldValue;
>            }
>        }
>        modCount++;
>        
>        addEntry(hash, key, value, i);--------->put的判断逻辑
>        
>        return null;
>    }
>`
>
>`
>
>    void addEntry(int hash, K key, V value, int bucketIndex) {
>        if ((size >= threshold) && (null != table[bucketIndex])) {
>            resize(2 * table.length);-----------------扩容为原来的length的两倍
>            hash = (null != key) ? hash(key) : 0;
>            bucketIndex = indexFor(hash, table.length);
>        }
>        createEntry(hash, key, value, bucketIndex);
>    }
>`
>
>`
>
>    void resize(int newCapacity) {
>        Entry[] oldTable = table;
>        int oldCapacity = oldTable.length;
>        if (oldCapacity == MAXIMUM_CAPACITY) {
>            threshold = Integer.MAX_VALUE;
>            return;
>        }
>        Entry[] newTable = new Entry[newCapacity];
>        boolean oldAltHashing = useAltHashing;
>        useAltHashing |= sun.misc.VM.isBooted() &&
>                (newCapacity >= Holder.ALTERNATIVE_HASHING_THRESHOLD);
>        boolean rehash = oldAltHashing ^ useAltHashing;
>        transfer(newTable, rehash);
>        table = newTable;
>        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
>    }
>    
>    /**
>     * Transfers all entries from current table to newTable.
>     */
>    void transfer(Entry[] newTable, boolean rehash) {
>        int newCapacity = newTable.length;
>        for (Entry<K,V> e : table) {
>            while(null != e) {
>                Entry<K,V> next = e.next;
>                if (rehash) {
>                    e.hash = null == e.key ? 0 : hash(e.key);
>                }
>                int i = indexFor(e.hash, newCapacity);
>    			/*
>    			* 头插法
>    			*/
>                e.next = newTable[i];
>                newTable[i] = e;
>                e = next;
>            }
>        }
>    }
>`
>
>3、hash方法
>
>4、indexFor方法--根据key的hash找到对应的数组index





>收获：
>
>1、HashMap初始化的时候应该考虑loadfactor和inital capacity 的大小，避免不必要的扩容。
>
>2、源码里面运用了很多关于位运算的加快计算和查找的搜索速度。

