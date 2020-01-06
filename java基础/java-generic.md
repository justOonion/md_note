### PECS原则 extends，super

如果你是想遍历collection，并对每一项元素操作时，此时这个集合是生产者（生产元素），应该使用 **Collection<? extends Thing>**.
如果你是想添加元素到collection中去，那么此时集合是消费者（消费元素）应该使用**Collection<? super Thing>**.

