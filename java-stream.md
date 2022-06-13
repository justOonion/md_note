```java
/** list 转map 并汇总*/
Map<String, BigDecimal> mapFromFacGNoToWaferAmount = decErpEListNS.stream()
                .filter(e->e.getWaferAmount()!=null)
                .collect(Collectors.toMap(DecErpEListN::getFacGNo, e->e.getWaferAmount(), BigDecimal::add));
```