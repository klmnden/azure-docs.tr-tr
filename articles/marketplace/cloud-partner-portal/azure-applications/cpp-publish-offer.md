---
title: Azure uygulama teklifi - Azure Marketi'nde yayımlama | Microsoft Docs
description: İşlem ve Azure uygulaması Azure Marketi'nde teklif yayımlamak için gereken adımları açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: pbutlerm
ms.openlocfilehash: 2adf07cf2337611b9136af47ce6a35b617e2e9ff
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177041"
---
# <a name="publish-azure-application-offer"></a>Azure uygulama teklifi yayımlama

Şirket bilgileri sağlayarak bir teklif oluşturduktan sonra **yeni teklif** sayfasında teklif yayımlayabilirsiniz. Seçin **Yayımla** yayımlama işlemini başlatmak için.

Aşağıdaki diyagramda "Canlı gitmek" bir teklif için yayımlama işlemi ana adımları gösterir.

![Teklif yayımlama adımları](./media/offer-publishing-steps.png)


## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımları ayrıntılı bir açıklaması

Aşağıdaki tabloda, listeler ve her yayımlama adımlarını açıklar ve her adımı tamamlamak için tahmini süre sağlar.  "Gün" iş günü tanımlanan tahmin, hafta sonları ve tatiller hariç tutun.

|  **Yayımlama Adım**           | **saat**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | < 15 dk    | Bilgi sunan ve ayarlar doğrulanır sunar.                        |
| Gelir etkileyen ayarları doğrulama | < 15 dk  | Azure kaynak kullanım attribution teklif için denetlenir.             |
| Sertifika                  | < 1 gün     | Teklif, Azure sertifika ekibi tarafından analiz edilir. Bu teklif, virüsler, kötü amaçlı yazılım, emniyet uyumluluk ve güvenlik sorunları için taranır. Bu teklif, tüm uygunluk ölçütlerini karşıladığını görmek için denetlenir. Daha fazla bilgi için [önkoşulları](./cpp-prerequisites.md). Bir sorun bulunursa geri bildirim sağlanır. |
| Sürücü doğrulama testi          | < 2 saat   | (İsteğe bağlı) Bir Test sürüşüne varsa, Microsoft, dağıtılan çoğaltılan ve olduğunu doğrular.  |
| Paketleme ve müşteri adayı oluşturma kaydı | 1 saatten az  | Teklife ilişkin teknik varlıkları müşteri kullanılmak üzere hazırlanmıştır ve müşteri adayı sistemleri yapılandırılmalı ve dağıtılmalıdır. |
|  Yayımcı oturum kapatma             |  El ile    | Son yayımcı gözden geçirme ve teklif Canlı geçmeden önce onay. Teklif Önizleme için kullanıma sunulmuştur.  Teklifinizi (adımlarda teklif bilgi) seçili Aboneliklerdeki tüm gereksinimleri karşıladığından emin doğrulamak için dağıtabilirsiniz.  Teklif doğruladıktan sonra seçin **Go Live** için teklifinizi sonraki adıma geçebilirsiniz. |
| Microsoft gözden geçirme                | 7 - 14 gün | Bütünlüklü olarak Microsoft Azure uygulamanızı inceler ve sorunları bulunursa size e-posta.  Bu adımı uzunluğunu uygulama, sorunları ortaya çıkardı ve onlara nasıl kısa bir süre içinde yanıt karmaşıklığına bağlıdır.  |
| Canlı                           | < 1 gün | Teklif serbest, belirli bölgelerde çoğaltılır ve genel kullanıma sunulan. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |

 
Yayımlama işlemini izleyebilirsiniz **durumu** bulut iş ortağı Portalı'nda teklifinizi için sekmesinde.

![Bir Azure uygulaması teklif durumu sekmesi](./media/offer-status-tab.png)

Yayımlama işlemini tamamladıktan sonra teklifinizi listelenir [Microsoft Azure Marketi'nde uygulama kategorisi](https://azuremarketplace.microsoft.com/marketplace/apps/).



## <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Teklifiniz, Yayımlama durumunu görüntüleme yanı sıra **durumu** sekmesi de görüntülenir hata iletileri ve görüşleri **Microsoft gözden geçirme** adım.  Genellikle, gözden geçirme sorunlar, çekme isteği (PR) başvurulur.  Her çekme isteği, bir çevrimiçi Visual Studio Team Services için bağlı (VSTS, olarak yeniden adlandırıldı [Azure DevOps](https://azure.microsoft.com/services/devops/)) sorun hakkında ayrıntılar içeren öğe.  Aşağıdaki resimde PR gözden geçirme başvuru örneği görüntüler.  Daha karmaşık durumlarda, gözden geçirme ve destek ekipleri size e-posta. 

![Durum sekmesinde görüntüleme gözden geçirme geri bildirim](./media/status-tab-ms-review.png)

Teklifi yayımlama işlemine devam etmeden önce rapor edilen her sorun incelemeniz gerekir.  Aşağıdaki diyagram, bu geri bildirim süreci için yayımlama işlemi ilişkisini gösterir.

![VSTS görüş yayımlama adımları](./media/pub-flow-vsts-access.png)


### <a name="vsts-access"></a>VSTS erişim

Gözden geçirme geri bildirim başvurulan VSTS öğeleri görüntülemek için uygun yetkilendirme yayımcılar verilmesi gerekir.  Aksi takdirde, yeni yayımcılar almak bir `401 - Not Authorized` yanıt sayfası.  Teklif gözden geçirme VSTS sisteme erişim izni istemek için aşağıdaki adımları gerçekleştirin:

1. Aşağıdaki bilgileri toplayın:
    - Yayımcı adı ve kimliği
    - Teklif türü (Azure uygulamasına), adı ve SKU kimliği
    - Çekme isteği bağlantı, örneğin: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>`  Bu URL, bildirim iletisini veya 401 yanıt sayfasının adresini alınabilir.
    - Yayımlama kuruluşunuzdan için erişim izni istediğiniz kişilerin e-posta adresi.  Bu, bulut iş ortağı Portalı'nda bir yayımcı olarak kaydolurken sağladığınız sahibi adresleri içermelidir.
2. Bir destek olayı oluşturun.  Bulut iş ortağı portalı başlık çubuğunda **yardımcı** düğmesine ve ardından **Destek** menüsünde.  Web varsayılan tarayıcıyı başlatın ve Microsoft yeni destek olay sayfasına gidin.  (Oturum açmanız gerekebilir.)
3. Belirtin **sorun türü** olarak **Market ekleme** ve **kategori** olarak **erişim sorunu**, ardından **Başlat İstek**.

    ![Destek bileti kategorisi](./media/support-incident1.png)

4. İçinde **adım 1 / 2** sayfasında, iletişim bilgilerinizi girin ve seçin **devam**.
5. İçinde **adım 2 / 2** sayfasında, bir olay başlığı belirtin (örneğin `Request VSTS access`) ve (yukarıda) İlk adımda toplanan bilgileri sağlayın.  Okuma ve sözleşmesini kabul edin ve ardından **Gönder**.

Olay oluşturma başarılı olduysa, bir onay sayfası görüntülenir.  Onay bilgileri daha sonra başvurmak üzere kaydedin.  Microsoft desteğine erişim isteğiniz birkaç iş günü içinde yanıt.


## <a name="next-steps"></a>Sonraki adımlar

Azure uygulama yayınlandıktan sonra yapabilecekleriniz [mevcut teklifi güncelleştirme](./cpp-update-existing-offer.md) değişen iş ya da teknik gereksinimlerine yansıtacak şekilde. 
