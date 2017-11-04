---
title: "Hazırlama ve teklifiniz Azure Marketi dağıtımını test | Microsoft Docs"
description: "Pazar içerik sağlayarak, fiyatlandırma planları yapılandırma ve Azure Marketi dağıtmadan önce teklifiniz test etme hakkında ayrıntılı yönergeler."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3ccd2448-895b-477e-adf6-ab655a21d2fa
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/17/2016
ms.author: hascipio
ms.openlocfilehash: 7db86716cdf8f9eb921c3c1813970acae7a3016b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="complete-the-offer-creation-with-marketing-content"></a>Teklif oluşturma pazarlama içerikle tamamlayın
Yayımlama işleminin bu adımında belirli pazarlama içeriği ve teklif ve/veya Azure Marketi SKU ayrıntılarını sağlamanız gerekir. Örneğin, ürün, şirket logoları, fiyat planları, plan ayrıntılarını ve teklif ve/veya SKU için hazırlama göndermek gerekli diğer bilgileri açıklamasını sağlayacaktır. Bu bilgiler, Azure portalında pazarlama içeriği olarak kullanılır. Bu işlemde başlayacak [yayımlama portalında][link-pubportal].

## <a name="step-1-provide-marketplace-marketing-content"></a>1. adım: içerik pazarlama Market sağlayın
**İngilizce varsayılandır ve yalnızca dili desteklenir.** Lütfen alanları sağlanan tüm bilgileri İngilizce olduğundan emin olun. Hazırlamaya göndermeden önce tüm bilgileri istediğiniz zaman düzenleyebilirsiniz.

