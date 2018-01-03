---
title: "Azure yığın sanal makineleri giriş"
description: "Azure yığın sanal makineler hakkında bilgi edinin"
services: azure-stack
author: anjayajodha
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: c37ad8ac5b6c37261e22237e843dd97e2bbd09f9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Azure yığın sanal makineleri giriş

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="overview"></a>Genel Bakış
Azure yığın sanal makine (VM) Azure yığın sunan isteğe bağlı, ölçeklenebilir bilgi işlem kaynak türüdür. Genellikle, sunulan diğer seçimlere göre bilgi işlem ortamınız üzerinde daha fazla denetime ihtiyacınız varsa, bir VM seçersiniz. Bu makalede VM oluşturmadan önce dikkat etmeniz gereken bilgilere, oluşturma yöntemine ve yönetim seçeneklerine yer verilmiştir.

Bir Azure yığın VM tek tek kümeleri veya makineler yönetmek zorunda kalmadan sanallaştırma esnekliği sağlar. Ancak yine de yapılandırma, düzeltme eki uygulama ve yazılım yükleme gibi görevleri gerçekleştirerek VM’nin bakımını yapmanız gerekir.

Azure yığın sanal makineleri çeşitli şekillerde kullanılabilir. Örneğin:

* **Geliştirme ve test** – Azure yığın VM'ler sunan bir hızlı ve kolay şekilde bir bilgisayar ile belirli bir yapılandırma oluşturmak kod ve bir uygulamayı test etmek için gerekli.

* **Bulut uygulamalarında** – uygulamanız için isteğe bağlı dalgalanma çünkü bunu çalıştırmak için bir VM'de Azure yığınında ekonomik mantıklı olabilir. İhtiyaç duyduğunuzda oluşturulan ek VM’ler için ödeme yapar, ihtiyaç kalmadığında bunları kapatırsınız.

* **Veri Merkezi Genişletilmiş** – Azure yığın sanal ağdaki sanal makinelerden kolayca, kuruluşunuzun ağ veya Azure bağlanması.

Uygulamanız tarafından kullanılan VM sayısı, ihtiyaçlarınıza göre ölçeklendirilebilir.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>VM oluşturmadan önce dikkat etmem gereken noktalar nelerdir?

Aynı zamanda Azure yığın uygulama altyapısında çıkışı derlerken her zaman tasarımları çok sayıda vardır. Başlamadan önce dikkat etmeniz gereken VM özellikleri şunlardır:

- Uygulama kaynaklarınızın adları
- VM’nin boyutu
- Oluşturulabilecek en fazla VM sayısı
- VM üzerinde çalışan işletim sistemi
- VM’nin başlatıldıktan sonraki yapılandırması 
- VM’nin ihtiyaç duyduğu kaynaklar

### <a name="naming"></a>Adlandırma

Bir sanal makineye atanmış olan bir ada sahip ve işletim sisteminin bir parçası olarak yapılandırılan bir bilgisayar adına sahip. VM adı en fazla 15 karakter uzunluğunda olabilir.

İşletim sistemi diski oluşturmak için Azure yığın kullanırsanız, bilgisayar adı ve sanal makine adı aynıdır. Karşıya yükleme ve önceden yapılandırılmış bir işletim sistemini içeren kendi görüntünüzü kullanma ve bir sanal makine oluşturmak için kullanmak, adları farklı olabilir. Kendi görüntü dosyasını karşıya yüklediğinde, işletim sisteminde bilgisayar adını yapın ve sanal makine aynı en iyi uygulama olarak adlandırın.

### <a name="vm-size"></a>VM boyutu

Kullandığınız VM boyutu çalıştırmak istediğiniz iş yükü tarafından belirlenir. Seçtiğiniz boyut işlemci gücü, bellek ve depolama kapasitesi gibi ölçütleri belirler. Azure yığını çok çeşitli sayıda kullanım türünü desteklemek üzere boyutları sunar.

### <a name="vm-limits"></a>VM sınırları

Aboneliğinizi projeniz için birçok VM dağıtımını etkileyebilir yerinde varsayılan kota sınırları vardır. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir.

