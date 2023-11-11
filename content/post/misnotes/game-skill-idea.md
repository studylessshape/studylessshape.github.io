---
title: "关于游戏技能的一些想法"
date: 2020-12-09T20:19:18+08:00
lastmod: 2020-12-19T17:13:00+08:00
categories: ["随记"]
tags: ["C++","游戏"]
draft: false
summary: "朋友的一个项目让我想了下组合元素发技能的方式。好久没写代码了，写了一点游戏技能的C++代码，感觉有了一点点思路。"
---

朋友的一个项目让我想了下组合元素发技能的方式。

好久没写代码了，写了一点游戏技能的C++代码，感觉有了一点点思路。

## 技能基础
虽然说少用继承，但是我还是写了一个父类来作为技能的基础。命名为`skill_base`，虽然看起来很蠢但很容易懂。三个字段：
 * `skill_id`:技能的id，用来索引比较方便
 * `skill_name`:技能名字
 * `eff`:技能的效果

顺带来个可以覆写的方法`use_skill()`，返回的就是定义的技能效果；和一个用来获取id的`get_id()`。<br/>
(skill_base.h)
```c++
class skill_base
{
protected:
    int skill_id;
    string skill_name;
    effect eff;

public:
    virtual effect use_skill()
    {
        cout << "use skill " << skill_name << endl;
        return eff;
    }

    int get_id()
    {
        return skill_id;
    }
};
```

`effect`是技能的效果，需要在`skill_base`前定义一个`effect`类：<br/>
(skill_base.h)
```c++
class effect
{
private:
    int damage;

public:
    effect()
    {
        damage = 0;
    }
    effect(int set_damage) : damage(set_damage)
    {
    }
    int get_damage()
    {
        return damage;
    }
};
```

这里仅定义了一个伤害（damage）的字段，可以在之后添加其他字段或者添加虚方法用于继承和实现多态。`effect`里加了两个构造，为了之后方便实现了无参构造，不需要的话可以加个`explicit`。这里的`get_damage`可以的话弄成其他的会更好，这里就不再更换了。

有了这两个，就可以开始想办法弄点其他的了。这里有两个方案：一个是用构造者模式，写一个builder类，另外一个是写一个list类。这里选后一种。当然推荐第一种。

技能的list可以封装一个`vector<skill_base>`字段，其他语言比如C#可以是`List<SkillBase>`。此处简单的定义了三个方法，分别是`add_skill`、`rm_skill`、`use_skill`，即添加技能、移除技能和使用技能。那么`skill_list`大致可以定义成这样：<br/>
(skill_base.h)
```c++
class skill_list
{
private:
    vector<skill_base> skills;

public:
    void add_skill(skill_base skill)
    {
        skills.push_back(skill);
    }

    void rm_skill(int skill_id)
    {
        for (auto i = skills.begin(); i != skills.end(); i++)
        {
            if (i->get_id() == skill_id)
            {
                //erase为vector移除元素的操作
                skills.erase(i, i);
                break;
            }
        }
    }

    effect use_skill(int id)
    {
        // snip
    }
};
```

`use_skill`根据id法则的不同有不同的实现细节。比如只是单纯的`push_back`的话，就需要遍历数组寻找id ~~，这种情况可能使用哈希表会更好~~ ；而在保证id是有序的或者是有一个生成法则，可以通过下标索引的花，时间可以降到`O(1)`。
```c++
    effect use_skill(int id)
    {
        effect e;
        for (auto i : skills)
        {
            if (i.get_id() == id)
            {
                e = i.use_skill();
            }
        }
        return e;
    }
```

## 技能实现
如果使用构造者的话可能可以忽略这一步。这里写了四个简单的技能：风（wind）、火（fire）、水（water）、雷（thunder）。<br/>
(skills.h)
```c++
#include "skill_base.h"
class wind : public skill_base
{
public:
    wind(int damage, int id)
    {
        eff = effect(damage);
        skill_id = id;
        skill_name = "wind";
    }
};

class fire : public skill_base
{
public:
    fire(int damage, int id)
    {
        eff = effect(damage);
        skill_id = id;
        skill_name = "fire";
    }
};

class water : public skill_base
{
public:
    water(int damage, int id)
    {
        eff = effect(damage);
        skill_id = id;
        skill_name = "water";
    }
};

class thunder : public skill_base
{
public:
    thunder(int damage, int id)
    {
        eff = effect(damage);
        skill_id = id;
        skill_name = "thunder";
    }
};
```

## 使用！
写了个小样例。前面init的部分在真正到项目中，应该是从配置文件中读取的。<br/>
(main.cpp)
```c++
#include <iostream>
#include <../include/skills.h>
// Windows将上面的注释，下面的取消注释
// #include "../include/skills.h"
using namespace std;
int main()
{
    //init
    wind wi(10, 1);
    fire fi(15, 2);
    water wa(8, 3);
    thunder th(18, 4);

    skill_list list;
    list.add_skill(wi);
    list.add_skill(fi);
    list.add_skill(wa);
    list.add_skill(th);
}
```

之后就能直接用了！此处为了方便，输入id选择技能使用。
```c++
    //input
    int id;
    cout << "input number to use skill:" << endl;
    cin >> id;

    auto e = list.use_skill(id);
    cout<<"get "<<e.get_damage()<<" damage!"<<endl;

```

效果：<br/>
![](game-skill-idea-running.png)

## 胡思乱想
也许可以使用键值索引（c#的dictionary）？或者还能进一步封装？等用到游戏里再想吧（Lazy）。

[源码仓库](https://gitee.com/study_less_shape/Practice_Saving/tree/master/FooIdea/game-skill)