1. Yayımlama Portalı gidin [https://publish.windowsazure.com](https://publish.windowsazure.com).
2. Sol menüsünde **pazarlama** sekmesi.
3. Ana panelinde tıklatın **İngilizce (ABD)** düğmesi.
   
   > [!IMPORTANT]
   > Tüm alanlar, hazırlama için anında yapabilmek görüntüleri dahil olmak üzere girişleri, olması gerekir.
   > 
   > 

### <a name="details-and-plans"></a>Ayrıntılar ve planları
1. Teklif başlığı (en fazla 50 karakter) girin, özeti (en fazla 100 karakter) sunmak, uzun özeti (en fazla 256 karakter) teklifi, teklif açıklaması (en fazla 1300 karakter), logolar altında **ayrıntıları** sekmesi
2. Planı başlık (en fazla 50 karakter), planı özeti (en fazla 100 karakter) girin, plan açıklaması (en fazla 2000 karakter) altında **planları** sekmesi.
   
   > [!NOTE]
   > Aşağıdaki özeti, uzun Özet biçimlendirmek için HTML etiketleri ve planları ve teklif açıklamasını kullanabilirsiniz. İzin verilen HTML etiketleri, h1, h2, h3, h4, h5, p, ol, ul, li, bir [hedef | href], güçlü, em, b, ediyorum.
   > 
   > 
3. Teklif ve planı açıklama altında yinelenen metin girmeyin.
4. Planın başlık ve teklif uzun özeti altında yinelenen metin girmeyin.
5. Planı başlığı altında yinelenen metni girin değil ve Özet sunar.
6. Birden çok planına sahip bir teklif için aynı planı başlıkları girmeyin.
7. (Yayımlama Portalı'nda belirtilen) gerekli belirtimleri görüntülerini PNG biçiminde, her boyut için bir tane karşıya yükleyin.
8. Logolar aşağıda belirtilen Azure Marketi logo yönergeleri izleyin emin olun.
   
   ![Çizim](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-details-02.png)

**Azure Market Logo yönergeleri**

Yayımlama portalında karşıya logolar izlemeniz gereken yönergeleri aşağıda:

* Azure tasarım basit renk paletini sahiptir. Sayı birincil ve ikincil renkleri logonuzu düşük tutun.
* Azure portalının Tema renkleri beyaz ve siyah. Bu nedenle bu renkler, logolar arka plan rengi olarak kullanmaktan kaçının. Logolar Azure portalında belirgin hale getirecektir bazı renk kullanın. Basit birincil renkleri öneririz. **Saydam arka plan kullanıyorsanız, logolar/metin olmadığından beyaz veya siyah ya da mavi emin olun.**
* Gradyan arka planı logosunu kullanmayın.
* Metin, bile, şirketiniz veya marka adı logosunu yerleştirmez. Logonuzun Görünüm ve yapısını 'düz' olmalı ve gradyan kaçınmalısınız.
* Logo uzatılabilir değil.
* Küçük logo 40 X 40 boyutunu olmalıdır piksel
* Orta logosu boyutu 90 X 90 olmalıdır piksel
* Büyük logo 115 X 115 boyutunu olmalıdır piksel
* Geniş logosu 255 X 115 boyutunu olmalıdır piksel
* Kahramanı logosu 815 X 290 boyutunu olmalıdır piksel

> [!NOTE]
> Kahramanı logosu isteğe bağlıdır. Yayımcı kahramanı logosu yüklemek değil seçebilirsiniz. Ancak bir kez karşıya yüklenen kahramanı simgesi yayımlama silinemiyor portal. O anda iş ortağı kahramanı simgeler için Azure Marketi yönergeleri izlemeniz gerekir.
> 
> 

  ![Çizim](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-details-03.png)

**(İsteğe bağlı) kahramanı logo simgesini için ek yönergeler**

* Kahramanı logosu isteğe bağlıdır. Yayımcı kahramanı logosu yüklemek değil seçebilirsiniz. **Ancak bir kez karşıya yüklenen kahramanı simgesi yayımlama silinemiyor portal. O anda iş ortağı üretime kahramanı simgeler başka teklif Onaylanmadı için Azure Marketi yönergeleri izlemeniz gerekir.**
* Yayımcı görünen adı, planı başlık ve uzun Özet teklif beyaz yazı tipi rengi görüntülenir. Bu nedenle açık bir renk kahramanı simgesi arka planda tutma kaçınmalısınız. Siyah, beyaz ve saydam arka plan kahramanı simgelerini izin verilmiyor.
* Yayımcı görünen adı, teklif gider sonra başlık, uzun Özet teklif ve Oluştur düğmesine program aracılığıyla içinde kahramanı logo gömülü planının. Bu nedenle kahramanı logosu tasarlarken herhangi bir metin girmemeniz. Yalnızca (örn. yayımcı görünen adı, planı başlık, uzun Özet teklif) metni program aracılığıyla tarafımızca orada dahil edilir çünkü boş alanı sağ tarafta bırakın. Metni boş alanı 415 x 100 sağdaki olmalıdır (ve 370px soluna göre uzaklığını).
  
  ![Çizim](media/marketplace-publishing-push-to-staging/pubportal-herobanner.png)

### <a name="links"></a>Bağlantılar
Üzerinde **bağlantılar** sekmesinde sol çubukta, müşterilere yardımcı olabilecek bilgiler ile tüm bağlantılar girin. Her bağlantı için bir ad ve URL girin.

![Çizim](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-link-01.png)

### <a name="sample-images-optional"></a>Örnek görüntü (isteğe bağlı)
> [!NOTE]
> Bir örnek görüntüsü de dahil olmak üzere isteğe bağlı bir adımdır.
> Birden çok örnek görüntüsü yayımlama yükleyebilirsiniz olsa bile portal, yalnızca bir görüntü (rastgele sistem tarafından seçilir) Azure portalında gösterilen. Bu nedenle, en çok bir örnek görüntüyü karşıya öneririz.
> 
> 

Üzerinde **örnek görüntüleri** sekmesinde soldaki menüden, tıklatarak yeni bir görüntü karşıya **yeni görüntüyü karşıya yüklemeden**. Varolan bir görüntü varsa ve değiştirmek istediğiniz **Değiştir görüntü**.

![Çizim](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-sampleimg-01.png)

### <a name="legal"></a>Yasal Bilgiler
Üzerinde **yasal** sekmesinde, ilkeleri/koşulları kullanım için bağlantı sağlayın. Girin veya koşullarını büyük yapıştırın **Kullanım Koşulları'nı** kutusu. Yasal koşulları karakter sınırı 1.000.000 karakterdir.

![Çizim](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-legal-01.png)

**Not:** sanal makine teklifleri için bir teklif/SKU Azure Portalı'nda hazırlanan sonra aşağıda belirtilen alanları değiştiremezsiniz:

* **Teklif tanımlayıcısı:** [-> sanal makineleri portalını yayımlama teklifiniz -> VM görüntüleri -> sekmesi -> Teklif tanımlayıcısı]
* **SKU tanımlayıcısı:** [-> sanal makineleri portalını yayımlama teklifiniz -> Seç -> sekmesi -> SKU'ları bir SKU Ekle]
* **Yayımcı Namespace:** [-> sanal makineleri portalını yayımlama sekmesi -> şirketiniz (bulundu altında "Adım 2 kayıt şirketiniz") Hakkımızda -> iletin izlenecek -> yayımcı Namespace Namespace ->]

Teklif/SKU Azure Marketi'nde listelenen sonra sanal makine teklifleri için aşağıda belirtilen alanları değiştiremezsiniz:

* **Teklif tanımlayıcısı:** [-> sanal makineleri portalını yayımlama teklifiniz -> Seç -> VM görüntüleri -> Teklif tanımlayıcısı]
* **SKU tanımlayıcısı:** [-> sanal makineleri portalını yayımlama teklifiniz -> Seç -> sekmesi -> SKU'ları bir SKU Ekle]
* **Yayımcı Namespace:** [-> sanal makineleri portalını yayımlama sekmesi -> Walkthrough -> söyleyin bize hakkında şirketiniz (bulundu altında adım 2 Kaydet) yayımcı Namespace Namespace ->]
* **Bağlantı noktaları:** [-> sanal makineleri portalını yayımlama teklifiniz -> VM görüntüleri -> sekme bağlantı noktalarını Aç ->]
* **Listelenen SKU(s) fiyatlandırma değişikliği**
* **Listelenen SKU(s) model değişikliği faturalama**
* **Listelenen SKU(s) bölgelerinin faturalama kaldırma**
* **Listelenen SKU(s) veri diski sayısı değiştirme**

## <a name="step-2-set-your-prices"></a>2. adım: fiyatları ayarlama
### <a name="pricing-models"></a>Fiyatlandırma modelleriyle
| Fiyatlandırma modeli | Açıklama |
| --- | --- |
| Taban |Düz aylık oranı satın alma aynı anda Ücretli; Örneğin, 10/ ay. |
| Tüketim (paketini Kullanım, ölçüm) |Teklif yayımcı tarafından tanımlanan kullanım başına ücret ödersiniz. Bir kullanıcının veya proration yapma yeteneği kesir kavramına olduğundan kullanıcı, vb. başına bilgisayar başına fazla kullanım tanımlanamaz. Kullanım saatlik olarak başka bir iş ortağı tarafından raporlanır. Müşterinin ödemesi adresindeki Önden LIKE aylık planları aksine aylık faturalama döngüsünün. |
| Ücretsiz deneme |Müşteri, ücretsiz, sınırlı bir süre için kullanın ve normal oranları bundan sonra ödeme olabilir. |
| Ücretsiz katmanı |Planı her zaman ücretsizdir. |
| Geçiş (paketini dönüştürme veya Yükseltme/indirgeme) planı |Bir kullanıcı kendi geçerli planından kabul edilebilir başka bir plana taşımak kavramı; iş ortağı tarafından tanımlanır. |

**Teklif türü tarafından modelleri fiyatlandırma**

> [!IMPORTANT]
> Belirli fiyatlandırma modelleri kullanılabilirliğini teklif türüne göre değişiklik gösterir. Aşağıdaki tabloya bakın.
> 
> 

|  | Yalnızca temel | Yalnızca tüketim | Temel + tüketim |
| --- | --- | --- | --- |
| Sanal makine görüntüsü |Hayır |Yes |Hayır |
| Geliştirici hizmeti |Evet |Evet |Evet |

### <a name="21-set-your-vm-prices"></a>2.1. VM fiyatları ayarlayın
Şu anda sanal makineler için aşağıdaki sahibiz **faturalama modelleri 3 türü:**

* **Saatlik:** müşteriler VM boyutları üzerinde yayımcılar tarafından ayarlanan oranları göre saat başına temelinde sizden ücret. Durumunda, **saatlik faturalandırma** modeli SKU, yayımcı tarafından ücret yazılım maliyet ve Microsoft tarafından ücret altyapı maliyetini Toplamı toplam fiyat olacaktır. Satın alma değerlendirirken bu toplam maliyeti müşteriye bir saatlik ve aylık fiyat görüntülenir (aşağıdaki ekran görüntüsüne bakın). **Yayımcı onlar tarafından ücret yazılım maliyet % 80'i alır.** Bu nedenle, SKU'ları için uygun şekilde önce ayarlama hesaplama fiyatları yapma Lütfen.
  
    ![Çizim](media/marketplace-publishing-push-to-staging/img2.1-01.png)
* **Ücretsiz deneme sürümü:** başka bir özellik saatlik modeli budur. Burada müşteri için ilk 30 days(Free) yazılım maliyeti VM dağıttıktan sonra sizden ücret değil. 30days sonra bunlar saatlik modelinde yayımcılar tarafından ayarlanan oranları göre saat başına temelinde ücret.
* **Getir bilgisayarınızı-kendi-lisans (KLG):** yayımcıları VM'de çalıştırılan yazılım lisans işlemlerini yönetme.

**Önemli:** teklif/SKU Azure Marketi'nde listelenen sonra aşağıda belirtilen alanları değiştiremezsiniz.

* **Listelenen SKU(s) fiyatlandırma değişikliği**
* **Listelenen SKU(s) model değişikliği faturalama**
* **Listelenen SKU(s) bölgelerinin faturalama kaldırma**
* **Listelenen SKU(s) veri diski sayısı değiştirme**
* **Teklif tanımlayıcısı:** [-> sanal makineleri portalını yayımlama teklifiniz -> Seç -> VM görüntüleri -> Teklif tanımlayıcısı]
* **SKU tanımlayıcısı:** [-> sanal makineleri portalını yayımlama teklifiniz -> Seç -> sekmesi -> SKU'ları bir SKU Ekle]
* **Yayımcı Namespace:** [-> sanal makineleri portalını yayımlama sekmesi -> Walkthrough -> söyleyin bize hakkında şirketiniz (bulundu altında adım 2 Kaydet) yayımcı Namespace Namespace ->]
* **Bağlantı noktaları:** [-> sanal makineleri portalını yayımlama teklifiniz -> VM görüntüleri -> sekme bağlantı noktalarını Aç ->]

### <a name="sell-to-countries-of-the-sku"></a>"Satış-için" ülkelerde sku'sunun
Burada, SKU'ları kullanılabilir yaptığınız dikkatlice gerekir. Bazı ülkelerde sınıflandırılır "Microsoft havale" ve diğerleri sınıflandırılır gibi olarak "ISV havale."

* "Microsoft havale" ülkede Microsoft vergileri müşterilerden toplar ve kamu (remits) vergileri ödeyen.
* "ISV havalesi" ülkede ortakları vergileri müşteriler toplama ve kamu vergileri ödeme sorumludur. "ISV havalesi" ülkede satmak seçerseniz, hesaplama ve seçtiğiniz ülkede vergileri ödeme yeteneğinin olması gerekir.

> [!NOTE]
> Veritabanlarının fiyatlandırma olarak ayarlamadığınız sürece, SKU ülkelerde kullanılamaz [portalını yayımlama](https://publish.windowsazure.com). Saatlik ve KLG SKU'ları fiyatlandırması ayarlamak için yönergeler aşağıda verilmiştir.
> 
> 

### <a name="211-how-to-setup-hourly-pricing-model-for-a-sku"></a>2.1.1 nasıl saatlik fiyatlandırma modeli SKU için Kur
Lütfen bir SKU için saatlik fiyatlandırma modeli kurulumu için aşağıda verilen adımları izleyin:

1. Oturum açma [portalını yayımlama](https://publish.windowsazure.com).
2. Gidin **sanal makineleri** sekmesinde ve teklifiniz seçin.
3. Sol taraftaki menüden **SKU'ları** sekmesi.
4. SKU "Saatlik faturalama modeli olarak" işaretlendiğinden emin olun. Değilse, tıklayın **Düzenle** fatura modelini dönmek için düğmesi. Bir pencere açılır. 'Faturalama ve lisanslama yapılır dışarıdan (diğer adıyla kendi lisansını Getir) Azure'dan' onay kutusunun işaretini kaldırın ve değişiklikleri kaydedin.
5. SKU dağıtımının ücretsiz deneme ilk 30days için etkinleştirmek istiyorsanız, ardından soru "Bir ay" seçeneğini "ücretsiz deneme sürümü var mı?" seçin Aksi takdirde, "No deneme" seçeneğini belirleyin. Şimdi aşağıda verilen adımları izleyin.
6. Sol taraftaki menüden **fiyatlandırma** sekmesi.
7. Temel bölgenizi seçin.
   
   ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.1_07.png)
8. Tüm çekirdekleri fiyatları ayarlayın. **SKU desteklemiyor olsa bile bir SKU, tüm çekirdek için fiyat sağlamanız gerekir.**
   
    ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.1_08.png)
9. Diğer bölgeler için fiyat el ile ayarlayın veya temel bölgeye göre diğer bölgelerdeki fiyatları ayarlamak için AUTOPRICE Sihirbazı'nı kullanabilirsiniz. Düğmeyi AUTOPRICE Sihirbazı'nı kullanmak için **AUTOPRICE diğer PAZARDA göre ON FİYATLAR IN birleşik DURUMLARINI.** **Not:** düğmenin etiketi seçtiğiniz bölge bağlı olarak farklı olabilir. Amerika Birleşik Devletleri bu belgeyi oluşturulurken seçtik olduğundan, "Fiyat otomatik olarak Amerika Birleşik Devletleri fiyatlarında göre diğer pazarda" aşağıdaki ekran görüntüsünde kadar düğme etiketlenir.
   
   ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.1_09.png)
10. Otomatik fiyat Sihirbazı açılır. İlk sayfa temel Pazar seçimini görüntüler. Bölümünüzü yapın ve "->" düğmesine tıklayarak sonraki sayfaya gidin.
    
    ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.1_10.png)
11. Çekirdek ve planları seçme seçeneği 2 sayfasında görüntülenir. İstenen planları seçin ve "düğmesine ->"'i tıklatın. Tıklatın **geçiş tüm** tüm seçmek için düğmesini **hizmet planları** ve **metre** veya onay kutularını el ile denetleyebilirsiniz. **SKU desteklemiyor olsa bile bir SKU, tüm çekirdek için fiyat sağlamanız gerekir.** Bu nedenle tüm çekirdek boyutlarını seçildiğinden emin olun.
    
    ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.1_11.png)
12. Sayfa 3 pazarda/bölgeleri görüntüler. Tıklatın **geçiş tüm** düğmesine tüm bölgeler seçin veya el ile bölge için kutuları işaretleyin. Sonraki sayfaya geçmek için "->" düğmesine tıklayın. **Not:** Microsoft vergi havale ülkelerde simgesi gibi bir ev tarafından gösterilir. Daha fazla ayrıntı için lütfen bölüm "Satış-için" ülkelere bu sayfanın sku'sunun bakın.
    
    ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.1_12.png)
13. Sayfa 4 döviz kuru görüntüler. Adımları tamamlamak için Son düğmesini tıklatın.

### <a name="212-how-to-setup-byol-pricing-model-for-a-sku"></a>2.1.2 Kurulum KLG fiyatlandırma modeli bir SKU nasıl
Lütfen bir SKU için fiyatlandırma modeli KLG kurmak için aşağıda verilen adımları izleyin:

1. Oturum açma [portalını yayımlama](https://publish.windowsazure.com).
2. Gidin **sanal makineleri** sekmesinde ve teklifiniz seçin.
3. Sol taraftaki menüden **SKU'ları** sekmesi.
4. SKU "SKU kendi lisansını getir"olarak işaretlendiğinden emin olun. Aksi durumda, fatura modelini dönmek için Düzenle düğmesini tıklatın. Bir pencere açılır. 'Faturalama ve lisanslama yapılır dışarıdan (diğer adıyla kendi lisansını Getir) Azure'dan' onay kutusunu işaretleyin ve değişiklikleri kaydedin.
   
   ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.2_04.png)
5. Sol taraftaki menüden **fiyatlandırma** sekmesi.
6. Temel bölgenizi seçin ve SKU bölgede EXTERNALLY-LICENSED (KLG) SKU kullanılabilirlik bölümünde SKU karşı onay kutusunu işaretleyerek kullanılabilmesini (aşağıdaki ekran görüntüsüne bakın).
   
   ![Çizim](media/marketplace-publishing-push-to-staging/img2.1.2_06.png)
7. SKU başka bölgelerde el ile kullanılabilmesini sağlar veya bu amaçla AUTOPRICE Sihirbazı'nı kullanabilirsiniz. (AUTOPRICE Sihirbazı'nı kullanımını açıklar) noktaları #9 için #13 bölümünde başvurmak **"2.1.1 SKU için fiyatlandırma modeli saatlik ayarlama"** bu sayfanın.

