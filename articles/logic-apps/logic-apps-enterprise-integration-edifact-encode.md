---
title: "EDIFACT iletileri - Azure mantıksal uygulamaları kodla | Microsoft Docs"
description: "EDI doğrulamak ve XML Azure Logic Apps için EDIFACT ileti Kodlayıcı Kurumsal tümleştirme paketi oluştur"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b8d577326d23ec45cb4a9ec0e450ebf7afd945f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Azure Logic Apps ile Kurumsal tümleştirme paketi için EDIFACT iletileri kodlama

Kodlama EDIFACT ileti bağlayıcısıyla EDI ve iş ortağı özgü özellikler doğrulamak, her işlem kümesi için bir XML belgesi oluşturmak ve teknik bir bildirim, işlevsel bildirim ya da her ikisini de isteyin.
Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızı varolan bir tetikleyicinin eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili. Kodlama EDIFACT ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabınızın olması gerekir. 
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış
* Bir [EDIFACT sözleşmesi](logic-apps-enterprise-integration-edifact.md) tümleştirme hesabınızda tanımlanan zaten

## <a name="encode-edifact-messages"></a>EDIFACT iletileri kodlama

1. [Mantıksal uygulama oluşturma](logic-apps-create-a-logic-app.md).

2. Bir istek tetikleyici gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz kodlamak EDIFACT ileti bağlayıcı tetikleyiciler, sahip değil. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem mantıksal uygulamanızı ekleyin.

3.  Arama kutusuna "EDIFACT", filtre olarak girin. Şunlardan birini seçin **EDIFACT iletiyle kodlamak anlaşma adı** veya **EDIFACT iletisi kimlikleri tarafından kodla**.
   
    ![EDIFACT arama](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Tümleştirme hesabınıza daha önce herhangi bir bağlantısı oluşturmadıysanız, bu bağlantı artık oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabı seçin.

    ![Tümleştirme hesap bağlantısı oluşturma](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Bir yıldız işareti özelliklerle gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabını * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabı ve mantığı uygulamanız aynı Azure konumuna olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı bilgilerinizi bu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için tercih **oluşturma**.

    ![Tümleştirme hesap bağlantı ayrıntıları](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    Bağlantınızı şimdi oluşturulur.

    ![oluşturulan tümleştirme hesap bağlantı](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>Anlaşma adı tarafından EDIFACT ileti kodlama

Anlaşma adı tarafından EDIFACT iletileri kodlamak seçerseniz, açık **adı, EDIFACT sözleşmesi** listesinde, girin veya EDIFACT sözleşmesi adınızı seçin. Kodlanacak XML ileti girin.

![EDIFACT sözleşmesi adını ve kodlamak için XML iletisini girin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>EDIFACT iletisi kimlikleri tarafından kodlama

EDIFACT iletileri tarafından kimlikleri kodlamak seçerseniz, gönderen tanımlayıcısı, gönderen Niteleyici, alıcı tanımlayıcısı ve EDIFACT sözleşmenizde yapılandırılan alıcı niteleyici girin. Kodlanacak XML iletiyi seçin.

![Göndereni ve alıcısı için kimlikleri sağlamak için kodlamak için XML ileti seçin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>EDIFACT kodlamak ayrıntıları

Kodlama EDIFACT connector, şu görevleri gerçekleştirir: 

* Gönderen niteleyicisi & tanımlayıcısı ve alıcı niteleyicisi ve tanımlayıcısı eşleştirerek anlaşmayı çözümleyin
* EDI işlem değişim kümelerinde XML olarak kodlanmış iletileri dönüştürme EDI değişim serileştirir.
* İşlem kümesi üstbilgi ve toplamı kesimleri uygular
* Bir değişim denetim sayısı, Grup denetim numarası ve her giden değişim için bir işlem kümesi denetim sayı oluşturur
* Ayırıcılar yükü veri değiştirir
* EDI ve iş ortağı özgü özellikleri doğrular
  * Şema doğrulama ileti şemayla işlem kümesi veri öğelerinin.
  * EDI, üzerinde işlem kümesi veri öğeleri doğrulamanın.
  * İşlem kümesi veri öğeleri üzerinde Genişletilmiş doğrulamanın
* Her işlem kümesi için bir XML belgesi oluşturur.
* Teknik ve/veya işlevsel bir bildirim (yapılandırılmışsa) ister.
  * Teknik bir bildirim CONTRL ileti bir değişim alınmasını gösterir.
  * İşlevsel bir bildirim CONTRL ileti, kabul veya alınan Değişim, Grup veya ileti reddine hataları veya desteklenmeyen işlevlerin listesini gösterir.

## <a name="view-swagger-file"></a>Görünüm Swagger dosyası
EDIFACT bağlayıcı Swagger ayrıntılarını görüntülemek için bkz: [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

