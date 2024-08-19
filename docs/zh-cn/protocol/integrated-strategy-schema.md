---
order: 5
icon: ri:game-fill
---

# 肉鸽辅助协议

::: tip
请注意 JSON 文件是不支持注释的，文本中的注释仅用于演示，请勿直接复制使用
:::

## 肉鸽资源存放位置

- `resource/roguelike/` 下按照主题存放各个肉鸽的作业资源
  - 主题文件夹：`Phantom/` 为傀影肉鸽资源，`Mizuki/` 为水月肉鸽资源, `Sami/` 为萨米肉鸽资源
    - `autopilot/` 内是各个关卡的作战 json
      - `关卡名.json` 关卡的作战逻辑
    - `encounter/` 内是不期而遇类（所有非战斗和商店节点）事件逻辑
      - `default.json` 刷等级模式
      - `deposit.json` 刷源石锭模式
    - `recruitment.json` 干员招募逻辑
    - `shopping.json` 商店购买藏品逻辑

- 特别地，在`Sami/`下的
  - `foldartal.json`表示萨米肉鸽密文板的使用逻辑
  - `collapsal_paradigms.json`表示萨米肉鸽坍缩范式的类型
  - `autopilot/关卡名_collapse.json` 关卡的作战逻辑（刷坍缩范式模式）
  - `encounter/collapse.json` 刷坍缩范式模式不期而遇逻辑

## 肉鸽第一步——干员招募

`resource/roguelike/主题名/recruitment.json` 描述了干员招募的逻辑

```json
{
    "theme": "Phantom",              //肉鸽主题（这里是傀影）
    "priority": [                    //群组的优先度，有序
             ...
        ],
    "team_complete_condition": [     //阵容完备度检测
             ...
        ]
}
```

### 干员分类

按照你的游戏理解将干员分成不同的 **_groups_** （群组，相关概念参考 [战斗流程协议](./copilot-schema.md)）

::: info 注意

1. 同一 group 内的干员和召唤物必须**部署方式一致**（即同为近战或者同为高台），**攻击范围尽量相同**

2. 允许同一干员或者召唤物根据用法不同，被分类至不同的 group

3. 请不要修改已经存在的 group 名称，以免在 MAA 更新时导致之前版本的作业无法使用

4. 请尽量不要新增 group，尽量将新添加进作业的单位按照用法纳入已经存在的 group
   :::

::: tip
默认仅招募精 1 55 级以上等级干员
:::

```json
{
    "theme": "Phantom",
    "priority": [                     // 群组，有序
        {
            "name": "棘刺",           // 群组名（这个群组名是棘刺）
            "opers": [                // 群组包含的干员，有序，代表部署这一组干员时的部署优先度，
                                      // （比如需要部署该组干员时，先检测有没有棘刺，有棘刺就部署棘刺，没有棘刺再检测有没有号角）
                {
                    "name": "棘刺",   // 干员名
                    ...
                },
                {
                    "name": "号角",
                    ...
                }
            ]
        },
       "team_complete_condition": [
             ...
        ]
    ]
}
```

