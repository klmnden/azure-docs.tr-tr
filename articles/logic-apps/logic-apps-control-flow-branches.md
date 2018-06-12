---
title: Oluşturma veya paralel dalları - Azure Logic Apps katılın | Microsoft Docs
description: Oluşturma veya Azure Logic Apps içinde iş akışları için paralel dalları katılma
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2a8dcd82b67ee64e5687d8687415056b0aab39aa
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298864"
---
# <a name="create-or-join-parallel-branches-for-workflow-actions-in-azure-logic-apps"></a>Oluşturma veya iş akışı eylemleri için paralel dalları Azure Logic Apps içinde katılın

Varsayılan olarak, eylemlerinizi mantığı uygulama iş akışlarında sırayla çalışır. Aynı anda bağımsız eylemleri gerçekleştirmek için oluşturabileceğiniz [paralel dalları](#parallel-branches)ve ardından [dalları katılma](#join-branches) akışınız daha sonra. 

> [!TIP] 
> Bir dizi alan ve her bir dizi öğesi için bir iş akışını çalıştırmak istediğiniz bir tetikleyici varsa *debatch* bu diziyle [ **SplitOn** tetiklemek özelliği](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-a-parallel-branch"></a>Parallel dal ekle

Aynı anda bağımsız adımlarını çalıştırmak için var olan bir adım yanındaki paralel dalları ekleyebilirsiniz. 

![Adımları paralel olarak çalıştır](media/logic-apps-control-flow-branches/parallel.png)

Mantıksal uygulamanız tüm dalları iş akışı devam etmeden önce tamamlanmasını bekler.
Paralel yalnızca çalıştırmak dalları kendi `runAfter` özellik değerlerini eşleşen tamamlanmış üst adımının durumu. Örneğin, her ikisi de `branchAction1` ve `branchAction2` yalnızca çalışacak şekilde ayarlanmış `parentAction` ile tamamlandıktan `Succeded` durumu.

> [!NOTE]
> Başlamadan önce mantıksal uygulamanızı zaten bir adım, paralel dalları ekleyebileceğiniz olması gerekir.

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portal</a>, mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

2. Paralel dalları eklemek istediğiniz Yukarıdaki adımı oku üzerinden farenizi taşıyın.

3. Seçin **artı** oturum (**+**), seçin **paralel dalı eklemek**ve eklemek istediğiniz öğeyi seçin.

   ![Paralel şube ekleme](media/logic-apps-control-flow-branches/add-parallel-branch.png)

   Seçili öğeyi paralel bir şube artık görünür.

4. Paralel her dal için istediğiniz adımlarını ekleyin. Paralel dala sıralı bir eylem eklemek için sıralı eylem eklemek istediğiniz eylem altında fareyi hareket ettirin. Seçin **artı** (**+**) oturum ve eklemek istediğiniz adımı.

   ![Paralel dala sıralı adım ekleme](media/logic-apps-control-flow-branches/add-sequential-action-parallel-branch.png)

5. Dalları geri birleştirmek [paralel dalları katılma](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Paralel şube tanımı (JSON)

Kod görünümünde çalışıyorsanız, paralel yapısı mantığı uygulamanızın JSON tanımında bunun yerine, örneğin tanımlayabilirsiniz:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<a name="join-branches"></a>

## <a name="join-parallel-branches"></a>Paralel dalları katılma

Paralel dalları birleştirmek için yalnızca en altındaki tüm dalları altında bir adımı ekleyin. Bu adım, tüm çalışan paralel dalları bitiş çalışır.

![Paralel dalları katılma](media/logic-apps-control-flow-branches/join.png)

1. İçinde [Azure portal](https://portal.azure.com), bulma ve mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

2. Birleştirmek istediğiniz paralel dalları altında gerçekleştirmek istediğiniz adımı ekleyin.

   ![Paralel dalları katıldığı bir adım ekleme](media/logic-apps-control-flow-branches/join-steps.png)

   Paralel dalları şimdi birleştirilir.

<a name="join-json"></a>

## <a name="join-definition-json"></a>Tanımı (JSON) katılma

Kod görünümünde çalışıyorsanız, birleştirme yapısı mantığı uygulamanızın JSON tanımında bunun yerine, örneğin tanımlayabilirsiniz:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "joinAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "branchAction1": [
          "Succeeded"
        ],
        "branchAction2": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderme veya özellikleri ve öneriler oylamak için ziyaret [Azure Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımları çalıştırın](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlerini (switch deyimleri) temel alan adımları çalıştırın](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve (döngüler) arasındaki adımları yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Gruplandırılmış eylem durumu (kapsam) temelinde adımları çalıştırın](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
