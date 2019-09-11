1.标题`<h1>`就是文字，通过color属性直接定义字体颜色。
2.边框的设置

    border:5px solid red;   //线宽、线型、颜色顺序
    border-width\border-style\border-color

3.空格：`&nbsp;`（注意必须加；）
4.段落首行缩进：
（1）使用空格`&nbsp;&nbsp;`（但是需要很多个空格可能才能达到效果）
（2）`<p style="text-indent: 2em">这是一个字</p>`
（3）加一个内联标签`<p><span style="padding-left: 2em;">这是一个字</span></p>`
（4）使用`<pre>`标签，保留文本原来的格式，保留标签内部的换行和空格
5.CSS3 `box-shadow` 属性，向元素添加边框阴影

    box-shadow:  X-offset Y-offset blur spread color inset;

（1） X-offset:是指阴影水平偏移量,其值可正可负，正值，则阴影在对象的右边，负值，阴影在对象的左边
（2）Y-offset:是指阴影的垂直偏移量，其值也可以是正负值，正值，阴影在对象的底部，负值时，阴影在对象的顶部
（3）阴影模糊半径：此参数是可选，只能为正值，如果其值为0时，表示阴影不具有模糊效果，值越大阴影的边缘就越模糊
（4）阴影扩展半径：此参数可选，其值可为正负值，正值，则整个阴影都延展扩大，反之，则缩小
（5）阴影颜色：此参数可选，不设定任何颜色时，浏览器会取默认色，但各浏览器默认色不一样，特别是在webkit内核下的safari和chrome浏览器将无色，也就是透明，建议不要省略此参数。
（6）阴影类型：此参数可选，默认的投影方式是外阴影；如果取其唯一值“inset”,就是将外阴影变成内阴影
注：多层阴影，最内层优先级最高，之后依次降低。使用逗号“，”隔开。
6.实现表单内容的居中，可以使用列表，只需设置css为

    ul li {
            list-style: none;
        }

让列表没有标识

```
 <form method="post" action="#" class="right-table">
            <ul>
                <li>
                    <label for="mail_address">请输入邮箱地址</label>
                    <input type="text" id="mail_address" value="这是一个文本输入框" name="mail_address" />
                    <span class="hint">邮箱地址请按要求格式输入</span>
                </li>
                <li>
                    <label for="password">请输入密码</label>
                    <input type="password" id="password" value="这是一个文本输入框" />
                    <br>
                </li>
                <li>
                    <label for="confirm-password">请重复输入密码</label>
                    <input type="password" id="confirm-password" value="这是一个文本输入框" name="password" />
                    <span class="hint">密码请为6-16位英文数字</span>
                </li>
                <li>
                    <h5>性别</h5>
                    <input type="radio" name="sex" value="男" id="male" checked="checked" />
                    <label for="male" class="uniqe">男</label>
                    <input type="radio" name="sex" value="女" id="female" />
                    <label for="female" class="uniqe">女</label>
                </li>
                <li>
                    <h5>城市</h5>
                    <select name="cities">
                        <option>北京</option>
                        <option>长沙</option>
                        <option>娄底</option>
                        <option>怀化</option>
                    </select>
                </li>
                <li>
                    <h5>爱好</h5>
                    <input type="checkbox" />运动
                    <input type="checkbox" />艺术
                    <input type="checkbox" />科学
                </li>
                <li>
                    <h5 style="vertical-align: top;">个人描述</h5>
                    <textarea cols="100" rows="8">这是一个多行输入框，输入您的个人描述</textarea>
                </li>
            </ul>
            <input type="button" name="submit" value="确认提交" class="confirm-btn" />
        </form>
```
效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911220024735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)