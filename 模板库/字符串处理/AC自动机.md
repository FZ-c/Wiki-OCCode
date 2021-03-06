---
title: AC自动机
description: 
published: true
date: 2021-11-19T08:01:22.306Z
tags: 模板-字符串处理, ac自动机
editor: markdown
dateCreated: 2021-11-19T03:12:27.630Z
---

```cpp
#include <cstring>
#include <queue>
using namespace std;
/*
 * HDU 2222
 * 求目标串中出现了几个模式串
 */
struct Trie
{
	int next[500010][26];
	int fail[500010];
	int end[500010];
	int root, L;
	int newNode()
	{
		for (int i = 0; i < 26; i++)
		{
			next[L][i] = -1;
		}
		end[L++] = 0;
		return L - 1;
	}
	void init()
	{
		L = 0;
		root = newNode();
	}
	void insert(char buf[])
	{
		int len = strlen(buf);
		int now = root;
		for (int i = 0; i < len; i++)
		{
			if (next[now][buf[i] - 'a'] == -1)
			{
				next[now][buf[i] - 'a'] = newNode();
			}
			now = next[now][buf[i] - 'a'];
		}
		end[now]++;
	}
	void build()
	{
		queue<int> Q;
		fail[root] = root;
		for (int i = 0; i < 26; i++)
		{
			if (next[root][i] == -1) next[root][i] = root;
			else
			{
				fail[next[root][i]] = root;
				Q.push(next[root][i]);
			}
		}
		while (!Q.empty())
		{
			int now = Q.front();
			Q.pop();
			for (int i = 0; i < 26; i++)
			{
				if (next[now][i] == -1) next[now][i] = next[fail[now]][i];
				else
				{
					fail[next[now][i]] = next[fail[now]][i];
					Q.push(next[now][i]);
				}
			}
		}
	}
	int query(char buf[])
	{
		int len = strlen(buf);
		int now = root;
		int res = 0;
		for (int i = 0; i < len; i++)
		{
			now = next[now][buf[i] - 'a'];
			int temp = now;
			while (temp != root)
			{
				res += end[temp];
				end[temp] = 0;
				temp = fail[temp];
			}
		}
		return res;
	}
	void debug()
	{
		for (int i = 0; i < L; i++)
		{
			printf("id = %3d,fail = %3d,end = %3d,chi = [", i, fail[i], end[i]);
			for (int j = 0; j < 26; j++) printf("%2d", next[i][j]);
			printf("]\n");
		}
	}
};
char buf[1000010];
Trie ac;
int main()
{
	int T;
	int n;
	scanf("%d", &T);
	while (T--)
	{
		scanf("%d", &n);
		ac.init();
		for (int i = 0; i < n; i++)
		{
			scanf("%s", buf);
			ac.insert(buf);
		}
		ac.build();
		scanf("%s", buf);
		printf("%d\n", ac.query(buf));
	}
	return 0;
}
```