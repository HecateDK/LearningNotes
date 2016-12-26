* [Foreword(by Jake Archibald)](#foreword)
* [Preface](#preface)
* [Chapter 1: Asynchrony: Now & Later](#chapter-1-asynchrony-now--later)
* [Chapter 2: Callbacks](#chapter-2-callbacks)
* [Chapter 3: Promises](#chapter-3-promises)
* [Chapter 4: Generators](#chapter-4-generators)
* [Chapter 5: Program Performance](#chapter-5-program-performance)
* [Chapter 6: Benchmarking & Tuning](#chapter-6-benchmarking--tuning)
* [Appendix A: asynquence Library](#appendix-a-asynquence-library)
* [Appendix B: Advanced Async Patterns](#appendix-b-advanced-async-patterns)
* [Appendix C: Acknowledgments](#appendix-c-acknowledgments)


### Foreword

这些年来，我的雇主对我的经手面试很放心。如果我们正在招聘JavaScript方面的人才，我第一个问题就是——当然这不是真的——我首先观察面试者是需要沐浴还是喝一杯，
因为感觉到舒适是很重要的。but once I'm past the bit about the candidate's fluid in/out-take,我就开始考擦面试者是真的懂JavaScript，还是只是懂得jQuery。  <br>
并不是jQuery有什么问题，jQuery能够让你在不知道JavaScript原理的情况下实现很多功能，这是一个功能我不是一个bug。如果你的工作对JavaScript性能和可维护性要求高的话，
你团队需要有人知道像jQuery的库如何放在一起。你需要像他们一样利用JavaScript的核心。  <br>
如果我想得到他们的JavaScript核心技能图，我更感兴趣的是做的闭包（你已经阅读过这个系列的书了，对吗？）和是如何充分利用异步性的，所以我们写了这本书。 <br>
对于初学者，你将通过异步编程的支柱——回调，当然，这不太令人满意，但是下一章节的课程promise就充满美味了。   <br>
如果你不知道promise，那么你现在就应该去学习它。Promise是在JavaScript和DOM中提供异步返回值的官方方式。未来所有关于异步DOM API都会使用promise，现在已经很多在使用中了，所以你应该做好充分准备去使用它。写这本书的时候，promise已经得到几乎所有主流浏览器的支持，IE也很快就会对其支持。当你完成学习promise的时候，我希望你能紧接着学习下一课程：Generators。   <br>

Generators在Chrome和FireFox中已经植入了稳定版本，没有太多的蠢事和仪式，坦白来说，对比起他们的有趣，他们更加复杂。知道我看到他们结合promise，这是我想到的。他们是我们开发可维护性和可读性高的重要工具。   <br>
我不会破坏你的惊喜，但是准备好迎接未来的JavaScript吧！特别是在并发和异步方面，你将能越来越能控制它们。   <br>
我不会再打扰你享受这本书。如果你在阅读本书之前阅读了这个章节，给你10个异步点，你值得拥有它们！   <br>

Jake Archibald  <br>
[jakearchibald.com](https://jakearchibald.com/)，[@jaffathecake](https://twitter.com/jaffathecake)

Google Chrome的开发人员支持者
