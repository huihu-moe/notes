# LRU缓存设计

```java
import java.util.*;

public class LRUCache{
    class Node{
        int key;
        int val;
        Node pre;
        Node next;
        Node(){

        }
        Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    private Map<Integer, Node> cache = new HashMap();
    private int size;
    private int capacity;
    private Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.head = new Node();
        this.tail = new Node();
        head.next = tail;
        tail.pre = head;
    }

    public int get(int key) {         
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            moveToHead(node);
            return node.val;
        }
        return -1;
    }

    public void moveToHead(Node node) {
        if (node.pre == head) {
            return;
        }
        node.pre.next = node.next;
        node.next.pre = node.pre;
        
        node.next = head.next;
        head.next.pre = node;
        
        node.pre = head;
        head.next = node;
    }

    public void set(int key, int value) {         
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.val = value;            
            moveToHead(node);
        }else {
            Node node = new Node(key, value);
            node.next = head.next;
            head.next.pre = node;
            head.next = node;
            node.pre = head;
            cache.put(key, node);
            size++;
            if (size > capacity) {
                Node dNode = tail.pre;
                dNode.pre.next = tail;
                tail.pre = dNode.pre;
                cache.remove(dNode.key);
                size--;
            }
        }
    }
}
```
