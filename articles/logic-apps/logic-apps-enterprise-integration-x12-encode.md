---
title: X12 kodlamak iletileri - Azure Logic Apps | Microsoft Docs
description: "EDI doğrulamak ve XML ile kodlanmış Dönüştür X12 iletilerle ileti Kodlayıcısı Kurumsal tümleştirme paketi ile Azure mantıksal uygulamaları için"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: f7408f240a1b05e0d53716764a9f8d1e19229ebe
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>X12 kodlamak Azure Logic Apps Enterprise tümleştirme paketi ile iletileri

Kodla X12 ileti bağlayıcısıyla EDI ve iş ortağı özgü özellikler doğrulamak, XML olarak kodlanmış iletileri değiş tokuş EDI işlem kümelerinde dönüştürmek ve teknik bir bildirim, işlevsel bildirim ya da her ikisini de isteyin.
Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızı varolan bir tetikleyicinin eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili. Kodla X12 ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabınızın olması gerekir.
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış
* Bir [X12 sözleşmesi](logic-apps-enterprise-integration-x12.md) tümleştirme hesabınızda tanımlanan zaten

## <a name="encode-x12-messages"></a>X12 kodlamak iletileri

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. Bir istek tetikleyici gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz kodla X12 ileti bağlayıcı tetikleyiciler, sahip değil. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem mantıksal uygulamanızı ekleyin.

3.  Arama kutusuna "x12" filtreniz için girin. Şunlardan birini seçin **X12-kodlamak için X12 ileti anlaşma adı tarafından** veya **X12-kodlamak için X12 kimlikleri ileti**.
   
    !["X12" için arama](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Tümleştirme hesabınıza daha önce herhangi bir bağlantısı oluşturmadıysanız, bu bağlantı artık oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabı seçin. 
   
    ![Tümleştirme hesap bağlantı](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Bir yıldız işareti özelliklerle gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabını * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabı ve mantığı uygulamanız aynı Azure konumuna olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı bilgilerinizi bu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için tercih **oluşturma**.

    ![oluşturulan tümleştirme hesap bağlantı](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    Bağlantınızı şimdi oluşturulur.

    ![Tümleştirme hesap bağlantı ayrıntıları](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>X12 kodlamak iletileri göre anlaşma adı

X12 kodlamak seçerseniz iletileri anlaşma adı ile açmak **X12 adını anlaşma** listesinde, girin veya mevcut X12 seçin anlaşma. Kodlanacak XML ileti girin.

![X12 girin anlaşma adı ve kodlamak için XML iletisi](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>X12 kodlamak kimlikleri iletileri

X12 kodlamak seçerseniz iletileri kimlikleri göre gönderen tanımlayıcısı, gönderen Niteleyici, alıcı tanımlayıcısı ve alıcı Niteleyici, X12 yapılandırıldığı gibi girin anlaşma. Kodlanacak XML iletiyi seçin.
   
![Göndereni ve alıcısı için kimlikleri sağlamak için kodlamak için XML ileti seçin](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 kodlamak ayrıntıları

X12 kodla bağlayıcı bu görevleri gerçekleştirir:

* Eşleşen göndereni ve alıcısı bağlam özellikleri tarafından sözleşmesi çözünürlüğü.
* EDI işlem değişim kümelerinde XML olarak kodlanmış iletileri dönüştürme EDI değişim serileştirir.
* İşlem kümesi üstbilgi ve toplamı kesimleri uygular
* Bir değişim denetim sayısı, Grup denetim numarası ve her giden değişim için bir işlem kümesi denetim sayı oluşturur
* Ayırıcılar yükü veri değiştirir
* EDI ve iş ortağı özgü özellikleri doğrular
  * Şema doğrulama ileti şema karşı işlem kümesi veri öğelerinin
  * EDI, üzerinde işlem kümesi veri öğeleri doğrulamanın.
  * İşlem kümesi veri öğeleri üzerinde Genişletilmiş doğrulamanın
* Teknik ve/veya işlevsel bir bildirim (yapılandırılmışsa) ister.
  * Teknik bir bildirim başlığı doğrulama sonucu olarak oluşturur. Teknik Bildirim bir değişim üstbilgisi ve adresi alıcı tarafından toplamı işlenmesini durumunu raporlar
  * İşlev bildirim gövde doğrulama sonucu olarak oluşturur. İşlev bildirim alınan belge işlerken bir hatayla karşılaştı her hata raporları

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/x12/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

