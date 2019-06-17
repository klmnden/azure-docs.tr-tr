---
title: Azure depolama kullanmak için SQL Server Yedekleme ve geri yükleme | Microsoft Docs
description: Azure depolama için SQL Server Yedekleme hakkında bilgi edinin. SQL veritabanlarını yedekleme için Azure depolama avantajlarını açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: MikeRayMSFT
manager: craigg
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: 1b6660a1565b3c119cc1dec0823870c7dd5bd24f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61477153"
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>SQL Server Yedekleme ve geri yükleme için Azure depolama kullanma
## <a name="overview"></a>Genel Bakış
SQL Server 2012 SP1 CU2 ile başlayarak, SQL Server yedeklemeleri doğrudan Azure Blob Depolama hizmeti artık yazabilir. Yedekleme ve bir şirket içi SQL Server veritabanı veya bir Azure sanal makinesinde SQL Server veritabanı ile Azure Blob hizmeti geri yüklemek için bu işlevi kullanabilirsiniz. Bulut yedekleme, kullanılabilirlik, coğrafi olarak çoğaltılmış sınırsız şirket dışı depolama ve veri geçişini kolaylığı buluttan avantajları sunar. Transact-SQL veya SMO kullanarak BACKUP veya RESTORE deyimleri verebilir.

