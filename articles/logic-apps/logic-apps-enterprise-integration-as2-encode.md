---
title: AS2 iletileri - Azure mantıksal uygulamaları kodla | Microsoft Docs
description: AS2 Kodlayıcı Kurumsal tümleştirme paketinde Azure Logic Apps için kullanma
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: jeconnoc
editor: ''
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: fe8a2b00f15fa737c8ed343a47e1cab1c260346b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297939"
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Azure Logic Apps ile Kurumsal tümleştirme paketi için AS2 iletileri kodlama

Güvenlik ve güvenilirlik aktaran iletileri sırasında kurmak için kodlama AS2 ileti Bağlayıcısı'nı kullanın. Bu bağlayıcı, dijital imza, şifreleme ve onayları aracılığıyla ileti Disposition bildirimler (hangi ayrıca inkar için desteklemek üzere müşteri adayları MDN), sağlar.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili. Kodlama AS2 ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabınızın olması gerekir.
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış
* Bir [AS2 sözleşmesi](logic-apps-enterprise-integration-as2.md) tümleştirme hesabınızda tanımlanan zaten

## <a name="encode-as2-messages"></a>AS2 iletileri kodlama

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. Bir istek tetikleyici gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz kodlamak AS2 ileti bağlayıcı tetikleyiciler, sahip değil. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem mantıksal uygulamanızı ekleyin.

3.  Arama kutusuna "AS2" filtreniz için girin. Seçin **AS2 - kodlamak AS2 ileti**.
   
    !["AS2" için arama](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Tümleştirme hesabınıza daha önce herhangi bir bağlantısı oluşturmadıysanız, bu bağlantı artık oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabı seçin. 
   
    ![Tümleştirme hesabı bağlantısı oluşturma](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Bir yıldız işareti özelliklerle gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabını * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabı ve mantığı uygulamanız aynı Azure konumuna olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı bilgilerinizi bu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için tercih **oluşturma**.
   
    ![Tümleştirme bağlantı ayrıntıları](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Bu örnekte gösterildiği gibi bağlantınızı oluşturulduktan sonra ayrıntılarını sağlamak **AS2-gelen**, **AS2-tanımlayıcıları** sözleşmenizde, yapılandırılan ve **gövde**, olduğu ileti yükü.
   
    ![zorunlu alanlar sağlayın](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>AS2 Kodlayıcı ayrıntıları

Kodlama AS2 Bağlayıcısı'nı bu görevleri gerçekleştirir: 

* AS2/HTTP üstbilgileri uygular
* İletiler (yapılandırılmışsa) giden işaretleri
* Giden iletileri şifreler (yapılandırıldıysa)
* İletisi (yapılandırılmışsa) sıkıştırır

## <a name="try-this-sample"></a>Bu örnek deneyin

Bir tam olarak işlevsel mantığı uygulamasını ve örnek AS2 senaryoyu dağıtmaya denemek için bkz: [AS2 mantıksal uygulama şablonu ve senaryo](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/as2/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

