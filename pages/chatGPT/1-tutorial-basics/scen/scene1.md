---
sidebar_position: 0
---

# 场景1：问答问题

<head>
  <script defer="defer" src="https://embed.trydyno.com/embedder.js"></script>
  <link href="https://embed.trydyno.com/embedder.css" rel="stylesheet" />
</head>

这个场景应该是使用 AI 产品最常见的方法。以 ChatGPT 为例，一般就是你提一个问题，ChatGPT 会给你答案，比如像这样：

![que_example.png](./assets/que_example.png)

在这个场景下，prompt 只要满足前面提到的基本原则，基本上就没有什么问题。但需要注意，不同的 AI 模型擅长的东西都不太一样，prompt 可能需要针对该模型进行微调。另外，目前的 AI 产品，也不是无所不能，有些问题你再怎么优化 prompt 它也没法回答你。以 ChatGPT 为例：

1. ChatGPT 比较擅长回答基本事实的问题，比如问 `什么是牛顿第三定律？` 。但不太擅长回答意见类的问题，比如问它 `谁是世界第一足球运动员？`，它就没法回答了。
2. 另外，ChatGPT 的数据仅有 2021 年 9 月以前的，如果你问这个时间以后的问题，比如 `现在的美国总统是谁？`它的答案是「截至2021年9月，现任美国总统是乔·拜登（Joe Biden）。」

:::info 🔴 求助
这种直接提问的 prompt ，我们称之为 Zero-shot prompt。模型基于一些通用的先验知识或模型在先前的训练中学习到的模式，对新的任务或领域进行推理和预测。你会在高级篇看到相关的介绍，以及更多有意思的使用方法。
:::

另外，正如我在前面基础用法一章中提到的那样，问答场景里还有一个很重要的玩法，就是多轮聊天，你可以针对某个问题，进行多轮的提问。

## **使用技巧一：To do and Not To do**

:::caution 注意
我介绍的技巧其实在各个场景都可以使用，我将其放在某个场景下解释，只是因为我觉得它更有可能在这个场景用到。你也会更容易记住这个用法。并不意味着这个技巧仅能在此场景使用。并且多技巧混用也是个不错的用法。
:::

在问答场景里，为了让 AI 回答更加准确，一般会在问题里加条件。比如让 AI 推荐一部电影给你 `Recommend a movie to me` 。但这个 prompt 太空泛了，AI 无法直接回答，接着它会问你想要什么类型的电影，但这样你就需要跟 AI 聊很多轮，效率比较低。

所以，为了提高效率，一般会在 prompt 里看到类似这样的话（意思是不要询问我对什么感兴趣，或者问我的个人信息）：

```other
DO NOT ASK FOR INTERESTS. DO NOT ASK FOR PERSONAL INFORMATION.
```

如果你在 ChatGPT 里这样提问，或者使用 ChatGPT 最新的 API ，它就不会问你问题，而是直接推荐一部电影给你，它的 Output 是这样的：

```other
Certainly! If you're in the mood for an action-packed movie, you might enjoy "John Wick" (2014), directed by Chad Stahelski and starring Keanu Reeves. The movie follows a retired hitman named John Wick who seeks vengeance against the people who wronged him. It's a fast-paced and stylish film with lots of thrilling action sequences and an engaging story. If you're looking for something that will keep you on the edge of your seat, "John Wick" is definitely worth a watch!
```

但如果你使用的是如 Davinci-003 这样的模型，它的 Output 很可能是这样的，它还会问你的兴趣爱好：

```other
Sure, I can recommend a movie based on your interests. What kind of movie would you like to watch? Do you prefer action, comedy, romance, or something else?
```

所以 OpenAI 的 API [最佳实践文档](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)里，提到了一个这样的最佳实践：

> **Instead of just saying what not to do, say what to do instead. 与其告知模型不能干什么，不妨告诉模型能干什么。** 

我自己的实践是，虽然现在最新的模型已经理解什么是 Not Todo ，但如果你想要的是明确的答案，加入更多限定词，告知模型能干什么，回答的效率会更高，且预期会更明确。还是电影推荐这个案例，你可以加入一个限定词：

```other
Recommend a movie from the top global trending movies to me.
```

当然并不是 Not Todo 就不能用，如果：

- 你已经告知模型很明确的点，然后你想缩小范围，那增加一些 Not Todo 会提高不少效率。
- 你是在做一些探索，比如你不知道如何做精准限定，你只知道不要什么。那可以先加入 Not Todo ，让 AI 先发散给你答案，当探索完成后，再去优化 prompt。

以下是一些场景案例，我整理了两个 Less Effective（不太有效的） 和 Better（更好的） prompt，你可以自己尝试下这些案例：

| 场景          | Less Effective                                                                   | Better                                                                           | 原因                                                                |
| ----------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| 推荐雅思必背英文单词    | Please suggest me some essential words for IELTS                                 | Please suggest me 10 essential words for IELTS                                   | 后者 prompt 会更加明确，前者会给大概 20 个单词。这个仍然有提升的空间，比如增加更多的限定词语，像字母 A 开头的词语。 |
| 推荐香港值得游玩的地方 | Please recommend me some places to visit in Hong Kong. Do not recommend museums. | Please recommend me some places to visit in Hong Kong including amusement parks. | 后者的推荐会更准确高效一些，但如果你想进行一些探索，那前者也能用。                                 |

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="Please suggest me some essential words for IELTS" initial-response="1. Essential: important, necessary, required, indispensable, vital
1. Important: critical, significant, major, key, essential
2. Necessary: essential, required, indispensable, mandatory, needful
3. Required: necessary, essential, mandatory, needed, called for
4. Indispensable: essential, necessary, vital, critical, required" max-tokens="256" box-rows="3" model-temp="0.7" top-p="1">
    <noscript>Failed to load Dyno Embed: JavaScript must be enabled</noscript>
</div>
