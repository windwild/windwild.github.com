---
layout: post
title: "Matlab Image Processing"
description: ""
category: Image Processing
tags: [Image Processing]
---
{% include JB/setup %}

发一段matlab图像处理的代码，主要的作用是识别出不同图形的形状，然后根据形状填充上不同的颜色。主要用到的就是Hough变换。

	pic = imread('~/sample_file.png');
	bw = rgb2gray(pic);
	bw_e = edge(bw,'canny');
	imshow(bw_e);

	% [centers, radii, metric] = imfindcircles(bw_e,[30 200]);
	% viscircles(centers, radii,'EdgeColor','b');

	[B,L] = bwboundaries(bw_e,'noholes');
	hold on
	for k = 1:length(B)
	    boundary = B{k};
	    temp = zeros(1000,1000);
	    for i = 1:length(boundary)
	        temp(boundary(i,1),boundary(i,2)) = 1;
	    end
	    [H,T,R] = hough(temp);
	    P  = houghpeaks(H,15,'threshold', ceil(0.3*max(H(:))));
	    peaknum = length(P);
	    if peaknum == 3
	        fill(boundary(:,2),boundary(:,1),'r');
	    else
	        if peaknum == 4
	            fill(boundary(:,2),boundary(:,1),'g');
	        else
	            fill(boundary(:,2),boundary(:,1),'b');
	        end
	    end
	end
