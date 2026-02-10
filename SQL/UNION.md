### UNION ALL
```
- 서로 다른 테이블의 데이터를 **하나의 결과 집합으로 보고 싶을 때** 주로 사용한다.
- UNION은 중복을 제거하지만 UNION ALL은 중복을 포함한다.
- 중복 제거 과정이 없기 때문에 **UNION보다 성능이 좋다.**
- UNION ALL을 사용할 때는 각 SELECT 문의 **컬럼 개수, 순서, 데이터 타입이 동일해야 한다.**
- 정렬이 필요하다면 ORDER BY는 **전체 UNION ALL 결과의 마지막에 한 번만** 작성한다.
```

 **해설**:
- 온라인은 ONLINE_SALE에서 **SALES_DATE / PRODUCT_ID / USER_ID / SALES_AMOUNT**를 그대로 조회한다.
- 오프라인은 OFFLINE_SALE에 **USER_ID 컬럼이 없으므로**, 문제 조건대로 **USER_ID를 NULL로 고정**해 맞춰준다.
- 두 테이블은 구조를 동일하게 만든 뒤 중복 제거가 필요 없으니 UNION ALL로 합친다.
- 2022년 3월 데이터만 필요하므로 DATE_FORMAT(SALES_DATE, '%Y-%m') = '2022-03' 조건으로 필터링한다.
- 정렬 조건은 **판매일 오름차순 → 상품ID 오름차순 → 유저ID 오름차순**이므로 ORDER BY SALES_DATE, PRODUCT_ID, USER_ID를 사용한다.

**예시**:
```sql
SELECT DATE_FORMAT(ON_S.SALES_DATE, "%Y-%m-%d") SALES_DATE,
    ON_S.PRODUCT_ID,
    ON_S.USER_ID,
    ON_S.SALES_AMOUNT
FROM ONLINE_SALE ON_S
WHERE DATE_FORMAT(ON_S.SALES_DATE, "%Y-%m") LIKE '2022-03%'

UNION ALL

SELECT DATE_FORMAT(OFF_S.SALES_DATE, "%Y-%m-%d") SALES_DATE,
    OFF_S.PRODUCT_ID,
    NULL AS USER_ID,
    OFF_S.SALES_AMOUNT
FROM OFFLINE_SALE OFF_S
WHERE DATE_FORMAT(OFF_S.SALES_DATE, "%Y-%m") LIKE '2022-03%'

ORDER BY SALES_DATE, PRODUCT_ID, USER_ID
```