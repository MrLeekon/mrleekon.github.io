# <Lisp> Ansi Common Lisp习题
## 第二章
1. 描述下列表达式求值之后的结果：
    1. (+ (- 5 1) (+ 3 7);;;14
    2. (list 1 (+ 2 3));;;(1 5)
    3. (if (listp 1) (+ 1 2) (+ 3 4));;;7
    4. (list (and (listp 3) t) (+ 1 2));;;(nil 3)
2. 给出三种表示(a b c)的 cons 表达式。

    ```Lisp
    (cons 'a '(b c))
    (cons (car '(a b)) '(b c))
    (cons 'a (cdr '(a b c)))
    ```

3. 使用 car 与 cdr 来定义一个函数，返回一个列表的第四个元素。

    ```Lisp
    (def four (x)
      (car (cdr (cdr (cdr x))))
    )
    ```

4. 定义一个函数，接受两个实参，返回两者当中较大的那个。

    ```Lisp
    (def max2 (x y)
      (if (> x y) x y)
    )
    ```

5. 这个函数做了什么？

    ```Lisp
    (defun enigma (x)
      (and (not (null x))
           (or (null (car x))
               (enigma (cdr x)))))
    ;;; #用于判断列表里是否有 NIL，有则输出 T ；没有则输出 Nil。
    ```
    ```Lisp
    (defun mystery (x y)
      (if (null y)
        nil
        (if (eql (car y) x)
        0
        (let ((z (mystery x (cdr y))))
          (and z (+ z 1))))))
    ;;;y为Nil或者不存在x均输出nil，否则输出x第一次出现在y的位置。
    ```

6. 下列表达式，x该是什么，才会得到相同的结果？

    ```Lisp
    (a) > (car (x (cdr '(a (b c) d))))
        B
        ;;;car
    (b) > (x 13 (/ 1 0))
        13
        ;;;if T
    (c) > (x #'list 1 nil)
        (1)
        apply
    ```


