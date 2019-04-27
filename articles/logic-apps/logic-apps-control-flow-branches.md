---
title: Oluşturun veya paralel dallarından - Azure Logic Apps katılın | Microsoft Docs
description: Oluşturma veya Azure Logic Apps'te iş akışları için paralel dalları birleştirme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: 2e1c155a371fa96e4f772f632a9585948b012e54
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60685181"
---
# <a name="create-or-join-parallel-branches-for-workflow-actions-in-azure-logic-apps"></a>Oluşturun veya Azure Logic Apps'te iş akışı eylemi için paralel dallarından katılın

Varsayılan olarak, mantıksal uygulama iş akışlarında eylemlerinizi sırayla çalışır. Aynı anda bağımsız işlemleri gerçekleştirmek için oluşturabileceğiniz [paralel dalları](#parallel-branches), ardından [dalları birleştirme](#join-branches) akışınız daha sonra. 

> [!TIP] 
> Bir dizi alır ve her dizi öğesi için bir iş akışını çalıştırmak istediğiniz bir tetikleyici varsa *debatch* ile bu diziyi [ **SplitOn** özellik tetikleyicisi](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-parallel-branch"></a>Paralel dal Ekle

Bağımsız adım aynı anda çalıştırmak için var olan bir adım yanında, paralel dallarından ekleyebilirsiniz. 

![Paralel çalıştırma adımları](media/logic-apps-control-flow-branches/parallel.png)

Mantıksal uygulamanızı tüm dallar, iş akışı devam etmeden önce tamamlanmasını bekler. Paralel dalları yalnızca çalıştırma kendi `runAfter` özellik değerlerini eşleşen tamamlanmış üst adımının durumu. Örneğin, her ikisi de `branchAction1` ve `branchAction2` yalnızca çalıştırmayı ayarlamak `parentAction` ile tamamlandıktan `Succeeded` durumu.

> [!NOTE]
> Başlamadan önce mantıksal uygulamanız zaten bir adım, paralel dallarından ekleyebileceğiniz olması gerekir.

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Paralel dalları eklemek istediğiniz Yukarıdaki adımı okun üzerinde işaretçiyi taşıyın. Seçin **yanı sıra** oturum (**+**), görünür ve ardından **parallel dal Ekle**. 

   ![Paralel dal Ekle](media/logic-apps-control-flow-branches/add-parallel-branch.png)

1. Arama kutusuna bulun ve istediğiniz eylemi seçin.

   ![Bulun ve istediğiniz eylemi seçin](media/logic-apps-control-flow-branches/find-select-parallel-action.png)

   Seçili eylemi artık paralel bir dalda örneğin görünür:

   ![Bulun ve istediğiniz eylemi seçin](media/logic-apps-control-flow-branches/added-parallel-branch.png)

1. Şimdi her paralel bir dalda istediğiniz adımları ekleyin. Dala başka bir eylem eklemek için işaretçinizi eylem altında sıralı bir eylem eklemek istediğiniz taşıyın. Seçin **yanı sıra** (**+**) görünür ve ardından oturum **Eylem Ekle**.

   ![Paralel dal için sıralı bir eylem ekleme](media/logic-apps-control-flow-branches/add-sequential-action.png)

1. Arama kutusuna bulun ve istediğiniz eylemi seçin.

   ![Bulma ve sıralı bir eylem seçin](media/logic-apps-control-flow-branches/find-select-sequential-action.png)

   Artık, seçili eylem içinde geçerli dal, örneğin görünür:

   ![Bulma ve sıralı bir eylem seçin](media/logic-apps-control-flow-branches/added-sequential-action.png)

Dalı geri birleştirmek [paralel Dallarınızı katılın](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Paralel dal tanımı (JSON)

Kod Görünümü'nde çalışıyorsanız, paralel yapısı mantıksal uygulamanızın JSON tanımında bunun yerine, örneğin tanımlayabilirsiniz:

``` json
{
  "triggers": {
    "myTrigger": {}
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": {},
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

## <a name="join-parallel-branches"></a>Paralel dalları birleştirme

Paralel dalları birbirine birleştirmek için yalnızca altındaki tüm dalları altında bir adımı ekleyin. Bu adım, tüm çalışan paralel dallarından son çalışır.

![Paralel dalları birleştirme](media/logic-apps-control-flow-branches/join.png)

1. İçinde [Azure portalında](https://portal.azure.com), bulmak ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

1. Paralel dalları katılmasını istediğiniz altında seçin **yeni adım**. 

   ![Katılmak için adım ekleme](media/logic-apps-control-flow-branches/add-join-step.png)

1. Arama kutusuna bulun ve dalları birleştiren bir adım olarak gerçekleştirmek istediğiniz eylemi seçin.

   ![Bulma ve paralel dallarından birleştiren bir eylem seçin](media/logic-apps-control-flow-branches/join-steps.png)

   Paralel Dallarınızı şu anda birleştirildi.

   ![Birleştirilmiş dallar](media/logic-apps-control-flow-branches/joined-branches.png)

<a name="join-json"></a>

## <a name="join-definition-json"></a>Tanımı (JSON) katılın

Kod Görünümü'nde çalışıyorsanız, birleşim yapısı mantıksal uygulamanızın JSON tanımında bunun yerine, örneğin tanımlayabilirsiniz:

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
* Gönderin veya özellikleri ve önerileri oylamak için şurayı ziyaret edin [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere (switch deyimleri) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve yineleme adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Gruplandırılmış eylem durumu (kapsamları) temelinde adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
