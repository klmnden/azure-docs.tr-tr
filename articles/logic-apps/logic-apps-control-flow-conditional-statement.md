---
title: "Koşullu deyimler - çalışma adımları bir koşula göre - Azure Logic Apps | Microsoft Docs"
description: "Adımlar, yalnızca bir koşul yerine getirdikten sonra mantıksal uygulamanızı çalıştırın. Belirtilen koşullara göre iş akışlarını çalıştırmak karar ağaçları oluşturun."
services: logic-apps
keywords: "Koşullu deyimler, karar ağaçları"
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 486c1053f42ed3becc2c4b60accc993db7f24baa
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="conditional-statements-run-steps-based-on-a-condition-in-logic-apps"></a>Koşullu deyimler: logic apps içinde bir koşula göre adımları çalıştırın

Yalnızca belirtilen bir koşul geçtikten sonra adımları gerçekleştirmek için bir *koşullu ifade*. Bu yapı, veri belirli değerleri veya alanlar karşı iş akışınızda karşılaştırır. Daha sonra verileri koşulu olup olmadığına karşılayan tabanlı çalıştırmak için farklı adımlar tanımlayabilirsiniz. Koşullar birbirine içinde yerleştirebilirsiniz.

Örneğin, yeni öğeler üzerinde bir Web sitesinin RSS akışı göründüğünde, çok fazla e-posta gönderen bir mantıksal uygulama olduğunu varsayalım. Yalnızca yeni öğe belirli bir dizeyi içeriyorsa e-posta göndermek için bir koşullu ifade ekleyebilirsiniz. 

> [!TIP]
> Farklı belirli değerlerine göre farklı adımlar çalıştırmak için kullandığınız bir [ *geçiş deyimi* ](../logic-apps/logic-apps-control-flow-switch-statement.md) yerine.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Bu makaledeki örnek izlemek için [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir Outlook.com veya Office 365 Outlook hesapla.

## <a name="add-a-condition"></a>Koşul Ekle

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portal</a>, mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

2. Bir koşul istediğiniz konuma ekleyin. 

   Adımlar arasındaki bir koşul eklemek için üzerinde oku koşulu eklemek istediğiniz işaretçiyi. Seçin **artı** (**+**) görünür, ardından **bir koşul eklemek**. Örneğin:

   ![Adımlar arasındaki koşul Ekle](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Mantıksal uygulamanızı sonundaki akışınıza sonuna bir koşul eklemek istediğinizde belirleyin **+ yeni adım** > **bir koşul eklemek**.

3. Altında **koşulu**, koşulunuz oluşturun. 

   1. Sol kutusunda veri veya karşılaştırmak istediğiniz alanı belirtin.

      Gelen **dinamik içerik eklemek** listesinde, var olan mantıksal uygulamanızı alanlardan seçebilirsiniz.

   2. Orta listesinde, işlemi gerçekleştirmek için seçin. 
   3. Sağ kutusunda bir değer veya alan ölçüt olarak belirtin.

   Örneğin:

   ![Koşul temel modunda düzenleme](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode.png)

   Tam koşul şöyledir:

   ![Tam koşul](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Daha gelişmiş bir koşul oluşturmak veya ifadeleri kullanmayı seçin **Gelişmiş modda Düzenle**. İfadeleri tarafından tanımlanan kullanabilirsiniz [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md).
   > 
   > Örneğin:
   >
   > ![Kod koşulunda Düzenle](./media/logic-apps-control-flow-conditional-statement/edit-condition-advanced-mode.png)

5. Altında **Evet IF** ve **IF Hayır**, gerçekleştirme adımları temel koşul karşılanır karşılanmaz ekleyebilirsiniz. Örneğin:

   ![Evet ve hiçbir yol koşulu](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Varolan eylemlere sürükleyebilirsiniz **Evet IF** ve **IF Hayır** yollar.

6. Mantıksal uygulamanızı kaydedin.

Şimdi bu mantıksal uygulama, yalnızca RSS akışı yeni öğeleri koşulunuz karşıladığında posta gönderir.

## <a name="json-definition"></a>JSON tanımı

Bir koşullu ifade kullanarak bir mantıksal uygulama oluşturduğunuza göre koşullu ifade arkasında üst düzey kod tanımı bakalım.

``` json
"actions": {
  "myConditionName": {
    "type": "If",
    "expression": "@contains(triggerBody()?['summary'], 'Microsoft')",
    "actions": {
      "Send_an_email": {
        "inputs": { },
        "runAfter": {}
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderme veya özellikleri ve öneriler oylamak için ziyaret [Azure Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Farklı değerlerini (switch deyimleri) temel alan adımları çalıştırın](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve (döngüler) arasındaki adımları yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırmak veya paralel adımları (dal) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsam) temelinde adımları çalıştırın](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)