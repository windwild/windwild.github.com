---
layout: post
title: "二分查找"
description: ""
category: Basic
tags: [Basic]
---
{% include JB/setup %}
二分查找比较完美的代码，像`middle = left + （right － left) / 2;`真的是细节决定成败了。

	int search4(int array[], int n, int v)
	{
	    int left, right, middle;

	    left = -1, right = n;

	    while (left + 1 != right)
	    {
	        middle = left + （right － left) / 2;

	        if (array[middle] < v)
	        {
	            left = middle;
	        }
	        else
	        {
	            right = middle;
	        }
	    }

	    if (right >= n || array[right] != v)
	    {
	        right = -1;
	    }

	    return right;
	}
