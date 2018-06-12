---
title: EDIFACT iletileri - Azure Logic Apps kod çözme | Microsoft Docs
description: EDI doğrulamak ve onayları EDIFACT ileti kod çözücü ile Azure Logic Apps için Kurumsal tümleştirme paketi oluştur
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: jeconnoc
editor: ''
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: bfb64b4abfca6e2a113489b79405b5df2b53c049
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299163"
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>EDIFACT iletileri Kurumsal tümleştirme paketi ile Azure mantıksal uygulamaları için kod çözme

Kod çözme EDIFACT ileti bağlayıcısıyla EDI ve iş ortağı özgü özellikleri doğrulayın, işlemleri kümeleri içine etkileşimler bölme veya tüm etkileşimler korumak ve işlenen işlemler için ilgili kaynaklar oluşturur. Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızı varolan bir tetikleyicinin eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili. Kod çözme EDIFACT ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabınızın olması gerekir. 
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış
* Bir [EDIFACT sözleşmesi](logic-apps-enterprise-integration-edifact.md) tümleştirme hesabınızda tanımlanan zaten

## <a name="decode-edifact-messages"></a>EDIFACT iletileri kod çözme

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. Bir istek tetikleyici gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz kod çözme EDIFACT ileti bağlayıcı tetikleyiciler, sahip değil. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem mantıksal uygulamanızı ekleyin.

3. Arama kutusuna "EDIFACT", filtre olarak girin. Seçin **EDIFACT ileti kod çözme**.
   
    ![EDIFACT arama](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Tümleştirme hesabınıza daha önce herhangi bir bağlantısı oluşturmadıysanız, bu bağlantı artık oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabı seçin.
   
    ![Tümleştirme hesabı oluşturma](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Bir yıldız işareti özelliklerle gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabını * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabı ve mantığı uygulamanız aynı Azure konumuna olduğundan emin olun. |

4. İşiniz bittiğinde bağlantınızı oluşturmayı tamamlamak için seçin **oluşturma**. Bağlantı ayrıntıları bu örneğe benzer görünmelidir:

    ![Tümleştirme hesap ayrıntıları](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Bu örnekte gösterildiği gibi bağlantınızı oluşturulduktan sonra çözecek EDIFACT düz dosya iletiyi seçin.

    ![oluşturulan tümleştirme hesap bağlantı](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Örneğin:

    ![Kod çözme için EDIFACT düz dosya iletiyi seçin](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIFACT kod çözücü ayrıntıları

Kod çözme EDIFACT connector, şu görevleri gerçekleştirir: 

* Zarf ticari ortak sözleşmesi karşı doğrular.
* Anlaşmayı gönderen Niteleyici Tanımlayıcısı ve alıcı niteleyicisi & tanımlayıcı eşleştirerek çözümler.
* Değişim ayarlarını yapılandırmayı alacağını birden fazla işlem anlaşmasına göre ait olduğunda bir değişim birden çok işlem halinde ayırır.
* Değişim ayrıştırır.
* EDI ve iş ortağı özgü özellikler de dahil olmak üzere doğrular:
  * Doğrulama değişim Zarf yapısı
  * Zarf denetim şemasına karşılık şema doğrulaması
  * Şema doğrulama ileti şemayla işlem kümesi veri öğelerinin
  * İşlem kümesi veri öğelerinde EDI doğrulamanın
* Değişim, Grup ve işlem kümesi denetim numaraları (yapılandırılmışsa) tekrarı olmayan olduğunu doğrular 
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
* Teknik (Denetim) ve/veya işlev bildirim (yapılandırılmışsa) oluşturur.
  * Teknik bir bildirim veya CONTRL ACK tam alınan değişim söz dizimi kontrol sonuçlarını raporlar.
  * İşlev bildirimi kabul ettikten kabul edin veya alınan Değişim veya bir grup Reddet

## <a name="view-swagger-file"></a>Görünüm Swagger dosyası
EDIFACT bağlayıcı Swagger ayrıntılarını görüntülemek için bkz: [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

