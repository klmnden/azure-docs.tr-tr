---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/06/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 7a0e628eed861767d1eeb50b0ded7bb3d8807328
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66271512"
---
Paylaşılan görüntü Galerisi yapısı ve yönetilen görüntülerinizi etrafında kuruluş oluşturmanıza yardımcı olan bir hizmettir. Paylaşılan resim galerileri sağlar:

- Görüntüleri yönetilen küresel çoğaltma.
- Sürüm oluşturma ve daha kolay yönetim için görüntüleri gruplandırmasıdır.
- Kullanılabilirlik alanlarını destekleyen bölgelerde bölgesel olarak yedekli depolama (ZRS) hesapları ile yüksek oranda kullanılabilir görüntüler. ZRS, bölgesel hatalarına karşı daha iyi esneklik sunar.
- Abonelikler arasında ve hatta RBAC kullanarak Active Directory (AD) kiracılar arasında paylaştırma.
- Her bölgede görüntü yinelemelerle dağıtımlarınızı ölçeklendirme.

Paylaşılan görüntü Galerisi kullanarak görüntülerinizi farklı kullanıcılar, hizmet sorumluları veya AD grupları kuruluşunuzun içinde paylaşabilirsiniz. Paylaşılan görüntüleri, dağıtımlarınıza daha hızlı ölçeklendirme için birden fazla bölgeyi çoğaltılabilir.

Yönetilen bir görüntü (herhangi bir bağlı veri diskleri dahil) bir tam VM veya yalnızca bir kopya olduğundan görüntü oluşturma bağlı olarak, işletim sistemi diski. Görüntüden VM oluşturduğunuzda, VHD'leri görüntüde bir kopyasını yeni VM için disk oluşturmak için kullanılır. Yönetilen bir görüntü depolama alanında kalır ve yeni sanal makineler oluşturmak için tekrar tekrar kullanılabilir.

Çok sayıda sürdürmeniz gerekir ve şirket içinde kullanılabilir hale getirmek istediğiniz yönetilen görüntüler varsa, paylaşılan görüntü Galerisi, görüntülerinizi paylaşmak kolay bir deposu olarak kullanabilirsiniz. 

Paylaşılan görüntü Galerisi özelliği, birden çok kaynak türü vardır:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen bir görüntü** | Tek başına kullanılan veya oluşturmak için kullanılan bir temel görüntü bir **görüntü sürümü** bir görüntü galerisinde. Yönetilen bir görüntü genelleştirilmiş sanal makinelerinden oluşturulur. Yönetilen bir görüntü, birden çok sanal makine sağlamak için kullanılabilir ve artık paylaşılan görüntü sürümlerini oluşturmak için kullanılan VHD özel türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** yönetmek ve görüntüler, ancak kimlerin erişebildiğini siz denetlersiniz paylaşımı için bir depodur. |
| **Görüntü tanımı** | Görüntüleri bir galeri içindeki tanımlanır ve görüntü ve kuruluşunuz içinde kullanmak için gereksinimleri hakkında bilgi Yürüt. Sürüm Notları ve görüntüyü Windows veya Linux, minimum ve maksimum bellek gereksinimlerini olup gibi bilgileri içerir. Görüntü türü bir tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** bir galeri kullanırken bir VM oluşturmak için kullanın. Görüntünün birden çok sürümü, ortamınız için gerektiği şekilde olabilir. Yönetilen bir görüntü kullanırken gibi bir **görüntü sürümü** bir VM oluşturmak için görüntü sürümü sanal makine için yeni bir disk oluşturmak için kullanılır. Yansıma sürümü birden çok kez kullanılabilir. |

<br>


