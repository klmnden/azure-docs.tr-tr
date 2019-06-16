---
title: Switch deyimleri ekleme iş akışı - Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps belirli değerlere göre iş akışı eylemleri denetleyen switch deyimleri oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 2a3f8ee5cba3110d392555fad78c1cb2513b5d4e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60683177"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Azure Logic Apps belirli değerlere göre iş akışının eylemlerini çalıştıran switch deyimleri oluşturma

Nesneleri, ifadeler veya belirteçleri değerlerine göre özel eylemler çalıştıracak şekilde ekleme bir *geçiş* deyimi. Bu yapı nesne, ifade ya da belirtecinde değerlendirir, sonuç eşleşir ve bu durum için yalnızca belirli eylemleri çalıştıran durum seçer. Switch deyimi çalıştığında, yalnızca bir örnek sonucu eşleşmesi gerekir.

Örneğin, e-posta ile bir seçeneğe bağlı olarak farklı adımlar alan bir mantıksal uygulama istediğinizi varsayalım. Bu örnekte, mantıksal uygulama için yeni içerik akışı bir Web sitesinin RSS denetler. RSS akışında yeni bir öğe göründüğünde mantıksal uygulama bir onaylayana e-posta gönderir. Mantıksal uygulama, onaylayan "Onayla" veya "Reddet" seçer bağlı olarak, farklı adımları izler.

> [!TIP]
> Tüm programlama dilleri gibi switch deyimleri yalnızca eşitlik işleçleri destekler. "Büyüktür", gibi diğer operatörler ihtiyacınız varsa bir [koşullu ifade](../logic-apps/logic-apps-control-flow-conditional-statement.md).
> Çalışmaları belirleyici yürütme davranış sağlamak için dinamik belirteçleri veya ifadeler yerine benzersiz ve statik bir değer içermesi gerekir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bu makaledeki örnek [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ile bir Outlook.com veya Office 365 Outlook hesabı.

  1. E-posta göndermek için eylem ekleme bulun ve bunun yerine şu eylemi seçin: **Bir onay e-posta Gönder**

     !["Onay e-posta Gönder" öğesini seçin](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  1. Onay e-postası alır kişiye e-posta adresi gibi gerekli alanları belirtin. 
  Altında **kullanıcı seçenekleri**, girin "Onayla, reddet".

     ![E-posta ayrıntılarını girin](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-switch-statement"></a>Switch deyimi ekleyin

1. Bu örnekte, bir switch ifadesi, örnek iş akışı sonuna ekleyin. Son adımdan sonra seçin **yeni adım**.

   Switch deyimi adımlar arasında eklemek istediğinizde, işaretçiyi switch ifadesi eklemek istediğiniz okun üzerine getirin. Seçin **artı** ( **+** ) görünür, ardından **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "geçiş" girin. Şu eylemi seçin: **Switch - denetim**

   ![Anahtar Ekle](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Switch deyimi bir örneği ve bir varsayılan örneği ile görünür. 
   Varsayılan olarak, en az bir durumu ve varsayılan durumda bir switch ifadesi gerektirir. 

   ![Varsayılan boş switch deyimi](./media/logic-apps-control-flow-switch-statement/empty-switch.png)

1. İçine tıklayın **üzerinde** dinamik içerik listesinde görünmesi kutusu. Bu listeden **SelectedOption** alan çıktısı gerçekleştirilecek eylemi belirler. 

   !["SelectedOption" seçin](./media/logic-apps-control-flow-switch-statement/select-selected-option.png)

1. Burada onaylayan seçer durumlarla için `Approve` veya `Reject`, başka bir örneği arasında ekleme **çalışması** ve **varsayılan**. 

   ![Başka bir servis talebi Ekle](./media/logic-apps-control-flow-switch-statement/switch-plus.png)

1. Bu eylemler için karşılık gelen durumlar ekleyin:

   | Durum # | **SelectedOption** | Eylem |
   |--------|--------------------|--------|
   | 1\. durum | **Onayla** | Outlook ekleme **bir e-posta** onaylayan seçildiğinde, RSS öğeyle ilgili ayrıntıların göndermek için eylem **Onayla**. |
   | 2\. durum | **Reddet** | Outlook ekleme **bir e-posta** diğer onaylayanlar RSS öğesinin reddedildiğini bildiren için eylem. |
   | Varsayılan | None | Hiçbir eylem gerekmiyor. Bu örnekte, **varsayılan** durumda boş olduğundan **SelectedOption** yalnızca iki seçenek vardır. |
   |||

   ![Tamamlanmış switch deyimi](./media/logic-apps-control-flow-switch-statement/finished-switch.png)

1. Mantıksal uygulamanızı kaydedin. 

   Bu örnekte el ile test edin, tercih **çalıştırma** kadar mantıksal uygulama, yeni RSS öğesi bulur ve bir onay e-posta gönderir. 
   Seçin **Onayla** sonuçlarını gözlemleyin.

## <a name="json-definition"></a>JSON tanımı

Switch deyimi kullanarak bir mantıksal uygulama oluşturdunuz, üst düzey kod tanımı switch deyimi arkasında göz atalım.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {}
         },
         "case" : "Approve"
      },
      "Case_2": {
         "actions": {
           "Send_an_email_2": {}
         },
         "case": "Reject"
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

| Etiket | Açıklama |
|-------|-------------|
| `"Switch"`         | Okunabilirlik için yeniden adlandırabilirsiniz switch ifadesi adı |
| `"type": "Switch"` | Eylem switch deyimi olduğunu belirtir |
| `"expression"`     | Bu örnekte, her durumda tanımı içinde bildirilen karşı değerlendirilir onayı veren kişinin belirlenen seçenek belirtir |
| `"cases"` | Herhangi bir servis talebi sayısını tanımlar. Her durum için `"Case_*"` okunabilirlik için yeniden adlandırabilirsiniz bu durumda, varsayılan adı |
| `"case"` | Switch deyimi için karşılaştırma kullanır. sabit ve benzersiz bir değer olmalıdır bir durumun değer belirtir. Hiçbir örnek anahtar ifadesi sonucu, Eylemler eşleşiyorsa `"default"` bölüm çalıştırılır. | 
| | | 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özellikleri veya öneri oylamak veya göndermek için şurayı ziyaret edin [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Çalıştırma ve yineleme adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırın veya paralel adımları (dallar) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsamları) temelinde adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
