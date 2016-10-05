# React Native 每日一学(Learn a little every day)
汇聚知识，分享精华。每天一个知识点（技巧，经验，填坑日记等
>如果你是一名React Native爱好者，或者有一颗热爱钻研新技术的心，喜欢分享技术干货、项目经验、以及你在React Naive学习研究或实践中的一些经验心得等等，欢迎投稿《React Native 每日一学》栏目。
如果你是一名Android、iOS、或前端开发人员，有者一颗积极进取的心，欢迎关注《React Native 每日一学》。本栏目汇聚React Native开发的技巧，知识点，经验等。

## 列表
1. [D1:React Native 钩子函数对源生库、第三方库的扩展 (2016-10-1)](#d1react-native-钩子函数对源生库第三方库的扩展-2016-10-1)

D1:React Native 钩子函数对源生库、第三方库的扩展 (2016-10-1)
------
经常用到源生的RN库、第三方库等等，有时候他们的库的功能无法满足我们的需求，某个函数的功能想调整一下,如:源生代码是向上移动，我想改为向下移动，这里我们可以使用钩子函数

## 代码演示
``` JavaScript
// Hook navigator method
function hookedDisableScene(sceneIndex) {
  const sceneConstructor = this.refs[`scene_${sceneIndex}`];

  // 修改的逻辑代码
  const nextRoute = this.state.routeStack[sceneIndex + 1];
  if (nextRoute && nextRoute.isModal) {
    sceneConstructor.setNativeProps({
      pointerEvents: 'none',
    });
  } else {
    sceneConstructor.setNativeProps(SCENE_DISABLED_NATIVE_PROPS);
  }
}

/* eslint-disable no-underscore-dangle */
/* eslint-disable no-param-reassign  */
export function hookNavigator(navigator) {
  if (!navigator._hookedForDialog) {
    navigator._hookedForDialog = true;
    navigator._disableScene = hookedDisableScene.bind(navigator);
  }
}
```

调动方法：
```JavaScript
componentWillMount() {
  hookNavigator(this.props.navigator);
}
```

## 代码解释
`navigator`在切换界面的时候，会隐藏下方的界面，这里我们使用钩子函数更改原有逻辑。
`hookNavigator`函数对`navigator`对象`_disableScene`的进行了覆盖扩展，通过`hookedDisableScene.bind(navigator)`的绑定函数，将`navigator`的`_disableScene`替换成了我们自己的`hookedDisableScene`函数,在里面进行了自己的逻辑编写。`_hookedForDialog`对象保证了钩子函数只调用一次。

>心得:可以对源生代码进行灵活的修改，不会破坏原有代码，不会因为升级版本而丢失代码。但是是建立在熟悉代码的基础上。
