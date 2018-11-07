---
title: Azure marketi'ndeki sanal makine görüntünüzü yönetme | Microsoft Docs
description: İlk yayımlandıktan sonra sanal makine görüntüsünü Azure Marketi'nde yönetme hakkında ayrıntılı kılavuz
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: d4c7dce1876e9838fe986aebb7e38a09e8a82baf
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51252981"
---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Azure Marketi'ndeki teklif sanal makine için üretim sonrası Kılavuzu
Bu makalede, Azure Marketi'nde bir dinamik sanal makine teklifi güncelleştirme nasıl açıklanmaktadır. Bunu var olan bir teklif için bir veya daha fazla yeni SKU'lara ekleme işleminde size rehberlik. Bu da dinamik sanal makine teklifi veya SKU marketten kaldırma işleminde size rehberlik.

Bir teklif/SKU içinde hazırlanmış sonra [Azure portalında](http://portal.azure.com), şu metin kutuları değiştiremezsiniz:

* **Teklif tanımlayıcısı**: gidin, yayımlama portalı **sanal makineler** ve teklifinizi seçin. Ardından **VM GÖRÜNTÜLERİ** > **teklif tanımlayıcısı**.
* **SKU tanımlayıcısı**: gidin, yayımlama portalı **sanal makineler** ve teklifinizi seçin. Ardından **SKU'ları** > **bir SKU ekleyin**.
* **Yayımcı Namespace**: gidin, yayımlama portalı **sanal makineler** > **izlenecek** > **söyleyin bize hakkında şirketiniz**("Adım 2 kayıt şirketiniz altında" bulunur) > **yayımcı Namespace** > **Namespace**.

Teklif/SKU listelenen sonra [Market](https://azure.microsoft.com/marketplace), şu metin kutuları değiştiremezsiniz:

* **Teklif tanımlayıcısı**: gidin, yayımlama portalı **sanal makineler** ve teklifinizi seçin. Ardından **VM GÖRÜNTÜLERİ** > **teklif tanımlayıcısı**.
* **SKU tanımlayıcısı**: gidin, yayımlama portalı **sanal makineler** ve teklifinizi seçin. Ardından **SKU'ları** > **bir SKU ekleyin**.
* **Yayımcı Namespace**: gidin, yayımlama portalı **sanal makineler** > **izlenecek** > **söyleyin bize hakkında şirketiniz**("Adım 2 kaydetme altında" bulunur) **yayımcı Namespace** > **Namespace**.
* **Bağlantı noktaları**: gidin, yayımlama portalı **sanal makineler** ve teklifinizi seçin. Ardından **VM GÖRÜNTÜLERİ** > **açık bağlantı noktaları**.
* **Fiyatlandırma değişikliği listelenen fiyatlarını**
* **Listelenen fiyatlarını faturalandırma modeli Değiştir**
* **Listelenen fiyatlarını bölgeleri faturalama kaldırılması**
* **Veri diski sayısı listelenen fiyatlarını değiştirme**

## <a name="update-the-technical-details-of-a-sku"></a>Bir SKU teknik ayrıntılarını güncelleştirme
Yeni bir sürümü için listelenen SKU ekleyin ve teklifinizi yeniden yayımlayın için bu adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **VM GÖRÜNTÜLERİ** sekmesi.
4. İçinde **SKU'ları** bölümünde, güncelleştirmek istediğiniz SKU'yu bulun.
5. SKU için yeni bir sürüm numarası ekleyin ve **+** düğmesi. Yeni sürüm X, Y ve Z tamsayılar olduğu bir X.Y.Z biçiminde olmalıdır. Sürüm değişikliklerini yalnızca artımlı olmalıdır.
6. İçinde **işletim sistemi VHD URL'si** kutusuna paylaşılan erişim imzası URI'si, işletim sistemi VHD'si için oluşturulan girin ve değişiklikleri kaydedin.

   > [!IMPORTANT]
   > Artırma/veri diski sayısı listelenen sku'sunun azaltma olamaz. Bu durumda yeni bir SKU'ya oluşturmanız gerekir. Ayrıntılı yönergeler için bölümüne başvurun [listelenen teklif altında yeni bir SKU ekleyin](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
8. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![VM Görüntüleri](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Bir teklif ve SKU yedeğin ayrıntılarını güncelleştirme
Yedeğin güncelleştirebilirsiniz (, yasal pazarlama desteği, kategoriler) Canlı teklif ya da Market'te SKU ayrıntıları.

### <a name="update-the-offer-description-and-logos"></a>Teklif açıklaması ve logoları güncelleştirme
Teklifinizi yeniden yayımlayın ve teklif ayrıntılarını güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **pazarlama** sekmesi.
4. Tıklayın **İngilizce (ABD)**.
5. Tıklayın **ayrıntıları** sekmesi. İçinde **açıklama** bölümünde, teklifi güncelleştirme **başlık**, **özeti**, ve **uzun özeti** ve değişiklikleri kaydedin.

   > [!NOTE]
   > SKU ayrıntılarını güncelleştirdiğinizde, aşağıdaki kısıtlamalara dikkat edin: 
   * Teklif açıklaması ve SKU açıklaması için yinelenen metin girmeyin.
   * SKU başlık ve uzun Özet teklif için yinelenen metin girmeyin. 
   * SKU başlığı ve teklif özeti için yinelenen metin girmeyin.
   >
   >

    ![Ayrıntılar](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. İçinde **LOGOLARI** bölümünü **ayrıntıları** sekmesinde, logolar güncelleştirin. Aşağıda logoların izlediğinden emin [Azure Market Kılavuzu](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > İsteğe bağlı bir hero simgedir. Hero simgeyi karşıya yükleyin değil seçebilirsiniz. Ancak, bir hero simgesini karşıya yüklendikten sonra yoktur yayımlama silmek için hiçbir sağlama portalı. İzleyin [hero simgesi yönergeleri](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
8. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Logo](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>SKU güncelleştirmesi
Teklifinizi yeniden yayımlayın ve SKU ayrıntılarını güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **pazarlama** sekmesi.
4. Tıklayın **İngilizce (ABD)**.
5. Tıklayın **planları** sekmesi. İçinde **SKU'ları** bölümünde, SKU güncelleştirme **başlık**, **özeti**, ve **açıklama** ve değişiklikleri kaydedin.

   > [!NOTE]
   > SKU ayrıntılarını güncelleştirdiğinizde, aşağıdaki kısıtlamalara dikkat edin: 
   * Teklif açıklaması ve SKU açıklaması için yinelenen metin girmeyin. 
   * SKU başlık ve uzun Özet teklif için yinelenen metin girmeyin. 
   * SKU başlığı ve teklif özeti için yinelenen metin girmeyin.
   >
   >
6. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
7. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Planlar](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Var olan bağlantıları değiştirebilir veya yeni bağlantı Ekle
Veya yeni bağlantılar eklediğiniz ve daha sonra teklifinizi yeniden yayımlayın var olan bağlantıları değiştirmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **pazarlama** sekmesi.
4. Tıklayın **İngilizce (ABD)**.
5. Tıklayın **bağlantıları** sekmesi.
6. Yeni bir bağlantı eklemek için **bağlantıları** bölümünde **+ Ekle bağlantısını**. İçinde **Bağlantı Ekle** iletişim kutusunda, ait bağlantıyı girin **başlık** ve **URL** ve değişiklikleri kaydedin. Müşterilere yardımcı bilgiler içeren herhangi bir bağlantıyı girebilirsiniz.
7. Güncelleştirmek veya varolan bir bağlantıyı silmek için bağlantıyı seçin ve **Düzenle** düğmesini veya **Sil** düğmesi.

   > [!NOTE]
   > Bu bağlantıları üretim isteği işleminiz sırasında doğrulanmak çünkü bu bölümde girdiğiniz bağlantıları düzgün çalıştığından emin olun.
   >
   >
8. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
9. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Bağlantılar](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Bağlantı Ekle](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Var olan örnek görüntüsünü değiştirebilir veya yeni bir örnek görüntü ekleyin
Var olan bir örnek görüntü değiştirin veya yeni örnek görüntüleri eklemeniz ve ardından teklifinizi yeniden yayımlayın için bu adımları izleyin:

> [!NOTE]
> Yalnızca bir örnek görüntü görüntülendiği [Azure portalında](https://portal.azure.com).
>
>

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **pazarlama** sekmesi.
4. Tıklayın **İngilizce (ABD)**.
5. Tıklayın **örnek GÖRÜNTÜLERİ** sekmesi.
6. Yeni bir örnek görüntü eklemek için **örnek görüntüleri** bölümünde **karşıya yeni bir görüntü** ve değişiklikleri kaydedin.

   > [!NOTE]
   > Bir örnek görüntü gibi isteğe bağlıdır.
   >
7. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
8. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Örnek görüntüleri](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>Yasal içeriği güncelleştirme
Teklifinizi yeniden yayımlayın ve yasal içeriği güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **pazarlama** sekmesi.
4. Tıklayın **İngilizce (ABD)**.
5. Tıklayın **yasal** sekmesi. İçinde **yasal** bölümünde, ilkelerinize/kullanım koşullarınıza güncelleştirin. Girin veya yapıştırın ilkeleri/koşullarında **kullanım** kutusunda ve değişiklikleri kaydedin.
6. Yasal koşulları için karakter sınırı 1 milyon karakter olabilir.
7. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
8. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Yasal Bilgiler](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Destek bilgilerini güncelleştirin
Teklifinizi yeniden yayımlayın ve destek bilgileri güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **Destek** sekmesi.
4. İçinde **mühendislik başvurun** bölümünde, mühendislik kişi ayrıntılarını güncelleştirin. Bu ayrıntılar arasındaki dahili iletişimi için Microsoft ve iş ortağı yalnızca kullanılır.
5. İçinde **müşteri desteği** bölümünde, güncelleştirme desteği kişi ayrıntıları ve **destek URL'si**. Bu ayrıntılar arasındaki dahili iletişimi için Microsoft ve iş ortağı yalnızca kullanılır.

   > [!NOTE]
   > Yalnızca e-posta desteği sağlamak istiyorsanız, bir işlevsiz bir telefon numarası girin **müşteri desteği** bölümü. Bu durumda, sağladığınız e-posta yerine kullanılır.
   >
   >
6. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
7. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Destek](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>Güncelleştirme kategorileri
Teklifiniz için kategorileri bölümünü güncelleştirmeniz ve teklifinizi yeniden yayımlayın için bu adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **KATEGORİLERİ** sekmesi.
4. İçinde **kategorileri** bölümünde teklifiniz için kategorileri güncelleştirin ve değişiklikleri kaydedin. Azure Market Galerisi için en fazla beş kategorileri seçebilirsiniz.
5. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
6. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Kategoriler](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Listelenen bir teklif altında yeni bir SKU ekleyin
Yeni bir SKU'ya Canlı teklife eklemek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **SKU'ları** sekmesi. Ardından **bir SKU ekleyin**. 
4. İletişim kutusuna bir **SKU tanımlayıcısı** küçük. Seçin **getir (KLG) lisans kendi faturalandırma modeli** bir KLG faturalandırma modeliyle yeni SKU yayımlamak istiyorsanız kutuyu. Aksi takdirde, onay kutusunu temizleyin. Yeni bir SKU'ya oluşturmak için onay işaretine tıklayın. KLG faturalandırma modeli seçmediyseniz, faturalandırma modeli saatlik olarak otomatik olarak ayarlanır. 30 günlük ücretsiz deneme için saatlik faturalandırma modeli istiyorsanız belirleyin **bir ay** için **ücretsiz bir deneme sürümü mevcuttur?** Aksi takdirde seçin **Hayır deneme**. (**Ücretsiz bir deneme sürümü mevcuttur?**  yeni SKU oluşturulurken KLG yalnızca seçmediniz görüntülenir.)

   > [!IMPORTANT]
   > **Bu her zaman bir çözüm şablonu satın alınması çünkü bu SKU Market Gizle** olmalıdır **Evet** *yalnızca* bir çözüm şablonu yayımlamak için onayından durumunda. Aksi takdirde, bu seçenek her zaman olmalıdır **Hayır**.
   >
   >
4. Soldaki menüde **VM GÖRÜNTÜLERİ** sekme ve oluşturduğunuz yeni SKU bulun.
5. Yeni SKU ayarlamak için bkz [VM görüntünüz için sertifika elde](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) Kılavuzu.
6. Yeni SKU pazarlama malzemeleri eklemek için bkz [içeriği pazarlama Market sağlamak](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Fiyatlandırma bilgileri için yeni SKU eklemek için bkz [fiyatlarınızı belirleme](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
9. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![SKU'lar](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Bir SKU ekleyin](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Veri diski sayısı listelenen SKU için değiştirin
Artırma/veri diski sayısı listelenen sku'sunun azaltma olamaz. Bu durumda yeni bir SKU'ya oluşturmanız gerekir. Ayrıntılı yönergeler için bölümüne başvurun [listelenen teklif altında yeni bir SKU ekleyin](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Marketten listelenen teklifi Sil
Çeşitli yönlerini canlı bir teklifin kaldırılması için istekte bulunması durumunda dikkate gerekir. Marketten bir listelenen teklif kaldırmak için destek ekibinden rehberlik almak için şu adımları izleyin:

1. Üzerinde bir destek bileti yükseltmek [bir olay](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) sayfası.

2. Seçin **sorun türü** olarak **tekliflerini yönetmenize**seçip **kategori** olarak **bir teklif ve/veya SKU üretimde zaten değiştirme**.
3. İstek gönderin.

Destek ekibine teklifini/SKU'yu silme işleminde size kılavuzluk eder.

> [!NOTE]
> Taslak durumu (ancak değil hazırlama veya üretim) olmakla birlikte, her zaman teklif silebilirsiniz. Üzerinde **GEÇMİŞİ** sekmesinde **taslağı at**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Marketten bir listelenen SKU Sil
Marketten bir listelenen SKU silmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).

2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki bölmede **SKU'ları** sekmesi.
4. SKU'ları silme ve istediğiniz seçin **Sil** düğmesi.
5. Git **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** Market teklifi yeniden yayımlamak için.
6. Teklif Marketinde yeniden yayımlamışsa belki sonra SKU Markete ve Azure portalından silindi.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>Marketten bir listelenen SKU'ın geçerli sürümü silin
Marketten bir listelenen SKU'ın geçerli sürümü silmek için aşağıdaki adımları izleyin: 

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).

2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **VM GÖRÜNTÜLERİ** sekmesi.
4. Geçerli sürümü silin ve istediğiniz SKU'yu seçin **Sil** düğmesi.
5. Git **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** Market teklifi yeniden yayımlamak için.
6. Teklif Marketinde yeniden yayımlamışsa belki sonra listelenen SKU'ın geçerli sürümü Markete ve Azure portalından silindi. SKU, önceki sürümüne geri alınır.

## <a name="revert-the-listing-price-to-production-values"></a>Üretim değerleri liste fiyatı geri döndür
Üretim değerleri liste fiyatı dönmek için aşağıdaki adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).
2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **fiyatlandırma** sekmesi.
4. Fiyatlandırması, sıfırlamak istediğiniz bir bölge seçin.

    ![Fiyatlandırma bölgeleri](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Seçilen bölge için üretim ortamında olduğu gibi bir saatlik faturalandırma modeliyle SKU'ları için tüm çekirdek fiyatı sıfırlayın. KLG faturalandırma modeliyle SKU'ları için SKU bölgede SKU için onay kutusunu seçerek kullanılabilmesini **EXTERNALLY-LICENSED (KLG) SKU'su kullanılabilirlik** bölümü.

    ![Fiyatlandırma modelleri](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Tıklayın **Birleşik Devletler FİYATINA temel AUTOPRICE diğer PAZARLARA**.

   > [!NOTE]
   > Düğmenin etiket, seçtiğiniz bölgeye bağlı olarak farklı olabilir. Amerika Birleşik Devletleri seçtik çünkü düğme etiketlenir **AUTOPRICE diğer PİYASALARI DAYALI ON FİYATLARI IN Birleşik Devletler**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Üzerinde sayfasında 1 Autoprice Sihirbazı temel Pazarı seçin ve tıklayın **ok** düğmesi.

    ![Temel Pazar](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Üzerinde 2 sayfasında, hizmet planlarını ve ölçümleri (çekirdek) seçin ve tıklayın **ok** düğmesi.

    ![Hizmet planları ve ölçümleri](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. 3. sayfasında tıklayın **iki durumlu tüm** tüm bölgeleri seçin. Veya özel bölgeler için onay kutularını el ile seçebilirsiniz. Tıklayın **ok** düğmesi.

    ![Pazarlara seçin](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Üzerinde sayfa 4, döviz kurları gözden geçirin ve tıklayın **son**. Sihirbaz, seçimlerinize uygun fiyatlandırma sıfırlar.

11. Üzerinde **fiyatlandırma** sekmesinde **özeti ve değişiklikleri görüntüleme**.
    İçin **sürümü görüntüle**seçin **taslak**ve **karşılaştırmak**seçin **üretim**. Fiyatlandırma fark görürseniz, fiyatlandırma üretim değerleri başarıyla geri döndürüldü.

    ![Özeti görüntüle ve değişiklikleri](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
13. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

## <a name="revert-the-billing-model-to-production-values"></a>Faturalandırma modeli üretim değerlere geri döndür
Faturalandırma modeli üretim değerlere geri almak için şu adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).

2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **SKU'ları** sekmesi.
4. Tıklayın **Düzenle** faturalandırma modeli geri düğmesi. Açılır pencerede seçin veya temizleyin **faturalama ve lisanslama yapılır harici olarak (diğer adıyla kendi lisansını Getir) Azure'dan** onay kutusu.

    ![Faturalandırma Düzenle](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. "Revert üretim değerleri liste fiyatı" Bu makaledeki adımları izleyin.
6. Git **Yayımla** sekmesine ve tıklayın **hazırlamaya Gönder AŞAMASINA**. Teklifiniz hazırlama ortamında test etme ile ilgili ayrıntılı yönergeler için bkz. [Market'e VM teklifinizi sınamak](marketplace-publishing-vm-image-test-in-staging.md).
7. Git hazırlık olarak teklifinizi test ettikten sonra **Yayımla** sekmesinde Yayımlama Portalı. Tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>Listelenen bir SKU üretim değerine görünürlük ayarlarını geri döndür
Listelenen bir SKU üretim değerine görünürlük ayarlarını geri almak için şu adımları izleyin:

1. Oturum [yayımlama portalı](https://publish.windowsazure.com).

2. Git **sanal makineler** sekmesini ve teklifinizi seçin.
3. Soldaki menüde **SKU'ları** sekmesi.
4. SKU'nuz seçin ve üretim değerine SKU görünürlük ayarlarını geri döndür.

    ![Görünürlük](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Değişiklikleri bitirdikten sonra tıklayın **istek onayı için gönderme için üretim** teklifiniz Market'te yeniden yayımlamak için.

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)
* [Ödeme raporlama anlama](marketplace-publishing-report-payout.md)
* [Bulut çözümü sağlayıcısı satıcınızla ıncentive değiştirme](marketplace-publishing-csp-incentive.md)
* [Market'te yayımlama yaygın sorunlarını giderme](marketplace-publishing-support-common-issues.md)
* [Bir yayımcı olarak destek alın](marketplace-publishing-get-publisher-support.md)
* [Şirket içi bir VM görüntüsü oluşturma](marketplace-publishing-vm-image-creation-on-premise.md)
* [Azure preview Portal'da Windows çalıştıran bir sanal makine oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
