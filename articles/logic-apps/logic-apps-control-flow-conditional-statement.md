---
title: Koşullu deyimler iş akışları - Azure mantıksal uygulamaları ekleme | Microsoft Docs
description: Azure mantıksal uygulamaları'nda iş akışları Eylemler denetim koşul oluşturma
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: e8d84944d44588602593c762c4f60c375e480343
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298177"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Azure mantıksal uygulamaları iş akışı eylemlerini denetlemek koşullu deyimler oluşturma

Yalnızca belirtilen bir koşul geçtikten sonra mantıksal uygulamanızı belirli eylemleri çalıştırmak için ekleyin bir *koşullu ifade*. Bu yapı verileri, iş akışındaki belirli değerleri veya alanlar karşı karşılaştırır. Daha sonra verileri koşulu olup olmadığına karşılayan dayalı olarak çalışan farklı eylemler tanımlayabilirsiniz. Koşullar birbirine içinde yerleştirebilirsiniz.

Örneğin, yeni öğeler üzerinde bir Web sitesinin RSS akışı göründüğünde, çok fazla e-posta gönderen bir mantıksal uygulama olduğunu varsayalım. Yalnızca yeni öğe belirli bir dizeyi içeriyorsa e-posta göndermek için bir koşullu ifade ekleyebilirsiniz. 

> [!TIP]
> Farklı belirli değerlerine göre farklı adımlar çalıştırmak için kullandığınız bir [ *geçiş deyimi* ](../logic-apps/logic-apps-control-flow-switch-statement.md) yerine.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Bu makaledeki örnek izlemek için [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir Outlook.com veya Office 365 Outlook hesapla.

## <a name="add-a-condition"></a>Koşul ekle

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

   ![Tamamlanan koşul](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

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
