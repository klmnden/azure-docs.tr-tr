---
title: "Bir Azure dosyaları dağıtımını planlama | Microsoft Docs"
description: "Azure dosyaları dağıtımı için planlama yaparken göz önünde bulundurmanız gerekenler hakkında bilgi edinin."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: c28f341fb64271e2173cd377fa06c567e0e054a6
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="planning-for-an-azure-files-deployment"></a>Azure Dosyaları dağıtımı planlama
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları, endüstri standardı SMB protokolü erişilebilir bulutta sunar. Azure dosyaları tam olarak yönetildiğinden üretim senaryolarında dağıtma dağıtma ve bir dosya sunucusu veya NAS cihazı yönetme daha kolaydır. Bu makale bir Azure dosya paylaşımı, kuruluşunuzdaki üretim kullanımı için dağıtırken dikkate alınacak konular giderir.

## <a name="management-concepts"></a>Yönetim Kavramları
 Aşağıdaki diyagram, Azure dosyaları yönetim yapıları gösterir:

![Dosya Yapısı](./media/storage-files-introduction/files-concepts.png)

* **Depolama Hesabı**: Tüm Azure Depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için, [Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) konusuna bakın.

* **Paylaşım**: Dosya Depolama paylaşımı Azure’daki bir SMB dosyası paylaşımıdır. Tüm dizinler ve dosyalar üst paylaşımda oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir ve bir paylaşım dosyaları, dosya paylaşımı 5 Tıb toplam kapasitesini kadar sınırsız sayıda depolayabilirsiniz.

* **Dizin:** Dizinlerin isteğe bağlı hiyerarşisi.

* **Dosya**: Paylaşımdaki bir dosya. Bir dosya boyutu en fazla 1 Tıb olabilir.

* **URL biçimi**: dosya REST protokolü ile yapılan bir Azure dosya paylaşımı için isteklerini dosyalar şu URL biçimi kullanılarak adreslenebilir:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Veri erişim yöntemi
Azure dosyaları teklifleri iki, ayrı ayrı veya birbirleriyle, verilerinize erişmek için kullanabileceğiniz yöntemleri yerleşik ve kolay veri erişim:

1. **Doğrudan bulut erişim**: Any Azure dosya paylaşımı takılı tarafından [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md), ve/veya [Linux](storage-how-to-use-files-linux.md) endüstri standart sunucu ileti bloğu (ile SMB) protokolü veya dosya REST API'si aracılığıyla. SMB ile doğrudan Azure dosya paylaşımı üzerinde okuma ve yazma işlemleri paylaşımında dosyalar için yapılır. Azure VM tarafından bağlamak için SMB istemci işletim sisteminde en az desteklemelidir SMB 2.1. Kullanıcının iş istasyonunda, iş istasyonu tarafından desteklenen SMB istemci en az desteklemelidir gibi şirket içi, bağlamak için SMB 3.0 (ile şifreleme). SMB ek olarak, yeni uygulamalar veya hizmetler dosya paylaşımını yazılım geliştirme için bir kolay ve ölçeklenebilir bir uygulama programlama arabirimi sağlar dosya REST aracılığıyla doğrudan erişebilirsiniz.
2. **Azure dosya eşitleme** (Önizleme): Azure dosya eşitleme paylaşımları Windows sunucuları şirket içi veya Azure çoğaltılabilir. Kullanıcılarınızın Windows Server ile dosya paylaşımı gibi bir SMB veya NFS paylaşım yoluyla erişir. Bu, hangi veriler erişilen ve uzakta bir Azure veri merkezinden gibi bir şube ofis senaryosunda değiştiren senaryolar için kullanışlıdır. Veri arasında birden fazla Windows Server uç noktası, gibi birden çok şube ofisleri arasında çoğaltılması. Son olarak, verileri Azure dosyaları için tüm verilerin Server hala erişilebilir olduğundan, ancak sunucu verilerin tam bir kopyasını yok, katmanlı. Bunun yerine, verileri, sorunsuz kullanıcı tarafından açıldığında çağrılır.

Aşağıdaki tabloda, kullanıcılar ve uygulamalar, Azure dosya paylaşımı nasıl erişebileceğinizi gösterilmektedir:

| | Doğrudan bulut erişimi | Azure dosya eşitleme |
|------------------------|------------|-----------------|
| Hangi protokolleri kullanmak üzere gerekiyor mu? | Azure dosyaları, SMB 2.1, SMB 3.0 ve dosya REST API'si destekler. | Azure dosya paylaşımı (SMB, NFS, FTPS, vb.) Windows Server'da desteklenen tüm protokol üzerinden erişme |  
| İş yükünüzün çalıştırdığınız? | **Azure'da**: Azure dosyaları verilerinizi doğrudan erişim sağlar. | **Yavaş ağ ile şirket içi**: Windows, Linux ve macOS istemcileri, Azure dosya paylaşımı bir hızlı önbellek yerel şirket içi Windows dosya paylaşımı bağlayın. |
| ACL'ler düzeyini gerekiyor? | Paylaşım ve dosya düzeyi. | Paylaşım, dosya ve kullanıcı düzeyi. |

## <a name="data-security"></a>Veri güvenliği
Azure dosyaları veri güvenliğini sağlamaya yönelik birkaç yerleşik seçeneğiniz vardır:

