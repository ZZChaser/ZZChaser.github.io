初识 TypeScript
写在前面：本篇文档是 TS 系列的第一篇，做为抛砖引玉的作用，主要对 TS 有个大概的、感性的认识，不对细节做过多的介绍。后续有高级类型、最佳实践相关的文章。因为时间和认识的有限，有不对的地方，欢迎大家斧正

矛盾是普遍的、绝对的，存在于事物发展的一切过程中，又贯串于一切过程的始终。
举几个例子
1. 转换小写
    function transformLowerCase(str) {
        return str.toLocalLowerCase();
    }

2. 一半的机率翻转硬币
    function flipCoin() {
        return Math.random < 0.5;
    }

3. 打印数组各项
    const testArray = ["Hello", "World"];
    function logArray(testArray) {
        for (let i = 0; i < testArray.length; i++) {
            console.log(testArray[i]);
        }
    }
    logArray();

4. 求面积
    function getArea(rect) {
        return rect.width * rect.height;
    }
    const testRect = { width: 10, heigth: 15 };
    console.log(getArea(testRect));

以上的几个例子都是有问题的，可能咋看之下很不容易发现，点击这儿查看效果。举这些例子，是想说明这是我们在写 Javascript 代码时容易出现的问题，为什么呢？我们可以追根溯源一下。
矛盾初现
旧过程和旧矛盾消灭，新过程和新矛盾发生
早期的浏览器只能用来浏览，不具备与访问者互动的能力。 随着 Web 开发领域的蓬勃发展，便出现了实现网页与浏览器交互的愿景。在这样的矛盾下，当时的网景公司（Netscape）有两个选择： 一是采用现有语言嵌入网页，这样就能充分利用现有代码和程序员；另一个是发明一门全新的语言，这样就能使用完全适用的方式实现。 采用哪种方式，网景内部也争执不下、难以抉择。
1995年Sun公司将Oak语言改名为Java，正式向市场推出并大肆宣传："一次编写，到处运行"（Write Once, Run Anywhere）。 这让网景公司动了心，甚至考虑直接将Java嵌入网页，但是由于这样会使HTML网页过于复杂，才不得不放弃。
在1995年5月，网景公司便决定采用全新的语言来实现，鉴于当时 Java 的影响力，这门语言需要看上去和 Java 相似，但是更简单。这个任务交给了Brendan Eich来实现， 但是Brendan Eich对Java没有兴趣，为了“应付”公司的任务，只用了10天便把这门新语言设计了出来，这门语言也就是后面的Javascript。
于是就出现了用 JavaScript 实现网页与浏览器交互的新过程，代替了浏览器不能交互的旧过程。
矛盾发展
不同的阶段有着不同的主要矛盾
在新的过程中，JavaScript 作为开发软件的一部分，整个开发过程也围绕着软件质量的 6 个特性：
1. 功能性
2. 可靠性
3. 易用性
4. 效率性
5. 维护性
6. 可移植性
早期只是做一些简单的动画和交互效果，因此代码量也比较小，不需要多人协作，更是没有专门的前端工程师。在这样的条件下，更多的是实现其功能性。
随着 JavaScript 的应用边界不断被突破，软件的需求越来越丰富并复杂，代码量也成倍增加。现在大多 Web 的开发现状就是，由多人协同开发并长期迭代维护的大型项目。在这样的条件下就不能只仅仅考虑其功能了，更是要考虑其可靠性、效率型、维护性和可移植性。
矛盾解决
矛盾着的事物总是由一种状态转化为另一种状态，经过第二种状态而达到矛盾的解决
随着矛盾的出现，业界也出现了很多的解决方案。比如组件化的前端库、模块化的演进、跨平台方案、tree-shake的打包工具等方案都一定程度的解决了上面提到的问题。
但是有些问题是由于 JavaScript 自身的特性造成的，难以避免，就如如下特性：
1. 解释型脚本语言：不需要进行先编译再执行，而是直接在平台下一边解释一边执行。
2. 动态类型：对数据没有严格的类型限制，可任意赋值。
这样的特性容易造成什么问题呢，从下图中我们便可见一斑（来源于 Top 10 JavaScript errors from 1000+ projects ）


从图中我们可以注意到大部分的错误都和 TypeError（用来表示值的类型非预期类型时发生的错误）相关。这就是动态类型的不严格性造成的，又由于没有编译过程，所以很多错误只能在线上发现。也是由于动态类型，我们需要联系上下文才能确定此时的类型，这就大大提高了后续维护的成本。
语义化 + 注释 + 文档
不就是动态类型导致不知道类型的原因吗，那语义化的命名加上必要的注释，再加上详尽的文档，就应该清楚了吧。这样确实能解决一部份问题，但又不能完全解决。比如复杂的结构注释会写得繁琐，比如缺少了一个检测的过程，错了而不自知，如文章开始提到的例子。
TypeScript
TypeScript 又是怎样来解决这样的矛盾呢？在其官网上我们可以看到如下这样的描述
1. TypeScript is JavaScript with syntax for types
2. TypeScript code converts to JavaScript...
第一点说明 TypeScript 是带有类型系统的 JavaScript ，自带类型系统，就不用写繁琐无用的注释了。第二点说明 TypeScript 最后是编译成 JavaScript ，有检查有提示，在编译阶段就能检测出一些基础错误。这样就从语言的层面上更好的解决了该矛盾。
我们可以用 TypeScript 的方式对文章开头的例子稍加改造。
1. 转换小写
    function transformLowerCase(str: string) {
        return str.toLocalLowerCase();
    }

