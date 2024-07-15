---
title: Markdown Diagrams
date: 2023-09-22 10:22
categories: Note
tags:
- markdown
thumbnail: "https://mermaidjs.github.io/img/class.png"
---


### https://support.typora.io/Draw-Diagrams-With-Markdown

### Flowchart
> note: graph TD[RL,LR];
```
graph LR;
    A[SFSFSFSDF SDF]-->|One| B[SFDSFSF SDF];
    A-->|One| C;
    B-->|One| D;
    C-->|One| D;
    F-->A
```

### Sequence diagram

**关键字**

-    title，定义序列图的标题
-    participant，定义时序图中的对象
-    note，定义对时序图中的部分说明

**方位控制**
1.  left of，表示当前对象的左侧
2.  right of，表示当前对象的右侧
3.  over，表示覆盖在当前对象（们）的上面

{actor}，表示时序图中的具体对象（名称自定义）

**箭头分为以下几种**：
1.    -> 表示实线实箭头
2.    –> 表示虚线实箭头
3.    ->> 表示实线虚箭头
4.    –>> 表示虚线虚箭头

```
sequenceDiagram
    
    participant Alice
    participant Bob
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->>John: Fight against hypochondria
    end
    Note right of John: Rational thoughts <br/>prevail!
    John-->>Alice: Great!
    John->>Bob: How about you?
    Bob-->>John: Jolly good!
```


```
sequenceDiagram
    #participant, 参与者 
    
    participant A 
    participant B 
    participant C 
    
    note left of A: A左侧说明 
    note over B: 覆盖B的说明 
    note right of C: C右侧说明 
    
    # - 代表实线, -- 代表虚线; > 代表实箭头, >> 代表虚箭头 
    
    A->A:自己到自己 
    A->B:实线实箭头 
    A-->C:虚线实箭头 
    B->>C:实线虚箭头 
    B-->>A:虚线虚箭头

```

### Gantt diagram
甘特图是一类条形图，由Karol Adamiechi在1896年提出, 而在1910年Henry Gantt也独立的提出了此种图形表示。通常用在对项目终端元素和总结元素的开始及完成时间进行的描述 

**关键字如下**

| Title      | 标题               |
| ---------- | ------------------ |
| dateFormat | 日期格式           |
| section    | 模块               |
| Completed  | 已经完成           |
| Active     | 当前正在进行       |
| Future     | 后续待处理         |
| crit       | 关键阶段           |
| 日期缺失   | 默认从上一项完成后 |

> **%%	<Name of Activity>		: crit if critical else empty,done, active or empty, reference name or empty, Start Date or dependency, End Date or Duration**


```
gantt
    dateFormat YYYY-MM-DD
    title Adding GANTT diagram functionality to mermaid
    
    
    section A section
    Complete task   :done,crit  des1,   2014-01-06,2014-01-20
    Active task     :active, des2 after des1,2014-01-09,2014-03-11
    Future task     :    dest3,after des2 2014-01-10,2014-08-11
    Future task2    :   dest4,  after dest3,2014-11-11
    
    section B secion
    complete task :crit, ref,2014-05-01
    active task :active after ref, 2014-07-01
    active task2    :active ,after ref 2014-12-11,2014-12-21
    
```

```
gantt
        dateFormat  DD/MM/YY
        title Project Name

	Section Pre-condition
	Activity 1      :21/12/16, 22/12/16
	Activity 2      :21/12/16, 22/12/16
	Activity 3      :16/01/17, 02/02/17
	Activity 4      :01/02/17, 02/02/17

	Section     Kick-off
	Activity 5  :01/02/17, 03/02/17
	Activity 6	:01/02/17, 03/02/17
	Project Initiated :01/02/17, 03/02/17

	Section Tech Design
	Technical Design		:crit, active, T1, 06/02/17, 21/03/17

	Section Delivery	
	Activity 7		:06/02/17, 10/02/17
	Activity 8			:10/02/17, 14/02/17
	Order				:15/02/17, 14/03/17
	Deployment			:crit, 15/03/17, 21/03/17
	Activity 9			:crit,T2, after T1, 23/03/17
	Project Close-Down		:after T2, 24/03/17
```

```
gantt
        dateFormat YYYY-MM-DD
        title <Name of the project>
        
        section Phase 1 Name
        Activity 1			:	 done,    des1, 2017-01-06, 2017-01-08
        Activity 2               	:	 active,  des2, 2017-01-09, 2017-01-12
        Activity 3               	:        	  des3, 2017-01-12, 5d
        Activity 4              	:         	  des4, after des3, 5d

        section Phase 2 Name
        Activity 5 			: crit, done,		2017-01-06, 24h
        Activity 6		        : crit, done, 		after des1, 2d
        Activity 7		        : crit, active, 		    3d
        Activity 8			: crit,			 	    5d
        Activity 9			:			 	    2d
        Activity 10			: 			 	    1d

        section Phase 3 Name
        Activity 11			: 	active,   a1,	after des1, 3d
        Activity 12			:			after a1  , 20h
        Activity 13			:		 doc1, 	after a1  , 48h

        section Phase 4 Name
        Activity 12			:			after doc1, 3d
        Activity 15			: 	  			    20h
        Activity 16			: 			   	    48h
```

---
















---

### Class diagram -: experimental

```
classDiagram
Class01 <|-- AveryLongClass : Cool
Class03 *-- Class04
Class05 o-- Class06
Class07 .. Class08
Class09 --> C2 : Where am i?
Class09 --* C3
Class09 --|> Class07
Class07 : equals()
Class07 : Object[] elementData
Class01 : size()
Class01 : int chimp
Class01 : int gorilla
Class08 <--> C2: Cool label
```
![classDiagram](https://mermaidjs.github.io/img/class.png)

### Git graph -: experimental

```
gitGraph:
options
{
    "nodeSpacing": 150,
    "nodeRadius": 10
}
end
commit
branch newbranch
checkout newbranch
commit
commit
checkout master
commit
commit
merge newbranch
```
![gitDiagram](https://mermaidjs.github.io/img/git.png)