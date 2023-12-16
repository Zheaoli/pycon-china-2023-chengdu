---
transition: fade-out
image-layout: image-right
image: https://github.com/Zheaoli/pycon-china-2023-beijing/assets/7054676/eee7e3f7-1e00-4988-8646-ecb9722e816f
---

#  What's the problem we are facing

What's the problem we are facing is the Python before 3.12

<v-clicks>

1. The observable of Python before 3.12
2. The ABI compatibility of Python before 3.12
3. The performance of Python before 3.12 

</v-clicks>

---
transition: fade-out
---

# What's the problem we are facing

<h2>For now, I think you guys may have any idea about I'm talking about. Let's go find some example</h2>

---
transition: fade-out
---

# What's the problem we are facing

<h2>The observable of Python before 3.12</h2>

If we got a Python process. How can we know what's happening in the process?

<v-clicks>

- We need monitor the exception in the process
- We need to collect some execution information in the process and export it as the metric
- etc...

</v-clicks>

<v-click>

What should we do?

</v-click>

---
transition: fade-out
---

# What's the problem we are facing

<h2>案例二</h2>

感谢 @yihong0618 大哥曾经给的灵感

假设你有一个链接 PGSQL 的服务，你又在经历如下的困惑

<v-clicks>

- 到 PGSQL 服务器延迟没有异常，但是你的查询时不时会玄学超时
- 你本地的 CPU 时不时的玄学过山车
- 你想知道在一个 SQL 从查询到返回的过程中，发生了什么，经历了哪些函数

</v-clicks>

---
transition: fade-out
---

# 为什么我们需要强调程序的可观测性

不知道你们发现没有，不知不觉中我已经把 Python 程序的可观测性变成程序的可观测性了

<v-clicks>

- 是的，我们今天会继续以 Python 作为样例来讨论一些可观测性的手段和思路（
- 本人擅长挂羊头卖狗肉

</v-clicks>
