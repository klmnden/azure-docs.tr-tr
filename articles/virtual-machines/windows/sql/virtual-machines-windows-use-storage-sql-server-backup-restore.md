---
title: Azure depolama kullanmak için SQL Server Yedekleme ve geri yükleme | Microsoft Docs
description: Azure Storage için SQL Server'ı Yedekle öğrenin. Azure depolama birimine SQL veritabanlarını yedekleme yararlarını açıklar.
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
ms.openlocfilehash: 39d4f452143454a345bd91f550e44c93651ff933
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29399058"
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>SQL Server Yedekleme ve geri yükleme için Azure Storage kullanma
## <a name="overview"></a>Genel Bakış
SQL Server 2012 SP1 CU2 ile başlayarak, doğrudan Azure Blob Depolama hizmetinin SQL Server yedekleri artık yazabilirsiniz. Yedekleme ve bir şirket içi SQL Server veritabanı veya bir Azure sanal makinesinde SQL Server veritabanı ile Azure Blob hizmetinden geri yüklemek için bu işlevi kullanabilirsiniz. Bulut yedekleme kullanılabilirlik, coğrafi olarak çoğaltılmış sınırsız şirket dışı depolama ve veri geçişini kolaylığı bulutta avantajları sunar. Transact-SQL veya SMO kullanarak yedekleme veya geri yükleme deyimleri verebilir.

