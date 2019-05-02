---
title: Azure Marketi için bulut iş ortağı Portalı'nda sanal makine SKU'ları sekmesi
description: Azure Marketi'nde sanal makine teklifi oluşturmak için kullanılan SKU'ları sekmesi açıklanır.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 04/25/2019
ms.author: pabutler
ms.openlocfilehash: e8148e3a26a236039736dede5a7fbc79075731ce
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938139"
---
# <a name="virtual-machine-skus-tab"></a>Sanal makine SKU'ları sekmesi

**SKU'ları** sekmesinde **yeni teklif** sayfası, bir veya daha fazla SKU'ları oluşturma ve bunları yeni teklifinizi ilişkilendirme olanak tanır.  Farklı SKU'ları, bir çözüm özellik kümeleri, VM görüntü türleri, aktarım hızı veya ölçeklenebilirliği, faturalandırma modelleri veya başka bir özellik tarafından ayırt edebilirsiniz.


## <a name="create-a-sku"></a>Bir SKU oluşturma

Tıklayarak oluşturacak şekilde başlangıçta, yeni bir teklif ilişkili tüm SKU'lar olmaz **yeni SKU**.

![Sanal makineler için sekmesinde yeni teklif yeni SKU düğmesi](./media/publishvm_005.png)

<br/>

**Yeni SKU** iletişim kutusu görüntülenir.  Yeni SKU için tanımlayıcı girin ardından tıklayın **Tamam**. (Aşağıda tanımlayıcı adlandırma kuralları için bkz.)  **SKU'ları** sekmesinde artık düzenleme için kullanılabilir alanları görüntüler.    Alan adı eklenmiş bir yıldız (*) gerekli olduğunu gösterir.

<!-- TD: This tab has been updated, now has "Old Pricing" and "Simplified Currency Pricing" sections"! -->

![Sanal makineler için yeni teklifi formdaki SKU sekmesi](./media/publishvm_006.png)

Amaç, içeriği, aşağıdaki tabloda açıklanmıştır ve bu alanlar biçimlendirme.  Gerekli alanlar yıldız (*) indicted.

<!-- TD: I took a new screenshot, and the fields differ somewhat from description in the VM Pub Guide.  Needs review. -->

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
|  *SKU ayarları*   |    |
| **SKU KİMLİĞİ\***       | Bu SKU için tanımlayıcı.  Bu ada sahip en fazla 50 karakterden oluşan, küçük harf alfasayısal karakterler veya tire (-), ancak bir kısa çizgi ile bitemez.  Teklif yayımlandıktan sonra değiştirilemez.  |
|  *SKU ayrıntıları*   |  |
| **Başlık\***        | Teklif için görüntü marketi'ndeki için kolay ad. En fazla 50 karakter uzunluğunda. |
| **Özeti\***      | Teklif için görüntü marketi'ndeki Sözün açıklaması. En fazla 100 karakter uzunluğunda. |
| **Açıklaması\***  | Açıklama metni teklif daha ayrıntılı bir açıklama sağlar.  <!-- TD: max len/guidance? 3k characters -->  |
| **Bu SKU Gizle\*** | SKU Market'te müşterilere görünür olup olmayacağını belirtir.  Yalnızca çözüm şablonları aracılığıyla yalnızca ve satın alma için kullanılabilen tek tek istiyorsanız SKU gizlemek isteyebilirsiniz.  Ayrıca ilk test etmek veya geçici veya dönemsel teklifler için yararlı olabilir. |
| **Bulut kullanılabilirlik\*** | SKU hangi bulutlarda kullanılabilir olması gerektiğini belirler.  Varsayılan Azure genel sürümüdür.  Microsoft Azure kamu, ABD Federal, durumu, yerel veya Kabile kamu kuruluşları ve bunların sertifikalı iş ortakları için denetimli erişim ile bir devlet kurumu-topluluk bulutudur.  Kamu Bulutu hakkında daha fazla bilgi için bkz: [Hoş Geldiniz Azure kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome). |
| **Bu, bir özel SKU mi?\*** | SKU özel veya genel olup olmadığını belirtir. Varsayılan değer **Hayır** (Genel).  Daha fazla bilgi için [genel ve özel'SKU ' ları](../../cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md). |
| **Ülke/bölge kullanılabilirliği\*** | Hangi ülkelerde veya dünya bölgeleri, SKU satın alınabilir olacak belirler. En az bir bölgeyi/ülkeyi seçin. <!-- TD: Is this parameter an AMP visibility control or a contractual one, or both? --> |  
|  *Fiyatlandırma*   |  |
| **Lisans modeli\***| Kullanmak için standart faturalandırma modeli.  Seçerseniz **aylık kullanım tabanlı fatura SKU**, çekirdek başına fiyatlandırma ayrıntılarını belirtmek sağlamak için bir accordion bölümü açılır ve istediğinizi ücretsiz deneme süresi sunuyor.  Bu bölümde Ayrıca, bu fiyatlandırma zamanlamasına Excel'e içeri ve dışarı aktarmak sağlar. Daha fazla bilgi için [faturalandırma seçenekleri Azure Marketi'nde](../../billing-options-azure-marketplace.md). | 
|  *VM görüntüleri*   |  |
| **İşletim sistemi ailesi\*** | Windows veya Linux tabanlı VM çözümü olup olmadığını gösterir. |
| **İşletim sistemi türünü seçin** | Belirli bir satıcı veya belirtilen işletim sistemi sürümü. |
| **İşletim sistemi kolay adı\*** | Müşterilere görüntülenecek işletim sistemi adı.  |
| **Önerilen VM boyutları\*** | Önerilen VM boyutları en fazla altı standartlaştırılmış bir listeden seçim sağlar.  Bu öneriler, potansiyel müşteriye dikkat çekecek şekilde gösterilir ancak çözüm görüntü ile uyumlu olan herhangi bir VM boyutunu belirtebilirsiniz. | 
| **Bağlantı noktalarını Aç**| Açın ve SKU için desteklemek için protokolü için bağlantı noktaları.  Bu yapılandırmalar, ' % s'çözüm VM ağı için yapılandırdığınız sanal ağ ile eşleşmesi gerekir. Bu ayarlar, sanal makine dağıtımı sırasında etkin hale gelir. Ancak, bağlantı noktası ayarlarını, bir SKU yayımladıktan sonra değiştirilebilir. Daha fazla bilgi için [Azure portalıyla bir sanal makineye bağlantı noktaları açma](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal). <br/>Aşağıdaki varsayılan ağ eşlemeleri, tüm sanal makinelere eklenir. &emsp; Windows: 3389 -> 3389 TCP, 5986 -> 5986 TCP; &emsp; Linux: 22, 22, TCP (SSH) -&GT;. |
| **Disk sürümü**  | Disk URL'si ve disk sürüm numarası ile belirtilen ilişkili bir çözüm VM. Disk sürümü olmalıdır [semantik sürüm](https://semver.org/) biçimi: `<major>.<minor>.<patch>`.  Paylaşılan erişim imzası URI'si işletim sistemi VHD'si için oluşturulan URL'dir.  Yalnızca en yüksek disk sürüm numarasını bir SKU için SKU başına en fazla sekiz disk sürümleri ekleyebilirsiniz, ancak, Azure Market'te görünür. Diğer sürümler yalnızca API görünür olur.  <!--TD: Add more specific link to API --> <br/> **Yeni veri diski** accordion bölümü, VM'ye veri diskleri en fazla 15 olanak sağlar.  Belirli bir sanal makine sürümünü ve ilişkili veri diskleri ile bir SKU yayımladıktan sonra bu yapılandırma değiştirilemez.  Ek VM sürümlerini için SKU eklenin, ayrıca veri diskleri aynı sayıda desteklemesi gerekir. <br/> Uygulamanızın Azure tabanlı VM görüntülerini oluşturmadıysanız ekleyebileceğiniz daha sonra bu alanı güncelleştirin.  İlişkili VM kaynak oluşturma hakkında daha fazla bilgi için konudaki [VM Oluştur teknik varlıkları](./cpp-create-technical-assets.md).  
|  |  |

