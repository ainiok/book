### 内核
1. print 有返回值，echo 没有返回值

```
void zend_do_print(znode *result，const znode *arg TSRMLS_DC)
{
    zend_op *opline = get_next_op(CG(active_op_array) TSRMLS_CC);
 
    opline->result.op_type = IS_TMP_VAR;
    opline->result.u.var = get_temporary_variable(CG(active_op_array));
    opline->opcode = ZEND_PRINT;
    opline->op1 = *arg;
    SET_UNUSED(opline->op2);
    *result = opline->result;
}
```
```
void zend_do_echo(const znode *arg TSRMLS_DC)
{
    zend_op *opline = get_next_op(CG(active_op_array) TSRMLS_CC);
 
    opline->opcode = ZEND_ECHO;
    opline->op1 = *arg;
    SET_UNUSED(opline->op2);
}
```