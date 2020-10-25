---
title: "Personal Assistant Kino Part 3 - 작업을 최대한 간편하게 관리하고, 데이터를 기록하자"
layout: single
classes: wide
date: 2017-10-15 23:30

category: 
    - Quantified Self

tag:
    - slack bot
    - quantified self
    - task
    - personal assistant
    - side project

toc: true
toc_label: "Table of Contents"
toc_icon: "list-alt"
toc_sticky: false
---


Kino 프로젝트는 QS를 통해서 자신에 대해서 알고, 불필요한 일들을 자동화시키고 삶의 질을 증진시키기 위한 프로젝트 입니다. 이번 편에는 사용하고 있는 다양한 앱들을 연결하여 사용하는 작업 간편 관리 기능에 대해서 이야기 합니다. 

![images](https://github.com/DongjunLee/BeAwesomeToday/raw/master/images/quantified_self_logo_2x.gif)
 <figcaption class="caption">출처 : http://quantifiedself.com/</figcaption>

지금까지의 시리즈

- [Personal Assistant Kino Part 1 - Overview](https://dongjunlee.github.io/quantified%20self/Personal_Assistant_Kino_Part_1_Overview/)
- [Personal Assistant Kino Part 2 - Chatbot의 기본구조 Skill & Scheduler](https://dongjunlee.github.io/quantified%20self/Personal_Assistant_Kino_Part_2_Skill_and_Scheduler/)
- [Personal Assistant Kino Part 3 - 작업을 최대한 간편하게 관리하고, 데이터를 기록하자](https://dongjunlee.github.io/quantified%20self/Personal_Assistant_Kino_Part_3_T3/)
- [Personal Assistant Kino Part 4 - 자주 읽은 글들은 자동으로 저장하는 Smart Feed](https://dongjunlee.github.io/quantified%20self/Personal_Assistant_Kino_Part_4_Smart_Feed/)
- [Quantified Self Part 5 - 데이터 시각화와 대쉬보드 with KPI](https://dongjunlee.github.io/quantified%20self/QS_Part_5_Data_Visualization_and_Dashboard/)
- [Quantified Self Part 6 - 생산적인 하루에 대한 정량적인 표현과 4년간의 데이터 이야기](https://dongjunlee.github.io/quantified%20self/QS_Part_6_Analysis_My_Life/)

Github: https://github.com/DongjunLee/quantified-self





저번 편인 `Part 2 - Skill & Scheduler` 은 Kino가 내가 원하는 대로 돌아가도록 준비하는 과정이였습니다. 이제 이 프로젝트의 목표인 Quantified Self를 다루려고 합니다. 나 자신에 대한 데이터를 손쉽게 모으고, 차트를 보며 피드백을 나 자신에게 주고, 이를 통해 삶의 질을 증진시키는 과정을 말이지요.


## T3 (Todoist + Toggl + Trello)

 오늘 다루려는 이야기는 Task에 관한 이야기입니다. 저는 [Todoist](https://ko.todoist.com/)를 굉장히 애용하고 있습니다. Premium 기능으로 업그레이드해서 사용할 정도로 말이죠. 모바일, 데스크탑 전부 사용할 수 있는 app이 있어서 언제 어디서든 간편하게 To do list를 관리할 수 있었습니다.

 여기서 더 나아가 나는 작업들을 진행할 때, 시간이 얼마나 걸리는지.. 그리고 이 작업에 대해서는 얼마나 집중을 했는지, 하루의 시간을 어떻게 보냈는지 알고 싶었습니다. 

 제가 원하는 것들을 충족하기 위해서는 Todoist를 사용하는 것만으로는 부족했습니다. 그래서 필요한 서비스들을 찾아보게 되었습니다. **시간 측정**에는 [Toggl](https://www.toggl.com/)이 가장 잘 만들어진 서비스였고, 작업을 시작하고 (Doing), 끝내는 것(Done)을 가장 쉽게 다룰 수 있는 것은 [Trello](https://trello.com)의 **칸반 보드**를 통해서 Task, Doing, Done 의 리스트로 관리 하는 것이라는 결론을 낼 수 있었습니다. 

그렇게해서 만들어진 것이 **T3**

즉, T3 = Todoist + Toggl + Trello 를 통한 Task 간편 관리 기능입니다.  
아래는 T3에 사용되는 **Skill**들 입니다.

### Todoist

- 🌆 today_briefing : Todoist에 등록된 Task들을 브리핑합니다.
- 📃 todoist_remain : 남은 작업들에 대해서 안내합니다.

### Toggl

- ⌚️ toggl_timer : Toggl Timer 시작 혹은 정지.
- 🔔 toggl_checker : 30분 마다 시간 체크. (150분 이상 작업 시, 휴식 추천)
- 📊 toggl_report : Toggl task 리포트.

### Trello

- 📋 kanban_init : Trello 보드를 초기화 합니다.
- 📋 kanban_sync : Todoist의 Task들과 Trello 보드의 싱크를 맞춥니다.

### Question

- ✍️ attention_question : 작업 후, 집중도 물어보기 (100점 만점)

- ✍️ attention_report : 집중도 리포트

  

## 작업 관리 시나리오

 이렇게 Kino Slack Bot에 연결된 스킬들을 통해서 유기적으로 작업들이 돌아가게 됩니다. 이 프로젝트에서 가장 초점을 맞추고 있는 것 중에 하나가 직접 데이터를 기록하기 위한 노력을 최대한 줄이는 것이기 때문이죠. 

 위의 스킬을 기준으로 조금 더 자세히 설명드리면 다음과 같습니다.
아침이 되면, Todoist 에 등록된 일감들이 Trello 보드에 자동으로 추가가 됩니다. (kanban_init)

![images](https://github.com/DongjunLee/BeAwesomeToday/raw/master/images/ko/kino-kanban-board.png)

 여기에서 Tasks 에 있는 작업을 Doing 으로 옮기게 되면, 그때 Toggl의 시간 기록이 동작하게 됩니다.

![images.png]({{ site.url }}{{ site.baseurl }}/assets/images/Quantified%20Self%20Part%203%20-%20T3/toggl_start.png)

 단순하게 해당 작업을 끝낸 후에는 Done으로 옮기면 Toggl 시간 기록 역시 정지가 되고, 집중도를 물어보게 되어있습니다.

![images.png]({{ site.url }}{{ site.baseurl }}/assets/images/Quantified%20Self%20Part%203%20-%20T3/toggl_end.png)

 그리고 이렇게 카드를 옮겨주면서 작업을 진행해주면, 밤에 이런 리포트를 생성해주도록 되어 있습니다.

![images](https://github.com/DongjunLee/BeAwesomeToday/raw/master/images/kino-toggl-report.png)
 <figcaption class="caption"> toggl_report Skill </figcaption>

 이상이 제가 하루의 Task를 관리하는 시나리오입니다.   
제가 하는 것이란 Trello의 카드를 옮기는 것 뿐이에요. 참 간단하죠?

 Quantified Self에서 가장 중요한 것은 데이터를 손쉽게 모을 수 있어야하고, 한눈에 볼 수 있는 차트를 제공함으로서 간편하게 자기자신에게 피드백을 줄 수 있어야 한다는 것 입니다. 그런 의미로 T3는 굉장히 간편하고 유용한 기능입니다. 그리고 Task 관련 데이터들을 Toggl에 쌓여있으므로, 언제든 데이터를 받을 수 있고 더 복잡한 분석 또한 할 수 있을 것 입니다.



## Trello Webhook

 Skill들을 Python으로 wrapping 되어있는 package 들을 사용하면 간편하게 내가 원하는 커스텀 스킬들을 만들 수 있습니다. 각각 서비스에 필요한 TOKEN, ACCESS_KEY 등을 준비하고 연결하면 손쉽게 끝낼 수 있습니다. 

 그 외에 작업이 필요한 부분은 Webhook입니다. IFTTT에서도 Trello를 연결해서 사용할 수 있지만, 기본적으로 IFTTT는 실시간이 아닙니다. 하지만 Trello로 Task들을 관리하려면 실시간으로 반응을 해야합니다. 작업을 시작한다고 Doing에 올린지 10분이나 지나서 Toggl Timer가 작동하는 것은 너무나도 불편하니까요.

 Trello에서는 Webhook를 추가할 수 있도록 지원하고 있습니다. 이 Webhook을 처리하려면 callback을 처리하는 간단한 서버가 있어야 합니다. 이 callback을 처리하기 위해서 Server를 빌리는 것은 너무 아깝습니다. 이럴때는 [Serverless Framework](https://serverless.com/)를 사용해서 간단하게 처리할 수 있습니다. AWS에 대한 간략한 설정을 마치고, callback에 대한 함수를 정의한다음 배포를 하면 AWS API Gateway + Lambda가 자동설정 되는 것을 보실 수 있습니다.

```python
def kanban_webhook(event, context):
    input_body = json.loads(event['body'])
    print(event['body'])

    action = input_body["action"]
    action_type = action["type"]

    if action_type == "createCard":
        list_name, card_name = get_create_card(action["data"])
    elif action_type == "updateCard":
        list_name, card_name = get_update_card(action["data"])

    kanban_list = ["DOING", "BREAK", "DONE"]
    if list_name in kanban_list:
        payload = make_payload(action=list_name, msg=card_name)
        r = send_to_kino({"text": payload})
    ...
```

[kino-webhook](https://github.com/DongjunLee/kino-bot/tree/master/kino-webhook) 여기에서 **kanban_webhook**이 구현되어 있습니다.

이제 Webhook을 다 정의했으니, 이 Webhook을 Kino가 처리하면 모든 문제는 끝이 납니다! 아래는 카드 이동에 따라서 Skill들의 연결을 정리해놓은 것입니다.

```python
   def KANBAN_handle(self, event):
        toggl_manager = TogglManager()

        action = event['action']
        description = event['msg']
        if action.endswith("DOING"):
            toggl_manager.timer(
                description=description,
                doing=True,
                done=False)
        elif action.endswith("BREAK"):
            toggl_manager.timer(doing=False, done=False)
        elif action.endswith("DONE"):
            toggl_manager.timer(doing=False, done=True) ## Todoist 연결되어 있음
```


- Doing : Toggl Timer 시작

- Done : Toggl Timer 정지 & Todoist 작업 완료

- Break : Toggl Timer 정지 

  

## Monotasking, 한 번에 한가지 일에만 집중을

 T3 라는 이름의 작업 관리기능을 만들게 된 것에 대해서는 데이터 수집 이외에도 이유가 하나 더 있습니다.
이렇게 칸반으로 작업을 관리하게 되면, 자연스럽게 강제되는 것이 있습니다. 바로 멀티테스킹을 하지 않도록 되는 것이죠. 
멀티태스킹은 여러가지 작업을 동시에 하면서 더 빠르게 작업들을 할 수 있다고 생각을 하게 되지만, 실상은 그렇지 않다고 합니다.

 [≪정리하는 뇌≫](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=61762093) 의 저자 대니얼 J. 래비틴에 말에 따르면 이렇습니다

>  우리는 자기가 멀티태스킹을 하고 있다고 생각하지만, 이것은 강력하고도 사악한 착각이다. MIT의 신경과학자이자 분할 주의(divided attention)의 세계적 권위자인 얼 밀러는 우리 뇌가 멀티태스킹에는 별로 적합하지 않게 만들어져 있다고 말했다. 사람들은 자기가 멀티태스킹을 하고 있다고 생각하지만, 실제로는 한 과제에서 다른 과제로 아주 신속하게 전환하고 있을 뿐이라는 것이다.  
> (중략 ...)
> 멀티태스킹은 투쟁-도피 호르몬인 아드레날린은 물론 스트레스 호르몬인 코르티솔의 생산도 증가시킨다. 또한 뇌를 과도하게 자극해 생각을 뒤죽박죽으로 만든다. 멀티태스킹은 도파인 중독 피드백 고리를 만들어내고, 보상작용을 통해 뇌가 초점을 잃고 끊임없이 외부자극을 찾아 나서게 만든다. 설상가상으로 전전두엽피질은 새로움 편향이 있다. 무언가 새로운 것이 등장하면 쉽게 주의를 뺏긴다는 의미다.  - 154 페이지 중에서

 물론, 멀티태스킹이 실제로 가능한 사람들도 있다고 합니다. 굉장히 소수의 사람들이라고 하고 대부분의 사람들에게는 한 번에 하나의 일에 집중하는 것이 더 효율적이라고 합니다. 저 역시 한번에 여러가지를 잘 하지는 못하는 사람이기에 이렇게 한번에 한가지 일에 집중하는 시스템을 통해서 강제해보려고 합니다. `Doing` 에 개발 작업을 옮겼다면, 딱 그 작업만 할 수 있게 말이죠.




## 끝으로

이번 T3에서는 여러가지 서비스들과 Kino를 전부 연결해서 작업을 간편하게 관리하고, 차트를 제공하는 T3 기능에 대해서 살펴보았습니다. 이렇게 하루하루 Task에 대한 데이터들을 모아서 내가 어떤 작업에 잘 집중하는지, 어느 시간대에 집중을 잘 하는지.. 이런 여러가지 분석을 할 수 있을 것 입니다. 저도 데이터를 모으는 중이라, 나중에 꼭 분석하려고 합니다. :D


모든 코드는 [여기서](https://github.com/DongjunLee/kino-bot) 확인하실 수 있습니다.  

다음에는 최신 Feed를 바로바로 알려주고, 자동으로 모은 데이터를 통해서 분류까지 알아서 하는 **Smart Feed** 기능에 대해서 알아보겠습니다.