2. 一半的机率翻转硬币
    function flipCoin(): boolean {
        return Math.random < 0.5;
    }

3. 打印数组各项
    const testArray = ["Hello", "World"];
    function logArray(testArray: string[]): void {
        for (let i = 0; i < testArray.length; i++) {
            console.log(testArray[i]);
        }
    }
    logArray();

4. 求面积
    function getArea(rect: { width: number; height: number }): number {
        return rect.width * rect.height;
    }
    const testRect = { width: 10, heigth: 15 };
    console.log(getArea(testRect));

点击这儿查看效果
这样就能有效的减少开发过程中一些低级的错误。
类型系统
鉴于篇幅，该部分只对 TypeScript 的一些基础类型和共性做个简单的介绍，高级类型后面另起文档再做说明。
基础类型
1. 第一类：number、string、boolean、bigint、symbol、null、undefined、object，用来描述 JavaScript 内置的数据。
    let count: number = 10;
    let success: boolean = true;

    function getKeys(obj: object): string[] {
        return Object.keys(obj);
    }
    const keys = getKeys({ name: 'test', age: 10 });
    // ...

2. 第二类：TypeScript 扩展的基础类型有：any、unknown、never、void。
  - any：既可以赋值给其它类型（除了never），其它类型也可以赋值给any类型。正常情况应该避免使用 any，可在 JavaScript 向 TypeScript 迁移的过程中使用。
    let testAny: any = undefined;
    const testUnknown: unknown = testAny; // 检测不通过
    testAny = 6; // 检测通过
    testAny = 'hello world'; // 检测通过
    testAny.sayHi(); // 检测通过

  - unknown：不可以赋值给其它类型（除了any），但其它类型可以赋值给unknown类型。更安全的 top type，可以替代 any 类型。
    let count: unknown = 10;
    count.width = 7; // 检测不通过
    count.sayHi(); // 检测不通过
    if (typeof count === 'number') {
        count.toFixed(2); // 检测通过
    }

  - never：可以赋值给其它类型，但其它类型不能赋值给never。用作在不会发生的数据和不会返回的函数。
    function throwError(message: string): never {
        throw new Error(message);
    } 

  - void：undefined 的子类型。用作没有返回值的函数。
    function logMessage(message: string): void {
        console.log(message);
    }

类型特点
1. 类型抹去
一旦我们的 TypeScript 代码经过编译，就会删去类型信息，转化成纯 js 代码。
     function throwError(message: string): never {
         throw new Error(message);
     } 
     function logMessage(message: string): void {
         console.log(message);
     }

进过编译后就转换成如下代码
    "use strict";
    function throwError(message) {
        throw new Error(message);
    }
    function logMessage(message) {
        console.log(message);
    }

点击这儿可进行验证
2. 类型推断
有时候不用显示的指定类型，ts 会帮我们自动推断出当前的类型。这种情况出现在给初始化变量、函数返回值等情况。
    const data = [6, 7];
    // data 推断为 number[]
    function getSum(a: number, b: number) {
        return a + b;
    }
    // getSum 返回值推断为 number
    getSum(data[0], data[1]);

    window.addEventListener('scroll', (e) => {
        console.log({ e });
        // e 推断为 Event 对象
    });

点击这儿可进行验证
3. 结构性类型
又称鸭子类型 - If it walks like a duck and it quacks like a duck, then it must be a duck。根据这个特点，在 TypeScript 中一个类型可以赋值给它的子类型，就像我们都知道函数没有显示的返回值，其实是返回的 undefined 而 void 是 undefined 的子类型，所以可以用 void 更语义话的表示没有显示返回值的函数。
    interface Point {
        x: number;
        y: number;
    }
    
    function logPoint(p: Point) {
        console.log(`${p.x}, ${p.y}`);
    }
    
    const point = { x: 12, y: 26, z: 50 };
    logPoint(point); // 检测通过

根据这个特点，如下图，我们可以把 TypeScript 中的类型简单的分个层次，便于理解


小结
现在来总结下本篇文章的内容
1. JavaScript 作为一门10天就被设计出的语言，解决了当时的矛盾，满足当时情况下的需求。但是随着不断的发展，其不足也就显现出来了。
2. TypeScript 是 JavaScript + type，解决了在多人协同开发并长期迭代维护的大型项目中由 JavaScript 的特性所造成的矛盾。当然不在这样的前提下，是否使用 TypeScript 替代 JavaScript 还是值得商榷的。
3. TypeScript 不仅涵盖了 JavaScript 中内置的数据类型，而且还扩展了很多实用的类型，其有类型抹去、类型推断、结构性类型的特点。
参考
- https://en.wikipedia.org/wiki/JavaScript
- https://www.ruanyifeng.com/blog/2011/06/birth_of_javascript.html
- https://www.typescriptlang.org/