### <a name="operating-system-disks-and-images"></a>İşletim sistemi diskleri ve görüntüleri

Sanal makineler, kendi işletim sistemlerini (OS) ve verilerini depolamak için sanal sabit diskleri (VHD) kullanır. VHD bir işletim sistemi yüklemek için seçebileceğiniz görüntüler için de kullanılır.
Azure yığın çeşitli sürümleri ve türlerde işletim sistemleri ile kullanmak için bir Market sunar. Market görüntüleri; görüntü yayımcısı, teklif, sku ve sürüm (genelde sürüm en son belirtilir) bilgileriyle tanımlanır.

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

* Özel komut dosyaları – çalıştırmak özel betik uzantısı iş yükleri VM'de VM sağlandığında, komut dosyasını çalıştırarak yapılandırmanıza yardımcı olur.
* Dağıtma ve yapılandırmalarını yönetme – PowerShell istenen durum yapılandırması (DSC) uzantısı, yapılandırmalarını ve ortamları yönetmek için bir VM üzerinde DSC kurmanıza yardımcı olur.
* Toplama tanılama verilerini – Azure tanılama uzantısını uygulamanızın durumunu izlemek için kullanılan tanılama verilerini toplamak için VM yapılandırmanıza yardımcı olur.

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

## <a name="how-do-i-create-my-first-vm"></a>İlk VM’mi nasıl oluşturabilirim?

Bir VM oluşturmak için birkaç seçeneğiniz vardır. Seçiminiz ortamınıza bağlıdır.
Aşağıdaki tabloda, bilgi sağlamak için VM oluşturmaya başlamanızı sağlar.


|Yöntem|Makale|
|---------|---------|
|Azure yığın portalı|Azure yığın portal ile bir Windows sanal makine oluşturma<br>[Azure yığın Portalı'nı kullanarak bir Linux sanal makine oluşturun](azure-stack-quick-linux-portal.md)|
|Şablonlar|Azure yığın hızlı başlangıç şablonları şu adreste bulunabilir:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Azure yığınında PowerShell kullanarak bir Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-powershell.md)<br>[Azure yığınında PowerShell kullanarak bir Linux sanal makine oluşturma](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Azure yığınında CLI kullanarak bir Windows sanal makine oluşturma](azure-stack-quick-create-vm-windows-cli.md)<br>[Azure yığınında CLI kullanarak bir Linux sanal makine oluşturma](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Oluşturduğum VM’yi nasıl yönetebilirim?

VM’ler tarayıcı tabanlı bir portal, betik oluşturma desteğine sahip komut satırı araçları kullanılarak veya doğrudan API’ler aracılığıyla yönetilebilir. Gerçekleştirmek isteyebileceğiniz genel yönetim görevlerinden bazıları VM hakkında bilgi alma, VM’de oturum açma, kullanılabilirlik durumunu yönetme ve yedekleme yapmadır.

### <a name="get-information-about-a-vm"></a>VM hakkında bilgi alma

Aşağıdaki tabloda, bir VM hakkında bilgi edinebilirsiniz yollardan bazılarını gösterir.


|Yöntem|Açıklama|
|---------|---------|
|Azure yığın portalı|Hub menüsünde, sanal makineleri tıklayın ve ardından VM listeden seçin. VM için sayfada genel bakış bilgileri için değerleri ayarlama ve ölçümleri izleme erişebilirsiniz.|
|Azure PowerShell|Sanal makineleri yönetme, Azure ve Azure yığın benzer. PowerShell'i kullanma hakkında daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[Oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|İstemci SDK'ları|Sanal makineleri yönetmek için C# kullanarak Azure ve Azure yığın benzer. Daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[Oluşturma ve C# kullanarak azure'da Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-the-vm"></a>VM’ye bağlanma

Kullanabileceğiniz **Bağlan** VM'nize bağlanmak için Azure yığın portalda düğmesine.

## <a name="next-steps"></a>Sonraki adımlar
* [Sanal makineler Azure yığınında dikkate alınacak noktalar](azure-stack-vm-considerations.md)

