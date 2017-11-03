---
title: "Veri Hizmeti teklifiniz Market için test etme | Microsoft Docs"
description: "Veri Hizmeti teklifiniz Azure Marketi için test etme anlayın."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Veri Hizmeti teklifiniz hazırlamada test etme
> [!IMPORTANT]
> **Şu anda artık ekleme duyuyoruz herhangi yeni veri hizmeti yayımcılar. Yeni dataservices listesine onaylanmamış.** Üzerinde AppSource yayımlamak istediğiniz bir SaaS iş uygulaması varsa daha fazla bilgi bulabilirsiniz [burada](https://appsource.microsoft.com/partners). Bir Iaas uygulamalar varsa veya Azure marketi, yayımlamak istediğiniz Geliştirici hizmet, daha fazla bilgi bulabilirsiniz [burada](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

İlk iki adımları tamamladıktan sonra [, Microsoft Developer hesabı oluşturma](marketplace-publishing-accounts-creation-registration.md) ve [yayımlama portalında oluşturma, veri hizmeti sunan](marketplace-publishing-data-service-creation.md) teklifiniz Azure kullanılabilir yapmak için hazır Market. Bu konuda ilk Ara "Hazırlama" adlı adım yol gösterir

Hazırlama teklifiniz özel ", test ve üretim göndermeden önce işlevselliğini doğrulayın sandbox" dağıtma anlamına gelir. Teklif hazırlama dağıtmış olan bir müşteriye gibi görünür.

## <a name="step-1-pushing-your-offer-to-staging"></a>1. Adım Teklifiniz hazırlama için iletme
Hazırlama teklif Ftp'den gelecekteki abonelere kullanılabilir olmadan önce teklif test etmenizi sağlar.  Teklifiniz nasıl görüntülenir ve bunlar verilerinizi abone için işlev görebilirsiniz.  

  ![Çizim](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Oturum [portalını yayımlama](https://publish.windowsazure.com)
2. Seçin **Veri Hizmetleri** sol gezinti penceresinde
3. Hazırlama için anında istediğiniz teklifiniz seçin. Yukarıdaki ekran görürsünüz.
4. Tıklatın **hazırlama anında** düğmesi.  
5. Hazırlama için itme önce tamamlanması gereken teklif ile ilgili sorunlar varsa, görüntülenen bir listesini görürsünüz.  Bu öğeler, listedeki her öğeye tıklayarak düzeltin. Yapılan, tüm düzeltmeler tıklattığınızda **anında için hazırlama** yeniden düğmesine tıklayın.

Teklifiniz ile herhangi bir sorun varsa açılır pencere görürsünüz.  

Değil planlama/değil teklifiniz Azure Portalı'nda ortaya çıkarma onaylanırsa (şu anda sınırlı kapasite), yalnızca açılır penceresini kapatın.

Veri Hizmeti (DataMarket portalı ek olarak) Azure Portalı'nda test test etmek için bir Azure abonelik kimliği gerekir.  Bu abonelik kimliği teklifiniz test etmek için izin verilecek hesap tanımlar.  

Kesme ve abonelik Kimliğinizi yapıştırın ve devam etmek için onay işaretine tıklayın.

  ![Çizim](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Bu Azure abonelik kimlikleri yalnızca olduğundan, hazırlama ve test için gereken [Azure Yönetim Portalı](https://manage.windowsazure.com). Azure Marketi'nde test olması gerekmez.
> 
> 

Yayımlama "Sürüyor" simge görüntüleyerek yer aldığını gösterir görüntülenen sonraki ekranında sarı aşağıdaki vurgulanır. Hazırlama için itme arasında 10-15 dakika sürer.  İlk uzun sürüyorsa, tarayıcınızı (IE içinde F5 tuşuna basın) yenileyin.  Bizimle bağlantı kişiyi burada teklifiniz hala Ftp'den bir saat sonra hazırlama için nadir durumlarda tıklatın bir sorun olduğunu bize bildirin.

  ![Çizim](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Hazırlama için anında iletme "Sürüyor" simgesini tamamlandığında Dur taşıma ve durumu "Aşamalı" güncelleştirilir.  Şimdi teklifiniz test etmek hazırsınız.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>2. Adım DataMarket içinde hazırlanmış teklifiniz test
Metin aşağıdaki bağlantıyı tıklatın **"adresindeki teklif hizmetinizi bkz..."** Abone teklifiniz üretime gittiğinde görür ve DataMarket içinde görünür ekranı görüntülemek için.

  ![Çizim](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Test veya 12 öğelerin her birini tüm logolar, Fiyatlar/işlemleri, metin, görüntüler, belgeleri emin olmak için işaretlenmiş yukarıda ve bağlantıları düzgün doğru ve çalışıyor olduğunu doğrulayın.  Bu, gerçek değerlerle teklifiniz oluştururken girdiğiniz tüm test değerleri değiştirilmiştir emin olmak için iyi bir zamandır.

1. Teklif logosu
2. Teklif adı
3. Yayımcı adı/bağlantı şirketinizin Web sitesi
4. Teklifiniz için arama kategorileri
5. Aboneler yardımcı olmak için teklifiniz 's destek bağlantısı
6. Teklifiniz için bağlamsal açıklaması
7. Fatura Ayrıntıları gösteren teklif planı
8. Uygulama kodu bağlantı
9. Teklif verilerini gösteren örnek görüntüleri kullanın
10. Teklif içinde her hizmet için giriş/çıkış meta verileri
11. Teklif'ın kullanım koşulları
12. Teklif ait veri önizlemesi

Son olarak, hizmet Datamarket "Keşfedin bu veri KÜMESİ" bağlantısını tıklatarak çalışır denetleyin.  Yeni bir pencerede açılır hizmetinize karşı bir sorgunun sonuçlarını önizlemek için aracı "Hizmet Gezgini" diyoruz.  Bu pencerede, gerekli parametreleri girin ve hizmetinizi sorgusundan görüntülenen sonuçlarını görebilirsiniz.   Ayrıca, görüntülenen URL'yi sorgunuz için geçerlidir.  

> [!NOTE]
> En üstte gösterilen hizmet metinsel açıklaması gözden geçirdiğinizden emin olun.  Ve teklifiniz birden fazla hizmeti oluşur, çağrı, gözden geçirin ve test etmek için İleri hizmetine geçiş yapmak için alttaki sekmeleri tıklatın.
> 
> 

## <a name="next-step"></a>Sonraki adım
Sorunu yaşıyor ve Yardım ihtiyacınız varsa bunları çözme Lütfen kişi [Azure yayımcı desteğine](http://go.microsoft.com/fwlink/?LinkId=272975).

Memnun ve okuyun teklifiniz yayımlamak hazır olup olmadığını [isteği onayı üretime anında](marketplace-publishing-push-to-production.md) belgeleri.

## <a name="see-also"></a>Ayrıca Bkz.
* [Başlarken: nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

