# 构建任务驱动式学习型团队

发布于: 2022.01.09

2022年上半年，我再次进入3年的一个项目，发现一个“有意思”的现象：没有“专职”的前端开发，全是后端开发。然后看到近期提交的代码，异步逻辑代码惨不忍睹。

当时发现的问题有：

- 团队没有能力识别出“最佳实践”和演进式的调整代码组织结构（或进程内架构），代码维护性低。典型的有，将RxJS使用成“回调地狱”，一个angular component文件近6000行代码，难以阅读。
- 团队不熟悉项目核心技术栈Angular、Typescript 和 RxJS，以及前端基础知识匮乏。只能“抄”老代码，只知其然，而不知其所以然。在 code review 会议上，团队也没有能力指出前端代码质量问题。
- 团队没有能力给出较好的前端交互方案，一直受到客户的思维约束，无法影响客户调整方案。

为此，我做了一次3个月的实践活动，期望提升团队的前端技能，需要在团队内做到：

- 定模式，持续迭代进程内架构
- 补基础，弥补核心技术栈知识

那么，怎么让团队有效的“补基础”呢？

## 实践的理论基础

曾经罗永浩一期“逻辑思维”节目，将认知方式分成三个层次：模仿、连接、创新。分别对应的对象是：初学者、有经验者和专家。同时映射到其思维模式是：守、破、离。那么，“初学者”要成长为“有经验者”，需要做的事情就是：学习基础概念，然后“连接”（应用）这些知识。

2018年，我参加过李乐山导师的一次线下分享，很认同他培养研究生的学习方法：1）作为成年人，是有能力自己阅读书本上的知识，老师没必要复述一遍书上的文字；2）老师的作用是给学生提供优质的知识资源，帮助学生掌握疑难知识，答疑解惑。

还有，之前一说到做技术培训，便会想到去做基础知识的培训。用线下时间写PPT，组织学员学习，费时费力效果不见的好。以及，大家在项目中更多是靠个人的主动性去学习知识，还兼受项目压力，很难保证所有人都会主动。

因此，需要一位“导师”，在平时的工作中发现和收集问题，然后再有针对性的引导学员学习，

- “导师”提供学员优质的视频教程和文档资源，学员自己学基础知识和概念
- “导师”设置问题，使用任务驱动（或 问题驱动）的方式，引导学员“连接”知识，解决问题
- 学员分享学习成果，“导师”再答疑解惑，巩固学员理解

## 实践步骤

- 在code review和阅读代码提交记录时，我会识别出需要弥补的知识和概念，并记录下来；然后，设置具体问题，定义学习目标，链接相关参考资料，标记问题复杂程度和优先级
- 在code review会议结束后，让学员自主领取一些共性问题，也会有针对性的建议个别学员单独领取，督促学习；然后和每位学员对齐问题和学习范围
- 最后，要求学员分享解决问题的成果，同时，我会再进行补充和解释。

这里列举一些问题

1）当看到angular官网文档的“变更检测”机制时，会涉及到JS的事件循环、宏任务和微任务的基础知识。我便创建了“JS事件循环”的学习任务，提供 [javascript.info/event-loop](https://zh.javascript.info/event-loop) 网站作为学习资料，需要学员回答：这段代码的输出结果是什么？

```text
console.log(1);
setTimeout(() => console.log(2));
Promise.resolve().then(() => console.log(3));
Promise.resolve().then(() => setTimeout(() => console.log(4)));
Promise.resolve().then(() => console.log(5));
setTimeout(() => console.log(6));
console.log(7);

```

2）当学员还在写 if (obj === undefined) 的代码时，则创建了“JS空值转换”的学习任务。提供 [ecma规范 9.2章节](https://262.ecma-international.org/5.1/#sec-9.2) 作为学习资料，需要学员回答：“if ( value ) { console.log(value) }  ，当value是什么值时，可以执行console.log 方法？”。

3）发现学员无法区分Subject 和 BehaviorSubject应用场景时，便创建了“RxJS Subject使用”的学习任务。提供 Udemy 里的视频教程“Reactive Angular Course”和在线编辑器 [stackblitz.com](https://stackblitz.com/)，需要回答：RxJS的4个Subject的用法和区别；如何使用RxJS做轻量的状态管理。

4）发现学员不会使用ES6的解构语法时，便创建学习任务“介绍 JavaScript ES6+新特性”。提供 Babel 的 [Learn ES2015](https://babeljs.io/docs/en/learn) 文章和 [caniuse.com](https://caniuse.com/) 网站，需要回答：介绍ES6至最新的JS特性；选择个别新特性介绍浏览器的兼容性；简单介绍 JS 规范的发布流程。

## 实践后的思考

经过3个月的观察与实践，同时结合我整理出当前项目的开发模式和单元测试模式，能看的到团队的前端能力有一些改变。

当然有同事会问到这种形式能否扩大，覆盖到整个OTR的前端团队？

我也做了尝试，认为还是需要让“导师”和“学员”在一个团队内。这样，“导师”才能更有效的发现问题和设置目标，学习任务与当前工作有关联，具有实效性，“学员”也会有兴趣学习。

此外，在实践的过程中发现了一个“有意思”的事情。一位同学知道“回调地狱”，用“callback”的方式不好，也知道Promise的概念，但不知道能否将“callback”转成Promise。这就更需要一位“导师”及时发现，指导他怎么“连接”或“转化”知识。