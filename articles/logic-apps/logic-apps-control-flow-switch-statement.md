---
title: İş akışları - Azure Logic Apps switch deyimleri ekleme | Microsoft Docs
description: Azure mantıksal uygulamaları belirli değerleri temel iş akışı eylemlerinin denetim switch deyimleri oluşturma
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: e15f89d4b7e33ce7e28676c219344f7d7d9cd465
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299625"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Azure mantıksal uygulamaları belirli değerleri temel iş akışı eylemleri çalıştırmak switch deyimleri oluşturma

Nesne, ifadeler veya belirteçleri değerlerine göre belirli eylemleri çalıştırmak için ekleyin bir *geçiş* deyimi. Bu yapı nesne, ifade ya da belirtecinde değerlendirir, sonuç eşleşen ve yalnızca o çalışması için belirli eylemleri çalıştırır çalışmasını seçer. Switch deyimi çalıştığında, yalnızca bir örnek sonuç eşleşmelidir.

Örneğin, e-posta ile bir seçeneğe bağlı farklı adımlar geçen bir mantıksal uygulama istediğinizi varsayalım. Bu örnekte, mantıksal uygulama bir Web sitesinin RSS için yeni içerik akışı denetler. Yeni bir öğe RSS akışı göründüğünde, mantıksal uygulama bir onaylayacak kişiye e-posta gönderir. Onaylayanın "Onayla" veya "Reddet" seçer bağlı olarak, mantıksal uygulama farklı adımları izler.

> [!TIP]
> Tüm programlama dilleri gibi switch deyimleri yalnızca eşitlik işleçleri destekler. "Büyüktür", gibi diğer operatörler gerekiyorsa kullanın bir [koşullu ifade](#conditions).
> Belirleyici yürütme davranışı sağlamak için durumlarda dinamik belirteçleri veya ifadeleri yerine benzersiz ve statik bir değer içermesi gerekir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bu makaledeki örnek izlemek için [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir Outlook.com veya Office 365 Outlook hesapla.

  1. E-posta göndermek için eylem eklediğinizde seçmeniz **bir onay e-posta Gönder** yerine.

     !["Bir onay e-posta Gönder"'i seçin](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  2. Onay e-posta alır kişiye e-posta adresi gibi gerekli alanları sağlar. 
  Altında **kullanıcı seçenekleri**, girin "Onaylama, reddetme".

     ![E-posta ayrıntılarını girin](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-a-switch-statement"></a>Switch deyimi ekleyin

1. Örnek iş akışı sonunda seçin **+ yeni adım** > **... Daha fazla** > **anahtar durumu ekleme**. 

   ![Switch deyimi ekleyin](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Switch deyimi bir durumda ve bir varsayılan durumu görüntülenir. 
   Switch deyimi en az bir servis talebi artı varsayılan çalışması gerekir. 

   Switch deyimi adımlar arasındaki eklemek istediğinizde, üzerinde oku switch deyimi eklemek istediğiniz işaretçiyi. 
   Seçin **artı** (**+**) görünür, ardından **anahtar durumu ekleme**.

4. İçinde **üzerinde** kutusunda **SelectedOption** alan çıktısı gerçekleştirilecek eylemi belirler. 
   
   Alanından seçebilirsiniz **dinamik içerik eklemek** görünür listesi.

5. Burada onaylayan seçer durumlarında `Approve` veya `Reject`, başka bir örneği arasında eklemek **durum** ve **varsayılan**. 
   
6. Bu eylemler için karşılık gelen durumları ekleyin:

   | Durum # | **SelectedOption** | Eylem |
   |:------ |:-------------------|:------ |
   | Örneği 1 | **Onayla** | Outlook ekleme **bir e-posta Gönder** onaylayan yalnızca seçili olduğunda, RSS öğeyle ilgili ayrıntıları göndermek için eylem **Onayla**. |
   | Durum 2 | **Reddet** | Outlook ekleme **bir e-posta Gönder** RSS öğesi reddedildi diğer onaylayanlar bilgilendirmek için eylem. |
   | Varsayılan | \<Yok\> | Kullanılabilir eylem gerekli. Bu örnekte, **varsayılan** durumdur boş olduğundan **SelectedOption** yalnızca iki seçenek vardır. |
   |         |          |

   ![Switch deyimi](./media/logic-apps-control-flow-switch-statement/switch.png)

7. Mantıksal uygulamanızı kaydedin. 

   Bu örnek el ile test edilmesini seçerseniz **çalıştırmak** kadar mantıksal uygulama yeni bir RSS öğesi bulur ve bir onay e-posta gönderir. 
   Seçin **Onayla** sonuçları gözlemlemek için.

## <a name="json-definition"></a>JSON tanımı

Switch deyimi kullanarak bir mantıksal uygulama oluşturduğunuza göre üst düzey kod tanımı switch deyimi arkasında bakalım.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case" : {
         "actions" : {
           "Send_an_email": { }
         },
         "case" : "Approve"
      },
      "Case_2" : {
         "actions" : {
           "Send_an_email_2": { }
         },
         "case" : "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

| Etiket              | Açıklama |
| :----------------- | :---------- |
| `"Switch"`         | Okunabilirlik için yeniden adlandırabilirsiniz switch deyimi adı |
| `"type": "Switch"` | Switch deyimi eylem belirtir |
| `"expression"`     | Bu örnekte, daha sonra tanımında bildirilen gibi her bir olay karşı değerlendirilen onaylayanın seçeneği belirtir |
| `"cases"` | Herhangi bir sayıda durumları tanımlar. Her bir olay `"Case_*"` okunabilirlik için yeniden adlandırabilirsiniz bu durumda, varsayılan adı. |
| `"case"` | Switch deyimi karşılaştırma için kullandığı sabit ve benzersiz bir değer olmalıdır durumun değerini belirtir. Hiçbir örnek anahtar ifadesi sonucu, Eylemler eşleşiyorsa `"default"` bölüm çalıştırılır.
|           |         |

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderme veya özellikleri veya önerileri oylamak için ziyaret [Azure Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımları çalıştırın](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Çalıştırma ve (döngüler) arasındaki adımları yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırmak veya paralel adımları (dal) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsam) temelinde adımları çalıştırın](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
