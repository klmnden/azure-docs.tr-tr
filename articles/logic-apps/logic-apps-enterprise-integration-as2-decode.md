---
title: "AS2 iletileri - Azure Logic Apps kod çözme | Microsoft Docs"
description: "Azure mantıksal uygulamaları için Kurumsal tümleştirme paketinde AS2 kod çözücü kullanma"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: a7920b2509fe368c6f7d55e17fe0bf0020c4562c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>AS2 iletileri Kurumsal tümleştirme paketi ile Azure mantıksal uygulamaları için kod çözme 

Güvenlik ve güvenilirlik aktaran iletileri sırasında kurmak için kod çözme AS2 ileti Bağlayıcısı'nı kullanın. Bu bağlayıcı, dijital imza, şifre çözme ve onayları ileti Disposition bildirimler (MDN) aracılığıyla sağlar.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili. Kod çözme AS2 ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabınızın olması gerekir.
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış
* Bir [AS2 sözleşmesi](logic-apps-enterprise-integration-as2.md) tümleştirme hesabınızda tanımlanan zaten

## <a name="decode-as2-messages"></a>AS2 iletileri kod çözme

1. [Mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

2. Bir istek tetikleyici gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz kod çözme AS2 ileti bağlayıcı tetikleyiciler, sahip değil. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem mantıksal uygulamanızı ekleyin.

3.  Arama kutusuna "AS2" filtreniz için girin. Seçin **AS2 - kod çözme AS2 ileti**.
   
    !["AS2" için arama](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Tümleştirme hesabınıza daha önce herhangi bir bağlantısı oluşturmadıysanız, bu bağlantı artık oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabı seçin.
   
    ![Tümleştirme bağlantısı oluşturma](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Bir yıldız işareti özelliklerle gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabını * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabı ve mantığı uygulamanız aynı Azure konumuna olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı bilgilerinizi bu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için tercih **oluşturma**.

    ![Tümleştirme bağlantı ayrıntıları](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. Bu örnekte gösterildiği gibi bağlantınızı oluşturulduktan sonra Seç **gövde** ve **üstbilgileri** isteği çıkışları gelen.
   
    ![oluşturulan tümleştirme bağlantı](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Örneğin:

    ![Gövde ve üstbilgileri isteği çıkışlarından seçin](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>AS2 decoder ayrıntıları

Kod çözme AS2 Bağlayıcısı'nı bu görevleri gerçekleştirir: 

* AS2/HTTP üstbilgileri işler
* (Yapılandırılmışsa) imzayı doğrular
* İletilerin şifresini çözer (yapılandırıldıysa)
* İletisi (yapılandırılmışsa) açar
* Alınan MDN özgün giden iletiyle çözümler
* Güncelleştirmeleri ve takası veritabanındaki kayıtları karşılık gelen
* AS2 durumu raporlama için kayıtları Yazar
* Base64 kodlu çıkış yükü dosyalarla
* Bir MDN gereklidir ve olup MDN zaman uyumlu veya zaman uyumsuz yapılandırmaya AS2 sözleşmesi göre belirler
* (Sözleşmesi yapılandırmalarını temel alan) zaman uyumlu veya zaman uyumsuz bir MDN oluşturur
* Bağıntı belirteçleri ve özellikler üzerinde MDN ayarlar

## <a name="try-this-sample"></a>Bu örnek deneyin

Bir tam olarak işlevsel mantığı uygulamasını ve örnek AS2 senaryoyu dağıtmaya denemek için bkz: [AS2 mantıksal uygulama şablonu ve senaryo](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/as2/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md) 

