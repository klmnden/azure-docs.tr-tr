---
title: Azure Stack sanal makinelerine giriş
description: Azure Stack sanal makineleri hakkında bilgi edinin
services: azure-stack
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: kivenkat
ms.openlocfilehash: d478ccd0895ad067657bce56469a3a61d4ea0e17
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139346"
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Azure Stack sanal makinelerine giriş

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack sanal makineleri (VM'ler), bir isteğe bağlı, ölçeklenebilir bilgi işlem kaynak türü sunar. Diğer seçimlere göre bilgi işlem ortamınız üzerinde daha fazla denetime ihtiyacınız olduğunda bir sanal makine seçebilirsiniz. Bu makalede, VM oluşturmadan önce Ayrıntılar sağlanır.

Azure Stack sanal kümeler ya da makineleri tek tek yönetmek zorunda kalmadan sanallaştırma esnekliği sunar. Ancak, yine de yapılandırma, düzeltme eki uygulama ve üzerinde çalıştığı yazılım yükleme gibi görevleri gerçekleştirerek VM'nin sürdürmeniz gerekir.

Azure Stack sanal makineleri çeşitli yollarla kullanabilirsiniz. Örneğin:

- **Geliştirme ve test**  
    Azure Stack sanal makineleri kodu için gereken belirli bir yapılandırmaya sahip bir bilgisayar oluşturmak ve bir uygulamayı test etme için hızlı ve kolay bir yolunu sunar.

- **Bulut uygulamaları**  
    Uygulamanız için isteğe bağlı dalgalanma çünkü bunu çalıştırmak için Azure Stack'te bir VM üzerinde ekonomik mantıklı olabilir. İhtiyaç duyduğunuzda oluşturulan ek VM’ler için ödeme yapar, ihtiyaç kalmadığında bunları kapatırsınız.

- **Genişletilmiş veri merkezi**  
    Azure Stack sanal ağdaki sanal makineleri kolayca kuruluşunuzun ağına veya azure'a bağlanabilir.

Uygulamanızın kullandığı ölçeği büyütün veya ihtiyaçlarınızı karşılamak için gerekli her şeyi gelir ölçeği VM'ler.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>VM oluşturmadan önce dikkat etmem gereken noktalar nelerdir?

Aynı zamanda Azure Stack'te uygulama altyapısı oluştururken her zaman çok sayıda tasarım konuları mevcuttur. Altyapınızı oluşturma işlemine başlamadan önce dikkat etmeniz gereken VM bu özellikleri şunlardır:

- Uygulama kaynaklarınızın adları.
- VM boyutu.
- Oluşturulabilecek VM'ler en fazla sayısı.
- VM çalışan işletim sistemi.
- VM'nin başlatıldıktan sonraki yapılandırması.
- VM'nin ihtiyaç duyduğu ilgili kaynaklar.

### <a name="naming"></a>Adlandırma

Bir sanal makineye atanmış bir ad ve işletim sisteminin bir parçası olarak yapılandırılan bir bilgisayar adı sahiptir. VM adı en fazla 15 karakter uzunluğunda olabilir.

İşletim sistemi diski oluşturmak için Azure Stack kullanırsanız, bilgisayar adı ve sanal makine adı aynıdır. Karşıya yükleme ve önceden yapılandırılmış bir işletim sistemini içeren kendi görüntünüzü kullanma ve bir sanal makine oluşturmak için bunu kullanın, adları farklı olabilir. Kendi görüntü dosyanızı yüklediğinizde, işletim sisteminde bilgisayar adını yapın ve sanal makine adını en iyi uygulama olarak aynı.

### <a name="vm-size"></a>VM boyutu

Kullandığınız VM'nin boyutunu, çalıştırmak istediğiniz iş yüküne göre belirlenir. Seçtiğiniz boyut işlemci gücü, bellek ve depolama kapasitesi gibi ölçütleri belirler. Azure Stack, çok çeşitli sayıda kullanım türünü desteklemek üzere boyutları sunar.

### <a name="vm-limits"></a>VM sınırları

Aboneliğinizi yerinde projeniz için çok sayıda VM dağıtımını etkileyebilecek varsayılan kota sınırları vardır. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir.

### <a name="operating-system-disks-and-images"></a>İşletim sistemi diskleri ve görüntüleri

Sanal makineler, kendi işletim sistemlerini (OS) ve verilerini depolamak için sanal sabit diskleri (VHD) kullanır. VHD bir işletim sistemi yüklemek için seçebileceğiniz görüntüler için de kullanılır.
Azure Stack çeşitli türlerde işletim sistemleri ve sürümleri ile kullanmak için bir pazar sağlar. Market görüntüleri görüntü yayımcısı, teklif, sku ve sürüm (genelde sürüm en son belirtilir.) tarafından tanımlanır

Aşağıdaki tablo, görüntü bilgilerini bulabilirsiniz bazı yollarını gösterir:

|Yöntem|Açıklama|
|---------|---------|
|Azure Stack portalı|Bir görüntüyü kullanmak istediğinizde değerler otomatik olarak belirtilir.|
|Azure Stack PowerShell|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST API'leri     |[Görüntü yayımcılarını listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Görüntü tekliflerini listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Görüntü SKU'ları Listele](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

Karşıya yükleme ve kendi görüntünüzü kullanma seçebilirsiniz. Bunu yaparsanız, yayımcı adı, teklif ve sku kullanılmaz.

### <a name="extensions"></a>Uzantılar

VM uzantıları, aracılığıyla dağıtım sonrası yapılandırma ve otomatik görevlerle VM'nize yeni özellikler sağlar.
Uzantıları kullanarak şu genel görevleri gerçekleştirebilirsiniz:

- **Özel betik çalıştırma**  
    Özel betik uzantısı, VM hazırlandığında kendi betiğinizi çalıştırarak iş yükleri VM'de yapılandırmanıza yardımcı olur.

- **Yapılandırma dağıtma ve yönetme**  
    PowerShell Desired State Configuration (DSC) uzantısı yapılandırmalarını ve ortamları yönetmek için bir VM üzerinde DSC ayarlamanıza yardımcı olur.

- **Tanılama verilerini toplama**  
    Azure tanılama uzantısını VM, uygulamanızın durumunu izlemek için kullanılabilecek tanılama verilerini toplayacak şekilde yapılandırmanıza yardımcı olur.

### <a name="related-resources"></a>İlgili kaynaklar

Aşağıdaki tablodaki kaynaklar VM tarafından kullanılır ve mevcut veya sanal makine oluşturulduğunda gerekir.


|Kaynak|Gerekli|Açıklama|
|---------|---------|---------|
|Kaynak grubu|Evet|VM bir kaynak grubunda yer almalıdır.|
|Depolama hesabı|Evet|VM, sanal sabit disklerini depolamak için bir depolama hesabına ihtiyaç duyar.|
|Sanal ağ|Evet|VM’in bir sanal ağa üye olması gerekir.|
|Genel IP adresi|Hayır|VM, uzaktan erişim için atanmış bir genel IP adresine sahip olabilir.|
|Ağ arabirimi|Evet|VM’in ağda iletişim kurabilmek için ağ arabirimine ihtiyacı vardır.|
|Veri diskleri|Hayır|VM, depolama olanaklarını genişletmek için veri disklerine sahip olabilir.|

## <a name="create-your-first-vm"></a>İlk VM'nizi oluşturun

Bir VM oluşturmak için birkaç seçeneğiniz vardır. Seçiminiz ortamınıza bağlıdır.
Aşağıdaki tabloda VM'nizi oluşturmak için giriş bilgileri ihtiyaç duyacağınız sağlar.


|Yöntem|Makale|
|---------|---------|
|Azure Stack portalı|Azure Stack portal ile bir Windows sanal makinesi oluşturma<br>[Azure Stack portalını kullanarak bir Linux sanal makinesi oluşturma](azure-stack-quick-linux-portal.md)|
|Şablonlar|Azure Stack hızlı başlangıç şablonları şu adreste bulunabilir:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Azure Stack'te PowerShell kullanarak Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-powershell.md)<br>[Azure Stack'te PowerShell kullanarak bir Linux sanal makinesi oluşturma](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Azure Stack'te CLI kullanarak bir Windows sanal makine oluşturun](azure-stack-quick-create-vm-windows-cli.md)<br>[Azure Stack'te CLI kullanarak bir Linux sanal makinesi oluşturma](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="manage-your-vm"></a>Sanal Makinenizi Yönetme

VM'ler tarayıcı tabanlı bir portal, betik oluşturma ya da doğrudan API'ler aracılığıyla desteğine sahip komut satırı araçları kullanarak yönetebilirsiniz. Gerçekleştirmek isteyebileceğiniz genel yönetim görevlerinden bazıları şunlardır:

- Bir VM hakkında bilgi alma
- Bir sanal Makineye bağlanma
- Kullanılabilirliği yönetme
- Yedeklemeler yapma

### <a name="get-information-about-your-vm"></a>Sanal makinenizin hakkında bilgi edinin

Aşağıdaki tabloda VM hakkında bilgi edinme yöntemlerinden bazıları gösterilmektedir.


|Yöntem|Açıklama|
|---------|---------|
|Azure Stack portalı|Hub menüsünde, sanal makineler tıklayın ve ardından listeden VM'yi seçin. VM sayfasında, Özet bilgilere, ayar değerlerine ve izleme ölçümlerine erişiminiz.|
|Azure PowerShell|Vm'leri yönetme, Azure ve Azure Stack benzerdir. PowerShell kullanma hakkında daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[Oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|İstemci SDK'ları|Vm'leri yönetmek için C# kullanarak Azure ve Azure Stack benzerdir. Daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[C# kullanarak azure'da Windows Vm'leri oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-your-vm"></a>Sanal Makinenize bağlanın

Kullanabileceğiniz **Connect** VM'nize bağlanmak için Azure Stack portalında düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack'te sanal makineler için dikkat edilmesi gerekenler](azure-stack-vm-considerations.md)