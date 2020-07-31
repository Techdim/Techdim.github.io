---
title: daily recording
date: 2020-07-10 10:57:17
tags:
---
# Argparse的基本用法

argparse是Python自带的命令行参数解析包

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description="Demo of argparse")
    parser.add_argument('-n','--name', default=' Li ')
    parser.add_argument('-y','--year', default='20')
    args = parser.parse_args()
    print(args)
    name = args.name
    year = args.year
    print('Hello {}  {}'.format(name,year))

if __name__ == '__main__':
    main()

```
<!-- more -->

上面这段代码当中首先导入一个argparse包，然后使用`argparse.ArgumentParser`生成一个argparse对象（参数解析器），其中`description`描述这个解析器的作用。使用`add_argument`来增加参数，使用`parser.parse_argu`来解析参数


