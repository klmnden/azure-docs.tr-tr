---
title: "Azure Dosya depolamaya giriş | Microsoft Docs"
description: "Endüstri standardını kullanarak Microsoft Bulut’ta ağ dosya paylaşımları oluşturmanıza ve kullanmanıza olanak tanıyan Azure Dosya depolama hizmetine genel bir bakış."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 16fdd3aafef1a50554932a0e7843d347182c9b6a
ms.contentlocale: tr-tr
ms.lasthandoff: 07/20/2017

---

# <a name="introduction-to-azure-file-storage"></a>Azure Dosya depolamaya giriş
Azure Dosya depolama, endüstri standardı [Sunucu İleti Blogu (SMB) Protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)’nü ve [Samba/Ortak İnternet Dosya Sistemi (CIFS)](https://technet.microsoft.com/library/cc939973.aspx) protokolünü kullanarak bulutta ağ dosya paylaşımları sağlar. Azure Dosya paylaşımları, Windows, macOS, Linux’ın şirket içi dağıtımları gibi istemciler veya Azure Sanal Makineleri tarafından eşzamanlı olarak bağlanabilir. Genel amaçlı depolama hesabı, tek bir hesap altında Azure Dosya Depolama'ya ve Bloblar, Azure sanal makinesi diskleri ve Sorgular gibi diğer hizmetlere erişim sağlar.



## <a name="videos"></a>Videolar
| Azure Dosya depolamaya giriş (27 dakika) | Azure Dosya depolama Öğreticisi (5 dakika)  |
|-|-|
| [![Azure Dosya depolamaya giriş videosunun ekranı - oynatmak için tıklayın!](media/storage-file-storage/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Azure Dosya depolama Öğreticisi ekranı - oynatmak için tıklayın!](media/storage-file-storage/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Azure Dosya depolama neden yararlıdır
Azure Dosya depolama, şirket içinde veya bulutta barındırılan Windows Server, Linux veya NAS tabanlı dosya sunucularını işletim sisteminden bağımsız bir bulut dosya paylaşımıyla değiştirmenize olanak tanır. Bunun şöyle avantajları vardır:

* **Paylaşılan erişim:**. Azure Dosya paylaşımları endüstri standardı SMB protokolünü destekler. Bu, şirket içi dosya paylaşımlarınızı uygulama uyumluluğu konusunda kaygılanmadan rahatça Azure Dosya paylaşımlarıyla değiştirebileceğiniz anlamına gelir. Dosya sistemini birden çok makine, uygulama/örnek arasında paylaşabilmek, paylaşılabilirlik gerektiren uygulamalarda Azure Dosya depolamanın önemli bir avantaj olmasını sağlar. 
* **Tam Olarak Yönetilir**. Azure Dosya paylaşımları donanım veya işletim sistemi yönetmeye gerek kalmadan oluşturulabilir. Bu, sunucu işletim sistemine kritik güvenlik yükseltmeleri için yama eklemekle veya arızalı sabit diskleri değiştirmekle uğraşmanız gerekmediği anlamına gelir.
* **Betik ve Araç Sağlama**. Azure uygulamalarının yönetimi kapsamında Dosya depolama paylaşımlarını oluşturmak, bağlamak ve yönetmek için PowerShell cmdlet’leri ve Azure CLI kullanılabilir. Azure Portal ve Azure Depolama Gezgini’ni kullanarak Azure dosya paylaşımlarını oluşturabilir ve yönetebilirsiniz. 
* **Dayanıklılık**. Azure Dosya depolama, baştan sonra her zaman kullanılabilir olacak şekilde hazırlanmıştır. Şirket içi dosya paylaşımlarını Azure dosya depolama ile değiştirmek, artık kalkıp bölgesel elektrik kesintileriyle veya ağ sorunlarıyla uğraşmak zorunda kalmayacağınız anlamına gelir. 
* **Tanıdık Programlanabilirlik**. Azure’da çalışan uygulamalar paylaşımdaki verilere [dosya sistemi G/Ç API’leri](https://msdn.microsoft.com/library/system.io.file.aspx) yoluyla erişebilir. Böylece geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yeteneklerden yararlanabilir. Sistem G/Ç API’lerine ek olarak, [Azure Depolama İstemcisi Kitaplıkları](https://msdn.microsoft.com/library/azure/dn261237.aspx) veya [Azure Depolama REST API’si](/rest/api/storageservices/file-service-rest-api) de kullanılabilir.

Azure Dosya paylaşımları şunları yapmak için kullanılabilir:

* **Şirket içi dosya sunucularını değiştirme**:  
    Azure Dosya depolama geleneksel şirket içi dosya sunucularında veya NAS cihazlarındaki dosya paylaşımlarını tamamen değiştirmek için kullanılabilir. Windows, macOS ve Linux gibi yaygın işletim sistemleri, dünyanın neresinde olursa olsun Azure Dosya paylaşımına kolayca bağlanabilir.

* **Uygulamaları "kaldırma ve kaydırma"**:  
    Azure Dosya depolama, uygulama bölümleri arasında veri paylaşımı için şirket içi paylaşımları kullanan uygulamaları buluta “kaldırma ve kaydırma” işlemini kolaylaştırır. Bunu gerçekleştirmek için, her sanal makine dosya paylaşımına bağlanır ve ardından aynı şirket içi dosya paylaşımında yapabileceği gibi dosyaları okuyup yazabilir.

* **Bulut Geliştirmeyi Basitleştirme**:  
    Azure Dosya depolama, yeni bulut geliştirme projelerini basitleştirmek üzere çeşitli yollarla kullanılabilir.
    * **Paylaşılan Uygulama Ayarları**:  
        Dağıtılmış uygulamalar için yaygın bir düzen, yapılandırma dosyalarının birçok farklı sanal makinenin erişebildiği merkezi bir konumda tutulmasıdır. Bu tür yapılandırma dosyaları artık Azure Dosya paylaşımında depolanabilir ve tüm uygulama örnekleri tarafından okunabilir. Bu ayarlar, yapılandırma dosyalarına dünya genelinde erişime olanak tanıyan REST arabirimi üzerinden de yönetilebilir.

    * **Tanılama Paylaşımı**:  
        Azure Dosya paylaşımı günlükler, ölçümler ve kilitlenme bilgi dökümleri gibi tanılama dosyalarını saklamak için de kullanılabilir. Bunların hem SMB hem de REST arabirimi aracılığıyla kullanılabilir olması, uygulamaların tanılama verilerini işlemek ve çözümlemek için çeşitli çözümleme araçları oluşturmasına veya böyle araçlardan yararlanmasına olanak sağlar.

    * **Geliştirme/Test/Hata Ayıklama**:  
        Geliştiriciler veya yöneticiler bulutta sanal makinelerle çalışırken, sıklıkla bir dizi araca veya yardımcı programa ihtiyaçları olur. Bu yardımcı programların gerekli oldukları her sanal makineye yüklenmesi ve dağıtılması, çok zaman alan bir çalışma olabilir. Azure Dosya depolama ile, geliştirici veya yönetici sık kullandığı araçları dosya paylaşımında depolayabilir ve bu dosya paylaşımına herhangi bir sanal makineden kolayca bağlanılabilir.
        
## <a name="how-does-it-work"></a>Nasıl çalışır?
Azure Dosya paylaşımlarını yönetmek, şirket içi dosya paylaşımlarını yönetmekten çok daha basittir. Aşağıdaki diyagramda Azure Dosya depolama yönetim yapıları gösterilir:

![Dosya Yapısı](../../includes/media/storage-file-concepts-include/files-concepts.png)

* **Depolama Hesabı**: Tüm Azure Depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için, Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri konusuna bakın.
* **Paylaşım**: Dosya Depolama paylaşımı Azure’daki bir SMB dosyası paylaşımıdır. Tüm dizinler ve dosyalar üst paylaşımda oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir; paylaşım da 5 TB toplam kapasiteye kadar dosya paylaşımıyla sınırsız sayıda dosya depolayabilir.
* **Dizin:** Dizinlerin isteğe bağlı hiyerarşisi.
* **Dosya**: Paylaşımdaki bir dosya. Bir dosyanın boyutu 1 TB'ye kadar olabilir.
* **URL biçimi**: Dosyalar şu URL biçimi kullanılarak adreslenebilir:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```
## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Dosya Paylaşımı Oluşturma](storage-file-how-to-create-file-share.md)
* [Windows’da Bağlama](storage-file-how-to-use-files-windows.md)
* [Linux’ta Bağlama](storage-how-to-use-files-linux.md)
* [macOS’ta Bağlama](storage-file-how-to-use-files-mac.md)
* [SSS](storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Kavramsal makaleler ve videolar
* [Azure Dosya depolama: Windows ve Linux için uyumlu bulut SMB dosya sistemi](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Azure Dosya depolama için araç desteği
* [Azure Depolama ile Azure PowerShell’i kullanma](storage-powershell-guide-full.md)
* [Microsoft Azure Depolama ile AzCopy kullanma](storage-use-azcopy.md)
* [Azure Depolama ile Azure CLI kullanma](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="blog-posts"></a>Blog yazıları
* [Azure Dosya Depolama genel kullanıma sunulmuştur](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure Dosya depolama incelemesi](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Microsoft Azure Dosya Hizmeti’ne Giriş](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Azure Dosya Hizmeti’ne verileri geçirme ](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Başvuru
* [.NET için Depolama İstemci Kitaplığı başvurusu](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Dosya Hizmeti REST API başvurusu](http://msdn.microsoft.com/library/azure/dn167006.aspx)