SQL Server 2016 yeni özellikleri sunar; kullanabileceğiniz [dosya anlık görüntüsü backup](http://msdn.microsoft.com/library/mt169363.aspx) neredeyse anlık yedeklemeler ve son derece hızlı geri yüklemeler gerçekleştirmeyi.

Bu konuda neden Azure depolama için SQL yedeklemeleri kullanmayı da seçebilirsiniz ve ilgili bileşenleri açıklar açıklanmaktadır. İzlenecek yollar ve bu hizmet, SQL Server Yedekleme ile kullanmaya başlamak için ek bilgiler erişmek için makalenin sonunda sağlanan kaynakları kullanabilirsiniz.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>SQL Server yedekleri için Azure Blob hizmeti kullanmanın yararları
SQL Server'ı yedeklerken yüz çeşitli zorluklar mevcuttur. Bu zorluklar Depolama Yönetimi, depolama hatası, şirket dışı depolama ve donanım yapılandırması erişim riskini içerir. Bu zorluklar birçoğu, Azure Blob Depolama hizmeti SQL Server yedekleri için kullanarak ele alınmıştır. Aşağıdaki yararları göz önünde bulundurun:

* **Kullanım kolaylığı**: Azure BLOB'ları yedekleri depolamak kullanışlı olabilir, esnek ve şirket dışı seçeneği erişmek kolaydır. SQL Server Yedeklemelerinizin varolan betikleri/kullanılacak işleriniz değiştirme olarak kadar kolay için şirket dışı depolama oluşturulurken **URL'ye yedekleme** sözdizimi. Şirket dışı depolama genellikle şirket dışı ve üretim veritabanı konumlarını etkileyebilecek tek bir olağanüstü durum önlemek için yetecek kadar üretim veritabanı konumdan olması gerekir. İçin seçerek [coğrafi çoğaltılır, Azure BLOB '](../../../storage/common/storage-redundancy.md), bütün bölgeyi etkileyebilecek bir olağanüstü durum korumasına fazladan bir katmanı vardır.
* **Yedekleme arşiv**: Azure Blob Depolama hizmeti yedeklemeleri arşivlemek için sık kullanılan bant seçeneği için daha iyi bir alternatif sunar. Bant Depolama site dışındaki bir tesis ve ölçülere medyayı korumak için fiziksel taşıma gerektirebilir. Yedeklemelerinizin Azure Blob Storage'da depolamak, anlık sağlar, yüksek oranda kullanılabilir ve bir dayanıklı arşivleme seçeneği.
* **Yönetilen donanım**: olmamasıdır Azure hizmetleriyle donanım Yönetimi yoktur. Azure Hizmetleri donanımını yönetme ve artıklık ve donanım hatalarına karşı koruma için coğrafi çoğaltma sağlayın.
* **Sınırsız depolama**: Azure BLOB'ları doğrudan yedek etkinleştirerek, neredeyse sınırsız depolama erişebilirsiniz. Alternatif olarak, yedekleme bir Azure sanal makine diski makine boyutuna göre sınırları vardır. Yedeklemeler için bir Azure sanal makinesi ekleyebilirsiniz disk sayısı için bir sınır yoktur. Bu, çok büyük bir örneği için 16 disk ve daha küçük örnekleri için daha az sınırıdır.
* **Yedekleme kullanılabilirlik**: Azure blob'larda depolanan yedeklemeleri her yerden ve herhangi bir zamanda kullanılabilir ve bir şirket içi SQL Server veya içinde bir Azure sanal makine, gerek kalmadan çalışan başka bir SQL Server geri yüklemeler için kolayca erişilebilir Veritabanı ekleme/ayırma veya indiriliyor ve VHD ekleniyor.
* **Maliyet**: yalnızca kullanılan hizmet için ödeme yaparsınız. Site dışındaki ve yedekleme arşiv isteğe bağlı olarak uygun maliyetli olabilir. Bkz: [Azure fiyatlandırma hesaplayıcısı](http://go.microsoft.com/fwlink/?LinkId=277060 "fiyatlandırma hesaplayıcısı")ve [Azure fiyatlandırma makale](http://go.microsoft.com/fwlink/?LinkId=277059 "fiyatlandırma makale") daha fazla bilgi için.
* **Depolama anlık görüntüleri**: veritabanı dosyalarını bir Azure blob depolanır ve SQL Server 2016 kullanıyorsanız, kullanabileceğiniz [dosya anlık görüntüsü backup](http://msdn.microsoft.com/library/mt169363.aspx) neredeyse anlık yedeklemeler ve son derece hızlı geri yüklemeler gerçekleştirmeyi.

Daha fazla ayrıntı için bkz: [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmetinin](http://go.microsoft.com/fwlink/?LinkId=271617).

Aşağıdaki iki bölümü gerekli SQL Server bileşenleri de dahil olmak üzere Azure Blob Depolama Birimi hizmeti tanıtır. Bileşenleri ve başarılı bir şekilde yedekleme kullanın ve Azure Blob storage hizmetinden geri yüklemek için kendi etkileşim anlamak önemlidir.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Depolama hizmet bileşenleri
Aşağıdaki Azure bileşenleri Azure Blob Depolama hizmetine yedekleme yapılırken kullanılır.

| Bileşen | Açıklama |
| --- | --- |
| **Depolama Hesabı** |Depolama hesabı, tüm depolama hizmetleri için başlangıç noktasıdır. Bir Azure Blob Storage hizmetine erişmek için ilk Azure Storage hesabı oluşturun. Azure Blob Depolama hizmeti hakkında daha fazla bilgi için bkz: [Azure Blob Depolama hizmetini kullanma](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| kapsayıcı |Bir kapsayıcı gruplandırma BLOB'lar kümesinin sağlar ve sınırsız sayıda BLOB depolayabilirsiniz. Bir SQL Server yazmak için bir Azure Blob hizmetine yedekleme, en az oluşturulan kök kapsayıcı olmalıdır. |
| **Blob** |Bir dosya türü ve boyutu. BLOB'ları şu URL biçimi kullanılarak adreslenebilir: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Sayfa Bloblarını hakkında daha fazla bilgi için bkz: [anlama blok ve sayfa BLOB'ları](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server bileşenleri
Aşağıdaki SQL Server bileşenleri, Azure Blob Depolama hizmetine yedekleme yapılırken kullanılır.

| Bileşen | Açıklama |
| --- | --- |
| **URL** |Tekdüzen Kaynak Tanımlayıcısı (URI) benzersiz bir yedekleme dosyası için bir URL belirtir. URL, SQL Server Yedekleme dosyasının adını ve konumunu sağlamak için kullanılır. URL, gerçek bir blob, yalnızca bir kapsayıcı işaret etmelidir. Blob mevcut değilse oluşturulur. Bir blob belirtilirse, yedekleme başarısız olursa, sürece > WITH FORMAT seçeneği belirtildi. Belirttiğiniz yedekleme komutta URL örneği aşağıda verilmiştir: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS önerilir ancak gerekli değildir. |
| **kimlik bilgisi** |Ve Azure Blob Depolama hizmeti için kimlik doğrulaması için gereken bilgiler bir kimlik bilgisi olarak depolanır.  SQL Server'ın yedeklemeler için bir Azure Blob veya geri yükleme ondan yazmak için sırayla bir SQL Server kimlik bilgisi oluşturulmalıdır. Daha fazla bilgi için bkz: [SQL Server kimlik bilgisi](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Kopyalayın ve bir yedekleme dosyası Azure Blob Depolama hizmetine yüklemek isterseniz, bu dosya geri yükleme işlemleri için kullanmayı planlıyorsanız, depolama seçeneği olarak sayfa blob türü kullanmanız gerekir. Bir blok blobu türü geri yükleme, bir hata ile başarısız olur.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
1. Zaten yoksa, bir Azure hesabı oluşturun. Azure değerlendirirken, düşünün [ücretsiz deneme sürümü](https://azure.microsoft.com/free/).
2. Ardından, bir depolama hesabı oluşturma ve geri yükleme gerçekleştirme aracılığıyla yol aşağıdaki öğreticiler biri aracılığıyla gidin.
   
   * **SQL Server 2014**: [Öğreticisi: SQL Server 2014 yedekleme ve geri yükleme için Microsoft Azure Blob Depolama hizmeti](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [Öğreticisi: SQL Server 2016 veritabanları ile Microsoft Azure Blob Depolama hizmetinin kullanma](https://msdn.microsoft.com/library/dn466438.aspx)
3. Ek belgeler başlayarak gözden [SQL Server Yedekleme ve geri yükleme ile Microsoft Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148.aspx).

Herhangi bir sorun varsa, konusunu gözden geçirmeniz [URL en iyi yöntemler ve sorun giderme için SQL Server Yedekleme](https://msdn.microsoft.com/library/jj919149.aspx).

Diğer SQL Server için yedekleme ve geri yükleme seçenekleri, bkz: [yedekleme ve geri yükleme Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-backup-recovery.md).

