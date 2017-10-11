---
title: "X12 kod çözme iletileri - Azure Logic Apps | Microsoft Docs"
description: "EDI doğrulamak ve X12 ile onayları oluşturmak Azure Logic Apps için Kurumsal tümleştirme paketindeki ileti Kodlayıcısı"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 18719a8f49c74973947517161f7306c233a9323f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>X12 kod çözme Azure Logic Apps Enterprise tümleştirme paketi ile iletileri

Kod çözme X12 ileti bağlayıcısıyla, ticari ortak sözleşmesi karşı Zarf doğrulamak, EDI ve iş ortağı özgü özellikler doğrulamak, işlemleri kümeleri içine etkileşimler bölme veya tüm etkileşimler korumak ve ilgili kaynaklar oluştur işlenen işlemleri için. Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızı varolan bir tetikleyicinin eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili. Kod çözme X12 ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabınızın olması gerekir.
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış
* Bir [X12 sözleşmesi](logic-apps-enterprise-integration-x12.md) tümleştirme hesabınızda tanımlanan zaten

## <a name="decode-x12-messages"></a>X12 kod çözme iletileri

1. [Mantıksal uygulama oluşturma](logic-apps-create-a-logic-app.md).

2. Bir istek tetikleyici gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz kod çözme X12 ileti bağlayıcı tetikleyiciler, sahip değil. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem mantıksal uygulamanızı ekleyin.

3.  Arama kutusuna "x12" filtreniz için girin. Seçin **X12-X12 kod çözme ileti**.
   
    !["X12" için arama](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Tümleştirme hesabınıza daha önce herhangi bir bağlantısı oluşturmadıysanız, bu bağlantı artık oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabı seçin. 

    ![Hesap bağlantı ayrıntıları tümleştirme sağlar](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Bir yıldız işareti özelliklerle gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabını * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabı ve mantığı uygulamanız aynı Azure konumuna olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı bilgilerinizi bu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için tercih **oluşturma**.
   
    ![Tümleştirme hesap bağlantı ayrıntıları](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Kod çözme için select X12 düz dosya ileti bu örnekte gösterildiği gibi bağlantınızı sonra oluşturulur.

    ![oluşturulan tümleştirme hesap bağlantı](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Örneğin:

    ![Kod çözme için dosya iletisi SELECT X12 düz](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a>X12 kod çözme ayrıntıları

X12 kod çözme bağlayıcı bu görevleri gerçekleştirir:

* Ticari ortak sözleşmesi karşı Zarf doğrular
* EDI ve iş ortağı özgü özellikleri doğrular
  * EDI yapısal doğrulama ve genişletilmiş şema doğrulaması
  * Değişim Zarf yapısını doğrulama.
  * Zarf denetim şemasına karşılık şema doğrulamasını.
  * Şema doğrulama ileti şemayla işlem kümesi veri öğelerinin.
  * İşlem kümesi veri öğelerinde EDI doğrulamanın 
* Değişim, Grup ve işlem kümesi denetim numaraları çoğaltmaları olmadığını doğrular
  * Değişim kontrol numarası önceden alınmış etkileşimler karşı denetler.
  * Grup denetim numarası değişim diğer Grup denetimi numaraları karşı denetler.
  * Bu gruptaki diğer işlem kümesi denetim numaraları karşı işlem kümesi denetim numarası denetler.
* İşlem kümeleri içine değişim böler veya tüm değişim korur:
  * Bölünmüş değişim işlem kümeleri - olarak askıya alma işlem kümeleri hatası: bölmelerini değişim hareket halinde ayarlar ve her işlem kümesi ayrıştırır. 
  Kod çözme eylem çıkarır yalnızca bu işlem ayarlar X12 için doğrulama başarısız `badMessages`, kalan işlemler ayarlar için çıkış `goodMessages`.
  * Bölünmüş değişim işlem kümeleri - olarak askıya alma değişim hatası: bölmelerini değişim hareket halinde ayarlar ve her işlem kümesi ayrıştırır. 
  Bir veya daha fazla işlem değişim ayarlarsa doğrulama, kod çözme eylem çıkarır tüm işlem ayarlar bu değişim X12 başarısız `badMessages`.
  * Değişim korumak - işlem kümeleri askıya alma hatası: değişim korumak ve tüm toplu değişim işlem. 
  Kod çözme eylem çıkarır yalnızca bu işlem ayarlar X12 için doğrulama başarısız `badMessages`, kalan işlemler ayarlar için çıkış `goodMessages`.
  * Değişim korumak - değişim askıya alma hatası: değişim korumak ve tüm toplu değişim işlem. 
  Bir veya daha fazla işlem değişim ayarlarsa doğrulama, kod çözme eylem çıkarır tüm işlem ayarlar bu değişim X12 başarısız `badMessages`. 
* Teknik ve/veya işlevsel bir bildirim (yapılandırılmışsa) oluşturur.
  * Teknik bir bildirim başlığı doğrulama sonucu olarak oluşturur. Teknik Bildirim bir değişim üstbilgisi ve adresi alıcı tarafından toplamı işlenmesini durumunu raporlar.
  * İşlev bildirim gövde doğrulama sonucu olarak oluşturur. İşlev bildirim alınan belge işlerken bir hatayla karşılaştı her hata raporları

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/x12/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

