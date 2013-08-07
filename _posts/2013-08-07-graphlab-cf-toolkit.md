---
layout: post
title: "GraphLab 协同过滤工具包介绍"
description: ""
category: MPI
tags: [GraphLab,协同过滤,MPI]
---
{% include JB/setup %}

## 格式
### 输入格式
输入格式是
    
    [ user ] [ item ] [ rating] \n

举个栗子：

    1000 2 5.0
    3 7 12.0
    6 2 2.1

对输入的文件名也是有一定的区分的，`.validate`结尾的都是测试数据，`.predict`结尾的都是要预测的数据，测试数据的输入和普通的输入格式是一样的，预测数据只要输入`user` `item`就可以了。预测数据会在程序结束的时候输出到文件。

### 输出格式
输出的文件分两种，一种是UV矩阵的具体值，文件名一般为`xxx_U_X_of_Y` `xxx_V_X_of_Y`，还有一种文件是预测的文件，根据用户输入的预测文件输出相应的预测值，文件名一般为`xxx_output_X_of_Y`。如果用的是`biassgd`的话，还会额外生成两个`bias`文件。

### 基本参数

1. `--D` 特征向量的维数（Feature Vector），越大计算时间会越长，但是能力越强。
2. `--lambda` 正则项所占比例，主要是用来防止过拟合现象产生的。
3. `--max_iter` 最大的迭代次数。
4. `--maxval` 最大的预测值
5. `--minval` 最小的预测值
6. `--predictions` 输出结果的文件名
7. `--gamma` 梯度下降的步长
8. `--step_dec` 梯度下降的步长变化比率

## 算法
### ALS
优点：简单易用，不需要配置太多参数就能跑。

缺点：准确度一般，计算开销较大！

### SGD
优点：速度快！

缺点：需要调整步长参数，相比`ALS`需要更多的迭代次数。

### BIAS-SGD
优点：速度快！

缺点：需要调整步长参数。

### SVD++
优点：在参数合适的情况下比SGD跟家准确，而且相对来讲速度挺快的。

缺点：有太多太多的参数需要调整了！

### Weighted-ALS
优点：简单易用，特别之处在于可以对打分设定置信度！

缺点：准确度一般，计算开销较大！

### Sparse-ALS
优点：不大清楚

缺点：不大清楚

### SVD
优点：速度快

缺点：准确度低

## 单机运行脚本样例

	#!/bin/bash
	graphlab/release/toolkits/collaborative_filtering/biassgd \
	        ~/dataset/smallnetflix/
	        --predictions ~/output/windwild_output \
	        --max_iter 500 \
	        --minval 0.5 \
	        --D 20 \
	        --step_dec 0.9 \
	        --lambda 0.005 \
	        --gamma 0.02

	sort ~/output/windwild_output_1_of_1
