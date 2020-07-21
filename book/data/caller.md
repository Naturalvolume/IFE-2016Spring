## caller 和 callee
caller 和 callee之间的关系就像是雇员employer 和 店员employee之间的关系，就是调用和被调用的关系，二者都返回函数对象的引用。
```javascript
  function parent(param1, param2, param3) {
		child(param1, param2, param3);
	}

	function child() {
    console.log(arguments); // { '0': 'mqin1', '1': 'mqin2', '2': 'mqin3' }
    // 调用的谁
    console.log(arguments.callee); // [Function: child]
    // 被谁调用
		console.log(child.caller); // [Function: parent]
	}

	parent('mqin1', 'mqin2', 'mqin3');
```