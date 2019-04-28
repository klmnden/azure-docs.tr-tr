---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 01/09/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 8c7da8d04b456642b158dda77d9c745891aa18e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60620404"
---
Paylaşılan görüntü Galerisi yapısı ve kendi özel VM görüntülerinizi yönetilen etrafında kuruluş oluşturmanıza yardımcı olan bir hizmettir. Paylaşılan görüntü Galerisi kullanarak görüntülerinizi farklı kullanıcılar, hizmet sorumluları veya AD grupları kuruluşunuzun içinde paylaşabilirsiniz. Paylaşılan görüntüleri, dağıtımlarınıza daha hızlı ölçeklendirme için birden fazla bölgeyi çoğaltılabilir.

Yönetilen bir görüntü (herhangi bir bağlı veri diskleri dahil) bir tam VM veya yalnızca bir kopya olduğundan görüntü oluşturma bağlı olarak, işletim sistemi diski. Görüntüden VM oluşturduğunuzda, VHD'leri görüntüde bir kopyasını yeni VM için disk oluşturmak için kullanılır. Yönetilen bir görüntü depolama alanında kalır ve yeni sanal makineler oluşturmak için tekrar tekrar kullanılabilir.

Çok sayıda sürdürmeniz gerekir ve şirket içinde kullanılabilir hale getirmek istediğiniz yönetilen görüntüler varsa, paylaşılan görüntü Galerisi, güncelleştirme ve kendi görüntülerinizi paylaşmak kolay bir deposu olarak kullanabilirsiniz. Paylaşılan görüntü Galerisi kullanım ücretlerini yalnızca görüntüleri artı tüm ağ çıkışı maliyeti görüntüleri kaynak bölgeden yayımlanan bölgelerine çoğaltmak için kullanılan depolama maliyetleri aşağıda sunulmuştur.

Paylaşılan görüntü Galerisi özelliği, birden çok kaynak türü vardır:

| Kaynak | Açıklama|
|----------|------------|
| **Yönetilen bir görüntü** | Bu, tek başına kullanılan veya oluşturmak için kullanılan bir temel görüntü, bir **görüntü sürümü** bir görüntü galerisinde. Yönetilen bir görüntü genelleştirilmiş sanal makinelerinden oluşturulur. Yönetilen bir görüntü, birden çok sanal makine sağlamak için kullanılabilir ve artık paylaşılan görüntü sürümlerini oluşturmak için kullanılan VHD özel türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** yönetmek ve görüntüler, ancak kimlerin erişebildiğini siz denetlersiniz paylaşımı için bir depodur. |
| **Görüntü tanımı** | Görüntüleri bir galeri içindeki tanımlanır ve görüntü ve dahili olarak kullanma gereksinimleri hakkında bilgi Yürüt. Bu, görüntünün Windows veya Linux, sürüm notları ve minimum ve maksimum bellek gereksinimleri olup olmadığını içerir. Görüntü türü bir tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** bir galeri kullanırken bir VM oluşturmak için kullanın. Görüntünün birden çok sürümü, ortamınız için gerektiği şekilde olabilir. Yönetilen bir görüntü kullanırken gibi bir **görüntü sürümü** bir VM oluşturmak için görüntü sürümü sanal makine için yeni bir disk oluşturmak için kullanılır. Yansıma sürümü birden çok kez kullanılabilir. |

<br>


![Galerinizdeki görüntüyü birden fazla sürümünü nasıl olabilir gösteren grafik](./media/shared-image-galleries/shared-image-gallery.png)

### <a name="regional-support"></a>Bölgesel destek

Paylaşılan resim galerileri için bölgesel destek, sınırlı Önizleme aşamasındadır ancak zaman içinde genişletilir. Sınırlı Önizleme, galeriler oluşturabileceğiniz bölgelerin listesini ve burada herhangi bir galeri görüntüsü çoğaltabilirsiniz bölgelerin listesi aşağıda verilmiştir: 

| Galeride oluşturma  | Sürüm çoğaltın |
|--------------------|----------------------|
| Batı Orta ABD    |Tüm genel bölgelerde&#42;|
| Doğu ABD 2          ||
| Orta Güney ABD   ||
| Güneydoğu Asya     ||
| Batı Avrupa        ||
| Batı ABD            ||
| Doğu ABD            ||
| Orta Kanada     ||
|                    ||



&#42;Avustralya Orta ve Avustralya Orta 2 için çoğaltmak için abonelik izin verilenler listesinde olması gerekir. Beyaz listeye ekleme isteği için şuraya gidin: https://www.microsoft.com/en-au/central-regions-eligibility/

