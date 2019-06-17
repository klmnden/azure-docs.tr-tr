---
title: EDIFACT iletilerini - Azure Logic Apps kod çözme | Microsoft Docs
description: EDI doğrulamak ve bildirimleri ile Azure Logic Apps Enterprise Integration Pack ile için EDIFACT message kod çözücü oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jonfan, divswa, LADocs
ms.topic: article
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.date: 01/27/2017
ms.openlocfilehash: ccad6eab68fff0891ba287a076692f9437495a4c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64696184"
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Azure Logic Apps Enterprise Integration Pack ile için EDIFACT iletileri kodunu çözme

EDIFACT kodunu çözme ileti bağlayıcısıyla EDI ve iş ortağı özgü özellikleri doğrulamak, değişim işlemleri kümelerine bölme veya tüm etkileşimler korumak ve işlenen işlemler için bildirimler oluşturmak. Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızda için var olan bir tetikleyici eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili. EDIFACT kodunu çözme ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabı olması gerekir. 
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış
* Bir [EDIFACT sözleşmesi](logic-apps-enterprise-integration-edifact.md) , tümleştirme hesabında zaten tanımlı

## <a name="decode-edifact-messages"></a>EDIFACT iletilerini kodunu çözme

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. EDIFACT kod çözme ileti bağlayıcı tetikleyicileri, sahip değil. istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleme ve ardından mantıksal uygulamanız için bir eylem ekleyin.

3. Arama kutusuna filtreniz olarak "EDIFACT" girin. Seçin **EDIFACT iletisinin kodunu çözün**.
   
    ![EDIFACT arama](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Daha önce tümleştirme hesabı için herhangi bir bağlantı oluşturmadıysanız, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabı seçin.
   
    ![Tümleştirme hesabı oluşturma](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Bir yıldız işareti ile özellikleri gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabı için bir ad girin. Tümleştirme hesabı ve mantıksal uygulamanızı aynı Azure konumda olduklarından emin olun. |

4. İşiniz bittiğinde, bağlantı oluşturmayı tamamlamak için seçin **Oluştur**. Bağlantı ayrıntılarınızı şu örneğe benzer olmalıdır:

    ![Tümleştirme hesabı ayrıntıları](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Bu örnekte gösterildiği gibi bağlantınızı oluşturulduktan sonra kodu çözülecek EDIFACT düz dosya iletisi seçin.

    ![oluşturulan tümleştirme hesabı bağlantısı](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Örneğin:

    ![Kod çözme için EDIFACT düz dosya iletisi seçin](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIFACT kod çözücü ayrıntıları

EDIFACT kodunu çözme bağlayıcı, bu görevleri gerçekleştirir: 

* Ticari ortak sözleşmesi karşı Zarf doğrular.
* Gönderen niteleyicisi & tanımlayıcı ve alıcı niteleyicisi & tanımlayıcı eşleştirerek anlaşmayı çözümler.
* Değişim ayarlarını yapılandırmayı alacağını anlaşmasına göre ait birden fazla işlem varken bir değişim birden çok işlem halinde böler.
* Değişim ayrıştırır.
* EDI ve iş ortağı özgü özellikler dahil olmak üzere doğrular:
  * Doğrulama değişimi Zarf yapısı
  * Denetim şemayla zarfının şema doğrulaması
  * İleti şemayla işlem kümesi veri öğelerinin şema doğrulaması
  * EDI doğrulaması işlem kümesi veri öğeleri üzerinde gerçekleştirilen
* Değişim, Grup ve işlem kümesi denetim numaraları çoğaltmaları (yapılandırılmışsa) olmadığını doğrular 
  * Değişim denetim numarası önceden alınmış değişim karşı denetler. 
  * Grup denetim numarası diğer Grup denetim numaraları Değişimdeki karşı denetler. 
  * Bu gruptaki diğer işlem kümesi denetim numaraları karşı işlem kümesi denetim numarası denetler.
* Değişimi işlem kümeleri halinde ayırır veya tüm değişim korur:
  * Bölünmüş değişimi işlem kümeleri - olarak işlem kümelerini Askıya Al hatası: Hareket halinde bölmelerini değişim ayarlar ve her işlem kümesi ayrıştırır. 
  Kod çözme eylemi, bu işlem yalnızca ayarlar çıkarır X12 için doğrulama başarısız `badMessages`, kalan işlemler ayarlar çıkış `goodMessages`.
  * Bölünmüş değişimi işlem kümeleri - olarak hata durumunda değişimi askıya: Hareket halinde bölmelerini değişim ayarlar ve her işlem kümesi ayrıştırır. 
  Bir veya daha fazla işlem içinde değişim ayarlar doğrulama, kod çözme eylemi çıkarır, değişim için tüm işlem ayarlar X12 başarısız `badMessages`.
  * Değişimi Koru - hata durumunda işlem kümelerini askıya: Değişimi Koru ve tüm toplu değişim işleyebilirsiniz. 
  Kod çözme eylemi, bu işlem yalnızca ayarlar çıkarır X12 için doğrulama başarısız `badMessages`, kalan işlemler ayarlar çıkış `goodMessages`.
  * Değişimi Koru - hata oluştuğunda değişimi Askıya Al: Değişimi Koru ve tüm toplu değişim işleyebilirsiniz. 
  Bir veya daha fazla işlem içinde değişim ayarlar doğrulama, kod çözme eylemi çıkarır, değişim için tüm işlem ayarlar X12 başarısız `badMessages`.
* Teknik (Denetim) ve/veya işlev bildirimi (yapılandırılmışsa) oluşturur.
  * Teknik bir bildirim ya da kontrol ACK bir söz dizimi denetimini tam alınan değişim sonuçlarını raporlar.
  * Bir işlev bildirimi kabul edin veya alınan Değişim veya bir grup Reddet bildirir

## <a name="view-swagger-file"></a>Swagger dosyasını görüntüle
EDIFACT bağlayıcının Swagger ayrıntılarını görüntülemek için bkz: [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

