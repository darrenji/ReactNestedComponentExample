> 在React中，当涉及组件嵌套，在父组件中使用`props.children`把所有子组件显示出来。如下：

	function ParentComponent(props){
		return (
			<div>
				{props.children}
			</div>
		)
	}

**如果想把父组件中的属性传给所有的子组件，该怎么做呢？**

--使用`React.Children`帮助方法就可以做到。

**比如，把几个Radio组合起来，合成一个RadioGroup,这就要求所有的Radio具有同样的name属性值。Radio设计成子组件，RadioGroup设计成父组件，name的属性值在RadioGroup这个父组件中设置。**

首先是子组件：

	//子组件
	function RadioOption(props) {
	  return (
	    <label>
	      <input type="radio" value={props.value} name={props.name} />
	      {props.label}
	    </label>
	  )
	}

然后是父组件，不仅需要把它所有的子组件显示出来，还需要为每个子组件赋上name属性和值：

	//父组件用,props是指父组件的props
	function renderChildren(props) {
	    
	  //遍历所有子组件
	  return React.Children.map(props.children, child => {
	    if (child.type === RadioOption)
	      return React.cloneElement(child, {
	        //把父组件的props.name赋值给每个子组件
	        name: props.name
	      })
	    else
	      return child
	  })
	}
	
	//父组件
	function RadioGroup(props) {
	  return (
	    <div>
	      {renderChildren(props)}
	    </div>
	  )
	}
	
	function App() {
	  return (
	    <RadioGroup name="hello">
	      <RadioOption label="选项一" value="1" />
	      <RadioOption label="选项二" value="2" />
	      <RadioOption label="选项三" value="3" />
	    </RadioGroup>
	  )
	}
	
	export default App;
以上，`React.Children.map`让我们对父组件的所有子组件又更灵活的控制。