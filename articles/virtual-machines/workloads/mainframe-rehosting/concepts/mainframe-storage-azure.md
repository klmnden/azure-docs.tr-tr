---
title: Ana bilgisayar depolama, Azure Depolama'ya taşıma
description: Yüksek düzeyde ölçeklenebilir bir Azure storage kaynakları, geçirme ve IBM z14 uygulamaları modernleştirin, kuruluşların ana bilgisayar tabanlı yardımcı olur.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: storage
ms.openlocfilehash: dc78f87d9b47745119da91b8ed1f8f6c8572968c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65190438"
---
# <a name="move-mainframe-storage-to-azure"></a>Ana bilgisayar depolama Azure'a taşıyın

Ana bilgisayar iş yüklerini Microsoft Azure'da çalıştırmak için ana bilgisayar ait özellikleri Azure'a nasıl karşılaştırma bilmeniz gerekir. Yüksek düzeyde ölçeklenebilir depolama kaynaklarını bunlar uygulamalardan vazgeçmek olmadan modernleştirin başlamak kuruluşların yardımcı olur.

Azure ana bilgisayar gibi özellikleri ve IBM z14 tabanlı sistemlerde (şu andan itibaren en güncel modeli) için karşılaştırılabilir depolama kapasitesi sağlar. Bu makalede, Azure üzerinde benzer sonuçlar elde etmek nasıl bildirir.

## <a name="mainframe-storage-at-a-glance"></a>Bir bakışta anabilgisayar depolama

IBM ana bilgisayar, iki yolla depolama belirtir. Doğrudan erişim depolama cihazı (DASD) davranıştır. İkinci sıralı depolamadır. Depolama Yönetimi için ana bilgisayar veri tesis Depolama Yönetimi alt sisteminin (DFSMS) sağlar. Bu, çeşitli depolama cihazları veri erişimini yönetir.

