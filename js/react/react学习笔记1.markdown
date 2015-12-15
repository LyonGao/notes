# react学习笔记

标签： react

---

[toc]

## 创建组件 React.Component instead of React.createClass

创建组件的时候想把
``` javascript
import React from 'react';

let CommentForm = React.createClass({
    getInitialState() {
        return {author: '', text: ''};
    },

    authorChange(e) {
        this.setState({author: e.target.value});
    },

    textChange(e) {
        this.setState({text: e.target.value});
    },

    submitForm(e) {
        e.preventDefault();
        this.props.onCommentSubmit({author:this.state.author, text:this.state.text});
    },

    render(){
        return (
            <form className="commentForm" onSubmit={this.submitForm}>
                <input
                    type="text"
                    placeholder="Your name"
                    value={this.state.author}
                    onChange={this.authorChange}
                    />
                <input
                    type="text"
                    placeholder="Say something..."
                    value={this.state.text}
                    onChange={this.textChange}
                    />
                <input type="submit" value="submit" />
            </form>
        )
    }
});

export default CommentForm;
```

换成下面这种形式
```javascript
import React from 'react';

export default class CommentForm extends React.Component{
    constructor(props) {
        super(props);
        this.state = {author: 'adsf', text: 'asdfadsfa'};
    }

    authorChange(e) {
        this.setState({author: e.target.value});
        console.log(this.state.author);
    }

    textChange(e) {
        this.setState({text: e.target.value});
        console.log(this.state.text);
    }

    submitForm(e) {
        e.preventDefault();
        this.props.onCommentSubmit({author:this.state.author, text:this.state.text});
    }

    render(){
        return (
            <form className="commentForm" onSubmit={this.submitForm.bind(this)}>
                <input
                    type="text"
                    placeholder="Your name"
                    value={this.state.author}
                    onChange={this.authorChange.bind(this)}
                    />
                <input
                    type="text"
                    placeholder="Say something..."
                    value={this.state.text}
                    onChange={this.textChange.bind(this)}
                    />
                <input type="submit" value="submit" />
            </form>
        )
    }
}

```

有一点要特别注意，绑定事件是必须绑定this。[bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
```javascript
onChange={this.authorChange.bind(this)
```

##多属性传递

当需要向一个组件传递多个props时，可以使用js的新特性扩展运算符来传递多个props

```javascript
let props = {
    onSubmit: this.onFormSubmit.bind(this),
    text: 'submit'
};

<Component {...props}/>
```

##属性验证(props validation)
一个component可以对接受的props进行验证，验证其的类型和是否为空，如果验证不通过则会在控制台上输出警告。出于性能考虑，这个功能只能在开发环境中使用，线上环境不用。[更多属性验证](http://facebook.github.io/react/docs/reusable-components.html)

```javascript
import React from 'react';

export default class CommentForm extends React.Component{
    constructor(props) {
        super(props);
        this.state = {author: '', text: ''};
    }

    // prop validation
    // 属性验证 onCommentSubmit 属性必须是函数类型，并且为必须
    static propTypes = {
        onCommentSubmit: React.PropTypes.func.isRequired
    }
}
```

##默认属性(default props)
```javascript
import React from 'react';

export default class CommentForm extends React.Component{
    constructor(props) {
        super(props);
        this.state = {author: '', text: ''};
    }

    // prop validation
    // 属性验证 onCommentSubmit 属性必须是函数类型，并且为必须
    static propTypes = {
        onCommentSubmit: React.PropTypes.func.isRequired
    }
    
    static defaultProps = {
        heihei: "haha"
    }
}
```





