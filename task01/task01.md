1.`<head>`里必须包含`<meta>`和`<title>`标签

     <meta charset="UTF-8">
     <title>task01</title>
2.表格的使用

    <table>
         <tr>
                <th>表头</th>
                <th>表头</th>
                <th>表头</th>
         </tr>
         <tr>
                <td>表内容单元格</td>
                <td>表内容单元格</td>
                <td><a href="#">操作</a></td>
            </tr>
            <tr>
                <td>表内容单元格</td>
                <td>表内容单元格</td>
                <td><a href="#">操作</a></td>
            </tr>
            <tr>
                <td>总计</td>
                <td colspan="2">1000</td>
            </tr>
    </table>

colspan属性可以合并两列单元格
3.表单form中注意使用`<label>`分割
4.`<img>`必须加"alt"属性，防止图片加载不出来

    <img src="img/logo.png" alt="logo">

5.绝对路径：完整的网址，前加https://或者http://表示，缺点：不易移植，若项目域名发生变化，路径就会失效。
  相对路径：相对于当前文件的路径
  ./	文件所在目录（可以省略不写）   
 ../	文件所在的父级目录
 ../../	父级目录的父级目录
  /	根目录