### shell脚本编程基础

#### 十一. 构建基本脚本

1. shell脚本

   1. 可以用命令列表执行多个命令

   2. 写shell脚本执行多个命令
   3. shell脚本第一行 `#!{shell path}` 指定执行脚本的shell
   4. 执行shell脚本需要添加 `PATH` 或者将该文件改为可执行文件 `chmod u+x script.sh`

2. 使用变量

   1. 使用环境变量，用 `$USER` ，$加变量名调用
   2. 用户变量，定义时 `=` 两边不能有空格
   3. 用变量给变量赋值时，也需要用 $

3. **命令替换**

   1. **把命令的输出赋值给变量**

   2. 两种方式：

      - 反引号 ``
      - $()格式

   3. 举例

      ```shell
      test=`date`
      test=$(date)
      echo The result is $test
      ```

4. 重定向

   1. 输出重定向
      - `ls -l > output.txt`
      - `date >> output.txt` 追加不覆盖
   2. 输入重定向
      - `wc < input.txt` 统计文件中的字数，行数，字节数
      - 内联重定向
        - `command << marker` 命令行自行输入直到遇到 `marker` 结束

5. **管道**

   1. 将一个命令的输出作为另一个命令的输入

   2. ```shell
      ls -l | sort
      ```

6. 执行数学运算

   1. 可以用Bourne shell中的 `expr` 命令，运算符中间必须用空格，而且 * 要转义

   2. `bash` 中用 `$[expression]` 运算

   3. `bc` 计算器

      - -q：不打印冗余信息

      - scale=x：规定保留了x位小数

      - quit退出

      - 在脚本中使用 `bc`

        ```shell
        var1=$(echo "scale=4; 3.44 / 5" | bc)
        # 使用内联重定向，命令行输入操作数
        var1=1
        var2=2
        var3=$(bc << EOF
        scale=4
        ans = ($var1 * $var2)
        EOF
        )
        echo result is $var3
        ```

7. 退出脚本

   1. exit
   2. 返回值 `0-255` ，超出取模
   3. 用 `$?` 查看上一个命令的退出值