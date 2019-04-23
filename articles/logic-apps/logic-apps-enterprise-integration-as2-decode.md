---
title: AS2 iletilerini - Azure Logic Apps kod çözme | Microsoft Docs
description: Azure Logic Apps ve Enterprise Integration Pack ile iletileri olarak kod çözme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jonfan, divswa, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 08/08/2018
ms.openlocfilehash: ca297e1b4a007db3020b4369132b190608484738
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001635"
---
# <a name="decode-as2-messages-with-azure-logic-apps-and-enterprise-integration-pack"></a>AS2 iletilerini Azure Logic Apps ve Enterprise Integration Pack ile kod çözme 

Güvenlik ve güvenilirlik sırasında aktaran iletileri oluşturmak için AS2 kod çözme ileti Bağlayıcısı'nı kullanın. Bu bağlayıcı, dijital imza, şifre çözme ve onayları ileti değerlendirme bildirimleri (MDN) aracılığıyla sağlar.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili. AS2 kod çözme ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabı olması gerekir.
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış
* Bir [AS2 sözleşmesi](logic-apps-enterprise-integration-as2.md) , tümleştirme hesabında zaten tanımlı

## <a name="decode-as2-messages"></a>AS2 iletilerini kodunu çözme

1. [Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. AS2 kod çözme ileti bağlayıcı tetikleyicileri, sahip değil. istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleme ve ardından mantıksal uygulamanız için bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "AS2" girin. Seçin **AS2 - AS2 kod çözme ileti**.
   
    !["AS2" için arama](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Daha önce tümleştirme hesabı için herhangi bir bağlantı oluşturmadıysanız, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabı seçin.
   
    ![Tümleştirme bağlantı oluşturma](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Bir yıldız işareti ile özellikleri gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabı için bir ad girin. Tümleştirme hesabı ve mantıksal uygulamanızı aynı Azure konumda olduklarından emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınızı şu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için seçin **Oluştur**.

    ![Tümleştirme bağlantı ayrıntıları](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. Bu örnekte gösterildiği gibi bağlantınızı oluşturulduktan sonra seçin **gövdesi** ve **üstbilgileri** isteği çıkışları öğesinden.
   
    ![oluşturulan tümleştirme bağlantı](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Örneğin:

    ![Gövde ve üstbilgileri, istek çıkışları seçin](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 


## <a name="as2-decoder-details"></a>AS2 kod çözücü ayrıntıları

AS2 kod çözme bağlayıcı, bu görevleri gerçekleştirir: 

* AS2/HTTP üst bilgilerini işleme
* (Yapılandırılmışsa) imzayı doğrular.
* İletiler (yapılandırılmışsa) şifresini çözer.
* İletisi (yapılandırılmışsa) açar.
* Denetleyin ve (yapılandırılmışsa) ileti kimliği yinelenmesine izin verme
* Alınan bir MDN özgün giden ileti ile mutabık kılar
* Güncelleştirmeleri ve takası veritabanında kayıt ilişkilendirir
* AS2 durum raporlama için kayıtları Yazar
* Çıktı yükü içeriği base64 olarak kodlanmış olan
* Bir MDN gereklidir ve olup MDN zaman uyumlu veya zaman uyumsuz AS2 anlaşması'nda yapılandırmasına göre belirler
* (Sözleşmesi yapılandırmalarına göre) zaman uyumlu veya zaman uyumsuz bir MDN oluşturur
* MDN üzerinde korelasyon belirteçleri ve özelliklerini ayarlar


  > [!NOTE]
  > Sertifika yönetimi için Azure anahtar kasası kullanıyorsanız, izin vermek için anahtarları yapılandırdığınızdan emin olun. **şifresini** işlemi.
  > Aksi takdirde, AS2 kod çözme başarısız olur.
  >
  > ![Keyvault şifresini çözer.](media/logic-apps-enterprise-integration-as2-decode/keyvault1.png)

## <a name="try-this-sample"></a>Bu örnek deneyin

Bir tam olarak işlevsel bir mantıksal uygulama ve örnek AS2 senaryo dağıtmak denemek için bkz [AS2 mantıksal uygulama şablonunu ve senaryo](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/as2/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md) 