1. 已有群组介绍

    以萨米肉鸽的作业为例：主要将干员分为了
    | 分组 | 主要考量 | 主要包括职业 | 举例干员 |  
    | :--- | :--- | :--- | :--- |
    | **_地面阻挡_** | 站场和清杂 | 重装、近卫 | 奶盾、基石、羽毛笔、山、M3、令和稀音的召唤物、斑点、重装预备干员 |
    | **_地面单切/处决者_** | 单独对战精英怪 | 处决者特种 | 史尔特尔、异德、麒麟 R 夜刀、M3、红 |
    | **_高台 C_** | 常态和决战输出 | 狙击、术士 | 假日威龙陈、澄闪、艾雅法拉、肥鸭 |
    | **_高台输出_** | 对空和常态输出 | 狙击、术士 | 空弦、能天使、克洛丝、史都华德 |
    | **_速狙_** | 物理输出，标准射程 | 狙击 | 艾拉、能天使、跃跃、克洛丝 |
    | **_术师_** | 法术输出，标准射程 | 单法 | 艾雅法拉、逻各斯、史都华德 |
    | **_辅助_** | 可以打到身后 1 格 | 辅助 | 塑心、铃兰、跃跃、梓兰 |
    | **_狙击_** | 射程较远的高台 | 投掷手、攻城手 | 维什戴尔、提丰、早露 |
    | **_奶_** | 治疗能力 | 治疗、辅助 | 凯尔希、浊心斯卡蒂、芙蓉、安赛尔 |
    | **_单奶_** | 攻击范围纵深>=4 | 治疗 | 凯尔希、焰影苇草、纯烬艾雅法拉、芙蓉、安赛尔 |
    | **_群奶_** | 攻击范围纵深<4,可以奶身后 | 治疗、辅助 | 夜莺、白面鸮、浊心斯卡蒂 |
    | **_回费_** | 回复 cost | 先锋 | 桃金娘、伊内丝、芬、香草 |
    | **_地刺_** | 无阻挡，在盾前提供输出或者减速 | 伏击客 | 归溟幽灵鲨、阿斯卡纶、狮蝎 |
    | **_地面远程_** | 地面长手，可以在盾后输出 | 教官、领主 | 号角、帕拉斯、棘刺、银灰 |
    | **_领主_** | 地面攻击范围纵深>4，可对空 | 要塞、领主 | 棘刺、银灰、仇白 |
    | **_盾法_** | 攻击范围短，有一定承伤能力 | 阵法术师 | 林、卡涅利安 |
    | **_炮灰_** | 吸收炮弹、再部署 | 特种、召唤物 | M3、红、桃金娘、预备干员 |
    | **_大龙_** | 盾前承伤、紧靠阻挡方便合体 | 召唤物 | 令的小龙、涤火杰西卡的盾牌 |
    | **_补给站_** | 给主 c 干员加快回转 | 召唤物 | 支援补给站、工匠类干员的召唤物 |
    | **_无人机_** | 无视高台地面的治疗召唤物 | 召唤物 | 斯卡蒂的海嗣、赫默的无人机 |
    | **_支援陷阱_** | 部署在地面的爆炸物 | 召唤物 | 支援雾机、支援轰隆隆 |
    | **_障碍物_** | 不吃部署位吸引仇恨或者阻挡 | 召唤物 | 鸟笼、障碍物 |
    | **_其他地面_** | 不希望被优先使用的地面 | 推拉、挡 1 先锋、剑豪 | 风笛、可颂 |
    | **_高台预备/其他高台_** | 不希望被优先使用的高台 | 群法、链法、战术家 | 梓兰、预备干员 |

    ::: tip
    地面阻挡主要考量干员防守的综合能力（有时候全杀了也是防守强的表现对吧），包括地面远程和领主组

    奶主要考虑综合治疗能力，包括单奶和群奶，需要考虑攻击范围覆盖时要单独使用单奶（奶 4 格）或者群奶（奶身后）

    高台输出只考虑输出能力，主要是狙击和术师职业的混杂排序，需要考虑输出类型、攻击范围等限制条件时单独使用速狙、术师、辅助、盾法

    夹子类召唤物以为数量较多，不要放在支援陷阱中，让 maa 自动部署效果比较好
    :::

2. 需要特殊操作的群组

    除了上面那些比较笼统的分组，我们有时候需要对一些干员或者干员种类进行一些定制的精细化操作，比如
    | 分组 | 包括干员 | 主要特点 |
    | :--- | :--- | :--- |
    | 益达 | 维什戴尔 | 高台输出，优先部署可以减轻压力 |
    | 棘刺 | 棘刺、号角 | 地面远程输出，傀影肉鸽有些图有非常适配的位置 |
    | 召唤类 | 凯尔希、令、稀音 | 自带地面阻挡，有的图需要优先部署，召唤物可以当阻挡也可以当炮灰 |
    | 情报官 | 晓歌、伊内丝 | 既可以回费、又可以侧面输出、还可以单切 |
    | 浊心斯卡蒂 | 浊心斯卡蒂 | 低压时奶量尚可，但是范围特殊，一些图有比较适配的位置 |
    | 焰苇 | 焰影苇草 | 萨米肉鸽常用开局干员,兼具治疗和输出，一些图有比较适配的位置 |
    | 银灰 | 银灰、玛恩纳 | 地面大范围决战输出，可以针对 boss 进行部署 |
    | 史尔特尔 | 史尔特尔 | 由于精二后固定携带 3 技能，这时站场能力几乎为零，需要阻挡位的部署优先度极低 |
    | 骰子 | 骰子 | 水月肉鸽中的骰子需要单独操作 |

