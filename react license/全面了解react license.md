### 背景
react是用于构建用户界面的 JAVASCRIPT 库， 2013年被facebook开源。
世界上许多著名公司都采用该框架开发产品, 包括microsoft，airbnb, 国内包括百度，阿里巴巴，腾讯，美团，携程，去哪儿，知乎等，使用非常广泛。
近期随着[react16](https://github.com/facebook/react/issues/10294#issuecomment-330087866)的即将完成，react社区又即将迎来了一波新的增长，但是随着react地广泛使用，各大公司越来越关注react license问题，虽然facebook多次在github和官方网站上做出解释，但不同的公司和组织有自己的考虑。本文整理搜集关于react license第一手资料，供大家阅读讨论。

> 1. [React offical site](http://facebook.github.io/react)
> 2. [React中文文档](https://doc.react-china.org/)
> 3. [React github repo](http://www.github.com/facebook/react)

#### 相关事件

**2017.4月** Apache软件基金会法律事务委员会[宣布](https://issues.apache.org/jira/browse/LEGAL-303)，所谓的“Facebook BSD +专利许可”不再被允许用作Apache项目的直接依赖。

**2017.9.14日** wordpress的母公司发博文宣布由于react license问题将移除react的使用

**2017.9.16日** 百度公司宣布将在用户端产品中禁止使用react和react native，已经使用相关技术的产品，半年内迁移到[vue](http://www.vuejs.org)或者自研的[san](https://github.com/ecomfe/san)

### 官方对react license的表述
1. [React License](https://github.com/facebook/react/blob/master/LICENSE)
2. [Explaining React's license](https://code.facebook.com/posts/112130496157735/explaining-react-s-license/)
3. [Open Source License FAQ](https://code.facebook.com/pages/850928938376556)
4. [Updating Our Open Source Patent Grant](https://code.facebook.com/posts/1639473982937255/updating-our-open-source-patent-grant/)
> Facebook提出BSD+ PATENT License的目的是防范不良专利起诉，相比于其他公司，facebook将其用于核心产品的技术开源，没有专利保护，每年将会面临大量不良专利诉讼。

**Open Source License FAQ 解答了关于大家专利授权方面的疑惑**
> 1. 如果我创建一个竞争产品，Facebook BSD +专利许可证中的附加专利授权是否终止？
> **不会**

> 2. 如果我对Facebook进行专利侵权以外的其他专利诉讼，Facebook BSD +专利授权中的附加专利授权是否终止？
> **不会**

> 3. Facebook的BSD +专利许可证中的附加专利授权如果首先针对专利侵权我的诉讼终止，那么我就会针对Facebook回应专利反诉？
> **不会，除非您的专利反诉与Facebook的BSD +专利许可证授权的Facebook软件有关。**

> 4. 在Facebook BSD +专利许可证中终止额外的专利授权会导致版权许可也终止？
> **不会**

### Github上相关issues

1. [Consider re-licensing to AL v2.0, as RocksDB has just done](https://github.com/facebook/react/issues/10191)
2. [Update React license FAQ/update license itself ](https://github.com/facebook/react/issues/10719)

###  国外社区相关讨论
1. [The React license for founders and CTOs](https://medium.com/@ji/the-react-license-for-founders-and-ctos-b38d2538f3e5)
2. [On React and WordPress](https://ma.tt/2017/09/on-react-and-wordpress/)
3. [
2003
Apache Foundation bans use of Facebook BSD+Patents licensed libraries like React.js](https://www.reddit.com/r/programming/comments/6nnxir/apache_foundation_bans_use_of_facebook_bsdpatents/)

### 国内相关讨论

1. [阿里还会使用react吗？](https://www.zhihu.com/question/65446071/answer/231113168)
2. [如何看待百度要求内部全面停止使用React / React Native?](https://www.zhihu.com/question/65437198/answer/231116042)
3. [法律角度你可以放心使用React吗？](https://zhuanlan.zhihu.com/p/27990414)
4. [React 路/粉/黑 都该了解的 React license 争议](https://juejin.im/entry/59a55d27f265da248808ae39)
5. [关于百度停用React](https://zhuanlan.zhihu.com/p/29420396?group_id=892880397443166208)


### 国内外知名React开发者谈React license

1. [Sebastian Markbåge](https://github.com/sebmarkbage)
> facebook 科学家 tc39成员 react团队负责人

![](./sebmarkbage.png)

2. [Dan Abramov](https://github.com/gaearon)
> [redux](http://www.reduxjs.org)和[create-react-app](https://github.com/facebookincubator/create-react-app)作者，react核心团队成员，react社区极度活跃者

**观点**

并不是react license的问题，而是人们对react license的不同解读，而采取的不同措施

![](./gaearon.png)

3. [流形](https://www.zhihu.com/people/arcthur)
> 现任阿里巴巴数据技术与产品部前端团队负责人，专注在React、数据可视化、Node等领域，《深入 React 技术栈》作者，知乎专栏《pure render》创办人

![](./reactbook.png)

4. [程墨](https://www.zhihu.com/people/morgancheng)
> 《深入浅出React和Redux》作者
![](./reactbook2.png)

#### React相关回退方案

1. [preact](https://github.com/developit/preact)
2. [react-lite](https://github.com/Lucifier129/react-lite)
3. [anu](https://github.com/RubyLouvre/anu)
