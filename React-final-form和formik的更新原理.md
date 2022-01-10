### 先说说formik的更新原理 ###
formik有两个字段组件，<Field>和<FastField>,<Field>value update->form context update -> 所有<Field>的全量更新。并没有节省性能。
<FastField>value update -> form context update -> shouldComponentUpdate来过滤更新实现按需更新，所以当需要性能优化的时候用这个组件就ok，这也是react的一个设计哲学，避免过早的优化。

### react-final-form的更新原理 ###
useField 创建了一个state，然后订阅了form的context，控制反转，把state和setState register到form中，当form内的值变化之后，会筛选后去执行需要更新的Field的callback，实现了按需更新。
其本质原理是发布订阅模式，Form负责发布，Field订阅相关内容。

总结：两者都做了按需更新，formik的文档更好看，api更加简洁，但是并没有默认的性能优化。而redux-final-form默认就是有性能优化。而且还能进一步定制，虽然我认为不太必要。
