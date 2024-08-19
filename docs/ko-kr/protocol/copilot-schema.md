---
order: 3
icon: ph:sword-bold
---

# 전투 스키마

::: tip
주의: JSON 형식은 주석을 지원하지 않으므로 아래의 예시에서 주석은 제거해주시기 바랍니다.
:::

`resource/copilot/*.json` 및 필드 설명의 사용법.

## 개요

```json
{
    "stage_name": "暴君",           // 스테이지 이름, 필수 입력 항목입니다. 레벨, 코드, stageId, levelId 등 중복되지 않는 중국어 이름이면 어떤 것이든 사용 가능합니다.
    "opers": [                      // 오퍼레이터 목록
        {
            "name": "棘刺",         // 오퍼레이터 이름

            "skill": 3,             // 스킬 ID (범위: [1, 3]), 선택 사항, 기본값은 1입니다.

            "skill_usage": 0,       // 스킬 사용법, 선택 사항, 기본값은 0입니다.
                                    // 0 - 'actions'에서 사용됨
                                    // 1 - 준비되면 사용 (예: 쏜즈: 데스트레자, 머틀: 지원지령β)
                                    // 2 - 준비되면 사용되며, 한 번만 사용됨 (예: 마운틴: 스탠스 스위칭)
                                    // 3 - 자동 결정 (구현되지 않음)
                                    // 0은 자동 발동 스킬(예: 강타, 패시브 등)입니다.

            "requirements": {       // 요구 사항, 예약된 필드이며 구현되지 않았습니다. 선택 사항이며 기본적으로 비어 있습니다.
                "elite": 2,         // 정예화 요구 사항, 선택 사항, 기본값은 0입니다.
                "level": 90,        // 레벨 요구 사항, 선택 사항, 기본값은 0입니다.
                "skill_level": 10,  // 스킬 레벨 요구 사항, 선택 사항, 기본값은 0입니다.
                "module": 1,        // 모듈 요구 사항, 선택 사항, 기본값은 0입니다.
                "potentiality": 1   // 잠재능력 요구 사항, 선택 사항, 기본값은 0입니다.
            }
        },
    ],
    "groups": [
        {
            "name": "任意正常群奶",  // 그룹 이름, 필수 입력 항목입니다.
                                    // `deploy` 아래의 `name` 필드와 일치하는 모든 이름이 허용됩니다.

            "opers": [              // 랜덤하게 선택되는 오퍼레이터들로 이루어진 목록입니다. 위의 `opers` 필드와 유사합니다. 레벨이 높은 오퍼레이터일수록 높은 우선순위를 갖습니다.
                {
                    "name": "夜莺", // Nightingale
                    "skill": 3,
                    "skill_usage": 2 // 스킬 재사용 대기 시간이 다를 수 있음
                },
                {
                    "name": "白面鸮", // Ptilopsis
                    "skill": 2,
                    "skill_usage": 2
                }
            ]
        }
    ],
    "actions": [                    // 순차적으로 실행되는 작업들로 이루어진 목록입니다. 필수 입력 항목입니다.
        {
            "type": "部署",         // 작업 유형, 선택 사항, 기본값은 "Deploy"입니다.
                                    // "Deploy" | "Skill" | "Retreat" | "SpeedUp" | "BulletTime" | "SkillUsage" | "Output" | "SkillDaemon"
                                    // "部署"   |  "技能"  |  "撤退"   | "二倍速"   |  "子弹时间"  |  "技能用法"   | "打印"  |  "摆完挂机"
                                    // 영어와 중국어 모두 지원됩니다.
                                    // "Deploy"는 비용이 충분해질 때까지 대기합니다 (타임아웃이 아닌 한).
                                    // "Skill"은 스킬이 준비될 때까지 대기합니다 (타임아웃이 아닌 한).
                                    // "SpeedUp"은 전환 가능하며, 사용 후 2배 속도가 됩니다. 다시 사용하면 정상 속도로 돌아갑니다.
                                    // "BulletTime"은 예약된 필드이며 구현되지 않았으며, 모든 다른 작업과 함께 정상 속도로 돌아갑니다.
                                    // "Output"은 문서의 내용을 출력합니다(자막 등).
                                    // "SkillDaemon"은 스킬을 사용 가능할 때 사용하고, 끝까지 아무 작업도 수행하지 않습니다.

            // 다음 세 가지 필드 사이의 관계는 AND(논리곱)입니다, 즉 &&
            "kills": 0,             // 필요한 킬 수에 도달할 때까지 대기합니다. 선택 사항, 기본값은 0입니다.

            "costs": 50,            // 특정 코스트가 될 때까지 대기합니다. 선택 사항, 기본값은 0입니다.
                                    // 잠재력으로 인해 코스트가 변경될 수 있으므로, 이 필드는 쉬운 전투에만 사용하세요. 그렇지 않으면 "cost_changes"를 사용하세요.
                                    // 코스트가 두 자리 수인 경우에만 정확하게 인식됩니다. 세 자리 수 비용에는 권장하지 않습니다.

            "cost_changes": 5,      // 특정 코스트가 변경될 때까지 대기합니다. 선택 사항, 기본값은 0입니다.
                                    // 작업 시작 시 코스트를 기준으로 계산합니다 (이전 작업의 비용을 기준으로 함).
                                    // 코스트가 두 자리 수인 경우에만 정확하게 인식됩니다. 세 자리 수 비용에는 권장하지 않습니다.

            "cooling": 2,           // 스킬 쿨타임에 있는 오퍼레이터의 수가 특정 숫자에 도달할 때까지 대기합니다. 선택 사항, 기본값은 "ignore"(-1)입니다.

            // TODO: 다른 조건
            // TODO: "condition_type": 0,    // 조건 집계 유형, 선택 사항, 기본값은 0입니다.
            //                        // 0 - AND(논리곱)； 1 - OR(논리합)

            "name": "棘刺",         // 오퍼레이터/그룹 이름, "Deploy" 유형일 때 필수 입력 항목입니다. "Skill" 또는 "Retreat" 유형일 때 선택 사항입니다.

            "location": [ 5, 5 ],   // 배치 위치
                                    // "Deploy" 유형일 때 필수 입력 항목입니다.
                                    // "Skill" 또는 "Retreat" 유형일 때 선택 사항입니다.
                                    // "Skill": 일반 오퍼레이터에게 스킬을 사용할 때 위치 대신 스킬 사용을 위한 위치 시설을 권장합니다. 스킬을 사용하기 위해 이름 대신 위치를 사용하세요.
                                    // "Retreat": 여러 소환물이 존재할 때 위치 대신 소환을 위해 철수를 권장합니다. 이름 대신 위치를 사용하세요.

            "direction": "左",      // 배치 방향, "Deploy" 유형일 때 필수 입력 항목입니다.
                                    // "Left" | "Right" | "Up" | "Down" | "None"
                                    // "左"   |  "右"   | "上"  | "下"   |  "无"
                                    // 영어와 중국어 모두 지원됩니다.

            "skill_usage":  1,      // 스킬 사용 설정을 재정의합니다. "SkillUsage" 유형일 때 필수 입력 항목입니다.
                                    // 예: Myrtle은 처음에 스킬을 사용하지 않고 공격해야 하며, 나중에 자동으로 스킬을 사용해야 합니다.
                                    // 이 경우 1로 설정하세요.

            "pre_delay": 0,         // 선행 지연 시간 (밀리초), 선택 사항, 기본값은 0입니다.
            "post_delay": 0,        // 후행 지연 시간 (밀리초), 선택 사항, 기본값은 0입니다.

            // "timeout": 999999999,   // 예약된 필드이며 구현되지 않았습니다.
                                    // 제한 시간 (밀리초), "Deploy" 또는 "Skill" 유형일 때 선택 사항, 기본값은 INT_MAX입니다.
                                    // 타임아웃 시 다음 작업으로 진행합니다.

            "doc": "下棘刺了！",     // 설명, 선택 사항, 효과는 없지만 UI에 표시됩니다.
            "doc_color": "orange"   // 설명 색상, 선택 사항, 효과는 없지만 UI에 표시됩니다.
        },
                                    // 예시 1
        {
            "name": "任意正常群奶",
            "location": [ 5, 6 ],
            "direction": "右"
        },
                                    // 예시 2
        {
            "name": "史尔特尔",
            "location": [ 4, 5 ],
            "direction": "左",
            "doc": "你史尔特尔奶奶来啦！",
            "doc_color": "red"
        },
                                    // 예시 3
        {
            "type": "二倍速"
        }
    ],
    "minimum_required": "v4.0",     // 필요한 최소한의 MAA 버전, 필수 입력 항목, 예약된 필드이며 구현되지 않았습니다.
    "doc": {                        // 설명, 선택 사항입니다.
        "title": "低练度高成功率作业",
        "title_color": "dark",
        "details": "对练度要求很低balabala……",      // 여기에 이름, 비디오 링크, 공략글 링크 등을 쓸 수 있습니다.
        "details_color": "dark"
    },
}
```

## 예시

[OF-1](https://github.com/MaaAssistantArknights/MaaAssistantArknights/blob/master/resource/copilot/OF-1_credit_fight.json)