---
title: Yazar sanal makine teklifi | Microsoft Docs
description: Bir sanal makine, Azure Market'te yayımlayın.
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 3b046022990e95e65ed02880bd3fefbd78bcad28
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387370"
---
# <a name="publish-a-virtual-machine-to-azure-marketplace"></a>Bir sanal makine, Azure Market'te yayımlama

Bu makalede, bir sanal makine teklifi Azure Marketinde yayımlama için gereken adımları sağlar.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki teknik ve teknik olmayan önkoşulları bir sanal makine Azure Marketi'nde yayımlama için geçerlidir.

### <a name="technical"></a>Teknik

-   [Azure Market sanal makine görüntüsü oluşturmak için teknik Önkoşullar](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Linux VHD'si oluşturma ve yükleme](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Bir Linux VM görüntüsünden test & Oluştur](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Bir Windows VHD'si oluşturma ve yükleme ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Bir Windows VM görüntüsünden test & Oluştur](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [VHD oluşturma sırasında karşılaşılan yaygın sorunları giderme](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

-   [Azure Market görüntüleri için güvenlik önerileri](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images)


### <a name="non-technical-business-requirements"></a>Teknik olmayan (iş gereksinimlerini)

 -   Şirketinizin (veya yan kuruluşunun), Azure Marketi tarafından desteklenen bir satış ülkede bulunur

-   Ürününüzün Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanması gerekir

-   Teknik Destek kullanılabilir müşterilere ticari açıdan makul bir şekilde yapmaktan sorumlu olursunuz. Bu destek, ücretsiz, ücretli veya topluluk desteği aracılığıyla.

-   Yazılımınızı ve üçüncü taraf yazılım bağımlılıkları lisansı sağlamaktan sorumlu.

-   Teklifinizin Azure Market'te ve Azure Yönetim Portalı'nda listelenmesi ölçütleri karşılayan içeriği sağlar.

-   Azure Marketi katılım ilkeleri ve yayımcı Sözleşmesi koşullarını kabul etmiş olursunuz.

-   Uyacağınızı kabul edersiniz [kullanım](https://azure.microsoft.com/support/legal/website-terms-of-use/) , [Microsoft gizlilik bildirimi](http://www.microsoft.com/privacystatement/default.aspx), ve [Microsoft Azure sertifikası Program sözleşmesi](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="before-you-begin"></a>Başlamadan önce

Tüm önkoşulların yerine getirdikten sonra çözüm şablonu teklifinizi yazma başlayabilirsiniz. Başlamadan önce aşağıdaki teklif ve SKU bilgileri gözden geçirin.

**Teklif**

Bir Azure uygulaması teklif sunan bir yayımcıdan ürün sınıfının karşılık gelir. Azure Marketi'nde kullanılabilir olmasını istediğiniz çözüm/uygulama için yeni bir tür varsa, yeni bir teklif için en iyi yaklaşımdır. Teklif bir SKU koleksiyonudur. Her teklif, Azure Market'te kendi varlık olarak görünür.

**SKU**

SKU, bir teklife ilişkin en küçük satın alınabilir birimdir. Aynı ürün sınıf içinde sırada (teklif), SKU'ları farklı desteklenen özellikler arasında ayırt etmenize olanak sağlar. Örneğin, teklif yönetilen veya yönetilmeyen ve farklı faturalama modellerini desteklenir.

Birden çok SKU'ları, aşağıdaki senaryolarda ekleyin:
- Kendi lisansını getir (KLG) ya da Kullandıkça Öde (PAYG) gibi farklı faturalama modellerini desteklemek istediğiniz.
- Her SKU farklı özellik kümesini destekler ve her bir özellik kümesi hesabın fiyatı farklıdır.

Bir SKU üst teklifi Azure Marketi bölümünde gösterilir ve Azure Portalı'nda satın alınabilir kendi varlık olarak gösterilir.

## <a name="to-create-a-new-offer"></a>Yeni bir teklif oluşturmak için

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com/).

2.  Sol gezinti çubuğunda **+ yeni teklif**ve ardından **sanal makineler**.

    ![Sanal makineler için yeni teklif](./media/cloud-partner-portal-publish-virtual-machine/publishvm1.png)

3.  Altında **yeni teklif**seçin **Düzenleyicisi**.

![Yeni Teklif Düzenleyicisi](./media/cloud-partner-portal-publish-virtual-machine/publishvm2.png)

4.  Altında **Düzenleyicisi**, aşağıdaki görünümleri bilgileri sağlarız:
    - Teklif ayarları
    - SKU'lar
    - Market
    - Destek alanları doldurmak size bir dizi her görünümü içerir. Gerekli alanları bir kırmızı yıldız gösterilir (\*)

## <a name="to-configure-offer-settings"></a>Teklif ayarları yapılandırmak için

1. Aşağıdaki yapılandırma **Teklif kimliği** alanları ayarları sunar.

    **Teklif kimliği**

     Bir yayımcı profilinde teklif için benzersiz bir tanımlayıcı. Bu kimliği ürün URL'leri, Azure Resource Manager şablonları, görünür ve faturalandırma bildirir. Yalnızca küçük harf alfasayısal karakterler veya tire (-) kullanabilirsiniz. Kimliği tire bitemez ve en çok 50 karakter olabilir. Örneğin, bir yayımcı varsa **contoso** teklif Kimliğini içeren bir teklif yayımlar **örnek vm**, Azure Marketi'nde göründüğü **https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview**  
    >[!Note]
    >Bir teklif etkin durumda olduğunda bu alan kilitli.

    **Yayımcı kimliği**

    Yayımcı profil için bir açılır liste. Bu teklif altında yayımlamak istediğiniz profili seçin. 
    >[!Note]
    >Bir teklif etkin durumda olduğunda bu alan kilitli.

    **Ad**

    Teklifiniz için görünen adı. Bu ad, Azure Marketi'nde hem de Azure portalında gösterilir. En fazla 50 karakter olabilir. Teklif adı için aşağıdaki yönergeleri kullanın:
    -  Ürününüz için tanınabilir bir marka adı ekleyin. 
    - Nasıl teklif pazarlanmadığından olmadığı sürece, şirketinizin adını buraya dahil değildir.
    - Kendi Web sitesi bu teklif pazarlama, kendi Web sitenizin adıyla aynı adı olduğundan emin olun.

2. Seçin **Kaydet** teklif ayarları tamamlanması.

## <a name="to-create-a-sku"></a>Bir SKU oluşturma

1. Seçin **SKU'ları**. 
2. Seçin **bir SKU ekleyin** SKU kimliği girmek için SKU kimliği, içinde bir teklif SKU için benzersiz bir tanımlayıcıdır. Bu kimliği ürün URL'leri, Azure Resource Manager şablonları, görünür ve faturalandırma bildirir. SKU kimliği:
    - Yalnızca en çok 50 karakter olabilir.
    - Yalnızca küçük harf alfasayısal karakterler veya tire (-) oluşturulabilir.
    - Kimlik tire ile bitemez.

    ![Bir SKU ekleyin](./media/cloud-partner-portal-publish-virtual-machine/publishvm4.png)


## <a name="configure-sku-details"></a>SKU ayrıntılarını Yapılandır

Bir SKU ekledikten sonra SKU'ları görünümünde SKU'lar listesinde görünür. SKU ayrıntıları görmek için SKU Adı'nı seçin. Ayrıntılar görünümü, aşağıdaki alanlara bilgi eklemek için kullanabilirsiniz.

### <a name="hide-this-sku"></a>Bu SKU Gizle

SKU görünürlüğünü yönetmek için bu ayarı kullanın. "Bu SKU gizle" kapalıysa, SKU görünür [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com/) müşterilere. Yalnızca çözüm şablonları aracılığıyla ve satın alma için kullanılabilen tek tek istiyorsanız SKU gizlemek isteyebilirsiniz.

### <a name="cloud-availability"></a>Bulut kullanılabilirlik

Bu alan, SKU'nuzu kullanılabilirliğini farklı Azure bulutlarında tanımlamanızı sağlar.

**Genel Azure**

Bu SKU, Market tümleştirmesine sahip tüm genel Azure bölgelerinde müşterilere dağıtılabilir.

**Azure kamu Bulutu**

Bu SKU, Azure kamu bulutunda dağıtılabilir. Yayımlama için önce [Azure kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), yayımcılar, test ve beklendiği gibi çalışır, doğrulama öneririz. Aşama ve test, [bir deneme hesabı istek](https://azure.microsoft.com/offers/ms-azr-usgov-0044p). 

>[!Note]
>Microsoft Azure kamu, ABD Federal, eyalet, yerel veya Kabile, ve iş ortakları uygun bu kuruluşlara hizmet müşteriler için denetimli erişimi olan bir devlet kurumu-topluluk bulutudur.

### <a name="countryregion-availability"></a>Ülke/bölge kullanılabilirliği

Bu alan satışta olabilir SKU'nuz kolaylaştıracağını bölgeleri tanımlar. Burada, SKU'larınız kullanılabilir duruma dikkatlice gerekir. Bazı ülkeler "Microsoft vergi havale ülkesi" olarak sınıflandırılır.

-   "Microsoft vergi havale ülkesinde", Microsoft müşterilerin vergileri toplar ve devlet kurumuna (remits) vergileri öder.

-   Diğer ülkelerde, iş ortakları vergileri müşteriler toplama ve devlet kurumuna vergileri ödeme yükümlü olursunuz. Bu ülkelerde satmak kullanmayı tercih ederseniz hesaplamak ve bunları vergileri ödeme yeteneği olmalıdır.

![Ülke/bölge kullanılabilirliği seçin](./media/cloud-partner-portal-publish-virtual-machine/publishvm5.png)

### <a name="pricing"></a>Fiyatlandırma

İki fiyatlandırma modelleri şu anda desteklenmiyor.

#### <a name="bring-your-own-license-byol"></a>-Kendi-lisansını getir (KLG)

VM'de çalışan Yazılım Lisanslama yönettiğiniz. Microsoft, yalnızca altyapı maliyetleri ücretlendirir.

#### <a name="usage-based-monthly-billed-sku"></a>Aylık olarak faturalandırılan SKU kullanım tabanlı

Müşteriler, VM boyutlarına yayımcılar tarafından belirlenen oranları üzerinden saatlik olarak ücretlendirilirsiniz. Durumunda, **saatlik faturalandırma** modeli SKU'ları, yayımcı tarafından ücret yazılım maliyeti ve Microsoft tarafından tahsil altyapı maliyetini Toplamı toplam fiyatı olacaktır. Satın alma değerlendirirken bu toplam maliyeti bir saatlik ve aylık ücreti müşteriye görüntülenir. Faturalandırma aylık olarak bu durumda olacaktır.

Kullanım tabanlı model içinde ek ayar yapmanıza gerek vardır.

**Ücretsiz deneme**

Müşterileriniz için ücretsiz bir deneme sağlamak isteyip istemediğinizi belirtebilirsiniz.
Burada müşteri yazılım maliyeti (seçime olarak) ilk 30/90 gün için sanal Makineyi dağıttıktan sonra ücretlendirilirsiniz değil. Ücretsiz deneme süresi dolduktan sonra saatlik modelinde yayımcılar tarafından belirlenen oranları üzerinden saatlik olarak ücretlendirilirsiniz.

**Çekirdek başına fiyatlandırma**

SKU'nuz için çekirdek fiyatlandırma ayarlayabilirsiniz. Bu, bir çekirdek için taban fiyat girmeniz yeterlidir ve biz otomatik-işlem çekirdek geri kalanı için fiyatlar. Fiyatlar ABD Doları portalda girin ve biz otomatik-başka bir bölgeye yönelik fiyatları hesaplar. Diğer bölgelerde fiyatlar kullanarak doğrulayabilirsiniz **fiyatlandırma verilerini dışarı aktar**

![Çekirdek başına fiyatlandırma](./media/cloud-partner-portal-publish-virtual-machine/publishvm6.png)


**Ayrık fiyatlandırması**

Her çekirdek ayrı olarak fiyat istiyorsanız, her çekirdek kümeleri için ayrı ayrı fiyatlandırma ayarlayabilirsiniz.

![Ayrık fiyatlandırması](./media/cloud-partner-portal-publish-virtual-machine/publishvm7.png)

**İçeri dışarı aktarma fiyatlandırması**

Excel arabirimi üzerinden değişiklik yapmak için portal aracılığıyla yapılandırılan fiyatlandırma dışarı aktarmak için esnekliğiniz vardır. Bu ayrıca, bölge başına fiyatlandırma ve yerel para biriminde fiyatlandırma doğrulamanızı sağlar.
Tıklayarak **dışarı aktarma fiyatlandırması** fiyatlandırma ayrıntıları önceden doldurulmuş olan bir excel dosyası indirir. Bu excel içinde düzenleme ve ardından mümkün olacaktır **içeri aktarma fiyatlandırması** yapılan değişiklikleri aktarmak.
İçeri aktarılan fiyatlandırma portalında ücreti yansıtılır.

Bu fiyatlandırma Excel'den fiyatlar bölgelere yerel para biriminde listelenir. Kullandığımız döviz kuru günlük olarak yenilenir.

![İçeri dışarı aktarma fiyatlandırma ile döviz kuru örnekleri](./media/cloud-partner-portal-publish-virtual-machine/publishvm8.png)

>[!IMPORTANT]
>-   Fiyatlar, canlı bir teklife geçtikten sonra değiştirilemez. Bununla birlikte, yine de desteklenen bir bölge ekleme veya kaldırma mümkün olabilir.
>-   Bu fiyata ek olarak bir kullanıcıya ücretlendirilir [Azure\'s sanal makine fiyatlandırma](http://aka.ms/vmpricingdetails).
>-   Fiyatlar, fiyatları sırasında kullanılabilir para birimi oranlarını kullanarak, yerel para birimindeki tüm bölgeler için ayarlanır.
>-   Ayarlayın ya da her bölgenin fiyat tek tek görüntülemek için fiyatlandırma elektronik tabloya dışarı aktarın ve özel fiyatlandırma ile alın.

## <a name="vm-images"></a>VM Görüntüleri

Tamamlamak için sonraki bölümde VM görüntüleri bölümü olacaktır. Bu bölüme geçmeden önce hazır yayımlamak istediğiniz VHD'yi olması gerekir. VHD'nizi oluşturmanıza yardımcı olacak bazı bağlantılar aşağıda verilmiştir:

-   [Azure Market sanal makine görüntüsü oluşturmak için teknik Önkoşullar](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Oluşturma ve bir Linux VHD karşıya yükleme](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Bir Linux VM görüntüsünden test & Oluştur](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Oluşturma ve bir Windows VHD karşıya yükleme ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Bir Windows VM görüntüsünden test & Oluştur](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [VHD oluşturma sırasında karşılaşılan yaygın sorunları giderme](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

VHD hazır olduktan sonra bu formu doldurmak başlayabilirsiniz.
Aşağıda, bazı alanlar için bazı ayrıntılar verilmiştir.

### <a name="recommended-vm-sizes"></a>Önerilen VM boyutları

En fazla altı adet önerilen sanal makine boyutu seçin. Bu satın alma ve görüntünüzü dağıtmak karar müşterinin Azure Marketi'nde ve fiyatlandırma katmanı dikey penceresinden Azure portalında gösterilen önerilerdir. **Bunlar yalnızca öneridir. Müşterinin belirtilen diskler karşılar herhangi bir VM boyutunu seçebilir.**  Aşağıdaki ekran görüntüsü yakalamayı önerilen VM boyutları, bir müşteri Azure Portal'da görürsünüz gösterir.


![Önerilen VM boyutları](./media/cloud-partner-portal-publish-virtual-machine/publishvm9.png)


### <a name="open-ports"></a>Bağlantı noktalarını Aç

Açık ve kullanılabilir yaptığı istediğiniz bağlantı noktalarını belirtin. Bu VM dağıtımı sırasında bu bağlantı noktalarının açıldığından.

### <a name="adding-vm-images"></a>VM görüntüleri ekleme

Sonraki adım, SKU'nuz için bir VM görüntüsü eklemektir. SKU başına 8 adede kadar disk sürümleri ekleyebilirsiniz. Yalnızca en yüksek disk sürüm numarasını belirli bir SKU için Azure Market'te görünür. Diğer API'ler görünür olur.

Altında **Disk sürümü**seçin **+ yeni sürüm**. Bu, doldurmanız gereken aşağıdaki alanları gösterir.

#### <a name="vm-image-version"></a>VM görüntü sürümü

VM görüntü sürümü izlemesi gereken [semantik sürüm](http://semver.org/) biçimi. Sürümleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır. Farklı sku'lardaki görüntüler, farklı bir birincil ve ikincil sürüme sahip olabilir. Bir SKU içinde sürümleri yalnızca düzeltme eki sürümü (Z X.Y.Z gelen) artırmak artımlı değişiklikler olmalıdır.

#### <a name="os-vhd-url"></a>İŞLETİM SİSTEMİ VHD URL'Sİ

Girin [paylaşılan erişim imzası URI'si](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) işletim sistemi VHD'si için oluşturulan.

Bu SKU ile ilişkili veri diskleri varsa, bu diskleri seçerek eklenecek seçebilirsiniz **+ yeni veri diski** bağlantı. Bu eylem, doldurmak ek alanlar görüntülenir.

#### <a name="lun-vhd-url"></a>LUN VHD URL'Sİ

Girin [paylaşılan erişim imzası URI'si](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) veri diskiniz için.

#### <a name="lun-number"></a>LUN numarasını

Bu LUN birkaç atayın. Bu sayı, bu veri diskinin bu SKU için ayrılacaktır.

>[!Note]
>Belirli bir sanal makine sürümünü ve veri diskleri ile bir SKU yayımladıktan sonra bunlar kilitli ve değiştirilemez. SKU'ya eklenin herhangi ek VM sürümleri için onu desteklemek için gereken veri diski sayısı değiştirilemez.

#### <a name="common-sas-url-issues--fixes"></a>Ortak bir SAS URL'si sorunları ve düzeltmeleri

| Sorun                                                                 | İleti                                                                           | Düzelt                                                           |  Belgeleri bağlantısı                                                                                |
|---------------------------------------------------------------------  |-------------------------------------------------------------------------------    |-----------------------------------------------------------    |---------------------------------------------------------------------------------------------------    |
| Kopyalama hatası görüntüleri - "?" SAS url bulunamadığında                | Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor.       | Önerilen araçlar kullanarak SAS URL'sini güncelleştirme                    | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Görüntüleri kopyalama hatası - SAS URL'si değil, "s" ve "se" parametreleri   | Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor.        | Başlangıç ve bitiş tarihlerini üzerindeki ile SAS URL'sini güncelleştirme             | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Görüntüleri "sp rl SAS URL'si değil =" - kopyalama hatası                    | Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor         | "Okuma" ve "liste ayarlanan izinler ile SAS URL'sini güncelleştirme     | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Görüntüleri - SAS URL'sini kopyalama hatası vhd adında boşluk olması     | Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor.        | Beyaz boşluk olmadan SAS URL'sini güncelleştirme                       | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Görüntüleri: SAS Url Yetkilendirme hatası kopyalama hatası               | Hata: Görüntüleri kopyalanıyor. Yetkilendirme hatası nedeniyle blobu indirmek karşılaştırılamıyor     | SAS URL'sini yeniden oluştur                                        | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |


## <a name="to-configure-the-marketplace"></a>Market yapılandırmak için

Teklif için görüntülenen alanları yapılandırmak için Market görünümünü kullanın [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Önizleme abonelik kimlikleri

Teklif yayımlandığında teklif erişmesini istediğiniz Azure abonelik kimlikleri listesi. Bu teknik listelenen abonelikler test bunu yapmadan önce önizlenen teklifini izin Canlı. İş ortağı portalı beyaz liste 100 abonelikleri kadar olanak tanır.

### <a name="suggested-categories"></a>Önerilen kategorileri

En fazla 5 kategorileri, teklifinizi en iyi ile ilişkilendirilebilen sağlanan listeden seçin. Seçili kategorilerdeki teklifinizi, bulunan ürün kategorilerini eşlemek için kullanılan [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com/).

Aşağıdaki örnekler, Azure Marketi'nde hem de Azure portalında Market bilgileri gösterir.

**Azure Market**


![publishvm10](./media/cloud-partner-portal-publish-virtual-machine/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-virtual-machine/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-virtual-machine/publishvm15.png)


**Azure Portal**


![publishvm12](./media/cloud-partner-portal-publish-virtual-machine/publishvm12.png)



![publishvm13](./media/cloud-partner-portal-publish-virtual-machine/publishvm13.png)


### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut iş ortağı portalına karşıya logo için aşağıdaki yönergeleri izleyin:

-   Azure tasarımının basit bir renk paleti vardır. Logonuzu düşük tutmak için ikincil renk ve birincil sayısı.

-   Azure portal'ın Tema renkleri beyaz ve siyah. Bu renkler, logolar arka plan rengi kullanmaktan kaçının. Logo, Azure portalında belirgin hale getirir bir renk kullanın. Basit birincil renkleri öneririz.

    >[!Note] 
    >Saydam arka plan kullanıyorsanız, ardından logoları/metin beyaz, olmayan emin olun, beyaz veya mavi.

-   Logoda gradyan arka plan kullanmayın.

-   Metin logosunu yerleştirmekten kaçının. Bu, şirketinizin veya marka adı içerir. Logonuzu Görünüm ve yapısını olmalıdır *düz* gradyanlar kaçınmalısınız.

-   Logo esnetilmiş olmamalıdır.

#### <a name="hero-logo"></a>Hero logosu

Hero logosu isteğe bağlıdır. Yayımcı Hero logoyu karşıya yükleyin değil seçebilirsiniz. Ancak, logoyu karşıya yüklendikten sonra silinemez. İş ortağı Hero simgeler için Azure Marketi yönergelere uyması gerekir.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Hero logosu simgesi için yönergeler

-   Yayımcı görünen adı, planı başlık ve teklife ilişkin uzun özeti beyaz bir renkli yazı tipi kullanarak görüntülenir. Açık bir renk arka planda kullanmaktan kaçının. Siyah, beyaz ve saydam arka planlar için Hero simgeler izin verilmez.

-   Yayımcı görünen adı, plan teklife ilişkin listede, başlık, uzun Özet teklif ve Oluştur düğmesine program aracılığıyla içinde Hero logosu katıştırılır. Hero logosu tasarlarken, herhangi bir metin girmeyin. Logo sağ tarafındaki bir boşluk bırakın. Bu alanı 415 x 100 piksel olmalı ve 370 tarafından uzaklık piksel soldan.

![Kahraman logosu örneği](./media/cloud-partner-portal-publish-virtual-machine/publishvm14.png)

### <a name="lead-management"></a>Tedarik Yönetimi

Tekliften müşteri adayı yönetim ayarlarını yapılandırmak için izleyin [bu yönergeleri](./cloud-partner-portal-get-customer-leads.md).

## <a name="to-configure-support"></a>Desteğini yapılandırmak için

Aşağıdaki bilgileri sağlamak için destek görünümünü kullanın:

- Mühendislik gibi şirketiniz kişilerden destekler.
- Müşteri desteği kişiler.

## <a name="to-publish-the-offer"></a>Teklif yayımlamak için

Son adım, teklif yayımlamaktır. İzleyin [teklifinizle kullanılabilmesi için bu yönergeleri](./cloud-partner-portal-make-offer-live-on-azure-marketplace.md).
