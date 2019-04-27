---
title: Azure veri aktarımı, büyük veri kümeleri, Orta yüksek ağ bant genişliği için Seçenekler | Microsoft Docs
description: Veri aktarımı için bir Azure çözümü seçin, Orta yüksek ağ bant genişliği ortamınızda yüklü ve büyük veri kümelerini aktarmak planlama hakkında bilgi edinin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 8dd55032c933cdc31b848addfdac991550376dcf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729248"
---
# <a name="data-transfer-for-large-datasets-with-moderate-to-high-network-bandwidth"></a>Yüksek ağ bant genişliği ile orta, büyük veri kümeleri için veri aktarımı
 
Bu makalede ortamınızda Orta yüksek ağ bant genişliği olması ve büyük veri kümelerini aktarmak planlama yaparken çözümleri veri aktarımı için genel bir bakış sağlar. Makalede de önerilen veri aktarımı seçenekleri ve bu senaryo için ilgili önemli özellik matrisi açıklanır.

Kullanılabilir veri aktarımı seçeneklerini tümü bir genel bakış anlamak için Git [bir Azure veri aktarım çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="scenario-description"></a>Senaryo açıklaması

Büyük veri kümelerinde veri boyutları PBs TB'a sırasına göre bakın. Orta yüksek ağ bant genişliği için 100 MB / 10 GB/sn ifade eder.

## <a name="recommended-options"></a>Önerilen seçenekleri

Bu senaryoda, önerilen seçenekler Orta ağ bant genişliği veya yüksek ağ bant genişliği olmasına bağlıdır.

### <a name="moderate-network-bandwidth-100-mbps---1-gbps"></a>Orta ağ bant genişliği (100 MB - 1 GB/sn)

Orta ağ bant genişliği ile ağ üzerinden veri aktarımı için zaman proje gerekir.

Zaman tahmin edin ve bu, temel ağ aktarımı üzerinden veya çevrimdışı bir aktarım arasında seçin için aşağıdaki tabloyu kullanın. Ağ veri aktarımı için tahmini süre (% 90'ını kullanımı varsayılarak) çeşitli kullanılabilir ağ bant genişlikleri için tablo gösterir.  

![Ağ aktarım veya çevrimdışı aktarımı](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

- Çok yavaş olması için Ağ aktarımını öngörülen, fiziksel bir cihaz kullanmanız gerekir. Önerilen seçenekleri bu durumda Azure Data Box ailesi veya Azure içeri/dışarı kendi disklerinizi kullanarak çevrimdışı aktarımı cihazlardır.

    - **Azure Data Box ailesi çevrimdışı aktarımları** – zaman, ağ kullanılabilirliği veya maliyet sınırlama getirilir, büyük miktarda veriyi Azure'a taşımak için Microsoft tarafından sağlanan veri kutusu cihazlarından cihazları kullanın. Robocopy gibi araçları kullanarak şirket içi veri kopyalayın. Veri boyutu aktarım için hedeflenen bağlı olarak Data Box Disk, Data Box veya veri kutusu ağır seçebilirsiniz.
    - **Azure içeri/dışarı aktarma** – büyük miktarda veriyi Azure Blob Depolama ve Azure dosyaları için güvenli bir şekilde içeri aktarmak için kendi disk sürücüleri sevkiyat tarafından kullanımı Azure içeri/dışarı aktarma hizmeti. Bu hizmet, verileri Azure Blob depolama alanından disk sürücülerine aktarmak ve şirket içi sitelerinize teslim etme için de kullanılabilir.

- Uygun olması için Ağ aktarımını öngörülen sonra ayrıntılı aşağıdaki araçlardan herhangi birini kullanabilirsiniz [yüksek ağ bant genişliği](#high-network-bandwidth).


### <a name="high-network-bandwidth-1-gbps---100-gbps"></a>Yüksek ağ bant genişliği (1 Gbps - 100 GB/sn)

Kullanılabilir ağ bant genişliği yüksekse, aşağıdaki araçlardan birini kullanın.

- **AzCopy** - kolayca ve dosyaları, Azure bloblarından veri kopyalamak için bu komut satırı aracını kullanın ve tablo depolama ile en iyi performans. AzCopy, eşzamanlılık ve paralellik ve kopyalama işlemleri kesintiye olduğunda sürdürebilme destekler.
- **Azure depolama REST API'ler/SDK'lar** – bir uygulama oluştururken, Azure depolama REST API'leri karşı uygulama geliştirmek ve Azure birden çok dilde sunulan SDK'ları kullanın.
- **Azure Data Box ailesi çevrimiçi aktarımları** – veri kutusu Edge ve veri kutusu ağ geçidi, verileri azure'a veya Azure dışına taşıma çevrimiçi ağ cihazlardır. Veri kutusu yüklenecek olduğunda eşzamanlı bir fiziksel cihaz gereken sürekli alımı ve öncesinde verileri ön işleme için Edge kullanın. Veri kutusu ağ geçidi cihazı ile aynı veri aktarımı özellikleri sanal bir sürümüdür. Her durumda, veri aktarımı, cihaz tarafından yönetilir.
- **Azure Data Factory** – Data Factory kullanılmalıdır üzere bir aktarım işlemi ölçeklendirmek ve varsa bir gereksinim düzenleme ve kurumsal izleme özellikleri kselt için. Data Factory, düzenli olarak çeşitli Azure Hizmetleri, şirket içinde veya ikisinin birleşimini arasında dosyaları aktarmak için kullanın. Data Factory ile oluşturun ve farklı veri depolarından veri alabilen ve veri hareketi ve veri dönüştürmeyi otomatik hale getirin (işlem hatları olarak adlandırılır) veri odaklı iş akışları zamanlama.

## <a name="comparison-of-key-capabilities"></a>Önemli özellikleri karşılaştırma

Aşağıdaki tablolarda, önerilen seçenek için temel işlevleri farklılıkları özetler.

### <a name="moderate-network-bandwidth"></a>Orta ağ bant genişliği

Çevrimdışı veri aktarımı kullanırken, temel işlevleri farklılıkları anlamak için aşağıdaki tabloyu kullanın.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy            |    İçeri/Dışarı Aktarma                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    Veri boyutu                        |    35 TB'a kadar                 |    Cihaz başına en fazla 80 TB'a                       |    Cihaz başına en fazla 800 TB               |    Değişken                            |
|    Veri türü                        |    Azure BLOB'ları                  |    Azure BLOB'ları<br>Azure Dosyaları                    |    Azure BLOB'ları<br>Azure Dosyaları            |    Azure BLOB'ları<br>Azure Dosyaları          |
|    Form faktörü                      |    Sipariş başına 5 SSD             |    1 x 50-lb. Sipariş başına Masaüstü boyutlu cihaz    |    1 X ~ 500-lb. Sipariş başına büyük cihaz    |    Sipariş başına en fazla 10 HDD'ler/SSD        |
|    İlk kurulum süresi               |    Düşük <br>(15 dakika)            |    Orta için düşük <br> (< 30 dakika)               |    Orta<br>(1-2 saat)               |    Çok zor Orta<br>(değişken) |
|    Verileri Azure'a Gönder               |    Evet                          |    Evet                                           |    Evet                                   |    Evet                                 |
|    Azure'dan veri dışarı aktarma           |    Hayır                           |    Hayır                                            |    Hayır                                    |    Evet                                 |
|    Şifreleme                       |    AES 128 bit                  |    AES 256 bit                                   |    AES 256 bit                           |    AES 128 bit                         |
|    Donanım                         |     Microsoft tarafından sağlanan          |    Microsoft tarafından sağlanan                            |    Microsoft tarafından sağlanan                    |    Müşteri tarafından sağlanan                   |
|    Ağ arabirimi                |    USB 3.1/SATA                 |    RJ 45, SFP+                                   |    RJ45, QSFP+                           |    SATA II/SATA III                    |
|    İş ortağı tümleştirmesi              |    Belirli Kullanıcılar                         |    [Yüksek](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                          |    [Yüksek](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                  |    Belirli Kullanıcılar                                |
|    Sevkiyat                         |    Microsoft tarafından yönetilen            |    Microsoft tarafından yönetilen                             |    Microsoft tarafından yönetilen                     |    Müşteri tarafından yönetilen                    |
| Veri hareket ettiğinde kullanın         |Bir ticari sınır içinde|Bir ticari sınır içinde|Bir ticari sınır içinde|Coğrafi sınırlar arasında örneğin BİZE AB|
|    Fiyatlandırma                          |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


Çevrimiçi veri aktarımı kullanıyorsanız, tablo aşağıdaki bölümde yüksek ağ bant genişliği için kullanın.

### <a name="high-network-bandwidth"></a>Yüksek ağ bant genişliği

|                                     |    Araçlar AzCopy <br>Azure PowerShell <br>Azure CLI             |    Azure depolama REST API, SDK'ları                   |    Veri kutusu ağ geçidi veya veri kutusu kenar          |    Azure Data Factory                                            |
|-------------------------------------|------------------------------------|----------------------------------------------|----------------------------------|-----------------------------------------------------------------------|
|    Veri türü                  |    Azure Blobları, Azure dosyaları, Azure tabloları    |    Azure Blobları, Azure dosyaları, Azure tabloları    |    Azure Blobları, Azure dosyaları                           |   Veri depoları ve biçimlerine 70'ten fazla veri bağlayıcıları destekler.    |
|    Form faktörü                |    Komut satırı araçları                        |    Programlama arabirimi                    |    Microsoft sanal sağlar <br>ya da fiziksel cihaz     |    Azure portalında hizmet                                            |
|    İlk tek seferlik Kurulum     |    Kolay               |    Orta                       |    Kolay (< 30 dakika) denetlemek (1-2 saat)            |    Kapsamlı                                                          |
|    Veri ön işleme              |    Hayır                                        |    Hayır                                        |    Evet (Edge ile işlem)                               |    Evet                                                                |
|    Diğer bulutlardan aktarma       |    Hayır                                        |    Hayır                                        |    Hayır                                                    |    Evet                                                                |
|    Kullanıcı türü                        |    BT Pro ya da geliştirme                                       |    Geliştirme                                       |    BT Uzmanı                                                |    BT Uzmanı                                                             |
|    Fiyatlandırma                          |    Ücretsiz, veri çıkış ücretleri uygulanır         |    Ücretsiz, veri çıkış ücretleri uygulanır         |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                                               |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/)                                                            |

## <a name="next-steps"></a>Sonraki adımlar

- [İçeri/dışarı aktarma ile veri aktarmayı öğrenmek](/azure/storage/common/storage-import-export-data-to-blobs).
- Anlamak için nasıl

    - [Data Box Disk ile veri aktarımı](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal).
    - [Data Box ile veri aktarımı](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal).
- [AzCopy ile veri aktarma](/azure/storage/common/storage-use-azcopy-v10).
- Anlamak için nasıl:
    - [Veri kutusu ağ geçidi ile veri aktarımı](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Azure'a göndermeden önce veri kutusu Edge ile verileri dönüştürün](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Azure Data Factory ile veri aktarmayı öğrenmek](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal).
- Veri aktarımı için REST API'lerini kullanma

    - [. NET'te](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [Java'da](https://docs.microsoft.com/java/api/overview/azure/storage/client)
