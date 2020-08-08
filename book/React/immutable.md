# immutable数据结构
immutable 数据是利用结构共享形成的持久化数据结构，一旦有一部分被修改，将会返回一个全新的对象，并且原来相同的节点会直接共享。

immutable 对象数据内部采用多叉树结构，只要有节点被改变，那么它和它相关的所有上级节点都更新，且返回全新的引用。

所以immutable能最大效率更新数据结构，且和现有的 PureComponent/React.memo() 结合，提高react渲染性能。

immutable使用时，要注意和js数据的转换，并且在store中，使用的全部都是immutable数据，所以存取数据要用set和get
[immutable](https://juejin.im/book/6844733816460804104/section/6844733816548884487)