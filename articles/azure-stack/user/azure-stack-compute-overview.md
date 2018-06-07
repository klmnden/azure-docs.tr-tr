---
title: Azure Stack sanal makinelerine giriş
description: Azure yığın sanal makineler hakkında bilgi edinin
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 967fcb86c1bf0c85517bc13c2066ed32e8fa28d9
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604140"
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Azure Stack sanal makinelerine giriş

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın isteğe bağlı, ölçeklenebilir bir bilgisayar kaynağına bir tür sanal makineleri (VM'ler) sunar. Bilgi işlem ortamınız üzerinde daha fazla denetim ve diğer seçenekleri daha ihtiyacınız olduğunda, bir VM seçebilirsiniz. Bu makalede, VM oluşturmadan önce ayrıntılar sağlar.

Bir Azure yığın VM kümeleri ya da makineleri tek tek yönetmek zorunda kalmadan sanallaştırma esnekliği sağlar. Ancak, yine yapılandırma, düzeltme eki uygulama ve bunun üzerinde çalıştırılır yazılım yükleme gibi görevleri gerçekleştirerek VM korumak gerekir.

Azure yığın sanal makineleri çeşitli şekillerde kullanabilirsiniz. Örneğin:

- **Geliştirme ve test**  
    Azure yığın VM'ler kodu için gereken belirli bir yapılandırmaya sahip bir bilgisayar oluşturmak ve bir uygulamayı test etmek için hızlı ve kolay bir yol sunar.

- **Bulut uygulamaları**  
    Uygulamanız için isteğe bağlı dalgalanma çünkü bunu çalıştırmak için bir VM'de Azure yığınında ekonomik mantıklı olabilir. İhtiyaç duyduğunuzda oluşturulan ek VM’ler için ödeme yapar, ihtiyaç kalmadığında bunları kapatırsınız.

- **Genişletilmiş veri merkezi**  
    Bir Azure yığın sanal ağdaki sanal makinelerden kuruluşunuzun ağına veya Azure kolayca bağlanabilir.

Uygulamanız tarafından kullanılan ölçeği veya ne olursa olsun gereksinimlerinizi karşılamak için gerekli olan için ölçeğini VM'ler.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>VM oluşturmadan önce dikkat etmem gereken noktalar nelerdir?

Aynı zamanda Azure yığın uygulama altyapısında çıkışı derlerken her zaman çok sayıda tasarım konuları vardır. VM şu yönlerini altyapınızı oluşturmaya başlamadan önce dikkat etmeniz gereken önemli şunlardır:

- Uygulama kaynakları adları.
- VM boyutu.
- Oluşturulabilir VM'ler maksimum sayısı.
- VM çalıştıran işletim sistemi.
- Başladıktan sonra VM yapılandırması.
- VM gereken ilgili kaynaklar.

### <a name="naming"></a>Adlandırma

Bir sanal makineye atanmış olan bir ada sahip ve işletim sisteminin bir parçası olarak yapılandırılan bir bilgisayar adına sahip. VM adı en fazla 15 karakter uzunluğunda olabilir.

İşletim sistemi diski oluşturmak için Azure yığın kullanırsanız, bilgisayar adı ve sanal makine adı aynıdır. Karşıya yükleme ve önceden yapılandırılmış bir işletim sistemini içeren kendi görüntünüzü kullanma ve bir sanal makine oluşturmak için kullanmak, adları farklı olabilir. Kendi görüntü dosyasını karşıya yüklediğinde, işletim sisteminde bilgisayar adını yapın ve sanal makine aynı en iyi uygulama olarak adlandırın.

### <a name="vm-size"></a>VM boyutu

Kullandığınız VM boyutu çalıştırmak istediğiniz iş yükü tarafından belirlenir. Seçtiğiniz boyut işlemci gücü, bellek ve depolama kapasitesi gibi ölçütleri belirler. Azure yığını çok çeşitli sayıda kullanım türünü desteklemek üzere boyutları sunar.

### <a name="vm-limits"></a>VM sınırları

Aboneliğinizi projeniz için birçok VM dağıtımını etkileyebilir yerinde varsayılan kota sınırları vardır. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir.

### <a name="operating-system-disks-and-images"></a>İşletim sistemi diskleri ve görüntüleri

Sanal makineler, kendi işletim sistemlerini (OS) ve verilerini depolamak için sanal sabit diskleri (VHD) kullanır. VHD bir işletim sistemi yüklemek için seçebileceğiniz görüntüler için de kullanılır.
Azure yığın çeşitli sürümleri ve türlerde işletim sistemleri ile kullanmak için bir Market sunar. Market görüntülerini görüntü Yayımlayıcı, teklif, sku ve sürümü (genellikle sürüm en son belirtilir.) tarafından tanımlanır

Aşağıdaki tabloda, bir görüntü bilgilerini bulabilirsiniz bazı yollarını gösterir:

|Yöntem|Açıklama|
|---------|---------|
|Azure yığın portalı|Bir görüntüyü kullanmak istediğinizde değerler otomatik olarak belirtilir.|
|Azure Stack PowerShell|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST API'leri     |[Görüntü yayımcılarını listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Görüntü tekliflerini listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Görüntü SKU'ları Listele](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

Karşıya yükleme ve kendi görüntünüzü kullanma seçebilirsiniz. Bunu yaparsanız, yayımcı adını, teklif ve sku kullanılmaz.

### <a name="extensions"></a>Uzantılar

VM uzantıları VM ek yetenekler post dağıtım yapılandırması ve otomatik görevler üzerinden verin.
Uzantıları kullanarak şu genel görevleri gerçekleştirebilirsiniz:

- **Özel komut dosyalarını çalıştır**  
    Özel betik uzantısının VM sağlandığında, komut dosyasını çalıştırarak VM iş yüklerini yapılandırmanıza yardımcı olur.

- **Dağıtma ve yapılandırmalarını yönetme**  
    PowerShell istenen durum yapılandırması (DSC) uzantısı bir VM'de DSC yapılandırmalarını ve ortamları yönetmek için ayarlamanıza yardımcı olur.

- **Tanılama verileri toplama**  
    Azure tanılama uzantısını uygulamanızın durumunu izlemek için kullanılan tanılama verilerini toplamak için VM yapılandırmanıza yardımcı olur.

### <a name="related-resources"></a>İlgili kaynaklar

Aşağıdaki tabloda kaynakları VM tarafından kullanılır ve mevcut ya da VM oluşturulduğunda gerekir.


|Kaynak|Gerekli|Açıklama|
|---------|---------|---------|
|Kaynak grubu|Evet|VM bir kaynak grubunda yer almalıdır.|
|Depolama hesabı|Evet|VM, sanal sabit disklerini depolamak için bir depolama hesabına ihtiyaç duyar.|
|Sanal ağ|Evet|VM’in bir sanal ağa üye olması gerekir.|
|Genel IP adresi|Hayır|VM, uzaktan erişim için atanmış bir genel IP adresine sahip olabilir.|
|Ağ arabirimi|Evet|VM’in ağda iletişim kurabilmek için ağ arabirimine ihtiyacı vardır.|
|Veri diskleri|Hayır|VM, depolama olanaklarını genişletmek için veri disklerine sahip olabilir.|

## <a name="create-your-first-vm"></a>İlk VM oluşturma

Bir VM oluşturmak için birkaç seçeneğiniz vardır. Seçiminiz ortamınıza bağlıdır.
Aşağıdaki tabloda, bilgi sağlamak için VM oluşturmaya başlamanızı sağlar.


|Yöntem|Makale|
|---------|---------|
|Azure yığın portalı|Azure yığın portal ile bir Windows sanal makine oluşturma<br>[Azure yığın Portalı'nı kullanarak bir Linux sanal makine oluşturun](azure-stack-quick-linux-portal.md)|
|Şablonlar|Azure yığın hızlı başlangıç şablonları şu adreste bulunabilir:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Azure yığınında PowerShell kullanarak bir Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-powershell.md)<br>[Azure yığınında PowerShell kullanarak bir Linux sanal makine oluşturma](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Azure yığınında CLI kullanarak bir Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-cli.md)<br>[Azure yığınında CLI kullanarak bir Linux sanal makine oluşturma](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="manage-your-vm"></a>Sanal Makinenizi Yönetme

VM'ler tarayıcı tabanlı bir portal desteğiyle API'leri aracılığıyla doğrudan veya komut dosyası için komut satırı araçlarını kullanarak yönetebilirsiniz. Gerçekleştirebileceğiniz bazı tipik yönetim görevleri şunlardır:

- VM hakkında bilgi alma
- Bir VM'ye bağlanma
- Kullanılabilirlik yönetme
- Yedekleme yapmak

### <a name="get-information-about-your-vm"></a>VM hakkında bilgi edinin

Aşağıdaki tabloda, bir VM hakkında bilgi edinebilirsiniz yollardan bazılarını gösterir.


|Yöntem|Açıklama|
|---------|---------|
|Azure yığın portalı|Hub menüsünde, sanal makineleri tıklayın ve ardından VM listeden seçin. VM için sayfada genel bakış bilgileri için değerleri ayarlama ve ölçümleri izleme erişebilirsiniz.|
|Azure PowerShell|Sanal makineleri yönetme, Azure ve Azure yığın benzer. PowerShell'i kullanma hakkında daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[Oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|İstemci SDK'ları|Sanal makineleri yönetmek için C# kullanarak Azure ve Azure yığın benzer. Daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[Oluşturma ve C# kullanarak azure'da Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-your-vm"></a>VM'nize bağlanmak

Kullanabileceğiniz **Bağlan** VM'nize bağlanmak için Azure yığın portalda düğmesine.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makineler Azure yığınında dikkate alınacak noktalar](azure-stack-vm-considerations.md)