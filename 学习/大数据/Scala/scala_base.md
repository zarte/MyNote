##scala中的yield
```
Scala中的yield
作者：2gua
链接：http://zhuanlan.zhihu.com/guagua/19752481
Scala中的yield不像Ruby里的yield，Ruby里的yield象是个占位符。Scala中的yield的主要作用是记住每次迭代中的有关值，并逐一存入到一个数组中。用法如下：for {子句} yield {变量或表达式}
具体举例如下，该例子获取文本文件中包含指定关键字的相关行，并统计各相关行字数，先把文本文件内容贴出来：I like Scala.
Hah Hah, Well!
Nice Scala-lang! Right? Are u ready?
Let's go...太棒咯~
非常好！Scala
Scala特棒！
我们一起来玩函数式编程吧！
开心就好哈！
下面是程序代码：/*
* Scala yield用法
* 作者：2gua
* 2014/05/12
*/
object YieldDemo {
    private val files = (new java.io.File(".")).listFiles
    private def fileLines(file: java.io.File) =
        scala.io.Source.fromFile(file).getLines.toList
    def main(args: Array[String]): Unit = {
        val lineLengths =
        for {
            file <- files
            if file.getName.endsWith(".txt")
            line <- fileLines(file)
            trimmedLine = line.trim
            if trimmedLine.matches(".*棒.*")
        } yield line + "：合计" + trimmedLine.length + "个字。"
        lineLengths.foreach(println)
    }
}
先来看看输出：Let's go...太棒咯~：合计15个字。
Scala特棒！：合计8个字。
[Finished in 3.2s]
注意点：yield最终会将每次迭代的line + "：合计" + trimmedLine.length + "个字。"结果存放到一个数组中，在这里是一条表达式，如果你用trimmedLine.length替代这条语句，则将每次迭代的trimmedLine.length值存放到数组中。如果将yield改为：} yield {
            println(line)
            trimmedLine.length
        }
则在每次迭代中会打印各相关行内容。各相关行字数会存入到数组中，并通过程序最后一条代码lineLengths.foreach(println)打印出来。要记住，要将结果存放到数组的变量或表达式必须放在yield{}里最后位置。结果如下：Let's go...太棒咯~
Scala特棒！
15
8
[Finished in 2.6s]


```