::: info 注意
目前固定将未识别到的地面干员归入倒数第二个编组后面（其他地面），未识别到的高台干员归入倒数第一个编组后面（其他高台）
:::

### 预设阵容——阵容完备检测

在你预计能够通关或者打到高层的队伍中，哪些干员属于基本核心阵容？是比不可少的？又需要几个?

::: info 注意
目前脚本的招募逻辑是在阵容未满足阵容完备度之前，只招募 0 希望（暂时只有三星）和 key（可以理解为白名单）干员，存希望给高星 key 干员

作战编队时也会优先编入满足阵容完备度的干员，剩下的空位按鹰序。这里因为没有验证干员是否重复（有重复会点两下反而不编入），所以阵容完备度检测的组中尽量不要有重复干员

所以不要将完备度数字设置太高，建议所有需要的干员数量（基本核心阵容，一般是 1 核心，2 地面，1-2 奶，1 高台）加起来在 4-8 位

在队伍满足阵容完备度检测之后，每次拿到招募券会按照干员的评分和精二优先度进行招募，所以为了避免希望浪费，不希望招募的干员可以不设置评分或者设置在同职业三星/预备队员评分以下

对于萌新号：在 10 次招募没有满足半队 key 干员的情况下，会放弃完备度检测，按照评分有啥用啥，所以没练三星的萌新号会出现只拿了两三个六星，剩下拿了一堆预备干员的情况
:::

```json
{
    "theme": "Phantom",              //肉鸽主题
    "priority": [                    //群组
             ...
        ],
    "team_complete_condition": [     //阵容完备度检测
             {
            "groups": [              //需要哪些群组中的干员
                "高台C"
            ],
            "threshold": 1           //这些干员需要多少数量
        },                           //（这里表示需要高台C组的干员1名）
        {
            "groups": [
                "棘刺",              //（这里表示棘刺、地面阻挡、地面单切、炮灰分组的干员最少需要2个）
                "地面阻挡",
                "地面单切",
                "炮灰"
            ],
            "threshold": 2
        },
        ...
        ]
}
```

### 调整干员招募参数

1. 群组内的顺序代表部署该组时的优先度

2. 临时招募干员会在原有的评分基础上加 600 分

3. 随机直升干员的评分是招募评分+精二评分加起来计算

