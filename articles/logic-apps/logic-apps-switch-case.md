---
title: "Azure Logic Apps içinde deyimi farklı eylemler için geçiş | Microsoft Docs"
description: "Switch deyimi kullanarak ifade değerlerine göre logic apps gerçekleştirmek için farklı eylemleri seçin"
services: logic-apps
keywords: Switch deyimi
author: ecfan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; estfan
ms.openlocfilehash: 8f11d18009d60ea5c74781ccef2ff7d811516750
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Logic apps ile switch deyimi içinde farklı eylemler gerçekleştirme

Bir iş akışı yazma, genellikle nesne veya ifade değere göre farklı eylemleri sahiptir. Örneğin, farklı bir HTTP isteğinin ya da bir e-posta içinde seçili bir seçenek durum kodunu göre davranmasına mantıksal uygulamanızı isteyebilirsiniz.

Bu senaryolar uygulamak için bir anahtar ifadesi kullanabilirsiniz. Mantıksal uygulamanızı bir belirteç veya ifade değerlendirin ve belirtilen eylemleri yürütmek için aynı değere sahip bir servis talebi seçin. Switch deyimi yalnızca bir örnek eşleşmelidir.

> [!TIP]
> Tüm programlama dilleri gibi switch deyimleri yalnızca eşitlik işleçleri destekler. Diğer ilişkisel işleçleri gerekirse "büyüktür gibi", bir koşul deyimi kullanın.
> Belirleyici yürütme davranışı sağlamak için durumlarda dinamik belirteçleri veya ifade yerine benzersiz ve statik bir değer içermesi gerekir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir Azure aboneliği. Etkin bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/), veya deneyin [Logic Apps için ücretsiz](https://tryappservice.azure.com/).
- [Mantıksal uygulamalar hakkındaki temel bilgileri](logic-apps-overview.md)

## <a name="add-a-switch-statement-to-your-workflow"></a>Switch deyimi akışınıza ekleme

Switch deyimi nasıl çalıştığını göstermek için bu örnek için Dropbox karşıya yüklenen dosyaların izler bir mantıksal uygulama oluşturur. Yeni dosyalar yüklenirken mantıksal uygulama aktarılmayacağını SharePoint'e bu dosyaları seçen onaylayıcı e-posta gönderir. Uygulama onaylayan seçtiği değere göre farklı eylemler gerçekleştiren bir anahtar deyimi kullanır.

1. Bir mantıksal uygulama oluşturun ve bu Tetikleyici seçin: **bir dosya oluşturulduğunda, Dropbox -**.

   ![Bir dosya tetikleyici oluşturulduğunda, Dropbox - kullanın](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. Bu eylem tetikleyici altında ekleyin: **Outlook.com - onay e-postası gönderme**

   > [!TIP]
   > Logic apps ayrıca bir Office 365 Outlook hesaptan gönderme onay e-posta senaryoları destekler.

   - Varolan bir bağlantıyı sahip değilseniz, birini oluşturmanız istenir.
   - Gerekli alanları doldurun. Örneğin, altında **için**, onaylayan e-posta göndermek için e-posta adresi belirtin.
   - Altında **kullanıcı seçenekleri**, girin `Approve, Reject`.

   ![Bağlantıyı yapılandır](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Switch deyimi ekleyin.

   - Seçin **+ yeni adım** > **... Daha fazla** > **anahtar durumu ekleme**. 
   - Gerçekleştirilecek eylemi seçin istiyoruz artık temel `SelectedOptions` çıktısı *onay e-posta Gönder* eylem. 
   Bu alanda bulabileceğiniz **dinamik içerik eklemek** Seçici.
   - Kullanım *durum 1* onaylayan seçtiğinde işlemek için `Approve`.
     - SharePoint Online ile onaylanırsa, özgün dosya kopyalama [ **SharePoint Online - dosyası oluşturma** eylem](../connectors/connectors-create-api-sharepointonline.md).
     - Yeni bir dosya SharePoint üzerinde kullanılabilir olduğunu kullanıcılara bildirmek için durum içindeki başka bir eylem ekleyin.
   - Kullanıcı seçtiğinde işlemek için başka bir örneği eklemek `Reject`.
     - Reddedilirse, diğer onaylayanlar dosya reddedilir ve başka bir eylem gerekli değildir bildiren bir bildirim e-postası gönderin.
   - `SelectedOptions`Biz bırakabilirsiniz, bu nedenle yalnızca iki seçenek sunar **varsayılan** durum boş.

   ![Switch deyimi](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > Switch deyimi varsayılan durumda yanı sıra en az bir örnek gerekiyor.

4. Switch deyimi sonra bu eylem ekleyerek Dropbox'a karşıya özgün dosyayı silmektir: **Dropbox - dosya silinemiyor**

5. Mantıksal uygulamanızı kaydedin. Dropbox için bir dosyayı karşıya yükleyerek uygulamanızı test edin. Kısa süre içinde bir onay e-posta alacaksınız. Bir seçenek belirleyin ve davranışı uyun.

   > [!TIP]
   > Nasıl yapılır kullanıma [mantıksal uygulamalarınızı izleme](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-the-code-behind-switch-statements"></a>Switch deyimleri arka plan kodu anlama

Switch deyimi kullanarak bir mantıksal uygulama başarıyla oluşturuldu, switch deyimi arkasındaki kod tanımı bakalım.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
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

* `"Switch"`Okunabilirlik için yeniden adlandırabilirsiniz switch deyimi adıdır. 
* `"type": "Switch"`Eylem switch deyimi olduğunu gösterir. 
* `"expression"`Bu örnekte onaylayanın seçili seçenektir ve tanımı içinde bildirilen her durumda karşı değerlendirilir. 
* `"cases"`herhangi bir sayıda durumları içerebilir. Her bir olay `"Case *"` okunabilirlik için yeniden adlandırabilirsiniz durumunun varsayılan addır. 
`"case"`anahtar ifadesi karşılaştırma için kullanır, servis talebi etiketini belirtir ve sabit ve benzersiz bir değer olmalıdır. Örneklerin hiçbiri anahtar ifadesi, Eylemler altında eşleşiyorsa `"default"` yürütülür.

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını görmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Azure Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [koşulları ekleme](logic-apps-use-logic-app-features.md)
- Hakkında bilgi edinin [hata ve özel durum işleme](logic-apps-exception-handling.md)
- Daha fazla araştırmak [iş akışı dil özellikleri](logic-apps-author-definitions.md)