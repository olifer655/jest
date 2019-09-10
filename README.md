# 1. 前端自动化测试
目前开发大型应用，测试是一个非常重要的环节，但是大多数前端对测试相关的知识是比较匮乏的，因为可能项目开发周期短，根本没机会写。所以你没有办法体会到前端自动化测试的重要性！

来说说为什么前端自动化测试如此重要！

先来看看常见的前端的问题：

* 修改某个模块的时候，其他模块也受影响，很难快速定位bug
* 多人开发代码越来越难以维护
* 不方便迭代，代码无法重构
* 代码质量差

增加自动化测试后：

* 我们为核心功能编写测试后可以保障项目的可靠性
* 强迫开发者，编写更容易别测试的代码，提高代码质量
* 编写的测试有文档的作用，方便维护

## 1.1 测试

### 1.1.1 黑盒测试和白盒测试

* 黑盒测试一般也称为功能测试，黑盒测试要求测试人员将程序看作一个整体，不考虑起内部结构和性能， 只是按照期望验证程序是否正常工作。

* 白盒测试是基于代码本身的测试，一般指对代码逻辑结构的测试。

### 1.1.2 测试分类

> 单元测试（ Unit Testing ）

单元测试是指对程序中最小的测试单元进行的测试，例如测试 <strong>一个函数、一个模块、一个组件...</strong>

> 集成测试（ Integration Testing ）

将已测试过的单元测试函数进行组合集成暴露出的高层函数或类的封装，对这些函数或类进行的测试

> 端到端测试（ E2E Testing ）

打开应用程序模拟输入，检查功能以及界面是否正确

### 1.1.3 TDD & BDD

> TDD 是测试驱动开发 ( Test-Driven Development )

TDD 的原理是在开发功能代码之前，先编写单元测试用例代码

> BDD 是行为驱动开发 ( Behavior-Driven Development )

系统业务专家、开发者、测试人员一起合体，分析软件的需求，然后将这些需求写成一个个的故事。开发者负责填充这些故事的内容，保证程序实现效果与用户需求一致。

总结： TDD 是先写测试再开发（一般都是单元测试，白盒测试），而 BDD 则是按照用户的行为来开发，在根据用户的行为编写测试用例（一般是集成测试和黑盒测试）

### 1.1.4 测试框架

* Karma Karma为前端自动化测试提供了跨浏览器测试的能力，可以在浏览器中执行测试用例
* Mocha 前端自动化测试框架, 需要配合其他库一起使用， 像chai、sinon... 
* Jest 是facebook推出的一款测试框架，集成了 Mocha、chai、sinon、jsdom等功能。
* ....

## 1.2 Jest 的核心应用

在说Jest测试之前，先来看看我们以前是怎么样测试的：
```js
// name=lzr&age=20 => {name: 'lzr', age: '20'} 
const parser = str => {
	const obj = {}; 
	str.replace(/([^=&]+)=([^=&]+)/g, function() {
		obj[arguments[1]] = arguments[2];
	});
	return obj;
}

// {name: 'lzr', age: 20} => name=lzr&age=20 
const stringify = obj => {
  const arr = []

  Reflect.ownKeys(obj).forEach(key => {
    arr.push(`${key}=${obj[key]}`);
  })
  return arr.join("&");
}

console.log(parser('name=lzr&age=20'));
console.log(stringify({name: 'lzr', age: 20}));
```
但是 `console` 不能出现在正式环境代码中保留，所以我们可以引入测试代码

### 1.2.1 分组、用例

```js
// it 用例

it('测试parser是否能解析数据', () => {
  // 断言
  expect(parser("name=lzr&age=20")).toEqual({name:'lzr', age: "20"})
}) 

```
```js
// describe 是分组

describe('测试qs库是否合法', () => {
  it('测试parser是否能解析数据', () => {
    // 断言
    // toEqual 对象相等
    expect(parser("name=lzr&age=20")).toEqual({name:'lzr', age: "20"})
  }) 
})
```

### 1.2.2 matchers 匹配器

按照匹配器的分类不同，我们将常见api分为 <strong>相等、不相等、包含</strong> 三类。

```js

it('测试两个人是否全等', () => {
  expect(1+1).toBe(2); // js 中的 ===
  expect({ name: 1 }).toEqual({ name: 1 });
  expect(true).toBeTruthy();
  expect(false).toBeFalsy();
})


it('测试不相等', () => {
  expect(1+1).not.toBe(3); 
  expect(3).toBeLessThan(5);
  expect(10).toBeGreaterThan(5);
})

it('测试包含', () => {
  expect('hello world').toContain('hello'); 
  expect('hello world').toMatch(/hello/); 
})

```