4. 群组内干员各个字段的意思和脚本相关的逻辑

    ```json
    {
        "theme": "Phantom",
        "priority": [
            "name": "地面阻挡",                                // 群组名（这里是地面阻挡组）
            "doc": "标准线为1档(清杂能力或者站场能力比山强)>山>2档(阻挡>2,可自回)>斑点,站场能力小于斑点放到单切或者炮灰组",
                                                              // 带“doc”字段均为json内备注文档，对程序运行没有影响
            "opers": [                                        // 该包括哪些干员，有序，代表部署优先度
                {
                    "name": "百炼嘉维尔",                      // 干员名称（这里是百嘉，在组内第1位，表示需要部署地面阻挡组的时候，首先检测是不是百嘉）
                    "skill": 3,                               // 使用几技能（这里举例使用3技能）
                    "skill_usage": 2,                         // 技能使用模式，参考3-3战斗流程协议，不填或者1为自动放，2为只放x次（x通过"skill_times"字段设置），3暂时不支持
                    "skill_times": 2,                         // 技能使用次数，默认为1，在"skill_usage"字段为2时生效
                    "alternate_skill": 2,                     // 当没有指定技能时使用的备选技能，一般是6星干员未精二且精二后使用3技能时才需要指定（这里指没有3技能时使用2技能）
                    "alternate_skill_usage": 1                // 备选技能的技能使用模式（该字段尚未实现）
                    "alternate_skill_times": 1                // 备选技能的技能使用次数（该字段尚未实现）
                    "recruit_priority": 900,                  // 招募优先级，数字越大优先级越高，900以上属于看到必招，400以下招募优先级比一些关键干员精二优先度还低
                                                              // 临时招募的干员优先度自动+600
                    "promote_priority": 600,                  // 进阶优先级，数字越大优先级越高，900以上属于有希望就精二，400以下招募优先级低于招募普通三星干员
                                                              // 小技巧:当你将招募优先度压低或者不写,拉高一些精二优先度,实际上就是在拉高临时招募到这些干员的精二优先度
                    "is_key": true,                           // true为key（关键）干员，false或省略为非key干员。在阵容完备检测未通过时，仅招募key干员与0希望干员，保存希望。
                    "is_start": true,                         // true为开局选择干员，false或省略为非开局干员。在队伍中没有start干员时，仅招募start干员与0希望干员，用户填写的干员会强制未start干员
                    "auto_retreat": 0,                        // 部署几秒后自动撤退，整数，大于 0 时生效，主要用于特种干员和投锋，由于肉鸽一般是2倍速，推荐设置为技能持续时间/2
                    "recruit_priority_when_team_full": 850,   // 无需单独设置，满足阵容完备度时，招募优先级，默认招募优先级-100
                    "promote_priority_when_team_full": 850,   // 无需单独设置，满足阵容完备度时，进阶优先级，默认精二优先级+300
                    "recruit_priority_offsets": [             // 按当前阵容调控招募优先级
                        {
                            "groups": [                       // 需要哪些组满足条件，是按组计数，这些组里不要出现重复干员，否则会重复计数
                                "凯尔希",
                                "地面阻挡",
                                "棘刺"
                            ],
                            "is_less": false,                 // 条件是大于还是小于, false或者省略是大于, true是小于
                            "threshold": 2,                   // 满足条件的数量
                            "offset": -300                    // 满足后对招募优先级的调整
                                                              // (这里表示当 凯尔希, 地面阻挡, 棘刺 这三个群组中有2名以上干员时, 百炼嘉维尔的招募优先级减300)
                        }
                    ]
                },
                ...
            ],
        ],
        "team_complete_condition": [
            ...
        ]
    }
    ```

    ::: info 注意
    `recruit_priority_offsets`的`group`中的群组不要有重复干员

    `auto_retreat`设置后一般不需要再在作战计划中为其设置`retreat_plan`
    :::

5. 按照你的理解新增群组和干员

    新增群组后，你可以从已有的群组中复制干员过来，参考大佬们已有的评分，在此基础上修改

## 肉鸽第二步——战斗逻辑

`resource/roguelike/主题名/autopilot/关卡名.json` 描述了每个关卡的作战策略

### MAA 肉鸽基本战斗逻辑——牛牛高血压之源

1. 根据地图上格子类型进行基本的战斗操作

    - MAA 会根据地图上的格子是蓝门还是红门，是高台还是地面，能不能被部署来进行基本的战斗操作

    - MAA 仅根据地图名称或者编号决定使用哪份作业，不会判断地图的**普通**、**紧急**、**路网**、**密文板使用**等情况
    - MAA 不会判断**作战中地图上无法确定的格子的情况**，比如`驯兽小屋`的祭坛位置，`从众效应`是从左边还是从右边出怪

    所以在后面，你需要尽量设计一套能够应付一个地图名**所有不同情况**（上面提到的几种情况）的战斗逻辑,小心被大家挂到 issue 上说这张图操作高血压哦（笑）

2. MAA 的基本作战策略--堵蓝门

    1. 地面干员会优先部署在蓝门的格子上（为什么是格子上，请往下看）或者周围，方向朝向红门（自动计算），

    2. 优先部署地面，然后部署治疗干员和高台干员，一圈一圈的由蓝门向四周部署，

    3. 会不停的按照上面的逻辑部署可以部署的东西（干员、召唤物、支援物品等等）

### 优化基本战斗策略