SQL Server 2016 yeni özellikler sunar; kullanabileceğiniz [dosya anlık görüntüsü backup](https://msdn.microsoft.com/library/mt169363.aspx) neredeyse anında yedekleme ile son derece hızlı geri yüklemeler gerçekleştirmeyi.

Bu konuda, neden Azure depolama için SQL yedeklerini kullanmayı seçebilirsiniz ve ardından ilgili bileşenleri açıklar açıklanmaktadır. İzlenecek yollar ve SQL Server Yedeklemelerinizin bu hizmeti kullanmaya başlamak için ek bilgiler erişmek için makalenin sonunda sağlanan kaynakları kullanabilirsiniz.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>SQL Server yedeklemeleri için Azure Blob hizmetini kullanmanın avantajları
SQL Server'ı yedeklerken karşılaştığı çeşitli zorluklar vardır. Bu zorluklar, Depolama Yönetimi, erişim şirket dışı depolama ve donanım yapılandırması için depolama hatası riskini içerir. Bu zorluklar birçoğu, SQL Server yedeklemeleri için Azure Blob Depolama hizmetini kullanarak ele alınır. Şu avantajları göz önünde bulundurun:

* **Kullanım kolaylığı**: Yedeklemelerinizi Azure bloblarında depolamaya kullanışlı olabilir, esnek ve şirket dışı seçeneğine erişmek kolaydır. SQL Server yedeklemeleriniz mevcut betikleri/kullanmak için işlerinizi değiştirme gibi kolay için şirket dışı depolama alanı oluşturulurken **URL'ye yedekleme** söz dizimi. Şirket dışı depolama, genellikle şirket dışı ve üretim veritabanı konumları etkileyebilecek tek bir olağanüstü durum önlemek için yeteri kadar üretim veritabanı konumdan olmalıdır. İçin seçerek [coğrafi çoğaltma, Azure BLOB'ları](../../../storage/common/storage-redundancy.md), ek bir tam bölge etkileyebilecek olağanüstü bir koruma katmanı vardır.
* **Yedekleme arşiv**: Azure Blob Depolama hizmetinin yedeklemeleri arşivlemek için sık kullanılan bant seçeneği için daha iyi bir alternatif sunar. Bant Depolama fiziksel nakliye medyayı korumak için bir şirket dışı tesis ve ölçüler gerektirebilir. Anlık sağlar, yedeklemelerinizi Azure Blob Storage'da depolamak, yüksek oranda kullanılabilir ve dayanıklı bir seçenek arşivleme.
* **Yönetilen donanım**: Azure hizmetleriyle donanım yönetim zahmetine yoktur. Azure Hizmetleri donanımını yönetme ve coğrafi çoğaltma, yedeklilik ve donanım hatalarına karşı koruma sağlayın.
* **Sınırsız depolama**: Azure BLOB'ları için doğrudan bir yedekleme etkinleştirerek, neredeyse sınırsız depolama erişebilirsiniz. Alternatif olarak, azure'a yedekleme bir Azure sanal makine diski makine boyutuna göre limitleri vardır. Yedeklemeler için bir Azure sanal makinesi ekleyebileceğiniz disk sayısı için bir sınır yoktur. Bu, çok büyük bir örnek için 16 disk ve daha küçük örnekler için daha az sınırdır.
* **Yedekleme kullanılabilirlik**: Azure blob'larda depolanan yedeklemeler her yerden ve herhangi bir zamanda kullanılabilir ve bir şirket içi SQL Server veya başka bir SQL çalıştıran bir Azure sanal Makinesi'nde, veritabanı İliştir/Ayır gerek kalmadan veya indirme sunucuya geri yüklemeler için kolayca erişilebilir ve VHD ekleniyor.
* **Maliyet**: Kullanılan yalnızca hizmeti Kullandıkça Ödeme yaparsınız. Şirket dışı ve yedekleme arşiv isteğe bağlı olarak uygun maliyetli hale getirilebilmektedir. Bkz: [Azure fiyatlandırma hesaplayıcısı](https://go.microsoft.com/fwlink/?LinkId=277060 "fiyatlandırma hesaplayıcısı")ve [Azure fiyatlandırma makalesi](https://go.microsoft.com/fwlink/?LinkId=277059 "fiyatlandırma makalesi") daha fazla bilgi için.
* **Depolama anlık görüntüleri**: Veritabanı dosyalarını bir Azure blob içinde depolanır ve SQL Server 2016'yı kullanıyorsanız, kullanabileceğiniz [dosya anlık görüntüsü backup](https://msdn.microsoft.com/library/mt169363.aspx) neredeyse anında yedekleme ile son derece hızlı geri yüklemeler gerçekleştirmeyi.

Daha fazla ayrıntı için [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmetinin](https://go.microsoft.com/fwlink/?LinkId=271617).

Aşağıdaki iki bölümü gerekli SQL Server bileşenleri dahil olmak üzere Azure Blob Depolama hizmeti sunar. Bileşenleri ve bunların etkileşim başarıyla yedekleme ve geri yükleme Azure Blob Depolama hizmetinden anlamak önemlidir.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Depolama hizmet bileşenleri
Aşağıdaki Azure bileşenleri için Azure Blob Depolama hizmetinin yedekleme yapılırken kullanılır.

| Bileşen | Açıklama |
| --- | --- |
| **Depolama Hesabı** |Depolama hesabı, tüm depolama hizmetleri için başlangıç noktasıdır. Bir Azure Blob Depolama hizmetine erişmek için öncelikle bir Azure depolama hesabı oluşturun. Azure Blob Depolama hizmeti hakkında daha fazla bilgi için bkz: [Azure Blob Depolama hizmetini kullanma](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Kapsayıcı** |Bir kapsayıcı, BLOB'ları kümesi bir gruplandırmasını sağlar ve sınırsız sayıda BLOB depolayabilir. SQL Server yazmak için bir Azure Blob hizmeti, yedekleme, en az oluşturulan kök kapsayıcı olmalıdır. |
| **Blob** |Bir dosya herhangi bir türü ve boyutu. BLOB'ları şu URL biçimi kullanılarak adreslenebilir: **https://[storage account].blob.core.windows.net/[container]/[blob]** . Sayfa BLOB'ları hakkında daha fazla bilgi için bkz: [anlama blok ve sayfa Blobları](https://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server bileşenleri
Aşağıdaki SQL Server bileşenleri için Azure Blob Depolama hizmetinin yedekleme yapılırken kullanılır.

| Bileşen | Açıklama |
| --- | --- |
| **URL** |Bir Tekdüzen Kaynak Tanımlayıcısı (URI) benzersiz bir yedekleme dosyası için bir URL belirtir. URL, SQL Server Yedekleme dosyasının adını ve konumunu sağlamak için kullanılır. URL, gerçek bir blob, yalnızca bir kapsayıcı işaret etmelidir. Blobun mevcut değilse oluşturulur. Mevcut bir bloba belirtilirse, yedekleme başarısız olursa sürece > WITH FORMAT seçeneği belirtildi. Belirttiğiniz BACKUP komutundaki URL örneği aşağıdadır: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]** . HTTPS önerilir, ancak gerekli değildir. |
| **Kimlik bilgisi** |Bağlanmak ve Azure Blob Depolama hizmetinde kimlik doğrulaması için gerekli olan bilgileri, bir kimlik bilgisi olarak depolanır.  SQL Server'ın yedeklemeler için bir Azure Blob veya geri yükleme ondan yazmak için sırada bir SQL Server kimlik bilgileri oluşturulmalıdır. Daha fazla bilgi için [SQL sunucusu kimlik bilgisi](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> SQL Server 2016, blok blobları desteklemek için güncelleştirildi. Lütfen bkz [Öğreticisi: Microsoft Azure Blob Depolama hizmetinin SQL Server 2016 veritabanlarıyla kullanarak](https://msdn.microsoft.com/library/dn466438.aspx) daha fazla ayrıntı için.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
1. Zaten yoksa, bir Azure hesabı oluşturun. Azure değerlendiriyorsanız göz önünde bulundurun [ücretsiz deneme sürümü](https://azure.microsoft.com/free/).
2. Ardından aracılığıyla bir depolama hesabı oluşturma ve geri yükleme gerçekleştirme konusunda sizi yönlendiren aşağıdaki öğreticilerden birine gidin.
   
   * **SQL Server 2014**: [Öğretici: SQL Server 2014'ü yedekleme ve geri yükleme için Microsoft Azure Blob Depolama hizmeti](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [Öğretici: Microsoft Azure Blob Depolama hizmetinin SQL Server 2016 veritabanları ile kullanma](https://msdn.microsoft.com/library/dn466438.aspx)
3. Ek belgeler başlayarak gözden [SQL Server Yedekleme ve geri yükleme işlemi Microsoft Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148.aspx).

Herhangi bir sorun varsa, bu konuyu gözden geçirin [URL en iyi yöntemler ve sorun giderme için SQL Server Yedekleme](https://msdn.microsoft.com/library/jj919149.aspx).

Diğer SQL Server için yedekleme ve geri yükleme seçenekleri için bkz [yedekleme ve geri yükleme için Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-backup-recovery.md).

