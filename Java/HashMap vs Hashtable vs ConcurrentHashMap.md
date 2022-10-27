### Contents

- [HashMap](#hashmap)
- [Hashtable](#hashtable)
- [ConcurrentHashMap](#concurrenthashmap)

# HashMap vs Hashtable vs ConcurrentHashMap

HashMap, Hashtable, ConcurrentHashMap 자료구조는 Map 자료구조를 상속하는 자료구조입니다. Map 자료구조는 Key-Value 형태의 자료구조로 하나의 Key에 하나의 Value 데이터가 매핑됩니다.

Map 자료구조를 구현하는 HashMap, Hashtable, ConcurrentHashMap 자료구조는 데이터를 저장하고 조회하는 공통 기능을 제공하지만, 동기화 처리 방식에 차이가 있습니다.

## HashMap

HashMap 자료구조는 동기화를 보장하지 않습니다. 그에 따라 데이터 조회 속도가 빠르다는 장점이 있습니다. 동기화를 보장하지 않으므로 싱글 스레드 환경에서 사용하는 것이 좋습니다.

### HashMap code in Java 11

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable Serializable {

    // ...

    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
}
```

## Hashtable

Hashtable 자료 구조는 동기화를 보장합니다. 따라서 멀티 스레드 환경에서 사용할 수 있습니다.

하지만 스레드 간에 동기화 락을 걸어 동기화를 보장하므로 속도가 느리다는 단점이 있습니다. 아래 코드에서 `put` 메서드를 보면 `synchronized` 키워드로 동기화를 하는 것을 확인할 수 있습니다.

### Hashtable code in Java 11

```java
public class Hashtable<K,V> extends Dictionary<K,V> implements Map<K,V>, Cloneable, java.io.Serializable {

    // ...

    public synchronized V put(K key, V value) {
        // Make sure the value is not null
        if (value == null) {
            throw new NullPointerException();
        }

        // Makes sure the key is not already in the hashtable.
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> entry = (Entry<K,V>)tab[index];
        for(; entry != null ; entry = entry.next) {
            if ((entry.hash == hash) && entry.key.equals(key)) {
                V old = entry.value;
                entry.value = value;
                return old;
            }
        }

        addEntry(hash, key, value, index);
        return null;
    }

}
```

## ConcurrentHashMap

ConcurrentHashMap 자료구조는 동기화를 보장합니다. 따라서 멀티 스레드 환경에서 사용할 수 있습니다.

HashMap의 동기화 문제를 보완하기 위해 등장한 자료구조입니다. Hashtable 자료구조와 마찬가지로 동기화를 보장하는데, 동기화를 하는 단위에 차이가 있습니다. Hashtable 자료구조는 스레드 간 동기화 락을 걸어 동기화를 보장하는 반면, ConcurrentHashMap 자료구조는 조작해야 하는 Entry만 락을 걸어 동기화를 보장합니다. 아래 코드를 보면 `f` 노드만 `synchronized` 키워드를 사용하여 동기화를 하는 것을 확인할 수 있습니다. 이에 따라 Hashtable 자료구조보다 데이터 접근 속도가 빨라지게 됩니다.

따라서 멀티 스레드 환경에서는 Hashtable 자료구조 대신 ConcurrentHashMap 자료구조를 사용하는 것이 좋습니다.

### ConcurrentHashMap code in Java 11

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {

    // ...

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    /** Implementation for put and putIfAbsent */
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh; K fk; V fv;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else if (onlyIfAbsent // check first node without acquiring lock
                     && fh == hash
                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))
                     && (fv = f.val) != null)
                return fv;
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key, value);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                        else if (f instanceof ReservationNode)
                            throw new IllegalStateException("Recursive update");
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
}
```
