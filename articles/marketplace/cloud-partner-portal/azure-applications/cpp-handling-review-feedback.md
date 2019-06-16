---
title: Azure uygulama İnceleme geri işleme | Azure Market
description: Azure DevOps için Azure Marketi teklifleri için Azure uygulama İnceleme geri işlemek için nasıl kullanılacağını açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: pabutler
ms.openlocfilehash: 1a45af2cb5eed8daa4b50bb6f0b504f9653c827a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068947"
---
# <a name="handling-review-feedback"></a>Gözden geçirme geri bildirimini işleme

Bu makalede, Microsoft Azure Marketi gözden geçirme ekibi tarafından kullanılan Azure DevOps ortamına erişmek açıklanmaktadır.  Azure uygulama teklifinizi sırasında kritik sorunlar bulunuyorsa **Microsoft gözden geçirme** adım (İnceleme görüşüne) Bu sorunlar hakkında ayrıntılı bilgi görüntülemek için bu sisteme imzalayabilirsiniz.  Bu sorunları giderdikten sonra teklifinizin Azure Market'te yayımlamak devam etmek için yeniden göndermeniz gerekir.  Aşağıdaki diyagram, bu geri bildirim süreci için yayımlama işlemi ilişkisini gösterir.

![Azure DevOps görüş yayımlama adımları](./media/pub-flow-vsts-access.png)

Genellikle, gözden geçirme sorunlar, çekme isteği (PR) başvurulur.  Her çekme isteği için bir çevrimiçi bağlı [Azure DevOps](https://azure.microsoft.com/services/devops/) (daha önce Visual Studio Team Services (VSTS) olarak adlandırılır) sorun hakkında ayrıntılar içeren öğe.  Aşağıdaki resimde PR gözden geçirme başvuru örneği görüntüler.  Karmaşık durumlarda, gözden geçirme ve destek ekipleri ayrıca size e-posta. 

![Durum sekmesinde görüntüleme gözden geçirme geri bildirim](./media/status-tab-ms-review.png)


## <a name="azure-devops-access"></a>Azure DevOps erişimi

Gözden geçirme geri bildirim başvurulan çekme isteği öğeleri görüntülemek için yayımcılar öncelikle uygun yetkilendirme verilmelidir.  Aksi takdirde, yeni yayımcılar almak bir `401 - Not Authorized` Pr'ler görüntülemeye çalışırken yanıt sayfası.  Bu Azure DevOps deposuna erişim istemek için aşağıdaki adımları gerçekleştirin:

1. Aşağıdaki bilgileri toplayın:
    - Yayımcı adı ve kimliği
    - Teklif türü (Azure uygulamasına), adı ve SKU kimliği
    - Çekme isteği bağlantı, örneğin: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>`  Bu URL, bildirim iletisini veya 401 yanıt sayfasının adresini alınabilir.
    - Yayımlama kuruluşunuzdan için erişim izni istediğiniz kişilerin e-posta adresi.  Bu liste, bulut iş ortağı Portalı'nda bir yayımcı olarak kaydolurken sağladığınız sahibi adresleri içermelidir.
2. Bir destek olayı oluşturun.  Bulut iş ortağı portalı başlık çubuğunda **yardımcı** düğmesine ve ardından **Destek** menüsünde.  Web varsayılan tarayıcıyı başlatın ve Microsoft yeni destek olay sayfasına gidin.  (Oturum açmanız gerekebilir.)
3. Belirtin **sorun türü** olarak **Market ekleme** ve **kategori** olarak **erişim sorunu**, ardından **Başlat İstek**.

    ![Destek bileti kategorisi](./media/support-incident1.png)

4. İçinde **adım 1 / 2** sayfasında, iletişim bilgilerinizi girin ve seçin **devam**.
5. İçinde **adım 2 / 2** sayfasında, bir olay başlığı belirtin (örneğin `Request Azure DevOps access`) ve (yukarıda) İlk adımda toplanan bilgileri sağlayın.  Okuma ve sözleşmesini kabul edin ve ardından **Gönder**.

Olay oluşturma başarılı olduysa, bir onay sayfası görüntülenir.  Başvuru amacıyla bu sayfadaki onay bilgileri kaydedin.  Microsoft Destek ekibine erişim isteğiniz birkaç iş günü içinde yanıt.


## <a name="reviewing-the-pull-request"></a>Çekme isteği gözden geçirme 

Çekme isteğinde belgelenen sorunları gözden geçirmek için aşağıdaki yordamı kullanın.

1. İçinde **Microsoft gözden geçirme** bölümünü **adımları yayımlama** formunda, tarayıcıyı başlatın ve gitmek için bir çekme isteği bağlantısına tıklayın **genel bakış** bu çekme isteğini (ana) sayfası  Aşağıdaki görüntüde bir örnek kritik bir sorunu ana sayfası Contoso örnek uygulama teklifini gösterilmektedir.  Bu sayfa, Azure uygulamasında bulunan gözden geçirme sorunları hakkında yararlı Özet bilgiler içerir.  

    [![Çekme isteği giriş sayfası](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)
    <br/> *Görüntüyü genişletmek için tıklatın.*
    
2. (İsteğe bağlı) Bölümünde penceresinin sağ tarafındaki **ilkeleri**, sorunu iletide tıklayın (Bu örnekte: **İlke doğrulanamadı**) ilişkili günlük dosyaları da dahil olmak üzere sorunu, alt düzey ayrıntılarını araştırmak için.  Hatalar, genellikle günlük dosyalarının en altında görüntülenir.

3. Giriş sayfasının sol taraftaki menüde **dosyaları** kapsayan bu teklif için teknik varlıkları listesi dosyaları görüntülemek için.  Microsoft inceleyenler eklenen açıklamalar bulunan kritik sorunları açıklayan.  Aşağıdaki örnekte, iki sorun bulundu. 

    [![Çekme isteği giriş sayfası](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)
    <br/> *Görüntüyü genişletmek için tıklatın.*

4. Her bir açıklama düğümü açıklamaya çevreleyen kod bağlamı içinde gezinmek için sol ağaçta tıklayın.  Takımınızın projesinde açıklamaya göre açıklanan sorunu gidermek için kaynak kodunuzu düzeltin.

> [!Note]
> Gözden geçirme takımın Azure DevOps ortamında teknik varlıkları ürününüzün düzenleyemezsiniz.  Yayımcılar için bağımsız kaynak kodu için salt okunur bir ortamı budur.  Ancak, yanıtları, okuyan kişinin yararına olması Microsoft gözden geçirme ekibi yorum bırakabilir.

   Aşağıdaki örnekte, yayımcı gözden düzeltildi ve ilk sorunla cevap verdi.

   ![İlk düzeltme ve yorumu Yanıtla](./media/first-comment-reply.png)


## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme PR(s) içinde belgelediğiniz kritik sorunları düzelttikten sonra yapmanız gerekenler [Azure uygulaması teklifinizi yeniden yayımlayın](./cpp-publish-offer.md).