![Galerinizdeki görüntüyü birden fazla sürümünü nasıl olabilir gösteren grafik](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>Görüntü tanımları

Görüntü tanımları, görüntü sürümleri için mantıksal gruplardır. Görüntü tanımı neden görüntü oluşturuldu hakkında bilgi, hangi işletim sistemi için ve görüntü kullanma hakkında bilgi içerir. Tüm Ayrıntılar geçici bir özel görüntü oluşturma için bir plan görüntü tanımı gibidir. Tanımından oluşturulan görüntü sürümü ancak bir görüntü tanımı bir VM dağıtmayın.


-Birlikte kullanılan her görüntü tanımı için üç parametre **yayımcı**, **teklif** ve **SKU**. Bunlar, bir özel görüntü tanımı bulmak için kullanılır. Bir veya iki, ancak tüm üç değerden paylaşan görüntü sürümleri olabilir.  Örneğin, üç görüntü tanımlar ve değerleri şunlardır:

|Görüntü Tanımı|Yayımcı|Sunduğu|Sku|
|---|---|---|---|
|myImage1|Contoso|Finans|Arka uç|
|myImage2|Contoso|Finans|Ön uç|
|myImage3|Test Etme|Finans|Ön uç|

Bu üç benzersiz değerler vardır. Biçimi nasıl şu anda yayımcı, teklif ve SKU için belirtebileceğiniz için benzer [Azure Market görüntüleri](../articles/virtual-machines/windows/cli-ps-findimage.md) Market görüntüsü en son sürümünü almak için Azure PowerShell'de. Her görüntü tanımı bu değerler benzersiz bir dizi olmalıdır.

Görüntü tanımınıza ayarlanabilir ve böylece kaynaklarınızı daha kolay izleyebilirsiniz diğer parametreler şunlardır:

* İşletim sistemi durumu - işletim sistemi durumunu üzere ayarlayabileceğiniz genelleştirilmiş veya özelleştirilmiş, ancak yalnızca genelleştirilmiş şu anda desteklenmiyor. Resimler için Sysprep Windows kullanılarak genelleştirilmiş sanal makinelerinden oluşturulmalı veya `waagent -deprovision` Linux için.
* İşletim sistemi - Windows veya Linux olabilir.
* Açıklama - neden da görüntü tanımı mevcut hakkında daha ayrıntılı bilgi vermek için açıklama kullanın. Örneğin, uygulamanın önceden yüklü olduğu, ön uç sunucusu için bir görüntü tanımı olabilir.
* EULA'sı - da görüntü tanımı için belirli bir son kullanıcı lisans sözleşmesi işaret etmek için kullanılabilir.
* Gizlilik bildirimi ve sürüm notları - sürüm notları ve gizlilik bildirimlerini Azure depolamada depolamak ve bunları da görüntü tanımı bir parçası olarak erişmek için bir URI sağlayın.
* Yaşam son tarih - eski görüntü tanımları silmek için Otomasyon kullanmak için görüntü tanımı için bir yaşam bitiş tarihi ekleyin.
* Görüntü tanımınızı oluşturduğunuzda, etiketi - etiketler ekleyebilirsiniz. Etiketler hakkında daha fazla bilgi için bkz. [kaynaklarınızı düzenlemek için etiketleri kullanma](../articles/azure-resource-manager/resource-group-using-tags.md)
* Görüntünüzü öneriler, vCPU ve bellek varsa, görüntü tanımı için en düşük ve en yüksek vCPU ve bellek önerileri - bu bilgileri ekleyebilirsiniz.
* Disk türleri - izin verilmeyen VM'niz için depolama gereksinimleri hakkında bilgi sağlayabilir. Görüntü standart HDD diskler için uygun değilse, örneğin, siz bunları izin verme listesine ekleyin.


## <a name="regional-support"></a>Bölgesel destek

Kaynak bölgeleri aşağıdaki tabloda listelenmiştir. Tüm genel bölgelerde hedef bölgeler olabilir, ancak Avustralya Orta ve Avustralya Orta 2 için çoğaltmak için abonelik izin verilenler listesinde olması gerekir. Beyaz listeye ekleme isteği için şuraya gidin: https://www.microsoft.com/en-au/central-regions-eligibility/


| Kaynak bölge |
|---------------------|-----------------|------------------|-----------------|
| Avustralya Orta   | Orta ABD EUAP | Kore Orta    | UK Güney 2      |
| Avustralya Orta 2 | Doğu Asya       | Kore Güney      | Birleşik Krallık Batı         |
| Avustralya Doğu      | Doğu ABD         | Orta Kuzey ABD | Batı Orta ABD |
| Avustralya Güneydoğu | Doğu ABD 2       | Kuzey Avrupa     | Batı Avrupa     |
| Güney Brezilya        | Doğu ABD 2 EUAP  | Orta Güney ABD | Batı Hindistan      |
| Orta Kanada      | Fransa Orta  | Güney Hindistan      | Batı ABD         |
| Doğu Kanada         | Fransa Güney    | Güneydoğu Asya   | Batı ABD         |
| Orta Hindistan       | Japonya Doğu      | UK Kuzey         | Batı ABD 2       |
| Orta ABD          | Japonya Batı      | Birleşik Krallık Güney         |                 |



## <a name="limits"></a>Limits 

Sınırları, paylaşılan resim galerileri kullanarak kaynakları dağıtmak için abonelik başına vardır:
- Bölge başına abonelik başına 100 paylaşılan resim galerileri
- Bölge başına abonelik başına 1.000 görüntü tanımları
- Abonelik, bölge başına 10.000 yansıma sürümü

Daha fazla bilgi için [sınırları karşı kaynak kullanımını denetleyin](https://docs.microsoft.com/azure/networking/check-usage-against-limits) örnekler geçerli kullanımınızı denetleme.
 

## <a name="scaling"></a>Ölçeklendirme
Paylaşılan görüntü Galerisi görüntülerini korumak için Azure istediğiniz yinelemeleri sayısını belirtmenizi sağlar. VM dağıtımları için tek bir kopyasını aşırı yükleme nedeniyle aşarak işleme örnek oluşturma olasılığını azaltmak farklı yinelemeler yayılabilen gibi çoklu VM dağıtım senaryolarında bu yardımcı olur.


Paylaşılan görüntü Galerisi ile artık bir sanal makine ölçek kümesi içinde 1.000 bir VM örneğine kadar dağıtabilirsiniz (yukarı yönetilen görüntülerle 600'den). Görüntü çoğaltmaları dağıtım daha iyi performans, güvenilirlik ve tutarlılık sağlar.  Bölge için ölçek gereksinimlerine göre farklı yineleme sayısı her hedef bölgede ayarlayabilirsiniz. Her yineleme görüntünüzü bir derin kopyası olduğundan, bu ek her yineleme ile doğrusal olarak dağıtımlarınızı ölçeklendirme yardımcı olur. Hiçbir iki görüntü anlıyoruz veya bölgeler aynıdır, ancak İşte bizim genel kılavuz bir bölgeye çoğaltma kullanma hakkında:

- Eşzamanlı olarak oluşturduğunuz her 20 VM için bir çoğaltma tutmak öneririz. Örneğin, aynı anda bir bölgede aynı görüntü kullanılarak 120 VM'ler oluşturuyorsanız, görüntünüzü en az 6 kopyasını tutmak öneririz. 
- En fazla 600 örnekleri ile her ölçek kümesi dağıtımı için en az bir çoğaltma tutmanızı öneririz. Örneğin, 5 ölçek kümeleri aynı anda tek bir bölgede aynı görüntü kullanılarak 600 VM örneğine sahip her oluşturuyorsanız görüntünüzü en az 5 kopyasını tutmak öneririz. 

Görüntü boyutu, içerik ve işletim sistemi türü gibi faktörleri nedeniyle çoğaltma sayısı fazladan her zaman önerilir.


![Görüntüleri nasıl ölçeklendirebilirsiniz gösteren grafik](./media/shared-image-galleries/scaling.png)



## <a name="make-your-images-highly-available"></a>Görüntülerinizin yüksek oranda kullanılabilir yap

[Azure bölgesel olarak yedekli depolama (ZRS)](https://azure.microsoft.com/blog/azure-zone-redundant-storage-in-public-preview/) bölgede bir kullanılabilirlik alanı hataya karşı dayanıklılığı sağlar. Paylaşılan görüntü Galerisi genel kullanılabilirlikle ZRS hesapları kullanılabilirlik alanları, görüntülerinizi depolamak seçebilirsiniz. 

Ayrıca her hedef bölgeler için hesap türünü seçebilirsiniz. Varsayılan depolama hesabı türü için Standard_LRS olsa da, kullanılabilirlik alanları için Standard_ZRS seçebilirsiniz. ZRS bölgesel kullanılabilirliği [burada](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs).

![ZRS gösteren grafik](./media/shared-image-galleries/zrs.png)


## <a name="replication"></a>Çoğaltma
Paylaşılan görüntü Galerisi için başka Azure bölgelerindeki görüntülerinizin otomatik olarak çoğaltılmasını sağlar. Her paylaşılan görüntü sürümü, kuruluşunuz için hangi anlamlı bağlı olarak farklı bölgelere çoğaltılabilir. Her zaman en yeni görüntüyü birden çok bölgede çoğaltma tüm eski sürümlerini yalnızca 1 bölgesinde kullanılabilir ancak bir örnektir. Bu kaydetme depolama maliyetlerine paylaşılan görüntü sürümleri için yardımcı olabilir. 

Paylaşılan görüntü sürümü çoğaltılır bölgeleri oluşturma zamanından sonra güncelleştirilebilir. Kopyalanan veri miktarı ve bölge sayısı sürüm çoğaltılır farklı bölgelere çoğaltma süresini bağlıdır. Bazı durumlarda bu işlem birkaç saat sürebilir. Çoğaltma gerçekleştiği sırada, bölge başına çoğaltma durumunu görüntüleyebilirsiniz. Bir bölgede görüntü çoğaltma tamamlandıktan sonra ardından bir VM veya bölgede, görüntü sürümü kullanarak ölçek kümesi dağıtabilirsiniz.

![Görüntüleri nasıl çoğaltabilirsiniz gösteren grafik](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access

Paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü tüm kaynaklar olduğundan yerel Azure RBAC denetimleri yerleşik kullanarak paylaşılabilir. RBAC kullanarak bu kaynakları diğer kullanıcılara, hizmet sorumluları ve grupları paylaşabilir. Hatta, içinde oluşturuldukları Kiracı dışındaki kişilere erişim paylaşabilirsiniz. Paylaşılan görüntü sürümü bir kullanıcının erişebileceği sonra bir sanal makine veya sanal makine ölçek kümesi dağıtabilirsiniz.  Hangi kullanıcı erişimi alır anlamanıza yardımcı olur. paylaşım matris şu şekildedir:

| Kullanıcıyla paylaşılan     | Paylaşılan Görüntü Galerisi | Görüntü Tanımı | Görüntü sürümü |
|----------------------|----------------------|--------------|----------------------|
| Paylaşılan Görüntü Galerisi | Evet                  | Evet          | Evet                  |
| Görüntü Tanımı     | Hayır                   | Evet          | Evet                  |

En iyi deneyim için Galeri düzeyinde paylaşımı öneririz. Tek tek yansıma sürümü paylaşımı önermiyoruz. RBAC hakkında daha fazla bilgi için bkz: [RBAC kullanarak Azure kaynaklarına erişimi yönetme](../articles/role-based-access-control/role-assignments-portal.md).

Görüntü ayrıca, uygun ölçekte bile çok kiracılı uygulama kaydı kullanılarak kiracılar genelinde paylaşılabilir. Kiracılar genelinde görüntülerini paylaşma hakkında daha fazla bilgi için bkz. [galeri VM görüntüleri Azure kiracılar arasında paylaşmak](../articles/virtual-machines/linux/share-images-across-tenants.md).

## <a name="billing"></a>Faturalandırma
Paylaşılan Görüntü Galerisi hizmetini kullanırken ekstra ücret ödemezsiniz. Aşağıdaki kaynaklar için ücretlendirilirsiniz:
- Depolama maliyetini paylaşılan görüntü sürümlerini depolamak için. Çoğaltma görüntü sürümü sayısı ve sürüm çoğaltılır bölge sayısı maliyeti bağlıdır. Örneğin, 2 görüntüler varsa ve her ikisi de 3 bölgeye çoğaltılır, ardından, kendi boyutunu temel alan 6 yönetilen diskler için değiştirilecek. Daha fazla bilgi için [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/).
- Çıkış ücretlerini kaynak bölgesinden ilk görüntü sürümü çoğaltılmış bölgelere çoğaltma için ağ. Herhangi bir ek ücret olduklarından izleyen yinelemeler bölge içinde işlenir. 

## <a name="updating-resources"></a>Kaynaklar güncelleştiriliyor

Oluşturulduktan sonra görüntü Galerisi kaynakları için bazı değişiklikler yapabilirsiniz. Sınırlı şunlardır:
 
Paylaşılan görüntü Galerisi:
- Açıklama

görüntü tanımı:
- Önerilen Vcpu
- Önerilen bellek
- Açıklama
- Sonu yaşam tarihi

Görüntü sürümü:
- Bölgesel çoğaltma sayısı
- Hedef bölgeler
- Son Dışla
- Sonu yaşam tarihi


## <a name="sdk-support"></a>SDK desteği

Aşağıdaki Sdk'lardan, paylaşılan resim galerileri oluşturmayı destekler:

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/azure-arm-compute/?view=azure-node-latest)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [Go](https://docs.microsoft.com/go/azure/)

## <a name="templates"></a>Şablonlar

Paylaşılan görüntü Galerisi kaynak şablonlarını kullanarak oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan bir görüntü Galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan bir görüntü galerisinde bir görüntü tanımı oluşturun](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan bir görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resmi sürümden bir VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**S.** Tüm paylaşılan görüntü Galerisi kaynakları'ı abonelikler arasında nasıl listeleyebilirsiniz? 
 
 A. Azure portalında erişimi olmasını Aboneliklerdeki tüm paylaşılan görüntü Galerisi kaynakları listelemek için aşağıdaki adımları izleyin:

1. [Azure portalı](https://portal.azure.com) açın.
1. Git **tüm kaynakları**.
1. Tüm kaynakları listelemek altında istediğiniz abonelikleri seçin.
1. Kaynak türü için konum **özel galeri**.
 
   Görüntü tanımları ve yansıma sürümü görmek için de seçmeniz **gizli türleri Göster**.
 
   İzniniz Aboneliklerdeki tüm paylaşılan görüntü Galerisi kaynakları listelemek için Azure CLI içinde aşağıdaki komutu kullanın:

   ```bash
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
   ```


**S.** Paylaşılan görüntü Galerisine mevcut Görüntümü taşıyabilir miyim?
 
 A. Evet. 3 senaryonun etkinleştirmiş olabilirsiniz görüntülerin türlerine bağlı vardır.

 Senaryo 1: Yönetilen bir görüntü varsa, daha sonra bir görüntü tanımı ve görüntü sürümü ondan oluşturabilirsiniz.

 Senaryo 2: Yönetilmeyen genelleştirilmiş görüntü varsa, yönetilen bir görüntü oluşturun ve ondan sonra bir görüntü tanımı ve görüntü sürümü oluşturun. 

 Senaryo 3: Yerel dosya sisteminizde bir VHD varsa, ardından VHD'yi karşıya yükleme, oluşturma ve resim tanımı ve görüntü sürümünden sonra yönetilen bir görüntü oluşturmak için ihtiyacınız.
- Windows sanal Makinesini VHD ise bkz [genelleştirilmiş VHD yükleme](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- Bir Linux VM için VHD'yi ise bkz [bir VHD'yi karşıya yükleme](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)


**S.** Özelleştirilmiş diskten bir görüntü sürümü oluşturabilir miyim?

 A. Hayır, şu anda özel disk görüntüleri desteklemiyoruz. Özelleştirilmiş disk varsa, yapmanız [VHD'den VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) yeni bir VM için özelleştirilmiş disk ekleyerek. Çalışan bir VM oluşturduktan sonra yönetilen bir görüntüden oluşturmak için yönergeleri takip etmeniz [Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) veya [Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Genelleştirilmiş bir yönetilen bir görüntü oluşturduktan sonra paylaşılan görüntü açıklaması ve görüntü sürümü oluşturma işlemini başlatabilirsiniz.

 
**S.** Oluşturulduktan sonra farklı bir aboneliğe paylaşılan görüntü Galerisi kaynağı taşıyabilirim?

 A. Hayır, paylaşılan görüntü Galerisi kaynağı farklı bir aboneliğe taşınamıyor. Ancak, Galeri görüntüsü sürümlerinde gerektiği gibi bölgelere çoğaltma mümkün olacaktır.

**S.** Görüntü sürümleri bulutlarda – çoğaltabilirsiniz Azure Çin 21Vianet, Azure Almanya ve Azure kamu Bulutu? 

 A. Hayır, yansıma sürümü bulutlarda çoğaltamazsınız.

**S.** Abonelikler arasında görüntü sürümleri çoğaltabilir miyim? 

 A. Hayır, bir abonelikte, bölgede yansıma sürümü çoğaltma ve diğer Aboneliklerdeki RBAC aracılığıyla kullanın.

**S.** Azure AD kiracılarında yansıma sürümü paylaşabilir miyim? 

 A. Evet, kiracılar genelinde kişilerle paylaşmak için RBAC kullanabilirsiniz. Ancak, uygun ölçekte paylaşmak için bkz: "Azure kiracılarında paylaşımı galeri görüntüleri" kullanarak [PowerShell](../articles/virtual-machines/windows/share-images-across-tenants.md) veya [CLI](../articles/virtual-machines/linux/share-images-across-tenants.md).


**S.** Ne kadar görüntü sürümleri hedef bölgeler arasında çoğaltmak için sürer?

 A. Görüntü sürümü çoğaltma süre, görüntü boyutuna ve bunun için çoğaltılmakta olan bölge sayısı tamamen bağlıdır. Ancak, görüntünün küçük tutun ve kaynak ve hedef bölgeler en iyi sonuçlar için Kapat'da, bir en iyi uygulama, önerilir. -ReplicationStatus bayrağını kullanarak çoğaltma durumunu kontrol edebilirsiniz.


**S.** Kaynak bölge ve hedef bölge arasındaki fark nedir?

 A. Kaynak bölgesi, görüntü sürümü oluşturulacak bölgedir ve hedef bölgeler, görüntü sürümünün bir kopyasını depolanacağı bölgelerdir. Her görüntü sürümü için yalnızca tek bir kaynak bölge olabilir. Ayrıca, bir görüntü sürümü oluşturduğunuzda kaynak bölgeyi hedef bölgeler biri olarak, geçirdiğinizden emin olun.  


**S.** Görüntü sürümü oluşturulurken kaynak bölgesi nasıl belirtebilirim?

 A. Kullanabileceğiniz bir görüntü sürümü oluşturulurken **--konum** CLI etiketinde ve **-konum** kaynak bölgeyi belirlemenize PowerShell'de etiketi. Görüntü sürümü oluşturmak için temel görüntü olarak kullanarak yönetilen bir görüntü, görüntü sürümü oluşturmak istediğiniz konumu ile aynı konumda olduğundan emin olun. Ayrıca, bir görüntü sürümü oluşturduğunuzda kaynak bölgeyi hedef bölgeler biri olarak, geçirdiğinizden emin olun.  


**S.** Her bölgede oluşturulacak görüntü sürümü yineleme sayısını nasıl belirtebilirim?

 A. Her bölgede oluşturulacak görüntü sürümü yineleme sayısını belirtebileceğiniz iki yolu vardır:
 
1. Bölge başına oluşturmak istediğiniz yinelemeleri sayısını belirten bölgesel yineleme sayısı. 
2. Varsayılan bölge sayısı başına bölgesel yineleme sayısı belirtilmemiş durumda olan genel yineleme sayısı. 

Bölgesel çoğaltma sayısını belirtmek için bu bölgede oluşturmak istediğiniz çoğaltmaları sayısının yanı sıra konumu geçirin: "Orta Güney ABD 2 =". 

Ardından bölgesel yineleme sayısı ile her konum belirtilmemişse, varsayılan yineleme sayısını belirttiğiniz yaygın çoğaltma sayısını olacaktır. 

CLI'daki yaygın çoğaltma sayısını belirtmek için kullanın **--yineleme sayısı** değişkeninde `az sig image-version create` komutu.


**S.** Paylaşılan görüntü Galerisi nereye görüntü sürümü ve görüntü tanımı oluşturmak istiyorsunuz farklı bir konumda bir oluşturabilir miyim?

 A. Evet, olabilir. Ancak, en iyi uygulama, aynı konumda kaynak grubu, paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü tutmanızı öneririz.


**S.** Paylaşılan görüntü Galerisi'ni kullanmaya yönelik ücretler nelerdir?

 A. Yansıma sürümü ve ağ çıkışı ücretleri yansıma sürümü kaynak bölgeden hedef bölgelere çoğaltma depolamak için depolama ücretleri dışında paylaşılan görüntü Galerisi hizmet kullanımı için herhangi bir ücreti yoktur.

**S.** Paylaşılan görüntü Galerisi, görüntü tanımı, görüntü sürümü ve görüntü sürümü dışında VM/VMSS oluşturma için API sürümü'ne kullanmalıyım?

 A. API sürümü 2018-04-01 kullanmanızı tavsiye ederiz görüntü sürümünü kullanarak VM ve sanal makine ölçek kümesi dağıtımları için ya da daha yüksek. Paylaşılan resim galerileri, görüntü tanımları ve yansıma sürümü ile çalışmak için API sürümü 2018-06-01 kullanmanızı öneririz. Bölgesel olarak yedekli depolama (ZRS) gerektiren 2019-03-01 sürümü veya üzeri.