### <a name="22-set-your-developer-service-prices"></a>2.2. Geliştirici servis fiyatlarını ayarlama
Planları temel + burada temel aylık fiyat ve kullanım başına ödeme fiyat aşma tüketim herhangi bir bileşimi olabilir. (Aşağıda daha fazla ayrıntı için bkz.)

**Örnek:** Contoso Geliştirici hizmet teklifi

| Planlama | Fiyat | İçerir | Geçiş yolu |
| --- | --- | --- | --- |
| Ücretsiz |0/ ay |Temel işlevleri. |Başka bir plana geçirebilirsiniz |
| Bronz |10/ ay |Temel işlevleri ve 1. 000'özelliğinin X kotası. |Bronz yanı sıra, Gümüş ve altın planlarına geçirebilirsiniz |
| Bronz artı |Ücretsiz deneme süresi: 0/ ay + $0/meter01 |Temel işlevleri ve kota 10.000 X özelliğinin değeri.  Kullanılan özellik X kotası sonra müşteri meter01 aracılığıyla kullanım başına ödeme. |Gümüş artı ve altın planlarına geçirebilirsiniz |
| Bronz artı |Dönemi (paketini Ücretli ücretsiz deneme süresi): $10/ay + $ 0,05/meter01 |Temel işlevleri ve kota 10.000 X özelliğinin değeri.  Kullanılan özellik X kotası sonra müşteri meter01 aracılığıyla kullanım başına ödeme. |Gümüş artı ve altın planlarına geçirebilirsiniz |
| Gümüş |$ 0,15/meter01 |Müşteri X özelliğidir meter01 aracılığıyla kullanım başına Öde. |Bronz ve altın planlarına geçirebilirsiniz |
| Gümüş artı |20/ ay + $ 0,15/meter01 $ 0,01/meter02 |Temel işlevleri ve 10. 000'özelliğinin X ve Y özelliğinin 100 kotası.  Kullanılan özellik X kotası sonra müşteri meter01 aracılığıyla kullanım başına ödeme.  Kullanılan özellik Y kotası sonra müşteri meter02 aracılığıyla kullanım başına ödeme. |Bronz artı ve altın planlarına geçirebilirsiniz |
| Altın |1,000/ ay |X, Y, özelliğinin 1.000 özelliğinin 10.000 kota ve Z özelliğinin sınırsız. |Serbest dışında tüm planlar geçirebilirsiniz |

