---
title: Bir Azure dosyaları dağıtımını planlama | Microsoft Docs
description: Bir Azure dosyaları dağıtımını planlarken dikkate almanız gerekenler öğrenin.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 19adbbfc456303b471251c28cd984d1676786b19
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783160"
---
# <a name="planning-for-an-azure-files-deployment"></a>Azure Dosyaları dağıtımı planlama
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı SMB protokolünü erişilebilen bulutta sunar. Azure dosyaları tam olarak yönetildiğinden, üretim senaryolarında dağıtma dağıtılması ve dosya sunucusu veya NAS cihazınızın yönetilmesi daha kolaydır. Bu makalede, kuruluşunuzdaki üretim kullanımı için Azure dosya paylaşımını dağıtırken göz önünde bulundurmanız konularını ele alır.

## <a name="management-concepts"></a>Yönetim Kavramları
 Aşağıdaki diyagramda Azure dosyaları yönetim yapıları gösterilir:

![Dosya Yapısı](./media/storage-files-introduction/files-concepts.png)

* **Depolama Hesabı**: Tüm Azure Depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için, [Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) konusuna bakın.

* **Paylaşım**: Dosya Depolama paylaşımı Azure’daki bir SMB dosyası paylaşımıdır. Tüm dizinler ve dosyalar üst paylaşımda oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir ve Paylaşım sınırsız sayıda dosya paylaşımı 5 TiB toplam kapasitesini dosya depolayabilir.

* **Dizin:** Dizinlerin isteğe bağlı hiyerarşisi.

* **Dosya**: Paylaşımdaki bir dosya. Bir dosya boyutu en fazla 1 TiB olabilir.

* **URL biçimi**: Azure dosya paylaşımına dosya REST protokolü ile yapılan istekler için dosyalar şu URL biçimi kullanılarak adreslenebilir:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Veri erişimi yöntemi
Azure dosyaları teklifleri iki, yerleşik, kullanışlı, ayrı ayrı veya birbiriyle birlikte verilerinize erişmek için kullanabileceğiniz yöntemler verilere:

1. **Doğrudan bulut erişimi**: herhangi bir Azure dosya paylaşımı tarafından bağlanabilir [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md), ve/veya [Linux](storage-how-to-use-files-linux.md) sektör ile standart sunucu ileti bloğu (SMB) protokol veya dosya REST API'si aracılığıyla. SMB ile Azure dosya paylaşımında doğrudan okuma ve yazma işlemleri için dosya paylaşımında yapılır. Azure'da VM tarafından bağlamak için SMB istemci işletim sisteminde en az desteklemelidir SMB 2.1. Şirket içinde kullanıcı iş istasyonunda en az iş istasyonu tarafından desteklenen SMB istemcisi desteklemelidir gibi bağlamak için SMB 3.0 (ile şifreleme). SMB ek olarak, yeni uygulamalar veya hizmetler dosya paylaşımını dosya yazılım geliştirme için bir kolayca ve ölçeklenebilir bir uygulama programlama arabirimi sağlayan REST aracılığıyla doğrudan erişebilir.
2. **Azure dosya eşitleme**: Azure dosya eşitleme paylaşımları Windows sunucuları şirket içi veya azure'de çoğaltılabilir. Kullanıcılarınızın Windows Server üzerinden dosya paylaşımı gibi SMB veya NFS paylaşım yoluyla erişir. Bu, hangi veriler erişilen ve uzakta bir Azure veri merkezlerinden gibi bir şube ofis senaryosunda değiştiren senaryoları için kullanışlıdır. Veri arasında birden fazla Windows Server uç noktası, gibi birden çok şube ofis arasında çoğaltılabilir. Son olarak, Azure dosyaları'na tüm verileri, sunucunun hala erişilebilir, ancak sunucu verilerin tam bir kopyasını yok şekilde katmanlanmış verileri. Bunun yerine, verileri sorunsuz bir şekilde, kullanıcı tarafından açıldığında çağrılır.

Aşağıdaki tabloda, kullanıcılar ve uygulamalar Azure dosya paylaşımınızı nasıl erişeceği gösterilmektedir:

| | Doğrudan bulut erişimi | Azure Dosya Eşitleme |
|------------------------|------------|-----------------|
| Hangi protokollerin kullanılacağını ihtiyacınız var? | Azure dosyaları SMB 2.1 ve SMB 3.0 dosya REST API'sini destekler. | Azure dosya paylaşımınızı (SMB, NFS, FTPS, vb.) Windows Server'da desteklenen bir protokolü üzerinden erişim |  
| İş yükünüz nerede kullanıyorsunuz? | **Azure'da**: Azure dosyaları, verilere doğrudan erişime olanak sağlar. | **Yavaş ağ ile şirket içi**: Windows, Linux ve Macos'ta istemcileri Azure dosya paylaşımınızın hızlı bir önbellek yerel şirket içi Windows dosya paylaşımı bağlama. |
| ACL'ler düzeyini ihtiyacınız var? | Paylaşım ve dosya düzeyi. | Paylaşım, dosya ve kullanıcı düzeyi. |

## <a name="data-security"></a>Veri güvenliği
Azure dosyaları, veri güvenliğini sağlamaya yönelik çeşitli yerleşik seçenekler vardır:

* Her iki üzerinden hat protokolleri şifreleme desteği: SMB 3.0 şifreleme ve HTTPS üzerinden dosya REST. Varsayılan olarak: 
    * SMB 3.0 şifrelemesini destekleyen istemciler, gönderin ve şifreli bir kanal veri alın.
    * SMB 3.0 desteklemeyen istemciler iletişim içi veri merkezi SMB 2.1 veya SMB 3.0 üzerinden şifreleme olmadan. İstemciler inter-datacenter SMB 2.1 veya SMB 3.0 üzerinden şifreleme olmadan iletişim kurmasına izin verilmiyor unutmayın.
    * İstemciler, HTTP veya HTTPS ile dosya REST üzerinden iletişim kurabilirsiniz.
* Bekleyen şifreleme ([Azure depolama hizmeti şifrelemesi](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): depolama hizmeti şifrelemesi (SSE), tüm depolama hesapları için etkinleştirildi. Bekleyen verileri tam olarak yönetilen anahtarlarla şifrelenir. Bekleyen şifreleme depolama maliyetlerini artırabilir veya performansı düşürebilir desteklemez. 
* Şifrelenmiş veriler aktarım sırasında isteğe bağlı gereksinimi: Seçili olduğunda, Azure dosyaları şifresiz kanal veri erişimi reddeder. Özellikle, yalnızca HTTPS ve SMB 3.0 şifreleme bağlantılarıyla izin verilir. 

    > [!Important]  
    > Güvenli veri aktarımı gerektiren başarısız olmasına eski SMB istemcileriniz değil şifrelemesi ile SMB 3.0 ile iletişim kurabilen neden olur. Bkz: [Windows üzerinde bağlama](storage-how-to-use-files-windows.md), [Linux üzerinde bağlama](storage-how-to-use-files-linux.md), [Macos'ta bağlama](storage-how-to-use-files-mac.md) daha fazla bilgi için.

En yüksek güvenlik için her zaman her iki şifreleme bekleyen etkinleştirme ve modern istemciler verilerinize erişmek için kullandığınız her veri aktarım sırasında şifreleme etkinleştirilmesi önerilir. Örneğin, bir Windows Server 2008 R2 yalnızca SMB 2.1 destekleyen, sanal makinede paylaşımını gerekiyorsa SMB 2.1 şifreleme desteklemediğinden depolama hesabınıza şifrelenmemiş trafiği izin vermeniz gerekir.

Azure dosya paylaşımınızı erişmek için Azure dosya eşitleme'ı kullanıyorsanız her zaman HTTPS ve SMB 3.0 şifrelemesi ile bekleyen verilerin şifrelenmesi gereksiniminiz olup bağımsız olarak, Windows sunucularını verilerinizi eşitlemek için kullanacağız.

## <a name="data-redundancy"></a>Veri yedekliği
Azure dosyaları, üç veri yedekliliği seçenekleri destekler: yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS) ve coğrafi olarak yedekli depolama (GRS). Aşağıdaki bölümlerde farklı yedekliliği seçenekleri arasındaki farklar açıklanmaktadır:

### <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Bölgesel olarak yedekli depolama
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Veri büyümesi deseni
Bugün, bir Azure dosya paylaşımı için boyut üst sınırı 5 TiB ' dir. Şu anki bu sınırlama nedeniyle, bir Azure dosya paylaşımı dağıtırken beklenen veri artışına düşünmelisiniz. Bir Azure depolama hesabı 500 TiB tüm paylaşımlarında depolanan toplam birden çok paylaşımlarıyla depolayabilir unutmayın.

Azure dosya eşitleme ile tek bir Windows dosya sunucusu için birden çok Azure dosya paylaşımları eşitlenecek mümkündür. Bu, şirket içi olabilir eski, çok büyük dosya paylaşımlarını Azure dosya eşitleme sağlanabildiğinden emin olmak sağlar. Lütfen [bir Azure dosya eşitleme dağıtımı planlama](storage-files-planning.md) daha fazla bilgi için.

## <a name="data-transfer-method"></a>Veri aktarım yöntemi
Var olan bir dosyadan veri paylaşımı, bir şirket içi dosya paylaşımı gibi Azure dosyalarına aktarma toplu olarak kolay pek çok seçenek vardır. Birkaç popüler olanları (kapsamlı olmayan liste) şunları içerir:

* **Azure dosya eşitleme**: bir Azure dosya paylaşımı ("bulut uç noktasına") ve bir Windows dizin ad alanı ("sunucu uç noktası") arasında bir ilk eşitleme işleminin bir parçası olarak Azure dosya eşitleme tüm veriler var olan bir dosya paylaşımından Azure dosyaları'na çoğaltır.
* **[Azure içeri/dışarı aktarma](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: Azure içeri/dışarı aktarma hizmeti, bir Azure veri merkezine sabit disk sürücüleri sevkiyat tarafından bir Azure dosya paylaşımına güvenli bir şekilde büyük miktarlarda veri aktarmanıza olanak tanır. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy, Windows ve Windows Server ile birlikte gelen bir bilinen kopya aracıdır. Robocopy, yerel dosya paylaşımını bağlama ve ardından hedef Robocopy komutunu olarak bağlı konumu kullanarak Azure dosyaları veri aktarmak için kullanılabilir.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: AzCopy en iyi performansı sunan basit komutlar kullanılarak Azure dosyaları yanı sıra, Azure Blob Depolama, gelen ve giden veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. AzCopy, Windows ve Linux için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
* [Azure dosyaları'nı dağıtma](storage-files-deployment-guide.md)
* [Azure dosya eşitleme dağıtımı](storage-sync-files-deployment-guide.md)
