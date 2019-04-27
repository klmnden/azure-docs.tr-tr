---
title: Düzenli aralıklarla veri aktarımı için bir Azure çözümü seçme | Microsoft Docs
description: Veri aktarımı için bir Azure çözümü seçin, verileri düzenli aralıklarla aktardığınız öğrenin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 8f106674c1b1ec90477c7c030dc55085fcf10656
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729928"
---
# <a name="solutions-for-periodic-data-transfer"></a>Düzenli aralıklarla veri aktarımı için çözümler
 
Düzenli aralıklarla veri aktarırken, bu makalede çözümleri veri aktarımı için genel bir bakış sağlar. Ağ üzerinden düzenli aralıklarla veri aktarımı, düzenli aralıklarla veya sürekli veri taşıma yinelenecek şekilde sınıflandırılabilir. Makalede de önerilen veri aktarımı seçenekleri ve bu senaryo için ilgili önemli özellik matrisi açıklanır.

Kullanılabilir veri aktarımı seçeneklerini tümü bir genel bakış anlamak için Git [bir Azure veri aktarım çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="recommended-options"></a>Önerilen seçenekleri

Düzenli aralıklarla veri aktarımı için önerilen seçenekleri aktarımı yinelenen veya sürekli olmasına bağlı olarak iki kategoriye ayrılır.

- **Komut dosyası/programlı Araçları** – veri aktarımı, komut dosyası ve programlama, REST API'leri AzCopy ve Azure depolama gibi araçları düzenli aralıklarla gerçekleşir. Bu araçlar, BT uzmanları ve geliştiricilere hedeflenir.

    - **AzCopy** - kolayca ve dosyaları, Azure bloblarından veri kopyalamak için bu komut satırı aracını kullanın ve tablo depolama ile en iyi performans. AzCopy, eşzamanlılık ve paralellik ve kopyalama işlemleri kesintiye olduğunda sürdürebilme destekler.
    - **Azure depolama REST API'ler/SDK'lar** – bir uygulama oluştururken, Azure depolama REST API'leri karşı uygulama geliştirmek ve Azure birden çok dilde sunulan SDK'ları kullanın. REST API'ler, özellikle yüksek performans verileri azure'a veya azure'dan kopyalamak için tasarlanan Azure Storage veri hareketi kitaplığı da yararlanabilirsiniz.

- **Sürekli veri alma Araçları** – sürekli, devam eden bir veri alımı için Data Box çevrimiçi aktarım cihazı veya Azure Data Factory birini seçebilirsiniz. Bu araçlar, BT uzmanları tarafından ayarlanan ve şeffaf bir şekilde veri aktarımı otomatik hale getirebilirsiniz.

    - **Azure Data Factory** – Data Factory kullanılmalıdır üzere bir aktarım işlemi ölçeklendirmek ve varsa bir gereksinim düzenleme ve kurumsal izleme özellikleri kselt için. Azure Data Factory, düzenli olarak çeşitli Azure Hizmetleri, şirket içinde veya ikisinin birleşimini arasında dosya aktaran bir bulut işlem hattı ayarlamak için kullanın. Azure Data Factory sağlar veri hareketi ve veri dönüştürmeyi otomatikleştirin ve farklı veri depolarından veri alabilen veri odaklı iş akışları düzenleyin.
    - **Azure Data Box ailesi çevrimiçi aktarımları** -veri kutusu Edge ve veri kutusu ağ geçidi, verileri azure'a veya Azure dışına taşıma çevrimiçi ağ cihazlardır. Veri kutusu Edge yapay zeka (AI) kullanır-karşıya yükleme öncesinde verilere önceden işlenecek Edge işlem etkin. Veri kutusu ağ geçidi cihazı ile aynı veri aktarımı özellikleri sanal bir sürümüdür.


## <a name="comparison-of-key-capabilities"></a>Önemli özellikleri karşılaştırma

Aşağıdaki tabloda temel işlevleri farklılıkları özetlemektedir.

### <a name="scriptedprogrammatic-network-data-transfer"></a>Komut dosyası programlama network veri aktarımı

| Özellik                  | AzCopy                                 | Azure depolama REST API'leri       |
|-----------------------------|----------------------------------------|-------------------------------|
| Form faktörü                 | Microsoft komut satırı aracı       | Müşteriler depolama karşı geliştirin <br> Azure istemci kitaplıkları kullanarak REST API'leri |
| İlk tek seferlik Kurulum     | En az                                | Orta, değişken geliştirme çabası    |
| Veri biçimi                 | Azure Blobları, Azure dosyaları, Azure tabloları | Azure Blobları, Azure dosyaları, Azure tabloları   |
| Performans                 | Zaten en iyi duruma getirilmiş                      | Geliştirirken en iyi duruma getirme                  |
| Fiyatlandırma                     | Ücretsiz, veri çıkış ücretleri uygulanır      | Ücretsiz, veri çıkış ücretleri uygulanır        |

### <a name="continuous-data-ingestion-over-network"></a>Ağ üzerinden sürekli veri alımı

| Özellik                                       | Data Box Gateway | Data Box Edge   | Azure Data Factory        |
|----------------------------------|-----------------------------------------|--------------------------|---------------------------|
| Form faktörü                                   | Sanal cihaz             | Fiziksel cihaz          | Azure portalında Aracısı şirket içi hizmeti                                                            |
| Donanım                                      | Hiper yönetici            | Microsoft tarafından sağlanan    | NA                                                            |
| İlk kurulum efor                          | Düşük (< 30 dakika.)            | Orta (~ eşleştiği saat) | Büyük (~ gün)                                                 |
| Veri biçimi                                   | Azure Blobları, Azure dosyaları   | Azure Blobları, Azure dosyaları | [Veri depoları ve biçimlerine 70'ten fazla veri bağlayıcıları destekler.](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| Veri ön işleme                           | Hayır                         | Evet, Edge işlem    | Evet                                                           |
| Yerel önbellek<br>(şirket içi verileri depolamak için)    | Evet                        | Evet                      | Hayır                                                            |
| Diğer bulutlardan aktarma                    | Hayır                         | Hayır                       | Evet                                                           |
| Fiyatlandırma                                       | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/gateway/)                    | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                  | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>Sonraki adımlar

- [AzCopy ile veri aktarma](/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json).
- [Depolama REST API'leri ile veriler hakkında daha fazla bilgi aktarımı](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
- Anlamak için nasıl:
    - [Veri kutusu ağ geçidi ile veri aktarımı](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Azure'a göndermeden önce veri kutusu Edge ile verileri dönüştürün](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Azure Data Factory ile veri aktarmayı öğrenmek](https://docs.microsoft.com/azure/data-factory/tutorial-bulk-copy-portal).