## <a name="step-3-provide-support-information"></a>3. adım: desteği bilgileri
İletişim ayrıntılarını iç iletişimler için ortak ve Microsoft arasında yalnızca kullanılır. Destek URL'si son kullanıcılara kullanılabilir.

1. Git **Destek** yayımlama portalında sol tarafta başlığı.
2. Bilgileri altında girin **mühendislik kişi**.
3. Bilgileri altında girin **müşteri desteği**. Yalnızca e-posta desteği sağlarsanız, bir kukla telefon numarası girin ve sağlanan e-posta yerine kullanılır.
4. Destek URL'si girin.

## <a name="step-4-choose-azure-marketplace-categories"></a>4. adım: Azure Marketi kategorileri seçin
**Kategorileri** sekmesi seçimleri dizisi sağlar. Teklifiniz bu altında kalan ve en fazla beş kategori seçebilirsiniz.

## <a name="how-your-marketing-will-appear"></a>Pazarlama nasıl görüntülenir
Aşağıdaki bilgileri pazarlama teklif nasıl kullanılacağı ayrıntılı bir görünüm olduğundan üzerinde [Azure Marketi Web sitesi](https://azure.microsoft.com/marketplace/) ve [Azure portal](https://portal.azure.com).

### <a name="azure-marketplace-website"></a>Azure Market Web sitesi
![Çizim](media/marketplace-publishing-push-to-staging/acom-catalog-01.png)

![Çizim](media/marketplace-publishing-push-to-staging/acom-catalog-02.png)

*Azure Market Web sitesinde tekliflerinden listeleme*

![Çizim](media/marketplace-publishing-push-to-staging/acom-listing-details-01.png)

*Azure Market Web sitesinde Açıklama ayrıntılarını sunar*

![Çizim](media/marketplace-publishing-push-to-staging/acom-listing-details-02.png)

*Teklif Azure Marketi Web sitesinde fiyatlandırma ayrıntıları açıklaması*

### <a name="azure-portal"></a>Azure Portal
![Çizim](media/marketplace-publishing-push-to-staging/azureportal-galleryblade-01.png)

*Azure Portalı'nda tekliflerinden listeleme*

![Çizim](media/marketplace-publishing-push-to-staging/azureportal-galleryblade-02.png)

*Azure portalında teklif açıklama Ayrıntıları*

## <a name="next-steps"></a>Sonraki adımlar
Market içeriğinizi yüklenir, şimdi teklifiniz hazırlamada testi ile gitmenizi sağlar. Teklif türü tarafından adımları farklılık olarak, aşağıdaki listeden uygun Teklif türü seçmelisiniz.

* [VM teklifiniz hazırlamada test etme](marketplace-publishing-vm-image-test-in-staging.md)
* [Çözüm şablonu teklifiniz hazırlamada test etme](marketplace-publishing-solution-template-test-in-staging.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

[img-map-acom]:media/marketplace-publishing-push-to-staging/pubportal-mapping-acom.jpg
[img-map-portal]:media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg
[img-map-link]:media/marketplace-publishing-push-to-staging/marketing-content-guide-links.jpg
[img-map-logo]:media/marketplace-publishing-push-to-staging/marketing-content-guide-logos.jpg
[img-map-title]:media/marketplace-publishing-push-to-staging/marketing-content-guide-publisher-offer.png

[link-pubportal]:https://publish.windowsazure.com
[link-push-to-production]:marketplace-publishing-push-to-production.md
