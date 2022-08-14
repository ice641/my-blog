---
title: fhq-treap笔记
mathjax: true
tags:
  - OI
  - 平衡树
categories:
  - 笔记
hidden: false
abbrlink: 6
date: 2022-08-10 14:13:08
update: 2022-08-10 00:00:00
---

放一下自己的板子。

<!--more-->

```cpp
#include <bits/stdc++.h>
using namespace std;

#define gc getchar

typedef long long ll;

const int N = 5e5 + 10,inf = 1e9;

ll read()
{
	ll a = 0,b = 0;char c = gc();
	while(c < '0'||c > '9') b = (c == '-'),c = gc();
	while(c >= '0'&&c <= '9') a = a * 10 + c - 48,c = gc();
	return b ? -a : a;
}

struct node
{
	int val,pri,siz;
	int ls,rs;
};

struct pir{int l,r;};

struct treap 
{
	node tr[N];
	int idx = 0;
	
	void upd(int u)
	{
		if(!u) return ;
		node &p = tr[u];
		p.siz = tr[p.ls].siz + tr[p.rs].siz + 1;
	}
	
	pir split(int rt,int val)
	{
		if(!rt) return pir{0,0};
		node &p = tr[rt];
		pir o;
		if(p.val < val)
		{
			o = split(p.rs,val);
			p.rs = o.l;
			o.l = rt;
		}
		else 
		{
			o = split(p.ls,val);
			p.ls = o.r;
			o.r = rt;		
		}
		upd(rt);
		return o;
	}
	
	int merge(int l,int r)
	{
		if(!l||!r) return l + r;
		node &lp = tr[l],&rp = tr[r];
		if(lp.pri < rp.pri)
		{
			rp.ls = merge(l,rp.ls);
			upd(r);
			return r;
		}
		else 
		{
			lp.rs = merge(lp.rs,r);
			upd(l);
			return l;
		}
	}
	
	int ins(int rt,int val)
	{
		pir o = split(rt,val);
		tr[++idx] = node{val,rand(),1,0,0};
		o.l = merge(o.l,idx);
		return merge(o.l,o.r);
	}
	
	int del(int rt,int val)
	{
		node &p = tr[rt];
		if(p.val < val) p.rs = del(p.rs,val);
		else if(p.val == val)
		{
			rt = merge(p.ls,p.rs);
			upd(rt);
			return rt;
		}
		else p.ls = del(p.ls,val);
		upd(rt);
		return rt;
	}
	
	int find_kth(int rt,int val)
	{
		if(!rt) return 1;
		node p = tr[rt];
		if(p.val < val) return find_kth(p.rs,val) + tr[p.ls].siz + 1;
		else return find_kth(p.ls,val);
	}
	
	int find_val(int rt,int k)
	{
		if(!rt) return -1;
		node p = tr[rt];
		if(tr[p.ls].siz + 1 > k) return find_val(p.ls,k);
		else if(tr[p.ls].siz + 1 == k) return p.val;
		else return find_val(p.rs,k - tr[p.ls].siz - 1);
	}
	
	int find_pre(int rt,int val)
	{
		int pre = -inf;
		while(rt)
		{
			if(tr[rt].val < val) pre = tr[rt].val,rt = tr[rt].rs;
			else rt = tr[rt].ls;
		}
		return pre;
	}
	
	int find_suc(int rt,int val)
	{
		int suc = inf;
		while(rt)
		{
			if(tr[rt].val > val) suc = tr[rt].val,rt = tr[rt].ls;
			else rt = tr[rt].rs;
		}
		return suc;			
	}
}t;
int main()
{
	int T = read(),rt = 0;
	while(T--)
	{
		int op = read(),p = read();
		if(op == 1) rt = t.ins(rt,p);
		if(op == 2)	rt = t.del(rt,p);
		if(op == 3)	printf("%d\n",t.find_kth(rt,p));
		if(op == 4)	printf("%d\n",t.find_val(rt,p));
		if(op == 5)	printf("%d\n",t.find_pre(rt,p));
		if(op == 6)	printf("%d\n",t.find_suc(rt,p));
	}
	return 0;
}
```