1. 蓝门替代方案

    仅仅把干员堆在蓝门门口显然不太聪明，有些关卡有格子是一夫当关万夫莫开，防守在这里显然效率很高，

    或者有些关卡有多个蓝门，MAA 不知道哪个蓝门对应哪个红门，也会胡乱部署，

    这个时候你需要打开 [地图 Wiki](https://map.ark-nights.com/areas?coord_override=MAA) 一边对着地图一边在脑海里指挥作战了

    首先在`设置`里将`坐标展示`切换为`MAA`

    然后根据你的经验寻找需要优先防守的点的坐标和朝向（一般是朝向来怪的方向），写入到 json 的`"replacement_home"`里面

    ```json
     {
         "stage_name": "蓄水池", // 关卡名
         "replacement_home": [ // 重要防守点（蓝门替代点），至少需要填写1个
             {
                 "location": [ // 格子坐标，从地图wiki获得
                     6,
                     4
                 ],
                 "direction_Doc1": "优先朝向，但并不代表绝对是这个方向（算法自行判断）",
                 "direction_Doc2": "不填默认 none，即没有推荐方向，完全由算法自行判断",
                 "direction_Doc3": "none / left / right / up / down / 无 / 上 / 下 / 左 / 右",
                 "direction": "left" // （这里表示将干员优先部署到坐标6,4的格子朝左，周围自动部署的干员也尽量朝向左）
             }
         ],
    ```

2. 部署格子黑名单

    有优先防守的点就有优先不部署干员的点，比如大火球经过的位置，boss 脚底下，一些不好输出的位置，

    这个时候我们引入了`"blacklist_location"`将不想让他部署干员的格子加到黑名单里

    ::: info 注意
    这里加入的格子，就算在后面的部署策略里面写进去的话，也是没法部署的
    :::

    ```json
        ...
        "blacklist_location_Doc": "这里是用法举例，不是说蓄水池这个图需要 ban 这两个点",
        "blacklist_location": [  // 禁止程序进行部署的位置
            [
                0,
                0
            ],
            [
                1,
                1
            ]
        ],
    ```

3. 其他地图策略

    比如水月肉鸽中如果蓝门进怪了是不是要用骰子，缓解堆怪压力

    ```json
        "not_use_dice_Doc": "蓝门干员撤退时是否需要用骰子，不写默认false",
        "not_use_dice": false,
    ```

### 还是高血压？是时候展现你真正的技术了——定制作战策略！

使用`"deploy_plan"`和`"retreat_plan"`实现定制化操作

定制化策略优先与基本战斗策略，当定制化策略中的所有步骤都尝试执行完毕后，省下的干员或者倒下再部署的干员会按照基本战斗策略不断部署

有时候不需要设置太多的定制策略，完成关键步骤后再交给 MAA，二者结合可能效果更好

1. 使用各个群组部署干员

    ```json
    "deploy_plan": [ // 部署逻辑，按从上到下、从左到右的顺序进行检索，并尝试部署找到的第一个干员，如果没有就跳过
        {
            "groups": [ "百嘉", "基石", "地面C", "号角", "挡人先锋" ],//这一步从这些群组中寻找干员
            "location": [ 6, 4 ],             //遍历百嘉群组、基石群组、地面C群组等等群组
            "direction": "left"               //将找到的第一个干员部署到6,4这个坐标上并朝向左边
        },                                    //没找到就进行下一个部署操作
        {
            "groups": [ "召唤" ],
            "location": [ 6, 3 ],
            "direction": "left"
        },
        {
            "groups": [ "单奶", "群奶" ],
            "location": [ 6, 2 ],
            "direction": "down"
        }
    ]
    ```

    ::: info 注意
    MAA 会将所有部署指令扁平化后，执行最优先级部署操作
    例：在[6,4]部署[ "百嘉", "基石", "地面 C"]，在[6,3]部署[ "基石", "地面 C"]，那么 MAA 会将部署指令扁平化成附带坐标的[ "百嘉", "基石", "地面 C"，"基石", "地面 C"]
    如果在战斗中[6,4]位置的"百嘉"组干员倒下，手里有有可部署的“基石”组干员，会优先布置到[6,4]而不是[6,3]
    :::

    ::: tip
    一些常用的干员组用法:

    1.很多作业中主要防守点的组合是[ "地面阻挡", "处决者", "其他地面"]，这意味着当主要抗伤位干员死了会尝试用处决者拖延 cd;当这个点生存压力过大时,考虑使用[ "重装","地面阻挡", "处决者", "炮灰", "其他地面"];盾后的点则可以优先盾后输出[ "地面远程","地面阻挡", "处决者", "其他地面"]；单纯吸引仇恨 or 送死可以使用[ "炮灰", "障碍物", "其他地面"]

    2.高台位组合常用[ "高台输出", "其他高台"]，想要无论什么高台都可以放置可以使用[ "高台输出", "狙击", "辅助", "盾法", "其他高台"]

    3.一些地面位置叔叔和地刺类干员都比较适合[ "玛恩纳", "地刺"]
    :::

2. 在某个时间点部署干员
   ::: tip
   适用于某些单切干员或者需要炮灰的使用场景
   :::

    ```json
    "deploy_plan": [
            {
                "groups": [ "异德", "刺客", "挡人先锋", "其他地面" ],
                "location": [ 5, 3 ],
                "direction": "left",
                "condition": [ 0, 3 ]       // 该操作仅在击杀数为0-3时进行
            },
            {
                "groups": [ "异德", "刺客", "挡人先锋", "其他地面" ],
                "location": [ 5, 3 ],
                "direction": "left",
                "condition": [ 6, 10 ]
            },
            ...
        ]
    ```

3. 在某个时间点撤退干员
   ::: tip
   有时候炮灰过强站住场或者需要部署位腾挪阵容怎么办，撤退！

    同一位置的部署和撤退命令的 condition 数字尽量不要重叠，否则会秒上秒下
    :::

    ```json
    "retreat_plan": [  // 在特定时间点撤退目标
            {
                "location": [ 4, 1 ],
                "condition": [ 7, 8 ] // 在击杀数为7-8时，撤去位置[4,1]的干员，没有就跳过
            }
        ]
    ```

4. 在某个时间点释放技能（to do）

5. 一些其他的字段（不推荐使用）

    ```json
        "role_order_Doc": "干员类型部署顺序，未写出部分以近卫，先锋，医疗，重装，狙击，术士，辅助，特种，召唤物的顺序补全，输入英文",
        "role_order": [ // 不推荐使用，请配置deploy_plan字段
            "warrior",
            "pioneer",
            "medic",
            "tank",
            "sniper",
            "caster",
            "support",
            "special",
            "drone"
        ],
        "force_air_defense_when_deploy_blocking_num_Doc": "场上有10000个阻挡单位时就开始强制部署总共1个对空单位（填不填写均不影响正常部署逻辑），在此期间不禁止部署医疗单位（不写默认false）",
        "force_air_defense_when_deploy_blocking_num": { // 不推荐使用，请配置deploy_plan字段
            "melee_num": 10000,
            "air_defense_num": 1,
            "ban_medic": false
        },
        "force_deploy_direction_Doc": "这些点对某些职业强制部署方向",
        "force_deploy_direction": [ // 不推荐使用，请配置deploy_plan字段
            {
                "location": [
                    1,
                    1
                ],
                "role_Doc": "填入的职业适用强制方向",
                "role": [
                    "warrior",
                    "pioneer"
                ],
                "direction": "up"
            },
            {
                "location": [
                    3,
                    1
                ],
                "role": [
                    "sniper"
                ],
                "direction": "left"
            }
        ],
    ```

### 对某个干员打法有特殊理解？——精细化操作特定干员

请将这位干员单独分组

编写作业时请考虑这位干员与现有作业的顺序优先度

你也可以不考虑那么多，单独为这个干员写一份作战逻辑

仅使用一位干员也是可行的！使用 MAA 来凹单人通关吧（由于其他逻辑的不完善，可能性很低）

参考例子：1.傀影肉鸽的棘刺 2.水月肉鸽的异德 3.萨米肉鸽的焰苇

## 肉鸽第三步——不期而遇类节点逻辑

`resource/roguelike/主题名/encounter/default.json` 描述了刷等级模式下不期而遇事件选择的策略

`resource/roguelike/主题名/encounter/deposit.json` 描述了刷源石锭模式和刷开局模式下不期而遇事件选择的策略

### MAA 现有对不期而遇的判断方法

OCR 识别不期而遇事件名称，但是选项是操作固定的位置

没有识别到时间的话会点最下面的选项

一般只需要微调或者不调整(大佬都写好了嘛)

### 优化不期而遇选项的优先度

请结合 [prts.wiki](https://prts.wiki/w/%E9%9B%86%E6%88%90%E6%88%98%E7%95%A5) 查看各个事件的选项效果，注意选项不一定固定

可以自己修改不期而遇事件选项以指引 MAA 走向某些图的特殊结局

```json
{
    "theme": "Sami",                              //肉鸽主题
    "stage": [                                    //不期而遇类事件
        {
            "name": "低地市集",                    //不期而遇事件名称
            "option_num": 3,                      //总共有几个选项（这里是3）
            "choose": 3,                          //优先选择第几个选项（这里优先选第三个），如果选不到就选跑路选项（基本上是最后一个）
            "choices": [                          //选择选项的要求（暂时不影响程序运行，只做适用情况的标注方便修改）
                {
                    "name": "选择碎草药",          //选项的名字
                    "ChaosLevel": {               //抗干扰/灯火等级
                        "value": "3",             //要求的数字
                        "type": ">"               //是大于还是小于（这里表示抗干扰/灯火大于3时才激活选择碎草药选项）
                    }
                },
                {
                    "name": "选择好看的织物",
                    "ChaosLevel": {
                        "value": "3",
                        "type": ">"
                    }
                },
                ...
```

### 根据队伍情况动态调整某些选项的优先度（TODO）

## 肉鸽第四步——商店藏品的优先度

`resource/roguelike/主题名/shopping.json` 描述了商店购买藏品(和战斗后选择藏品?)的策略

```json
{
    "theme": "Phantom",                                       //肉鸽主题名（这里是傀影）
    "priority": [                                             //优先度,有序,顺序即为优先级,越在前面越优先购买,
                                                              //但优先级判断在chars,roles的筛选之前, 可能出现高优先级商品被筛掉, 什么都不买的情况
        {
            "name": "金酒之杯",                                //藏品名字（这里金杯）
            "no_longer_buy": true,                             //true表示获得该藏品后不在花钱买藏品，false或者省略表示继续花钱买藏品
            "ignore_no_longer_buy": true,                      //true表示商店有该藏品时忽略"no_longer_buy",就是还会买它，false或者省略表示拿到有"no_longer_buy"的藏品后不会购买这个藏品
            "effect": "每有5源石锭，所有我方单位的攻击速度+7",    //藏品效果（不影响程序运行，备注效果方便排序）
            "no": 167                                          //藏品编号,wiki可查（不影响程序运行，备注方便排序）
        },

        ...
        {
            "name": "扩散之手",
            "chars": [                                         //队伍中有这些干员的时候买这件藏品
                "异客"                                         //（这里表示队伍中有异客时，遇到扩散之手，就会尝试购买）
            ],
            "effect": "【扩散术师】、【链术师】和【轰击术师】每对一个单位造成伤害就回复2点技力值",
            "no": 136
        },
        ...

        {
            "name": "折戟-破釜沉舟",
            "roles": [                                         //队伍中有这些职业的时候买这件藏品
                "WARRIOR"                                      //（这里表示队伍中有近卫干员时，遇到折戟-破釜沉舟，就会尝试购买）
            ],
            "effect": "所有【近卫】干员的防御力-40%，但攻击力+40%，攻击速度+30",
            "no": 16
        },
        ...

        {
            "name": "Miss.Christine摸摸券",
            "promotion": 2,                                   //队伍中有2个干员要晋升的时候购买（吧？）
            "effect": "立即进阶两个干员（不消耗希望）",
            "no": 15
        },
        ...

        {
            "name": "警戒篱木",
            "effect": "坍缩值-2，目标生命上限+2",
            "No": 198,
            "decrease_collapse": true                          // true表示获得该藏品会降低坍缩值。在mode为5时将不会购买
        },
        ...

    "others":                                                  // maa不会购买的收藏品，比如结局藏品和起重机
        {
            "name": "无人起重机"
        },
```

## 肉鸽特殊机制

### 萨米肉鸽——密文板

`resource/roguelike/Sami/foldartal.json` 描述了萨米肉鸽密文板的策略

```json
{
    "theme": "Sami",                                         //肉鸽主题名（这里是萨米）
    "groups": [                                              //对使用情况和用法的分组
        {
            "usage": "SkipBattle",                           //用法（这里是跳过战斗,用于刷钱和烧水模式的作战节点,使用板子跳过战斗以节省时间）
            "doc": "跳过战斗,刷钱和烧开水模式",
            "pairs": [                                       //板子对(遇到对应的节点时,会判断有没有符合下面设定的板子对,如果有,就使用能用的全用,如果没有就直接进点)
                {                                            //（这里伤痕只能和空无联结）
                    "up": [                                  //上板子
                        "伤痕"
                    ],
                    "down": [                                //下板子
                        "空无"
                    ]
                },
                {                                            //（这里会按顺序搜索 "黜人"+"惊讶","黜人"+ "疑惑","黜人"+...,"猎手"+"惊讶","猎手"+ "疑惑","猎手"+... , ...）
                    "up": [
                        "黜人",
                        "猎手",
                        ...
                    ],
                    "down": [
                        "惊讶",
                        "疑惑",
                        ...
                    ]
                }
            ]
        },
        {
            "usage": "Boss",                                 //（这里表示遇到boss点会用的板子对）
            "doc": "有的用全用了",
            ...
        }
    ],
    "foldartal": [                                          //密文板效果备注（这里仅做备注方便查看板子效果，不影响程序运行）
        {
            "name": "布局",                                  //密文板类型（上板子还是下板子）
            "foldartal": [
                {
                    "name": "黜人",                           //密文板名称
                    "effect": "选择所有右侧邻近的战斗节点"     //密文板效果
                },

```

### 萨米肉鸽——坍缩范式

当`check_collapsal_paradigms`为`true`时，MAA 将以两种不同的手段检查坍缩范式：

- 在选关界面点击屏幕中上方展开坍缩状态栏，以下称 Panel Check ；
- 观察屏幕右侧是否有坍缩范式通知 以下称 Banner Check 。

获取坍缩值的方法多种多样，我们考虑到了以下情况：

1. 战斗后因非完美战斗而使坍缩值增加，进行 Banner Check 。
2. 战斗后因获得收藏品而使坍缩值变动，进行 Banner Check 。
3. 在不期而遇等节点中因选择选项而使坍缩值变动，进行 Banner Check 。
4. 再诡异行商节点中因购买收藏品而使坍缩值变动，进行 Banner Check 。
5. 因使用密文版而使坍缩值减少，进行 Banner Check 。
6. 因进入新的一层而使坍缩值增加，进行 Panel Check 。
7. 若进行 Banner Check 时发现坍缩范式消退，因不知道会不会一次性消退两层（即便会也概率极低），会在下一次回到选关洁面的时候额外触发一次 Panel Check 。
8. 在`double_check_collapsal_paradigms`为`true`的情况下，每次回到选关界面的时候都会额外触发一次 Panel Check 以验证是否之前有漏、多记录坍缩范式。

以下为刷隐藏坍缩范式的任务配置示例：

```json
{
    "theme": "Sami",
    "mode": 5,
    "investment_enabled": false,
    "squad": "远程战术分队",
    "roles": "稳扎稳打",
    "core_char": "维什戴尔",
    "expected_collapsal_paradigms": ["目空一些", "睁眼瞎", "图像损坏", "一抹黑"]
}
```

当`mode`为`5`时：

- 优先使用`stage_name`为`关卡名_collapse`的作战策略，例如`resource/roguelike/Sami/autopilot/事不过四_collapse.json`；
- 使用`resource/roguelike/Sami/encounter/collapse.json`中描述的不期而遇事件选择的策略，
- 不会购买`decrease_collapse`为`true`的藏品。

当`mode`不为`5`但`check_collapsal_paradigms`为`true`时，仍会检测坍缩范式并在遇到`expected_collapsal_paradigms`列表中的坍缩范式时停止任务，但在遇到其他坍缩范式时将不会重开。

刷隐藏坍缩范式时，推荐 N10 难度，建议使用以下队伍：

- 维什戴尔 + 斑点 + 史都华德；
- 焰影苇草 + 梓兰 + 泡普卡；
- 提丰 + 斑点 + 史都华德。

## 希望实现的逻辑（todo）

### 自动编队逻辑

1. 可以根据不同地图设置不同的地图阵容完备性检测和技能优先度

2. 可以根据现有阵容避开一些难度高的作战

### ~~优化寻路算法~~（已初步实现）

比如可以实现前三层多战,后面的少战,这样发育会更好

### 技能保留

某格部署的干员，技能转好后等待 x 秒再开，方便对轴；可以写 Deploy_plan 下的 Skill_hold，也可以写 Skill_hold_plan

### 技能关闭

对弹药类干员有用

<!-- markdownlint-disable-file MD026 -->