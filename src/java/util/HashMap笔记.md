>���ݽṹ�� ���������



>Ӱ�����ܵ��������ԣ� initial capacity �� load factor-->(Ӱ����Һ�rehash)



>�ص����ԣ�initial capacity �� load factor, 
>
>?			Entry[] table
>
>?			threshold=capacity *   load factor     --��һ��resize������sizeֵ��---��ֵ���ٽ�ֵ



>�ص㣺
>
>1�� ʵ������������2����.ԭ��%���Ż����ӿ���ڸ���hashֵ���������λ��
>
>`static int indexFor(int h, int length) {    return h & (length-1);}`
>
>2������Ϊԭ����2��С������



>�ص㷽����
>
>1�����췽��
>
>        int capacity = 1;
>        while (capacity < initialCapacity)
>            capacity <<= 1;-----------------�ҵ��պô��ڴ���capacity��2����
>    
>        this.loadFactor = loadFactor;-------------------------------��������
>        threshold = (int)Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);-----��ֵ
>        table = new Entry[capacity];`----------------------�����СΪ�����capacity
>2�����ݷ���
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
>        addEntry(hash, key, value, i);--------->put���ж��߼�
>        
>        return null;
>    }
>`
>
>`
>
>    void addEntry(int hash, K key, V value, int bucketIndex) {
>        if ((size >= threshold) && (null != table[bucketIndex])) {
>            resize(2 * table.length);-----------------����Ϊԭ����length������
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
>    			* ͷ�巨
>    			*/
>                e.next = newTable[i];
>                newTable[i] = e;
>                e = next;
>            }
>        }
>    }
>`
>
>3��hash����
>
>4��indexFor����--����key��hash�ҵ���Ӧ������index





>�ջ�
>
>1��HashMap��ʼ����ʱ��Ӧ�ÿ���loadfactor��inital capacity �Ĵ�С�����ⲻ��Ҫ�����ݡ�
>
>2��Դ�����������˺ܶ����λ����ļӿ����Ͳ��ҵ������ٶȡ�

