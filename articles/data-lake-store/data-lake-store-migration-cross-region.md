---
title: Azure Data Lake depolama Gen1 bölgeler arası geçiş | Microsoft Docs
description: Azure Data Lake depolama Gen1 için bölgeler arası geçiş hakkında bilgi edinin.
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
ms.openlocfilehash: 0bf0843314f38c0de28820c82e95b7921297bf40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60518446"
---
# <a name="migrate-azure-data-lake-storage-gen1-across-regions"></a>Azure Data Lake depolama Gen1 bölgeler arasında geçirme

Azure Data Lake depolama Gen1 yeni bölgelerde kullanıma sunulduğunda, yeni bölge yararlanmak için tek seferlik bir geçiş yapmayı seçebilirsiniz. Planlama ve Geçişi tamamlamak dikkate almanız gerekenler öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Daha fazla bilgi için [ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Data Lake depolama Gen1 hesabını iki farklı bölgede**. Daha fazla bilgi için [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md).
* **Azure veri fabrikası**. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](../data-factory/introduction.md).


## <a name="migration-considerations"></a>Geçiş konuları

İlk olarak yazar, okuyan veya Data Lake depolama Gen1 verileri işleyen uygulamanız için en iyi geçiş stratejisini tanımlayın. Bir strateji seçtiğinizde, uygulamanızın kullanılabilirlik gereksinimlerini ve geçiş sırasında oluşan kapalı kalma süresi göz önünde bulundurun. Örneğin, "lift-and-shift" bulut geçişi modelini kullanmak için en kolay yaklaşım olabilir. Tüm verilerinizi kopyalanır sırada yeni bir bölgeye Bu yaklaşımda, uygulamanın mevcut bölgenizde duraklatın. Kopyalama işlemi tamamlandığında, uygulamanız yeni bölgedeki sürdürme ve eski Data Lake depolama Gen1 hesabı silin. Geçiş sırasında kapalı kalma süresi gereklidir.

Kapalı kalma süresini azaltmak için hemen yeni bölgedeki yeni veri alma başlayabilir. Gereken en düşük verileri varsa, yeni bölgede çalıştırın. Yeni bölge yeni Data Lake depolama Gen1 hesabında var olan Data Lake depolama Gen1 hesaptan eski veri kopyalamak arka planda devam edin. Bu yaklaşımı kullanarak, yeni bölge az kapalı kalma süresiyle geçiş yapabilirsiniz. Eski tüm verilerin kopyalandığından, eski Data Lake depolama Gen1 hesabı silebilirsiniz.

Geçişinizi planlarken dikkate alınması gereken diğer önemli ayrıntıları verilmiştir:

* **Veri hacmi**. Geçiş için gereken kaynakları ve saat (gigabayt olarak, dosyalar ve klasörler vb. sayısı) içindeki veri hacmi etkiler.

* **Data Lake depolama Gen1 hesap adı**. Yeni hesap adını yeni bölgedeki genel olarak benzersiz olmalıdır. Örneğin, Doğu ABD 2, eski Data Lake depolama Gen1 hesabının adını contosoeastus2.azuredatalakestore.net olabilir. Yeni Data Lake depolama Gen1 hesabınızda Kuzey AB contosonortheu.azuredatalakestore.net adını verebilirsiniz.

* **Araçlar**. Kullanmanızı öneririz [Azure Data Factory kopyalama etkinliği](../data-factory/connector-azure-data-lake-store.md) Data Lake depolama Gen1 dosyaları kopyalamak için. Data Factory veri taşıma yüksek performansı ve güvenilirliği ile destekler. Data Factory yalnızca klasör hiyerarşisini ve dosyaların içeriğini kopyalar göz önünde bulundurun. Eski hesap yeni bir hesap için kullandığınız tüm erişim denetim listeleri (ACL'ler) el ile uygulamanız gerekir. Performans hedefleri için best-case senaryoları da dahil olmak üzere daha fazla bilgi için bkz. [kopyalama etkinliği performansı ve ayarlama Kılavuzu](../data-factory/copy-activity-performance.md). Daha hızlı bir şekilde kopyalanan verileri istediğiniz ek bulut verisi taşıma birimlerini kullanmanız gerekebilir. AdlCopy gibi diğer bazı araçları bölgeler arasında veri kopyalamayı desteklemez.  

* **Bant genişliği ücretleri**. [Bant genişliği ücretleri](https://azure.microsoft.com/pricing/details/bandwidth/) bir Azure bölgesinin dışına aktarılan veriler için geçerlidir.

* **Verilerinizi ACL'lerin**. Yeni bölgede dosya ve klasörler için ACL'ler uygulayarak güvenli hale getirin. Daha fazla bilgi için [Azure Data Lake depolama Gen1 depolanan verilerin güvenliğini sağlama](data-lake-store-secure-data.md). Geçiş, ACL'ler ayarlamak ve güncelleştirmek için kullanmanızı öneririz. Geçerli ayarlarınızı benzer ayarları kullanmak isteyebilirsiniz. Azure portalını kullanarak herhangi bir dosyaya uygulanan ACL'leri görüntüleyebileceğiniz [PowerShell cmdlet'leri](/powershell/module/az.datalakestore/get-azdatalakestoreitempermission), ya da SDK'ları.  

* **Analiz Hizmetleri konumunu**. En iyi performans için Azure Data Lake Analytics veya Azure HDInsight gibi analiz hizmetlerinizi verilerinizi aynı bölgede olması gerekir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake depolama Gen1 genel bakış](data-lake-store-overview.md)
