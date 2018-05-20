---
title: Azure Data Lake Store bölgeler arası geçiş | Microsoft Docs
description: Azure Data Lake Store için bölgeler arası geçiş hakkında bilgi edinin.
services: data-lake-store
documentationcenter: ''
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: 1199eca457c3f06fdd6a4b68a05da3210ea9a2c9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="migrate-data-lake-store-across-regions"></a>Data Lake Store bölgeler arasında geçirme

Azure Data Lake Store yeni bölgelerde kullanılabilir hale geldiğinde, yeni bölge yararlanmak için bir kerelik geçiş yapmayı seçebilirsiniz. Planlama ve Geçişi tamamlamak dikkate almanız gerekenler hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Daha fazla bilgi için bkz: [ücretsiz Azure hesabınızı bugün oluşturmak](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Data Lake Store hesabı iki farklı bölgelerdeki**. Daha fazla bilgi için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).
* **Azure Data Factory**. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](../data-factory/introduction.md).


## <a name="migration-considerations"></a>Geçiş konuları

İlk olarak, yazar okur ve Data Lake Store'da verileri işleyen uygulamanızın en iyi şekilde çalışır geçiş stratejisini tanımlayın. Bir strateji seçtiğinizde, uygulamanızın kullanılabilirlik gereksinimlerini ve geçiş sırasında oluşan kapalı kalma süresi göz önünde bulundurun. Örneğin, "yükseltme ve shift" bulut geçiş modelini kullanmak için basit bir yaklaşım olabilir. Tüm verilerinizi kopyalanır ederken yeni bölge için bu yaklaşım, uygulamanın varolan bölgenizde duraklatın. Kopyalama işlemi tamamlandığında, yeni bölge uygulamanızda sürdürmek ve eski Data Lake Store hesabı silin. Geçiş sırasında kapalı kalma süresi gereklidir.

Kapalı kalma süresini azaltmak için hemen yeni bölgede yeni veri alma başlayabilir. Gereken en az veri varsa, yeni bölgede uygulamanızı çalıştırın. Eski verileri mevcut bir Data Lake Store hesabından yeni bölgeyi yeni Data Lake Store hesabına kopyalamak arka planda devam eder. Bu yaklaşımı kullanarak, yeni bölge az kapalı kalma süresi ile geçiş yapabilirsiniz. Tüm eski verileri kopyalanır, eski Data Lake Store hesabı silin.

Geçiş planlarken dikkate alınması gereken diğer önemli ayrıntılar verilmiştir:

* **Veri birimi**. Geçiş için gereken kaynakları ve saat verilerini (gigabayt, dosyalar ve klasörler vb. sayısı) hacmi etkiler.

* **Data Lake Store hesabı adı**. Yeni bölgede yeni hesap adı genel olarak benzersiz olmalıdır. Örneğin, Doğu ABD 2 eski Data Lake Store hesabınızın adını contosoeastus2.azuredatalakestore.net olabilir. Yeni Data Lake Store hesabınızda Kuzey AB contosonortheu.azuredatalakestore.net adı.

* **Araçlar**. Kullanmanızı öneririz [Azure Data Factory kopyalama etkinliği](../data-factory/connector-azure-data-lake-store.md) Data Lake Store dosyaları kopyalamak için. Veri Fabrikası yüksek performansı ve güvenilirliği ile veri hareketi destekler. Data Factory yalnızca klasör hiyerarşisini ve dosyaların içeriğini kopyalar göz önünde bulundurun. Eski hesabı yeni hesap için kullandığınız tüm erişim denetim listelerini (ACL'ler) el ile uygulamanız gerekir. Performans hedefleri iyi senaryoları için de dahil olmak üzere daha fazla bilgi için bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](../data-factory/copy-activity-performance.md). Daha hızlı kopyalanan verileri istiyorsanız, ek bulut veri taşıma birimleri kullanmanız gerekebilir. AdlCopy gibi diğer bazı araçları bölgeler arasında veri kopyalamayı desteklemez.  

* **Bant genişliği ücretleri**. [Bant genişliği ücretleri](https://azure.microsoft.com/pricing/details/bandwidth/) bir Azure bölgesinin dışına aktarılan verileri için geçerlidir.

* **Verilerinizi ACL'lerin**. Yeni bölge verilerinizi dosya ve klasörler için ACL'ler uygulayarak güvenli hale getirin. Daha fazla bilgi için bkz: [Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama](data-lake-store-secure-data.md). ACL'ler ayarlamak ve güncelleştirmek için geçiş kullanmanızı öneririz. Geçerli ayarlarınız benzer ayarlarını kullanmak isteyebilirsiniz. Azure portalını kullanarak herhangi bir dosyaya uygulanan ACL'leri görüntülemek [PowerShell cmdlet'leri](/powershell/module/azurerm.datalakestore/get-azurermdatalakestoreitempermission), ya da SDK'ları.  

* **Analiz Hizmetleri konumunu**. En iyi performans için Azure Data Lake Analytics veya Azure Hdınsight gibi Analiz Hizmetleri verilerinizi ile aynı bölgede olması gerekir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