<!-- TD: The CPP UX warning msg indicates that underscores are also supported in these SKU IDs. I suspect this might be true for other identifiers. --> 

<br/> Tıklayın **Kaydet** ilerlemenizi kaydetmek için. Sonraki sekmede teklifinizi destekleyip desteklemediğini belirteceği [Test Sürüşü](./cpp-test-drive-tab.md).


## <a name="additional-pricing-considerations"></a>Ek fiyatlandırma dikkat edilmesi gerekenler

Yukarıda belirtilen fiyatlandırma modeline temel bir açıklamasıdır.  Bu değişiklikler yapılıyor ve yerel ve bölgesel vergileri düzenlemelere ve ilkeleri fiyatlandırma Microsoft tarafından etkilenebilir. 

### <a name="simplified-currency-pricing"></a>Basitleştirilmiş bir para birimi fiyatlandırması

1 Eylül 2018'den başlayarak, yeni bir bölüm adı verilen **Basitleştirilmiş para birimi fiyatlandırma** portalına eklenmesi. Microsoft Azure Marketi iş daha öngörülebilir fiyatlandırma ve müşterilerinizin koleksiyonları dünya genelindeki etkinleştirerek hızlandırma. Bu hızlandırma biz müşterilerinizin Fatura para birimi sayısını azaltmayı içerir.  Daha fazla bilgi için [varolan bir VM'yi Azure Marketi'nde teklif güncelleştirme](./cpp-update-existing-offer.md).


### <a name="additional-information-on-taxes-and-prices"></a>Vergi ve fiyatlar konusunda ek bilgi

* Microsoft, bazı ülkeler/bölgeler olarak sınıflandırır *vergi havale ülkeleri*.  Bu ülkeler/bölgeler, Microsoft vergileri müşterilerden toplar ardından devlet kurumuna (remits) vergileri öder.  Diğer ülkeler/bölgeler iş ortakları, müşterilerin vergileri toplamak ve devlet kurumuna vergileri ödeme genellikle yükümlü olursunuz. İkinci ülkelerde/bölgelerde satmak seçerseniz, hesaplama ve bu vergileri yerel Öde yeteneği olmalıdır.  <!-- TD: Find a good reference on taxing policies. The best I found was in the UWP section: https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps -->
* Bir teklif etkin hale gelir sonra fiyatları takımdaki herhangi biri değildir. Ancak, yine de ekleyebilir veya desteklenen bölgelerin kaldırın. 
* Microsoft ücretlerinden bağımsız müşteri standart Azure VM kullanım ücretleri, zamanlanmış bir SKU ücretlerine ek olarak.
* Fiyatlar tüm bölgeler için kullanılabilen para birimi tarifeleri üzerinden yerel para biriminde fiyatları sırasında ayarlanır.  <!-- TD: Meaning? - Offer created, published, other? -->
* Tek tek her bölgenin fiyat ayarlamak için lütfen fiyatlandırma elektronik tabloya dışarı aktarma, özel fiyatlandırma için geçerlidir ve ardından içeri aktarın. 


## <a name="next-steps"></a>Sonraki adımlar

İsteğe bağlı olarak belirteceği [Test Sürüşü](./cpp-test-drive-tab.md) bilgileri, bu özellik; destekleniyorsa Aksi takdirde, sağladığınız [Market](./cpp-marketplace-tab.md) teklifiniz için verileri.
