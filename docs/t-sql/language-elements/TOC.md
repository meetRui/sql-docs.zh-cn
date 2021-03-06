# [概述](language-elements-transact-sql.md)  
# [--（注释）](comment-transact-sql.md)  
# [斜杠星型（块注释）](slash-star-comment-transact-sql.md)  
# [CREATE DIAGNOSTICS SESSION](create-diagnostics-session-transact-sql.md)  
# [NULL 和 UNKNOWN](null-and-unknown-transact-sql.md)  
# [USE](use-transact-sql.md)  
# [反斜杠（行继续符）](sql-server-utilities-statements-backslash.md)  
# [GO](sql-server-utilities-statements-go.md)  

# [控制流](control-of-flow.md)  
## [BEGIN...END](begin-end-transact-sql.md)  
## [BREAK](break-transact-sql.md)  
## [CONTINUE](continue-transact-sql.md)  
## [ELSE (IF...ELSE)](else-if-else-transact-sql.md)  
## [END (BEGIN...END)](end-begin-end-transact-sql.md)  
## [GOTO](goto-transact-sql.md)  
## [IF...ELSE](if-else-transact-sql.md)  
## [RETURN](return-transact-sql.md)  
## [THROW](throw-transact-sql.md)  
## [TRY...CATCH](try-catch-transact-sql.md)  
## [WAITFOR](waitfor-transact-sql.md)  
## [WHILE](while-transact-sql.md)  

# [游标](cursors-transact-sql.md)  
## [CLOSE](close-transact-sql.md)  
## [DEALLOCATE](deallocate-transact-sql.md)  
## [DECLARE CURSOR](declare-cursor-transact-sql.md)  
## [FETCH](fetch-transact-sql.md)  
## [OPEN](open-transact-sql.md)  

# [表达式](expressions-transact-sql.md)  
## [CASE](case-transact-sql.md)  
## [COALESCE](coalesce-transact-sql.md)  
## [NULLIF](nullif-transact-sql.md)  

# [运算符](operators-transact-sql.md)  
## [一元 - 正](unary-operators-positive.md)  
## [一元 - 负](unary-operators-negative.md)  
## [集合 - EXCEPT 和 INTERSECT](set-operators-except-and-intersect-transact-sql.md)  
## [集合 - UNION](set-operators-union-transact-sql.md)  
## [算术](arithmetic-operators-transact-sql.md)  
### [+（加）](add-transact-sql.md)  
### [+=（加法赋值）](add-equals-transact-sql.md)  
### [-（减）](subtract-transact-sql.md)  
### [-=（减法赋值）](subtract-equals-transact-sql.md)  
### [*（乘）](multiply-transact-sql.md)  
### [*=（乘法赋值）](multiply-equals-transact-sql.md)  
### [/（除）](divide-transact-sql.md)  
### [/=（除法赋值）](divide-equals-transact-sql.md)  
### [% 取模](modulo-transact-sql.md)  
### [%= 取模赋值](modulo-equals-transact-sql.md)  
## [= （赋值）](assignment-operator-transact-sql.md)  
## [位](bitwise-operators-transact-sql.md)  
### [&（位与）](bitwise-and-transact-sql.md)  
### [&= 位与赋值](bitwise-and-equals-transact-sql.md)  
### [|（位或）](bitwise-or-transact-sql.md)  
### [|（位或赋值）](bitwise-or-equals-transact-sql.md)  
### [^（位异或）](bitwise-exclusive-or-transact-sql.md)  
### [^=（位异或赋值）](bitwise-exclusive-or-equals-transact-sql.md)  
### [~（位非）](bitwise-not-transact-sql.md)  
## [比较](comparison-operators-transact-sql.md)  
### [=（等于）](equals-transact-sql.md)  
### [>（大于）](greater-than-transact-sql.md)  
### [<（小于）](less-than-transact-sql.md)  
### [>=（大于或等于）](greater-than-or-equal-to-transact-sql.md)  
### [<=（小于或等于）](less-than-or-equal-to-transact-sql.md)  
### [<>（不等于）](not-equal-to-transact-sql-traditional.md)  
### [!<（不小于）](not-less-than-transact-sql.md)  
### [!=（不等于）](not-equal-to-transact-sql-exclamation.md)  
### [!>（不大于）](not-greater-than-transact-sql.md)  
## [复合](compound-operators-transact-sql.md)  
## [逻辑](logical-operators-transact-sql.md)  
### [ALL](all-transact-sql.md)  
### [AND](and-transact-sql.md)  
### [ANY](any-transact-sql.md)  
### [BETWEEN](between-transact-sql.md)  
### [EXISTS](exists-transact-sql.md)  
### [IN](in-transact-sql.md)  
### [LIKE](like-transact-sql.md)  
### [NOT](not-transact-sql.md)  
### [或](or-transact-sql.md)  
### [SOME | ANY](some-any-transact-sql.md)  
## [::（作用域解析）](scope-resolution-operator-transact-sql.md)  
## [字符串](string-operators-transact-sql.md)  
### [+（字符串串联）](string-concatenation-transact-sql.md)  
### [+=（字符串串联赋值）](string-concatenation-equal-transact-sql.md)  
### [%（通配符 - 需匹配的字符）](percent-character-wildcard-character-s-to-match-transact-sql.md)  
### [[ ]（通配符 - 需匹配的字符）](wildcard-character-s-to-match-transact-sql.md)  
### [[^]（通配符 - 无需匹配的字符）](wildcard-character-s-not-to-match-transact-sql.md)  
### [_（通配符 - 匹配一个字符）](wildcard-match-one-character-transact-sql.md)  
## [运算符优先级](operator-precedence-transact-sql.md)  

# [中的](transactions-transact-sql.md)  
## [中的](transactions-sql-data-warehouse.md)  
## [事务隔离级别](transaction-isolation-levels.md)  
## [BEGIN DISTRIBUTED TRANSACTION](begin-distributed-transaction-transact-sql.md)  
## [BEGIN TRANSACTION](begin-transaction-transact-sql.md)  
## [COMMIT TRANSACTION](commit-transaction-transact-sql.md)  
## [COMMIT WORK](commit-work-transact-sql.md)  
## [ROLLBACK TRANSACTION](rollback-transaction-transact-sql.md)  
## [ROLLBACK WORK](rollback-work-transact-sql.md)  
## [SAVE TRANSACTION](save-transaction-transact-sql.md)  

# [变量](variables-transact-sql.md)  
## [SET @local_variable](set-local-variable-transact-sql.md)  
## [SELECT @local_variable](select-local-variable-transact-sql.md)  
## [DECLARE @local_variable](declare-local-variable-transact-sql.md)  
# [EXECUTE](execute-transact-sql.md)  
# [PRINT](print-transact-sql.md)  
# [RAISERROR](raiserror-transact-sql.md)  
# [CHECKPOINT](checkpoint-transact-sql.md)  
# [KILL](kill-transact-sql.md)  
# [KILL QUERY NOTIFICATION SUBSCRIPTION](kill-query-notification-subscription-transact-sql.md)  
# [KILL STATS JOB](kill-stats-job-transact-sql.md)  
# [RECONFIGURE](reconfigure-transact-sql.md)  
# [SHUTDOWN](shutdown-transact-sql.md)  
# [保留关键字](reserved-keywords-transact-sql.md)  
# [语法约定](transact-sql-syntax-conventions-transact-sql.md)  
