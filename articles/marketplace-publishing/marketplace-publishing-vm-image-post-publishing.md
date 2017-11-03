---
title: "Azure, sanal makine görüntüsünü yönetme | Microsoft Docs"
description: "Azure, sanal makine görüntüsünü sonra ilk yayını yönetme hakkında ayrıntılı kılavuz"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: e1f90650e71345957c2d353774cb8bef62c1868b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Sanal makine teklifleri için Azure markette sonrası üretim Kılavuzu
Bu makalede, Azure Marketi dinamik sanal makine teklifte nasıl güncelleştirebileceğinizi açıklanmaktadır. Bu, bir veya daha fazla yeni SKU'ları için mevcut bir teklif ekleme işlemini kılavuzluk eder. Bu da dinamik sanal makine teklif veya SKU marketten kaldırmanın sürecinde size kılavuzluk.

Bir teklif/SKU içinde hazırlanmış sonra [Azure portal](http://portal.azure.com), şu metin kutuları değiştiremezsiniz:

* **Tanımlayıcı teklif**: içinde yayımlama portal, Git **sanal makineleri** ve teklifiniz seçin. Ardından **VM GÖRÜNTÜLERİ** > **teklif tanımlayıcı**.
* **SKU tanımlayıcı**: içinde yayımlama portal, Git **sanal makineleri** ve teklifiniz seçin. Ardından **SKU'ları** > **bir SKU eklemek**.
* **Yayımcı Namespace**: içinde yayımlama portal, Git **sanal makineleri** > **izlenecek** > **söyleyin bize hakkında şirketiniz** ("Adım 2 kayıt şirketiniz altında" bulunur) > **yayımcı Namespace** > **Namespace**.

Teklif/SKU listelenen sonra [Market](http://azure.microsoft.com/marketplace), şu metin kutuları değiştiremezsiniz:

* **Tanımlayıcı teklif**: içinde yayımlama portal, Git **sanal makineleri** ve teklifiniz seçin. Ardından **VM GÖRÜNTÜLERİ** > **teklif tanımlayıcı**.
* **SKU tanımlayıcı**: içinde yayımlama portal, Git **sanal makineleri** ve teklifiniz seçin. Ardından **SKU'ları** > **bir SKU eklemek**.
* **Yayımcı Namespace**: içinde yayımlama portal, Git **sanal makineleri** > **izlenecek** > **söyleyin bize hakkında şirketiniz** ("Adım 2 Register altında" bulunur) **yayımcı Namespace** > **Namespace**.
* **Bağlantı noktaları**: içinde yayımlama portal, Git **sanal makineleri** ve teklifiniz seçin. Ardından **VM GÖRÜNTÜLERİ** > **açık bağlantı noktalarını**.
* **Listelenen SKU(s) değişikliği fiyatlandırma**
* **Listelenen SKU(s) faturalama modeli değişikliği**
* **Listelenen SKU(s) bölgelerinin faturalama kaldırma**
* **Listelenen SKU(s) veri diski sayısı değiştirme**

## <a name="update-the-technical-details-of-a-sku"></a>Bir SKU teknik ayrıntılarını güncelleştir
Teklifiniz yeniden yayımlamanız ve yeni bir sürümü için listelenen SKU eklemek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **VM GÖRÜNTÜLERİ** sekmesi.
4. İçinde **SKU'ları** bölümünde, güncelleştirmek istediğiniz SKU bulun.
5. SKU için yeni bir sürüm numarası ekleyin ve  **+**  düğmesi. Yeni sürüm X, Y ve Z tamsayılar olduğu bir X.Y.Z biçiminde olmalıdır. Sürüm değişikliklerini yalnızca artımlı olmalıdır.
6. İçinde **OS VHD URL'si** kutusunda, işletim sistemi VHD için oluşturulan URI paylaşılan erişim imzası girin ve değişiklikleri kaydedin.

   > [!IMPORTANT]
   > Artışı/veri diski sayısı listelenen sku'sunun azaltma olamaz. Bu durumda yeni bir SKU oluşturmanız gerekir. Ayrıntılı yönergeler için bölümüne bakın [listelenen teklif altında yeni bir SKU eklemek](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
8. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![VM Görüntüleri](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Bir teklif veya bir SKU yedeğin ayrıntılarını güncelleştir
Yedeğin güncelleştirebilir (, yasal pazarlama, destek, kategoriler) Canlı teklif veya Market'te SKU ayrıntılarını.

### <a name="update-the-offer-description-and-logos"></a>Teklif açıklaması ve logolar güncelleştirme
Teklifiniz yeniden yayımlamanız ve Teklif Ayrıntıları güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **pazarlama** sekmesi.
4. Tıklatın **İngilizce (ABD)**.
5. Tıklatın **ayrıntıları** sekmesi. İçinde **açıklama** bölümünde, teklif güncelleştirme **başlık**, **Özet**, ve **uzun özeti** ve değişiklikleri kaydedin.

   > [!NOTE]
   > SKU Ayrıntılar'ı güncelleştirdiğinizde bu kısıtlamalarını dikkat edin: 
   * Teklif açıklama ve SKU açıklama için yinelenen metin girmeyin.
   * SKU başlık ve uzun Özet teklif için yinelenen metin girmeyin. 
   * SKU başlık ve özeti için yinelenen metin girmeyin.
   >
   >

    ![Ayrıntılar](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. İçinde **LOGOLAR** bölümünü **ayrıntıları** sekmesinde, logolar güncelleştirin. Logo uyduğundan emin [Azure Marketi yönergeleri](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > İsteğe bağlı bir kahramanı simgedir. Kahramanı simgeyi karşıya yükleyin değil seçebilirsiniz. Ancak, bir kahramanı simgesi yüklendikten sonra yoktur yayımlama silmek için hiçbir sağlama portalı. İzleyin [kahramanı simgesi yönergeleri](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
8. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Logo](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>SKU güncelleştirmesi
Teklifiniz yeniden yayımlamanız ve SKU ayrıntıları güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **pazarlama** sekmesi.
4. Tıklatın **İngilizce (ABD)**.
5. Tıklatın **planları** sekmesi. İçinde **SKU'ları** bölümünde, SKU güncelleştirme **başlık**, **Özet**, ve **açıklama** ve değişiklikleri kaydedin.

   > [!NOTE]
   > SKU Ayrıntılar'ı güncelleştirdiğinizde bu kısıtlamalarını dikkat edin: 
   * Teklif açıklama ve SKU açıklama için yinelenen metin girmeyin. 
   * SKU başlık ve uzun Özet teklif için yinelenen metin girmeyin. 
   * SKU başlık ve özeti için yinelenen metin girmeyin.
   >
   >
6. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
7. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Planlar](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Varolan bağlantılar değiştirin veya yeni bağlantı Ekle
Varolan bağlantılar değiştirin veya yeni bağlantılar ekleyip teklifiniz yeniden yayımlamanız için şu adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **pazarlama** sekmesi.
4. Tıklatın **İngilizce (ABD)**.
5. Tıklatın **bağlantılar** sekmesi.
6. Yeni bir bağlantı eklemek için **bağlantılar** 'yi tıklatın **+ Ekle BAĞLANTISINA**. İçinde **Bağlantı Ekle** iletişim kutusunda, bağlantı girin **başlık** ve **URL** ve değişiklikleri kaydedin. Müşterilere yardımcı olabilecek bilgilerini içeren herhangi bir bağlantıyı girebilirsiniz.
7. Güncelleştirmek veya varolan bir bağlantıyı silmek için bağlantıyı seçin ve **Düzenle** düğmesini veya **silmek** düğmesi.

   > [!NOTE]
   > Bu bağlantılar, üretim isteği işlemi sırasında doğrulanmış için bu bölümdeki girmiş olduğunuz bağlantıları düzgün çalıştığından emin olun.
   >
   >
8. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
9. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Bağlantılar](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Bağlantı Ekle](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Var olan bir örnek görüntüsünü değiştirebilir veya yeni bir örnek görüntüsü ekleme
Var olan bir örnek görüntüsünü değiştirin veya yeni örnek görüntüleri eklemek ve teklifiniz yeniden yayımlamanız için şu adımları izleyin:

> [!NOTE]
> Yalnızca bir örnek görüntü görüntülenir [Azure portal](https://portal.azure.com).
>
>

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **pazarlama** sekmesi.
4. Tıklatın **İngilizce (ABD)**.
5. Tıklatın **örnek GÖRÜNTÜLERİ** sekmesi.
6. Yeni bir örnek görüntüsü eklemek için **örnek görüntüleri** 'yi tıklatın **yeni GÖRÜNTÜYÜ karşıya** ve değişiklikleri kaydedin.

   > [!NOTE]
   > Bir örnek görüntüsü de dahil olmak üzere isteğe bağlıdır.
   >
7. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
8. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Örnek görüntüleri](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>Yasal içeriği güncelleştirme
Teklifiniz yeniden yayımlamanız ve yasal içeriği güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **pazarlama** sekmesi.
4. Tıklatın **İngilizce (ABD)**.
5. Tıklatın **yasal** sekmesi. İçinde **yasal** bölümünde, ilkeleri/koşulları kullanım güncelleştirin. Girin veya yapıştırın ilkeleri/koşullarını **Kullanım Koşulları'nı** kutusunda ve değişiklikleri kaydedin.
6. Yasal koşulları karakter sınırı 1 milyon karakterdir.
7. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
8. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Yasal Bilgiler](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Destek bilgilerini güncelleştirin
Teklifiniz yeniden yayımlamanız ve destek bilgileri güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **Destek** sekmesi.
4. İçinde **mühendislik başvurun** bölümünde, güncelleştirme mühendislik kişi ayrıntıları. Bu ayrıntılar için iç iletişiminde ortak ve Microsoft arasında yalnızca kullanılır.
5. İçinde **müşteri desteği** bölümünde, güncelleştirme desteği kişi ayrıntıları ve **destek URL**. Bu ayrıntılar için iç iletişiminde ortak ve Microsoft arasında yalnızca kullanılır.

   > [!NOTE]
   > Yalnızca e-posta desteği sağlamak istiyorsanız, bir kukla telefon numarası girin **müşteri desteği** bölümü. Bu durumda, sağladığınız e-posta yerine kullanılır.
   >
   >
6. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
7. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Destek](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>Kategoriler güncelleştir
Teklifiniz yeniden yayımlamanız ve teklifiniz için kategoriler bölümüne güncelleştirmek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **KATEGORİLERİ** sekmesi.
4. İçinde **kategorileri** bölümünde, kategoriler teklifiniz için güncelleştirme ve değişiklikleri kaydedin. Azure Market galeri için en fazla beş kategorileri seçebilirsiniz.
5. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
6. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![Kategoriler](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Listelenen bir teklif altında yeni bir SKU ekleme
Yeni bir SKU Canlı teklifiniz eklemek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **SKU'ları** sekmesi. Ardından **bir SKU eklemek**. 
4. İletişim kutusuna bir **SKU tanımlayıcı** küçük. Seçin **kendi lisans (KLG) fatura modelini** yeni SKU KLG faturalama modelini ile yayımlamak istediğiniz onay kutusunu. Aksi takdirde onay kutusunu temizleyin. Yeni bir SKU oluşturmak için onay işaretine tıklayın. KLG fatura modelini seçmediyseniz, fatura modelini saatlik olarak otomatik olarak ayarlanır. Saatlik faturalama modeli için 30 günlük ücretsiz deneme istiyorsanız seçin **bir ay** için **ücretsiz deneme sürümü kullanılabilir?** Aksi takdirde seçin **Hayır deneme**. (**Ücretsiz deneme sürümü kullanılabilir?**  yalnızca KLG yeni SKU oluşturulurken henüz seçtiyseniz görünür.)

   > [!IMPORTANT]
   > **Bu her zaman bir çözüm şablonu satın alınması çünkü bu SKU marketten Gizle** olmalıdır **Evet** *yalnızca* bir çözüm şablonu yayımlamak için onaylanmış durumunda. Aksi takdirde, bu seçenek her zaman olmalıdır **Hayır**.
   >
   >
4. Soldaki menüde tıklatın **VM GÖRÜNTÜLERİ** sekmesinde ve oluşturduğunuz yeni SKU bulabilirsiniz.
5. Yeni SKU ayarlamak için bkz: [VM görüntüsü için sertifika elde](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) Kılavuzu.
6. Yeni SKU pazarlama malzemeleri eklemek için bkz: [içerik pazarlama Market sağlamak](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Yeni SKU fiyatlandırma bilgileri eklemek için bkz: [fiyatları ayarlamak](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
9. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

    ![SKU'ları](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Bir SKU ekleme](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Listelenen SKU veri diski sayısı değiştirme
Artışı/veri diski sayısı listelenen sku'sunun azaltma olamaz. Bu durumda yeni bir SKU oluşturmanız gerekir. Ayrıntılı yönergeler için bölümüne bakın [listelenen teklif altında yeni bir SKU eklemek](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Marketten listelenen teklifi Sil
Çeşitli yönlerini canlı bir teklif kaldırmak için bir istek olması durumunda dikkate gerekir. Marketten listelenen teklif kaldırmak için destek ekibinden kılavuzluk almak için şu adımları izleyin:

1. Üzerinde bir destek bileti Yükselt [bir olay oluşturmak](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) sayfası.

2. Seçin **sorun türü** olarak **teklifleri yönetme**seçip **kategori** olarak **bir teklif ve/veya SKU üretimde zaten değiştirme**.
3. İsteği gönderin.

Destek ekibi teklif/SKU silme işleminde size kılavuzluk eder.

> [!NOTE]
> Taslak durumuna (ancak hazırlama veya üretim içinde) olsa, her zaman teklif silebilirsiniz. Üzerinde **GEÇMİŞİ** sekmesini tıklatın, **ATMAK taslak**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Marketten listelenen SKU Sil
Marketten listelenen SKU silmek için aşağıdaki adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).

2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Sol bölmede **SKU'ları** sekmesi.
4. Silin ve tıklatın istediğiniz SKU seçin **silmek** düğmesi.
5. Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** Market'te teklif yeniden yayımlamak için.
6. Market teklifi yeniden yayımlanması sonra SKU Market ve Azure portalını silinir.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>Marketten listelenen SKU geçerli sürümü Sil
Marketten listelenen SKU geçerli sürümünü silmek için aşağıdaki adımları izleyin: 

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).

2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **VM GÖRÜNTÜLERİ** sekmesi.
4. Geçerli sürümü silin ve tıklatın istediğiniz SKU seçin **silmek** düğmesi.
5. Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** Market'te teklif yeniden yayımlamak için.
6. Teklif markette yeniden yayımlanması sonra listelenen SKU geçerli sürümü Market ve Azure portalını silinir. SKU, önceki sürüme geri alınır.

## <a name="revert-the-listing-price-to-production-values"></a>Üretim değerlere listeleme fiyat geri
Liste fiyat üretim değerlere döndürmek için şu adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).
2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **fiyatlandırma** sekmesi.
4. Fiyatlandırma sıfırlamak istediğiniz bir bölge seçin.

    ![Fiyatlandırma bölgeleri](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Seçilen bölge için üretimde oldukları gibi SKU'ları için saatlik bir faturalama modeli, tüm çekirdek fiyatlar sıfırlayın. SKU'ları için KLG faturalama modeli, SKU bölgede SKU'da için onay kutusunu seçerek kullanılabilmesini **EXTERNALLY-LICENSED (KLG) SKU kullanılabilirlik** bölümü.

    ![Fiyatlandırma modelleriyle](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Tıklatın **AUTOPRICE diğer PAZARDA DAYALI FİYATLARI Amerika Birleşik DEVLETLERİ'nde**.

   > [!NOTE]
   > Düğmenin etiketi seçtiğiniz bölgeye bağlı olarak farklı olabilir. Amerika Birleşik Devletleri seçtik çünkü düğme etiketlenir **AUTOPRICE diğer PAZARDA göre ON FİYATLAR IN birleşik DURUMLARINI**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Üzerinde sayfasında 1 Autoprice Sihirbazı'nın temel Pazar'ı seçin ve tıklayın **ok** düğmesi.

    ![Temel Pazar](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Üzerinde sayfa 2, hizmet planları ve ölçümler (çekirdek) seçin ve tıklatın **ok** düğmesi.

    ![Hizmet planları ve ölçümler](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. 3 sayfasında, tıklatın **geçiş tüm** tüm bölgeler seçin. Veya belirli bölgeler için onay kutularını el ile seçebilirsiniz. Tıklatın **ok** düğmesi.

    ![Pazarda seçin](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Üzerinde sayfasında 4 döviz kuru gözden geçirin ve tıklayın **son**. Sihirbaz, seçimlerinizi göre fiyatlandırma sıfırlar.

11. Üzerinde **fiyatlandırma** sekmesini tıklatın, **Görünüm Özeti ve değişiklikleri**.
    İçin **görünüm sürüm**seçin **taslak**ve **karşılaştırmak**seçin **üretim**. Fiyatlandırma fark görürseniz, fiyatlandırma üretim değerlerin başarıyla geri döndürüldü.

    ![Özet görüntüleyin ve değişiklikleri](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
13. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

## <a name="revert-the-billing-model-to-production-values"></a>Üretim değerleri faturalama modeline geri
Üretim değerleri faturalama modeline geri döndürmek için şu adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).

2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **SKU'ları** sekmesi.
4. Tıklatın **Düzenle** fatura modelini dönmek için düğmesi. Açılır pencerede seçin veya temizleyin **faturalama ve lisanslama yapılır dışarıdan (diğer adıyla kendi lisansını Getir) Azure'dan** onay kutusu.

    ![Faturalama Düzenle](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. "Revert üretim değerlere listeleme Fiyat", bu makaledeki adımları izleyin.
6. Git **Yayımla** sekmesine ve tıklayın **hazırlama anında**. Teklifiniz hazırlama ortamında test etme konusunda ayrıntılı yönergeler için bkz: [VM teklifiniz Market için Test](marketplace-publishing-vm-image-test-in-staging.md).
7. Teklifiniz hazırlamada test ettikten sonra Git **Yayımla** yayımlama sekmesinde portal. Tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>Listelenen SKU üretim değerine görünürlük ayarını geri
Listelenen SKU üretim değerine görünürlük ayarını dönmek için şu adımları izleyin:

1. Oturum [portalını yayımlama](https://publish.windowsazure.com).

2. Git **sanal makineleri** sekmesini tıklatın ve teklifiniz seçin.
3. Soldaki menüde tıklatın **SKU'ları** sekmesi.
4. SKU seçin ve üretim değerine SKU görünürlük ayarını geri.

    ![Görünürlüğü](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Değişikliklerle bitirdikten sonra tıklatın **isteği onayı için anında İLETME için üretim** teklifiniz Market'te yeniden yayımlamak için.

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)
* [Ödeme raporlama anlama](marketplace-publishing-report-payout.md)
* [Bulut çözümü sağlayıcısı satıcınızla ıncentive değiştirme](marketplace-publishing-csp-incentive.md)
* [Market'te ortak yayımlama sorunlarını giderme](marketplace-publishing-support-common-issues.md)
* [Bir yayımcı olarak destek alma](marketplace-publishing-get-publisher-support.md)
* [Şirket içi bir VM görüntüsü oluşturma](marketplace-publishing-vm-image-creation-on-premise.md)
* [Azure Önizleme Portalı'nda Windows çalıştıran bir sanal makine oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
