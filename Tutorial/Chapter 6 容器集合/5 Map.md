# Map

Map使用键值对来保存对象，这意味着在保存对象时，需要同时提供两个对象，一个作为键（key），一个作为值（value）。

创建一个Map类型的对象，可以使用任何一个实现了Map接口的类。下面的代码创建了一个HashMap对象：

```
Map<String, Integer> m = new HashMap<String, Integer>();
```

要向Map中添加数据可以使用put方法，它接受两个参数，即键和值。

```
m.put("key",10);
```

```
import java.util.Map;
import java.util.HashMap;

public class MapTest {
	public static void main(String[] args) {
		Map<String, Integer> m = new HashMap<String, Integer>();
		m.put("First", 1);
		m.put("Second", 2);
		m.put("Third", 3);
		System.out.println(m.get("Second"));
		m.put("Second", 8);
		System.out.println(m.get("Second"));
	}
}
```

## HashMap

## HashTable

## TreeMap