* Her iki üzerinden hat protokolleri şifreleme desteği: SMB 3.0 şifreleme ve dosya REST HTTPS üzerinden. Varsayılan olarak: 
    * SMB 3.0 şifrelemesi destekleyen istemcilerin veri gönderip şifrelenmiş bir kanal üzerinden.
    * SMB 3.0 desteklemeyen istemciler iletişim kurabilir içi veri merkezi SMB 2.1 veya SMB 3.0 üzerinden şifreleme olmadan. İstemcileri ağlar arası veri merkezi SMB 2.1 veya SMB 3.0 üzerinden şifreleme olmadan iletişim kurmasına izin verilmiyor unutmayın.
    * İstemcileri dosya REST HTTP veya HTTPS üzerinden iletişim kurabilir.
* Çalışmıyorken şifreleme ([Azure depolama hizmeti şifrelemesi](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): depolama hizmeti şifreleme (SSE) temel alınan Azure Storage platformunda etkinleştirme sürecinde duyuyoruz. Bu, şifreleme tüm depolama hesapları için varsayılan olarak etkin anlamına gelir. Şifreleme çalışmıyorken varsayılan ile bir bölgede yeni bir depolama hesabı oluşturuyorsanız, etkinleştirmek için bir şey yapmanız gerekmez. Tam olarak yönetilen anahtarlarla çalışmıyorken veri şifrelenir. Çalışmıyorken şifreleme bırakmaz depolama maliyetlerini artırabilir veya performansı düşürebilir. 
* Şifrelenmiş veriler aktarım sırasında isteğe bağlı gereksinimi: Bu onay kutusu seçildiğinde, Azure dosyaları verilere erişim şifrelenmemiş kanalları izin vermez. Özellikle, yalnızca HTTPS ve SMB 3.0 şifreleme bağlantılarıyla izin verilir. 

    > [!Important]  
    > Güvenli veri aktarımı gerektiren başarısız olmasına eski SMB istemcileriniz değil şifrelemesi ile SMB 3.0 ile iletişim kurabilen neden olur. Bkz: [bağlama Windows](storage-how-to-use-files-windows.md), [bağlama Linux'ta](storage-how-to-use-files-linux.md), [bağlama üzerinde macOS](storage-how-to-use-files-mac.md) daha fazla bilgi için.

En yüksek güvenlik için her zaman iki şifreleme çalışmıyorken etkinleştirme ve modern istemcileri verilerinize erişmek için kullandığınız her veri aktarım sırasında şifreleme etkinleştirilmesi önerilir. Örneğin, bir Windows Server 2008 R2 yalnızca SMB 2.1 destekleyen, VM, bir paylaşımı bağlamak gerekiyorsa SMB 2.1 şifreleme desteklemediğinden, depolama hesabınıza şifrelenmemiş trafiğine izin vermesi gerekir.

Azure dosya paylaşımına erişmek için Azure dosya eşitleme kullanıyorsanız, her zaman HTTPS ve SMB 3.0 şifrelemesi ile Windows çalışmıyorken verilerin şifrelenmesini gerektirir mi bağımsız olarak, sunucularınızın verilerinizi eşitlemek için kullanacağız.

## <a name="data-redundancy"></a>Veri yedekliği
Azure dosyaları iki veri artıklığı seçeneklerini destekler: yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS). Aşağıdaki bölümlerde, yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama arasındaki farklar açıklanmaktadır:

### <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="geo-redundant-storage"></a>Coğrafi Olarak Yedekli Depolama
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Veri büyüme deseni
Bugün, bir Azure dosya paylaşımı boyutu üst sınırı paylaşımı anlık görüntüleri planlayacak 5 Tıb ' dir. Şu anki bu sınırlama nedeniyle, bir Azure dosya paylaşımı dağıtırken beklenen veri büyüme düşünmelisiniz. Bir Azure depolama hesabı 500 Tıb tüm paylaşımlarında depolanan toplam çoklu paylaşımları saklayabilir unutmayın.

Azure dosya eşitleme ile birden çok Azure dosya paylaşımları tek bir Windows dosya sunucusu için eşitlenecek mümkündür. Bu, daha eski, çok büyük dosya paylaşımları, şirket içi yaptığınız Azure dosya eşitleme sağlanabildiğinden emin olmak sağlar. Lütfen bakın [bir Azure dosya eşitleme dağıtımını planlama](storage-files-planning.md) daha fazla bilgi için.

## <a name="data-transfer-method"></a>Veri aktarım yöntemi
Varolan bir dosyadan veri paylaşımı, bir şirket içi dosya paylaşımı gibi Azure dosyalarıyla aktarımı toplu olarak kolay pek çok seçenek vardır. Birkaç popüler olanlar (kapsamlı olmayan bir liste) şunlardır:

* **Azure dosya eşitleme**: ilk eşitleme bir Azure dosya paylaşımı ("bulut uç noktasına") ve bir Windows dizin ad alanı (bir "sunucusu uç noktası") arasında bir parçası olarak, Azure dosya eşitleme tüm verileri var olan dosya paylaşımından Azure dosyaları çoğaltır.
* **[Azure içeri/dışarı aktarma](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: Azure içeri/dışarı aktarma hizmeti, güvenli bir şekilde Azure veri merkezinde sabit disk sürücülerine sevkiyat tarafından büyük miktarlarda verinin bir Azure dosya paylaşım içine aktarma olanak sağlar. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy Windows ve Windows Server ile birlikte gelen bir araçtır iyi bilinen kopyalama. Robocopy yerel olarak dosya paylaşımı oluşturma ve ardından Robocopy komutunu hedef olarak bağlanmış konumu kullanarak Azure dosyalarına veri aktarmak için kullanılabilir.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: AzCopy en uygun performans ile basit komutları kullanarak Azure Blob Depolama yanı sıra Azure dosyaları gelen ve giden veri kopyalamak için tasarlanmış bir komut satırı yardımcı olan. AzCopy, Windows ve Linux için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md)
* [Azure dosyaları dağıtma](storage-files-deployment-guide.md)
* [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md)