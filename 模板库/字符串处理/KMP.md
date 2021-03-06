---
title: KMP
description: 
published: true
date: 2021-11-19T07:29:44.598Z
tags: 模板-字符串处理, kmp
editor: markdown
dateCreated: 2021-11-19T02:55:16.760Z
---

```cpp
/*
 * next[] 的含义 x[i-next[i]...i-1] = x[0...next[i]-1]
 * next[i] 为满足 x[i-z...i-1]=x[0...z-1] 的最大 z 值（就是x的自身匹配）
 */
void kmp_pre(char x[], int m, int next[])
{
	int i, j;
	j = next[0] = -1;
	i = 0;
	while (i < m)
	{
		while (-1 != j && x[i] != x[j]) j = next[j];
		next[++i] = ++j;
	}
}
/*
 * kmpNext[i] 的意思：next'[i] = next[next[...[next[i]]]]（直到next'[i] < 0 或者 x[next'[i]] != x[i]）
 * 这样的预处理可以快一些
 */
void preKMP(char x[], int m, int kmpNext[])
{
	int i, j;
	j = kmpNext[0] = -1;
	i = 0;
	while (i < m)
	{
		while (-1 != j && x[i] != x[j])j = kmpNext[j];
		if (x[++i] == x[++j])kmpNext[i] = kmpNext[j];
		else kmpNext[i] = j;
	}
}
/*
 * 返回 x 在 y 中出现的次数，可重叠
 */
int next[10010];
//x 是模式串， y 是主串
int KMP_Count(char x[],int m,char y[],int n)
{
	int i, j;
	int ans = 0;
// 	preKMP(x, m, next);
	kmp_pre(x, m, next);
	i = j = 0;
	while(i < n)
	{
		while (-1 != j && y[i] != x[j])j = next[j];
		i++; j++;
		if(j >= m)
		{
			ans++;
			j = next[j];
		}
	}
	return ans;
}
```