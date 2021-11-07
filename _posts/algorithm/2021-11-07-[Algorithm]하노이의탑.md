---
layout: post
title: "[Algorithm] 하노이의 탑"
subtitle: "하노이의 탑"
categories: study
tags: algo
---
> 하노이의 탑

- 재귀 사용

```python
def move_disk(disk_num, start_peg, end_peg):
    print("%d번 원판을 %d번 기둥에서 %d번 기둥으로 이동" % (disk_num, start_peg, end_peg))

def hanoi(num_disks, start_peg, end_peg):
    if num_disks == 0:
        return
    else :
        other_peg = 6 - start_peg - end_peg
    
        hanoi(num_disks - 1, start_peg, other_peg)
        
        move_disk(num_disks, start_peg, end_peg)
        
        hanoi(num_disks - 1, other_peg, end_peg)
```