[DASD](https://en.wikipedia.org/wiki/Direct-access_storage_device) (veri doğrudan erişim için kullanılacak benzersiz bir adresi izin veren ikincil değil bellek içi) depolama alanı için ayrı bir cihazı gösterir. İlk olarak, terim DASD diskler, manyetik tamburlar veya veri hücrelerini dönen için uygulanır. Ancak, terim (SSD) katı hal depolama cihazları için de uygulayabilirsiniz şimdi depolama alanı ağları (SAN'lar), ağa bağlı depolama (NAS) ve optik sürücüler. Bu belgenin amacı doğrultusunda, SAN'ları ve SSD diskleri dönen için DASD ifade eder.

DASD depolama aksine, bir ana bilgisayar üzerinde sıralı depolama burada veri erişilen bir başlangıç noktasından sonra okuyun veya bir satır olarak yazılmış bant sürücüleri gibi cihazları ifade eder.

Depolama cihazları genellikle bir fiber bağlantısı (FICON) kullanılarak bağlı veya doğrudan ana bilgisayar'ın g/ç yolu üzerinden erişilen [HiperSockets](https://www.ibm.com/support/knowledgecenter/zosbasics/com.ibm.zos.znetwork/znetwork_85.htm), bir IBM teknolojisi ile bir sunucu üzerinde bölümler arasında yüksek hızlı iletişimler için bir Hiper yönetici.

Ana bilgisayar sistemlerinin çoğu depolama iki türe ayırır:

- *Çevrimiçi depolama* (sık erişimli depolama alanı olarak da bilinir) günlük işlemleri için gereklidir. DASD depolama genellikle bu amaç için kullanılır. Ancak, günlük bant yedeklemeleri (mantıksal veya fiziksel) gibi sıralı depolama bu amaç için kullanılabilir.

- *Arşiv depolama* (soğuk depolama olarak da bilinir) belirli bir zamanda bağlanmasını garanti edilmez. Bunun yerine, bu bağlı ve gerektiği şekilde erişilebilir. Arşiv depolama, genellikle depolama için sıralı bant yedeklemeleri (mantıksal veya fiziksel) kullanılarak uygulanır.

## <a name="mainframe-versus-io-latency-and-iops"></a>Ana bilgisayar ile g/ç gecikmesi ve IOPS

Ana bilgisayarlar genellikle yüksek performanslı g/ç ve gecikme süresi düşük g/ç gerektiren uygulamalar için kullanılır. Bunlar, g/ç cihazları ve HiperSockets FICON bağlantıları kullanarak yapabilirsiniz. Uygulamalarınız ve cihazlarınız anabilgisayar'ın g/ç kanala doğrudan bağlantı için HiperSockets kullanıldığında mikrosaniye cinsinden gecikme süresi elde edilebilir.

## <a name="azure-storage-at-a-glance"></a>Bir bakışta Azure depolama

Azure hizmet olarak altyapı-a-([Iaas](https://azure.microsoft.com/overview/what-is-iaas/)) depolama seçeneklerini karşılaştırılabilir ana bilgisayar kapasite sağlayın.

Microsoft Azure'da barındırılan uygulamalar için depolama alanı petabaytlarca tutarında sunar ve çeşitli depolama seçenekleriniz vardır. Yüksek performans için SSD depolama alanından bu aralık için düşük maliyetli blob depolamaya yığın depolama ve arşivleri. Ayrıca, Azure depolama için veri yedekliliği seçeneğini sağlar; bir ana bilgisayar ortamı içinde ayarlamak için daha fazla çaba gereken bir şey.

Azure depolama alanı olarak kullanılabilir [Azure diskleri](/azure/virtual-machines/windows/managed-disks-overview), [Azure dosyaları](/azure/storage/files/storage-files-introduction), ve [Azure Blobları](/azure/storage/blobs/storage-blobs-overview) gibi aşağıdaki tabloda özetlenmiştir. Daha fazla bilgi edinin [her zaman](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Tür</th><th>Açıklama</th><th>Şunları yapmak istediğinizde kullanın:</th></tr>
</thead>
<tbody>
<tr><td>Azure Dosyaları
</td>
<td>
İstemci kitaplıkları, bir SMB arabirim sağlar ve bir <a href="https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api">REST</a> her yerden erişim sağlayan bir arabirimi depolanan dosyalar için.
</td>
<td><ul>
<li>Lift- and -uygulama ve Azure'da çalışan diğer uygulamalar arasında veri paylaşımı için yerel dosya sistemi API kullandığında bir uygulamayı buluta kaydır.</li>
<li>Geliştirme ve hata ayıklama birçok Vm'lerden erişilmesi gereken araçları Store.</li>
</ul>
</td>
</tr>
<tr><td>Azure BLOB'ları
</td>
<td>İstemci kitaplıkları sağlar ve bir <a href="https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api">REST</a> depolanabilir ve blok blobları, büyük bir ölçekte erişilen yapılandırılmamış veriyi arabirimi. Ayrıca destekler <a href="/azure/storage/blobs/data-lake-storage-introduction">Azure Data Lake depolama Gen2</a> Kurumsal büyük veri analizi çözümleri.
</td>
<td><ul>
<li>Bir uygulamada akış ve rastgele erişimli senaryoları destekler.</li>
<li>Uygulama verileri için konumdan erişebilirsiniz.</li>
<li>Bir kurumsal veri gölü, Azure üzerinde oluşturmayı ve büyük veri analizi gerçekleştirin.</li>
</ul></td>
</tr>
<tr><td>Azure Diskleri
</td>
<td>İstemci kitaplıkları sağlar ve bir <a href="https://docs.microsoft.com/rest/api/compute/disks">REST</a> kalıcı olarak depolanır ve bir bağlı sanal sabit diskten erişilen verilerin arabirimi.
</td>
<td><ul>
<li>Lift- and -shift okumak ve kalıcı diske veri yazmak için yerel dosya sistemi API'leri kullanan uygulamalar.</li>
<li>Diskin eklendiği VM'nin dışında erişilebilir için gerekli olmayan verileri Store.</li>
</ul></td>
</tr>
</tbody>
</table>
<!-- markdownlint-enable MD033 -->

## <a name="azure-hot-online-and-cold-archive-storage"></a>Azure (çevrimiçi) sıcak ve soğuk (arşiv) depolama

Belirli bir sistem için depolama türünü depolama boyutu, aktarım hızı ve IOPS gibi sistem gereksinimlerine bağlıdır. Bir ana bilgisayar üzerinde DASD türü depolama için Azure üzerinde uygulamalar genellikle Azure diskleri sürücü depolama kullanın. Ana bilgisayar arşiv depolama için blob depolama, Azure üzerinde kullanılır.

SSD'ler, Azure üzerinde en yüksek depolama performansı sağlar. (Bu belgede yazımını itibariyle) aşağıdaki seçenekler kullanılabilir:

| Tür         | Boyut           | IOPS                  |
|--------------|----------------|-----------------------|
| Ultra SSD    | 4 GB ile 64 TB  | 1\.200 için 160,000 IOPS |
| Premium SSD  | 32 TB'ye kadar 32 GB | 12 için 15.000 IOPS     |
| Standart SSD | 32 TB'ye kadar 32 GB | 12 ila 2.000 arasında IOPS      |

BLOB Depolama, Azure üzerinde en büyük depolama hacmi sağlar. Depolama boyutu ek olarak, Azure, hem yönetilen hem de yönetilmeyen depolama sunar. Yönetilen depolama ile Azure temel alınan depolama hesaplarını yönetme üstlenir. Yönetilmeyen depolama sayesinde, kullanıcı için depolama gereksinimlerini karşılamak Azure depolama hesapları uygun boyutta ayarlama sorumluluğunu üstlenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Azure sanal makineler üzerinde anabilgisayar yeniden barındırma](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Ana bilgisayar işlem Azure'a taşıyın](mainframe-compute-Azure.md)
- [Azure Blobları, Azure dosyaları veya Azure diskleri ne zaman kullanılacağını belirleme](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)
- [Standart SSD yönetilen diskler için Azure VM iş yükleri](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)

### <a name="ibm-resources"></a>IBM kaynakları

- [IBM Z üzerinde paralel Sysplex](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS ve bağ tesis: Ötesine](https://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Bir Db2 pureScale özelliği yükleme için gerekli kullanıcı oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - örnek bir komut oluşturun](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [Db2 pureScale kümelenmiş veritabanı çözümü](https://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM veri Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Kamu

- [Ana bilgisayar uygulamalarını Microsoft Azure kamu Bulutu](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft ve FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Daha fazla geçiş kaynakları

- [Platform Modernizasyonu Alliance: Azure üzerinde IBM Db2](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)
- [Azure sanal veri merkezi Lift- and -Shift Kılavuzu](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
