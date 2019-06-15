---
title: Koşullu ifadeleri iş akışı - Azure Logic Apps ekleyin | Microsoft Docs
description: Azure Logic Apps iş akışlarında eylemleri denetleyen bir koşul oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: 9ee484971e217b0ca4dd7ad855e9e6dc3313e5d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60684821"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Azure Logic Apps iş akışı eylemleri denetleyen koşullu deyimler oluşturma

Yalnızca belirli bir koşul denetimini geçtikten sonra mantıksal uygulamanızda belirli eylemleri çalıştırmak için ekleme bir *koşullu ifade*. Bu denetim yapısı, verileri iş belirli değerleri veya alanları karşı karşılaştırır. Ardından, verileri koşulu olup olmadığını sağlaması temelinde çalıştırılacak farklı eylemler de belirtebilirsiniz. Koşulların birbiriyle içinde iç içe yerleştirebilirsiniz.

Örneğin, bir Web sitesinin RSS akışına yeni öğeler göründüğünde çok fazla e-posta gönderen bir mantıksal uygulama olduğunu varsayalım. Yeni öğe belirli bir dize içeriyorsa, e-posta göndermek için bir koşullu ifade ekleyebilirsiniz. 

> [!TIP]
> Farklı belirli değerlere dayalı olarak farklı adımlar çalıştırmak için kullandığınız bir [ *geçiş deyimi* ](../logic-apps/logic-apps-control-flow-switch-statement.md) yerine.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Bu makaledeki örnek [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ile bir Outlook.com veya Office 365 Outlook hesabı.

## <a name="add-condition"></a>Koşul Ekle

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. İstediğiniz konumunda bir koşul ekleyin. 

   Adımlar arasında bir koşul eklemek için işaretçiyi koşulu eklemek istediğiniz okun üzerine getirin. Seçin **artı** ( **+** ) görünür, ardından **Eylem Ekle**. Örneğin:

   ![Adımları arasındaki Eylem Ekle](./media/logic-apps-control-flow-conditional-statement/add-action.png)

   Mantıksal uygulamanızı sayfanın alt kısmında akışınız sonunda bir koşul eklemek istediğinizde seçin **yeni adım** > **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "koşul" girin. Şu eylemi seçin: **Koşul - denetim**

   ![Koşul Ekle](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

1. İçinde **koşul** kutusunda, koşul oluşturun. 

   1. Sol kutusuna, veri veya karşılaştırmak istediğiniz alanı belirtin.

      Sol kutusunun içine tıkladığınızda, önceki adımlardan mantıksal uygulamanızda çıkışları seçebilmeniz için dinamik içerik listesi görüntülenir. 
      Bu örnekte, RSS akışı Özet'i seçin.

      ![Koşulunuzu oluşturun](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   1. Orta kutusuna işlemi gerçekleştirmek için seçin. 
   Bu örnekte, seçin "**içeren**". 

   1. Sağdaki kutuya bir değer ya da alan ölçüt olarak belirtin. 
   Bu örnek için şu dizeyi belirtin: **Microsoft**

   Tamamlanan koşul şu şekildedir:

   ![Tamamlanan koşul](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

   Koşulunuzu için başka bir satır eklemek için **Ekle** > **Satır Ekle**. 
   Subconditions bir grup eklemek için **Ekle** > **Grup Ekle**. 
   Varolan satırları gruplandırmak için bu satır için onay kutularını seçin, herhangi bir satır için üç nokta (...) düğmesini seçin ve ardından **yapma grubu**.

1. Altında **doğruysa** ve **false ise**, gerçekleştirme adımları bağlı koşul karşılanır ' ı ekleyin. Örneğin:

   ![İle koşul "doğru" ve "false ise" yolları](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Var olan eylemlere sürükleyebilirsiniz **doğruysa** ve **false ise** yolları.

1. Mantıksal uygulamanızı kaydedin.

RSS akışında yeni öğeler, koşulu karşıladığında bu mantıksal uygulama şimdi e-posta gönderir.

## <a name="json-definition"></a>JSON tanımı

Bir koşullu ifade arkasında üst düzey kod tanımı aşağıda verilmiştir:

``` json
"actions": {
  "Condition": {
    "type": "If",
    "actions": {
      "Send_an_email": {
        "inputs": {},
        "runAfter": {}
    },
    "expression": {
      "and": [ 
        { 
          "contains": [ 
            "@triggerBody()?['summary']", 
            "Microsoft"
          ]
        } 
      ]
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderin veya özellikleri ve önerileri oylamak için şurayı ziyaret edin [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Farklı değerlere (switch deyimleri) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve yineleme adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırın veya paralel adımları (dallar) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsamları) temelinde adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
