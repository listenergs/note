# RecyclerView

## 数据刷新

### 可用的刷新方法

1. notifyDataSetChanged()
2. notifyItemRangeRemoved()
3. notifyItemRangeInserted()
4. notifyItemRangeChanged()

### 使用

* 不推荐直接使用notifyDataSetChanged()
* 推荐写法

```java
class Adapter(){
    List datas = new ArrayList();
    public void update(List dataSource){
        if(dataSource == null){
            dataSource = new ArrayList();
        }
        int size = datas.size();
        datas.clear();
        notifyItemRangeRemoved(0, size);
        datas.addAll(dataSource);
        notifyItemRangeInserted(0,datas.size());
    }
}
```
