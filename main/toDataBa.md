<!-- 批量数据 -->

> ### 1. 代码批量生成

具体参考：[数据处理](main/pyData "title")

> ### 2. SQL 批量生成


``` sql
-- dataBatch.sql

DECLARE
    -- 设置变量
    V_CNT number(32) := 1000;
    V_COUNT number(6) := 0;
    V_S vachar(20) := '';

BEGIN

-- 1..1000  生成 1000 条数据
FOR i In 1..1000 LOOP

    -- 字符串类型
    -- Alan_1000 - Alan_2000
    V_S = concat('Alan_', cast(V_CNT as varchar))

    -- 修改主键，循环插入数据
    -- id：1000 - 2000
    INSERT INTO userInfo (id, name, age, address) VALUES (V_CNT, V_S, 18, 'ShenZhen');

    -- 数字类型
    V_CNT := V_CNT + 1;

    -- 每 100 条提交事务
    IF V_COUNT >= 100 THEN
        COMMIT;
        V_COUNT := 0;
    END IF;

    -- 结束循环
    END LOOP;
    -- 提交事务
    COMMIT;

END;
```