---
title: Kod çözme X12 iletileri - Azure Logic Apps | Microsoft Docs
description: EDI doğrulamak ve bildirimleri X12 ile oluşturma, Azure Logic Apps Enterprise Integration Pack ile ileti kod çözücü
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jonfan, divswa, LADocs
ms.topic: article
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.date: 01/27/2017
ms.openlocfilehash: 4a19462f4f849602fd14fe1204f1c7e3c01e6ec4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64701441"
---
# <a name="decode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kod çözme X12 Azure Logic Apps Enterprise Integration Pack ile iletileri

Kod çözme X12 ileti bağlayıcısıyla, Zarf ticari ortak sözleşmesi karşı doğrulamak, EDI ve iş ortağı özgü özellikleri doğrulamak, değişim işlemleri kümelerine bölme veya tüm etkileşimler korumak ve bildirimler oluşturma işlenen işlemleri için. Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızda için var olan bir tetikleyici eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili. Kod çözme X12 ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabı olması gerekir.
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış
* Bir [X12 sözleşmesi](logic-apps-enterprise-integration-x12.md) , tümleştirme hesabında zaten tanımlı

## <a name="decode-x12-messages"></a>Kod çözme X12 iletileri

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. İstek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz tetikleyicileri, kod çözme X12 ileti Bağlayıcısı yok. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleme ve ardından mantıksal uygulamanız için bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "x12" girin. Seçin **X12-kod çözme X12 ileti**.
   
    !["X12" için arama](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Daha önce tümleştirme hesabı için herhangi bir bağlantı oluşturmadıysanız, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabı seçin. 

    ![Tümleştirme hesabı bağlantı ayrıntılarını girin](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Bir yıldız işareti ile özellikleri gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabı için bir ad girin. Tümleştirme hesabı ve mantıksal uygulamanızı aynı Azure konumda olduklarından emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınızı şu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için seçin **Oluştur**.
   
    ![Tümleştirme hesabı bağlantı ayrıntıları](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Kodu çözülecek düz dosya iletisi seçin X12 Bu örnekte gösterildiği gibi bağlantınızı sonra oluşturulur.

    ![oluşturulan tümleştirme hesabı bağlantısı](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Örneğin:

    ![Select X12 düz dosya iletisi kod çözme](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

   > [!NOTE]
   > Gerçek ileti içeriği veya iyi veya kötü, ileti dizisi için yükü base64 kodlu ' dir. Bu nedenle, bu içeriği işleyen bir ifade belirtmeniz gerekir.
   > Kod görünümü veya Tasarımcısı'nda ifade oluşturucusu kullanarak girdiğiniz XML olarak içeriği işleyen bir örnek aşağıda verilmiştir.
   > ``` json
   > "content": "@xml(base64ToBinary(item()?['Payload']))"
   > ```
   > ![İçerik örneği](media/logic-apps-enterprise-integration-x12-decode/content-example.png)
   >


## <a name="x12-decode-details"></a>X12 ayrıntıları kodunu çözme

X12 kod çözme bağlayıcı, bu görevleri gerçekleştirir:

* Ticari ortak sözleşmesi karşı Zarf doğrular
* EDI ve iş ortağı özgü özellikleri doğrulama
  * EDI yapısal doğrulama ve genişletilmiş şema doğrulaması
  * Değişim Zarf yapısını doğrulama.
  * Denetim şemayla zarfının şema doğrulaması.
  * İleti şemayla işlem kümesi veri öğelerinin şema doğrulaması.
  * EDI doğrulaması işlem kümesi veri öğeleri üzerinde gerçekleştirilen 
* Değişim, Grup ve işlem kümesi denetim numaraları çoğaltmaları olmadığını doğrular
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
* Teknik ve/veya işlev bildirimi (yapılandırılmışsa) oluşturur.
  * Teknik bir bildirim başlığı doğrulama sonucu olarak oluşturur. Teknik Bildirim, bir değişim üstbilgi ve tanıtım adresi alıcı tarafından işlenmesini durumunu raporlar.
  * Bir işlev bildirimi gövdesi doğrulama sonucu olarak oluşturur. İşlev bildirimi alınan belge işlerken bir hatayla karşılaştı her bir hata raporları

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/x12/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