## <a name="scaling"></a>Ölçeklendirme
Paylaşılan görüntü Galerisi görüntülerini korumak için Azure istediğiniz yinelemeleri sayısını belirtmenizi sağlar. VM dağıtımları için tek bir kopyasını aşırı yükleme nedeniyle aşarak işleme örnek oluşturma olasılığını azaltmak farklı yinelemeler yayılabilen gibi çoklu VM dağıtım senaryolarında bu yardımcı olur.

![Görüntüleri nasıl ölçeklendirebilirsiniz gösteren grafik](./media/shared-image-galleries/scaling.png)


## <a name="replication"></a>Çoğaltma
Paylaşılan görüntü Galerisi için başka Azure bölgelerindeki görüntülerinizin otomatik olarak çoğaltılmasını sağlar. Her paylaşılan görüntü sürümü, kuruluşunuz için hangi anlamlı bağlı olarak farklı bölgelere çoğaltılabilir. Her zaman en yeni görüntüyü birden çok bölgede çoğaltma tüm eski sürümlerini yalnızca 1 bölgesinde kullanılabilir ancak bir örnektir. Bu kaydetme depolama maliyetlerine paylaşılan görüntü sürümleri için yardımcı olabilir. 

Paylaşılan görüntü sürümü çoğaltılır bölgeleri oluşturma zamanından sonra güncelleştirilebilir. Kopyalanan veri miktarı ve bölge sayısı sürüm çoğaltılır farklı bölgelere çoğaltma süresini bağlıdır. Bazı durumlarda bu işlem birkaç saat sürebilir. Çoğaltma gerçekleştiği sırada, bölge başına çoğaltma durumunu görüntüleyebilirsiniz. Bir bölgede görüntü çoğaltma tamamlandıktan sonra ardından bir VM veya VMSS bölgede, görüntü sürümü kullanarak dağıtabilirsiniz.

