---
title: "İçin veya SSIS bağlayıcıları kullanarak Azure Blob storage'da veri taşıma | Microsoft Docs"
description: "Veri veya SSIS bağlayıcıları kullanarak Azure Blob depolama biriminden taşıyın."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 24237173876f2b292141d9373b346721a489bc56
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Veri veya SSIS bağlayıcıları kullanarak Azure Blob depolama biriminden taşıyın
[Azure için SQL Server Integration Services Feature Pack](https://msdn.microsoft.com/library/mt146770.aspx) , Azure'a bağlanmak için Azure ve şirket içi veri kaynakları ve Azure'da depolanan verileri işlemek arasında veri aktarımı bileşenleri sağlar.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Müşterilerin şirket içi veri bulutunu taşırken, bunlar Azure teknolojiler paketimiz gücünü yararlanmak için tüm Azure hizmetinden erişebilir. Bu, örneğin, Azure Machine Learning veya Hdınsight kümesinde kullanılabilir.

Genellikle olması için ilk adım budur [SQL](sql-walkthrough.md) ve [Hdınsight](hive-walkthrough.md) izlenecek yollar.

Bir iş gereksinimlerini karma veri tümleştirme senaryolarına genel gerçekleştirmek için SSIS kullanan kurallı senaryoları tartışma için bkz [Bunun SQL Server Integration Services Feature Pack Azure ile daha](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogu.

> [!NOTE]
> Azure blob depolama tam bir giriş için bkz [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu makalede açıklanan görevleri gerçekleştirmek için bir Azure aboneliği ve kurulu bir Azure depolama hesabınız olması gerekir. Karşıya yükleme veya veri yüklemek için Azure depolama hesabı adını ve hesap anahtarını bilmesi gerekir.

* Ayarlamak için bir **Azure aboneliği**, bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Oluşturma yönergeleri için bir **depolama hesabı** ve hesabı ve anahtarı bilgilerini almak için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

Kullanılacak **SSIS Bağlayıcılar**, karşıdan yüklemeniz gerekir:

* **SQL Server 2014 veya 2016 standart (veya üstü)**: Install SQL Server Integration Services içerir.
* **Microsoft SQL Server 2014 veya 2016 tümleştirme hizmetleri özellik paketi Azure**: Bunlar indirilebilir, sırasıyla gelen [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) ve [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) sayfaları.

> [!NOTE]
> SSIS SQL Server ile birlikte yüklenir, ancak Express sürümünde bulunmaz. Hangi uygulamaların çeşitli SQL Server sürümlerinde bulunan hakkında daha fazla bilgi için bkz: [SQL Server sürümleri](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

SSIS üzerinde eğitim malzemelerini için bkz: [SSIS üzerinde ellerini eğitim](http://www.microsoft.com/download/details.aspx?id=20766)

Yukarı ve çalışan alma hakkında bilgi için kullanmaya SISS basit ayıklama, dönüştürme ve yükleme (ETL) paketleri, bkz: derleme [SSIS Öğreticisi: basit bir ETL paket oluşturma](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>NYC ücreti dataset indirin
Açıklanan örneği burada genel kullanıma açık bir veri kümesi--kullanmak [NYC ücreti dönüşleri](http://www.andresmh.com/nyctaxitrips/) veri kümesi. Veri kümesi hakkında 173 milyon ücreti üstündeçalýþan NYC içinde 2013 yıl oluşur. İki tür veri vardır: seyahat ayrıntıları veri ve ücreti verileri. Her ay için bir dosya gibi her biri sıkıştırılmamış yaklaşık 2 GB ise tüm 24 dosyalarında sahibiz.

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob depolama alanına veri yükleme
Örneği kullanırız SSIS kullanarak verileri özellik paketi şirket içi Azure blob depolama birimine taşımak için [ **Azure Blob karşıya yükleme görev**](https://msdn.microsoft.com/library/mt146776.aspx), burada gösterilen:

![yapılandırma verileri-Bilim-vm](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

Görev kullandığı Parametreler aşağıda açıklanmıştır:

| Alan | Açıklama |
| --- | --- |
| **AzureStorageConnection** |Blob dosyaları barındırıldığı işaret eden bir Azure depolama hesabı başvurduğu yeni bir tane oluşturur veya mevcut bir Azure depolama Bağlantı Yöneticisi belirtir. |
| **BlobContainer** |Karşıya yüklenen dosyaların bloblar tutun blob kapsayıcı adını belirtir. |
| **BlobDirectory** |Bir blok blobu olarak karşıya yüklenen dosyanın depolandığı blob dizini belirtir. Blob dizini sanal hiyerarşik bir yapıdır. Blob zaten varsa, BT IA değiştirildi. |
| **LocalDirectory** |Karşıya yüklenecek dosyalarını içeren yerel dizini belirtir. |
| **Dosya adı** |Belirtilen ad düzendeki dosyaları seçmek için bir ad filtre belirtir. Örneğin, MySheet\*.xls\* MySheet001.xls ve MySheetABC.xlsx gibi dosyalarını içerir |
| **TimeRangeFrom/TimeRangeTo** |Bir zaman aralığı filtresini belirtir. Değiştirilen dosyaları sonra *TimeRangeFrom* ve önce *TimeRangeTo* dahil edilir. |

> [!NOTE]
> **AzureStorageConnection** kimlik bilgilerinin doğru olması gerekir ve **BlobContainer** aktarımı denenmeden önce mevcut olması gerekir.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Verileri Azure blob depolama alanından karşıdan yükleme
SSIS ile şirket içi depolama için Azure blob depolama alanından veri indirmek için bir örneğini kullanması [Azure Blob karşıya yükleme görev](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Daha gelişmiş SSIS Azure senaryoları
SSIS özellik paketi birlikte paketleme görevler tarafından işlenecek daha karmaşık akışlar için sağlar. Örneğin, blob verilerini doğrudan bir Hdınsight kümesinde, çıktısı geri blob ve şirket içi depolama karşıdan yüklenemedi akış. SSIS Hive veya Pig işleri ek SSIS bağlayıcıları kullanarak bir Hdınsight kümesine çalıştırabilirsiniz:

* SSIS ile Azure Hdınsight kümesinde bir Hive betiği çalıştırmak için kullandığınız [Azure Hdınsight Hive görev](https://msdn.microsoft.com/library/mt146771.aspx).
* Azure Hdınsight kümesinde SSIS ile Pig betiği çalıştırmak için kullandığınız [Azure Hdınsight Pig görev](https://msdn.microsoft.com/library/mt146781.aspx).

