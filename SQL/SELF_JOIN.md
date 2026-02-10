### self JOIN
*JOIN 자체가 테이블 연결 시키는 용도*
```
Self Join은 동일한 테이블 내의 행(Row)들 사이에 관계가 있을 때 사용합니다. 주로 **계층 구조**나 **데이터 간의 비교**가 필요할 때 사용하며, 문법적으로는 일반 JOIN과 같으나 조인 대상이 자기 자신이라는 점이 다르다
```

> **핵심 원칙:** 반드시 테이블 별칭(`AS`)을 다르게 주어, DB 엔진이 '어느 쪽' 테이블의 컬럼을 지칭하는지 구분하게 해야 합니다.

**주의사항 (병목 방지)**
- ON 조건의 중요성: Self Join은 자칫 잘못하면 데이터가 기하급수적으로 불어나는 카테시안 곱(Cartesian Product)이 발생하기 쉽습니다. 반드시 `P.ITEM_ID = T.PARENT_ITEM_ID`처럼 명확한 연결 고리를 적어주어야 합니다.
- 인덱스: 자기 자신을 두 번 읽기 때문에, 조인 키에 인덱스가 없다면 부하가 정확히 **두 배**로 늘어납니다.

**예시:**
```sql
SELECT c.ITEM_ID,
    c.ITEM_NAME,
    c.RARITY
FROM ITEM_INFO c
    JOIN ITEM_TREE T ON c.ITEM_ID = T.ITEM_ID
    JOIN ITEM_INFO I ON t.PARENT_ITEM_ID = I.ITEM_ID
WHERE I.RARITY = 'RARE'
ORDER BY T.ITEM_ID DESC;
```
- `ITEM_INFO c` 를 `ITEM_TREE`를 통해 `ITEM_INFO I`와 묶어서
- 이 중에 I.RARITY = 'RARE' 인 결과값만 조회

WHERE 절 서브쿼리로 사용
```sql
SELECT c.ITEM_ID,
    c.ITEM_NAME,
    c.RARITY
FROM ITEM_INFO c JOIN ITEM_TREE T
    ON T.ITEM_ID = c.ITEM_ID

WHERE T.PARENT_ITEM_ID IN (
    SELECT ITEM_ID
    FROM ITEM_INFO
    WHERE RARITY = 'RARE'
)

ORDER BY T.ITEM_ID DESC;
```
- `ITEM_TREE`테이블과 JOIN
- 그 중에 `PARENT_ITEM_ID`이 `RARITY='RARE'`인 `ITEM_ID`와 같은 것만 추림