![Görüntüleri nasıl çoğaltabilirsiniz gösteren grafik](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access
Paylaşılan görüntü Galerisi, paylaşılan bir görüntü ve paylaşılan görüntü sürümü tüm kaynaklar olduğundan yerel Azure RBAC denetimleri yerleşik kullanarak paylaşılabilir. RBAC kullanarak, kuruluşunuzdaki diğer kullanıcılar, hizmet sorumluları ve gruplar için bu kaynakları paylaşabilirsiniz. Bu kaynakları paylaşma kapsamını aynı Azure AD kiracısı ' dir. Paylaşılan görüntü sürümü bir kullanıcının erişebileceği sonra bir VM veya bir sanal makine ölçek kümesi'nde paylaşılan görüntü sürümü içinde aynı Azure AD kiracısı için erişime sahip oldukları aboneliklerden herhangi birine dağıtabilirsiniz.  Hangi kullanıcı erişimi alır anlamanıza yardımcı olur. paylaşım matris şu şekildedir:

| Kullanıcıyla paylaşılan     | Paylaşılan Görüntü Galerisi | Paylaşılan görüntü | Paylaşılan görüntü sürümü |
|----------------------|----------------------|--------------|----------------------|
| Paylaşılan Görüntü Galerisi | Evet                  | Evet          | Evet                  |
| Paylaşılan görüntü         | Hayır                   | Evet          | Evet                  |
| Paylaşılan görüntü sürümü | Hayır                   | Hayır           | Evet                  |



## <a name="billing"></a>Faturalandırma
Paylaşılan görüntü Galerisi bu hizmeti kullanmak için fazladan bir ücret yoktur. Aşağıdaki kaynaklar için ücretlendirilirsiniz:
- Depolama maliyetini paylaşılan görüntü sürümlerini depolamak için. Bu sürümde çoğaltmaların sayısı ve bölge sayısı sürüm çoğaltılır bağlıdır.
- Çıkış ücretlerini çoğaltma kaynak bölgeden sürümünün bölgelerde için ağ.

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

**S.** Paylaşılan görüntü Galerisi genel önizlemesi için nasıl kaydolabilirim?
 
 A. Paylaşılan görüntü Galerisi genel önizlemeye kaydolmak için aşağıdaki komutları çalıştırarak her, düşündüğünüz bir paylaşılan görüntü Galerisi, görüntü tanımı veya görüntü sürümü kaynakları oluşturmak Abonelik özelliği kaydetmeniz gerekir ve Ayrıca burada görüntü sürümleri kullanan sanal makineler dağıtmayı planladığınız.

**CLI**: 

```bash 
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register --name Microsoft.Compute
```

**PowerShell**: 

```powershell
Register-AzProviderFeature -FeatureName GalleryPreview -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

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


**S.** Görüntülerim abonelikler arasında nasıl paylaşırım?
 
 A. Rol tabanlı erişim denetimi (RBAC) kullanarak abonelikler arasında görüntüleri paylaşabilir. Bir görüntü sürümü bile abonelikler arasında okuma izni herhangi bir kullanıcı görüntü sürümünü kullanarak bir sanal makineyi dağıtmak mümkün olacaktır.


**S.** Paylaşılan görüntü Galerisine mevcut Görüntümü taşıyabilir miyim?
 
 A. Evet. 3 senaryonun etkinleştirmiş olabilirsiniz görüntülerin türlerine bağlı vardır.

 Senaryo 1: Yönetilen bir görüntü varsa, daha sonra bir görüntü tanımı ve görüntü sürümü ondan oluşturabilirsiniz.

 Senaryo 2: Yönetilmeyen genelleştirilmiş görüntü varsa, yönetilen bir görüntü oluşturun ve ondan sonra bir görüntü tanımı ve görüntü sürümü oluşturun. 

 Senaryo 3: Yerel dosya sisteminizde bir VHD varsa, ardından VHD'yi karşıya yükleme, oluşturma ve resim tanımı ve görüntü sürümünden sonra yönetilen bir görüntü oluşturmak için ihtiyacınız.
- Windows sanal Makinesini VHD ise bkz [genelleştirilmiş VHD yükleme](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- Bir Linux VM için VHD'yi ise bkz [bir VHD'yi karşıya yükleme](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)


**S.** Özelleştirilmiş diskten bir görüntü sürümü oluşturabilir miyim?

 A. Hayır, şu anda özel disk görüntüleri desteklemiyoruz. Özelleştirilmiş disk varsa, yapmanız [VHD'den VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) yeni bir VM için özelleştirilmiş disk ekleyerek. Çalışan bir VM oluşturduktan sonra yönetilen bir görüntüden oluşturmak için yönergeleri takip etmeniz [Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) veya [Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Genelleştirilmiş bir yönetilen bir görüntü oluşturduktan sonra paylaşılan görüntü açıklaması ve görüntü sürümü oluşturma işlemini başlatabilirsiniz.


**S.** Paylaşılan görüntü Galerisi, görüntü tanımı ve Azure portalı üzerinden görüntü sürümü oluşturabilir miyim?

 A. Hayır, şu anda Azure Portalı aracılığıyla paylaşılan görüntü Galerisi kaynaklardan herhangi birini oluşturulmasını desteklemiyoruz. Ancak, CLI, şablonları ve SDK'lar aracılığıyla paylaşılan görüntü Galerisi kaynaklarının oluşturulmasını destekliyoruz. PowerShell de yakında kullanıma sunulacaktır.

 
**S.** Oluşturulduktan sonra da görüntü tanımı veya görüntü sürümü güncelleştirebilirim? Ne tür bir ayrıntıları değiştirebiliyorum?

 A. Her bir kaynağın güncelleştirilebilir ayrıntıları aşağıda belirtilmiştir:
 
Paylaşılan görüntü Galerisi:
- Açıklama

görüntü tanımı:
- Önerilen Vcpu
- Bellek
- Açıklama
- Sonu yaşam tarihi

Görüntü sürümü:
- Bölgesel çoğaltma sayısı
- Hedef bölgeler
- En son çıkarma
- Sonu yaşam tarihi


**S.** Oluşturulduktan sonra farklı bir aboneliğe paylaşılan görüntü Galerisi kaynağı taşıyabilirim?

 A. Hayır, paylaşılan görüntü Galerisi kaynağı farklı bir aboneliğe taşınamıyor. Ancak, Galeri görüntüsü sürümlerinde gerektiği gibi bölgelere çoğaltma mümkün olacaktır.

**S.** Görüntü sürümleri bulutlarda – çoğaltabilirsiniz Azure Çin 21Vianet, Azure Almanya ve Azure kamu Bulutu? 

 A. Hayır, yansıma sürümü bulutlarda çoğaltamazsınız.

**S.** Abonelikler arasında görüntü sürümleri çoğaltabilir miyim? 

 A. Hayır, bir abonelikte, bölgede yansıma sürümü çoğaltma ve diğer Aboneliklerdeki RBAC aracılığıyla kullanın.

**S.** Azure AD kiracılarında yansıma sürümü paylaşabilir miyim? 

 A. Hayır, şu anda paylaşılan görüntü Galerisi görüntüsünü sürümleri Azure AD kiracılarında paylaşımı desteklemez. Ancak, bunu başarmak için Azure Marketi'nde özel teklifler özelliği kullanabilir.


**S.** Ne kadar görüntü sürümleri hedef bölgeler arasında çoğaltmak için sürer?

 A. Görüntü sürümü çoğaltma süre, görüntü boyutuna ve bunun için çoğaltılmakta olan bölge sayısı tamamen bağlıdır. Ancak, görüntünün küçük tutun ve kaynak ve hedef bölgeler en iyi sonuçlar için Kapat'da, bir en iyi uygulama, önerilir. -ReplicationStatus bayrağını kullanarak çoğaltma durumunu kontrol edebilirsiniz.


**S.** Kaç tane paylaşılan resim galerileri kullanabilirsiniz bir abonelikte oluşturabilirim?

 A. Varsayılan kota şöyledir: 
- Bölge başına abonelik başına 10 paylaşılan resim galerileri
- Abonelik, bölge başına 200 görüntü tanımları
- Abonelik, bölge başına 2000 yansıma sürümü


**S.** Kaynak bölge ve hedef bölge arasındaki fark nedir?

 A. Kaynak bölgesi, görüntü sürümü oluşturulacak bölgedir ve hedef bölgeler, görüntü sürümünün bir kopyasını depolanacağı bölgelerdir. Her görüntü sürümü için yalnızca tek bir kaynak bölge olabilir. Ayrıca, bir görüntü sürümü oluşturduğunuzda kaynak bölgeyi hedef bölgeler biri olarak, geçirdiğinizden emin olun.  


**S.** Görüntü sürümü oluşturulurken kaynak bölgesi nasıl belirtebilirim?

 A. Kullanabileceğiniz bir görüntü sürümü oluşturulurken **--konum** CLI etiketinde ve **-konum** kaynak bölgeyi belirlemenize PowerShell'de etiketi. Görüntü sürümü oluşturmak için temel görüntü olarak kullanarak yönetilen bir görüntü, görüntü sürümü oluşturmak istediğiniz konumu ile aynı konumda olduğundan emin olun. Ayrıca, bir görüntü sürümü oluşturduğunuzda kaynak bölgeyi hedef bölgeler biri olarak, geçirdiğinizden emin olun.  


**S.** Her bölgede oluşturulacak görüntü sürümü yineleme sayısını nasıl belirtebilirim?

 A. Her bölgede oluşturulacak görüntü sürümü yineleme sayısını belirtebileceğiniz iki yolu vardır:
 
1. Bölge başına oluşturmak istediğiniz yinelemeleri sayısını belirten bölgesel yineleme sayısı. 
2. Varsayılan bölge sayısı başına bölgesel yineleme sayısı belirtilmemiş durumda olan genel yineleme sayısı. 

Bölgesel çoğaltma sayısını belirtmek için bu bölgede bu gibi oluşturmak istediğiniz çoğaltmaları sayısının yanı sıra konumu geçirin: "Orta Güney ABD 2 =". 

Ardından bölgesel yineleme sayısı ile her konum belirtilmemişse, varsayılan yineleme sayısını belirttiğiniz yaygın çoğaltma sayısını olacaktır. 

CLI'daki yaygın çoğaltma sayısını belirtmek için kullanın **--yineleme sayısı** değişkeninde `az sig image-version create` komutu.


**S.** Paylaşılan görüntü Galerisi nereye görüntü sürümü ve görüntü tanımı oluşturmak istiyorsunuz farklı bir konumda bir oluşturabilir miyim?

 A. Evet, bu mümkündür. Ancak, en iyi uygulama, aynı konumda kaynak grubu, paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü tutmanızı öneririz.


**S.** Paylaşılan görüntü Galerisi'ni kullanmaya yönelik ücretler nelerdir?

 A. Yansıma sürümü ve ağ çıkışı ücretleri yansıma sürümü kaynak bölgeden hedef bölgelere çoğaltma depolamak için depolama ücretleri dışında paylaşılan görüntü Galerisi hizmet kullanımı için herhangi bir ücreti yoktur.

**S.** Paylaşılan görüntü Galerisi, görüntü tanımı, görüntü sürümü ve görüntü sürümü dışında VM/VMSS oluşturma için API sürümü'ne kullanmalıyım?

 A. API sürümü 2018-04-01 kullanmanızı tavsiye ederiz görüntü sürümünü kullanarak VM ve sanal makine ölçek kümesi dağıtımları için ya da daha yüksek. Paylaşılan resim galerileri, görüntü tanımları ve yansıma sürümü ile çalışmak için API sürümü 2018-06-01 kullanmanızı öneririz. 
