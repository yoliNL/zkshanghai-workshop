# 第6课 课后作业

## 第1题 宽度为3的斐波那契AIR

| step | a | b |c|
|:-:|:-:|:-:|:-:|
| i=1 | 1 | 1 | 2 |
| i=2 | 1 | 2 | 3 |
| i=3 | 2 | 3 | 5 |

f_1(a,b,c,a_next,b_next,c_next) = c - a - b
f_2(a,b,c,a_next,b_next,c_next) = b - a_next
f_1(a,b,c,a_next,b_next,c_next) = a - c_next

## 第2题 宽度为3的斐波那契AIR

一种方式是单独搞一列用作集合检查的selector，且仅在i=1时selector=1，如果我们把集合检查的关系式写作f_check,那么f=selector*f_check

另一种方式是把selector写作(x-2)*(x-3)*...*(x-step_max), 仅当x=1时selector=1，然后同样的，把selector与f_check相乘。
