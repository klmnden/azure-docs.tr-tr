---
title: Genel amaçlı v2 depolama hesabı - Azure depolama için yükseltme | Microsoft Docs
description: Genel amaçlı v2 depolama hesapları için yükseltin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: tamram
ms.openlocfilehash: 6e77c4836531a7efd0b52b9a411ac40ff6a613fa
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224503"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı için yükseltme

Genel amaçlı v2 depolama hesabı için en son Azure depolama özelliklerini desteklemek ve tüm işlevleri, genel amaçlı v1 ve Blob Depolama hesapları dahil edilip derecelendirilir. Genel amaçlı v2 hesapları çoğu depolama senaryoları için önerilir. Genel amaçlı v2 hesapları Azure depolama, ek olarak sektörde rekabetçi işlem fiyatları düşük gigabayt başına kapasite fiyatlar sunar.

Genel amaçlı v2 depolama hesabı, genel amaçlı v1'den veya Blob Depolama hesapları için yükseltme basit bir işlemdir. Azure portal, PowerShell veya Azure CLI kullanarak yükseltebilirsiniz. Bir hesap yükseltme geri alınamaz ve faturalandırma ücretleri neden olabilir.

## <a name="upgrade-using-the-azure-portal"></a>Azure portalını kullanarak yükseltme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Depolama hesabınıza gidin.
3. İçinde **ayarları** bölümünde **yapılandırma**.
4. **Hesap Türü** altında **Yükselt**’e tıklayın.
5. **Yükseltmeyi Onayla** altında, hesabınızın adını yazın. 
6. Tıklayın **yükseltme** dikey pencerenin alt kısmındaki.

## <a name="upgrade-with-powershell"></a>Powershell ile yükseltme

Genel amaçlı v1 hesabı PowerShell kullanarak bir genel amaçlı v2 hesabına yükseltmek için önce en son sürümünü kullanacak şekilde güncelleştirin **AzureRm.Storage** modülü. PowerShell’i yükleme hakkında bilgi edinmek için bkz. [Azure PowerShell’i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). 

Ardından, depolama hesabı ve kaynak grubunuzun adını değiştirerek, hesabı yükseltmek için şu komuta çağrı yapın:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Azure CLI ile yükseltme

Genel amaçlı v1 hesabı, Azure CLI kullanarak bir genel amaçlı v2 hesabına yükseltmek için önce Azure CLI'nin en son sürümünü yükleyin. CLI yüklemesi hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

Ardından, depolama hesabı ve kaynak grubunuzun adını değiştirerek, hesabı yükseltmek için şu komuta çağrı yapın:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
``` 

## <a name="specify-an-access-tier-for-blob-data"></a>Blob veri erişim katmanı belirlemesine

Genel amaçlı v2 hesapları, tüm Azure depolama hizmetleri ve veri nesneleri destekler, ancak yalnızca blok blobları olarak Blob Depolama için erişim katmanları mevcuttur. Genel amaçlı v2 depolama hesabı için yükselttiğinizde, erişim katmanı için blob verilerinizi belirtebilirsiniz. 

Erişim katmanı, beklenen kullanım düzenlerini esas alarak en uygun maliyetli depolama seçmenize olanak sağlar. Blok blobları, sık erişimli, seyrek erişimli depolanabilir veya arşiv katmanı. Erişim katmanları hakkında daha fazla bilgi için bkz. [Azure Blob Depolama: sık erişimli, seyrek erişimli ve Arşiv depolama katmanları](../blobs/storage-blob-storage-tiers.md).

Varsayılan olarak, sık erişimli erişim katmanında yeni bir depolama hesabı oluşturulur ve bir genel amaçlı v1 depolama hesabı için sık erişim katmanı yükseltilir. Veri sonrası yükseltme için kullanılacak hangi erişim katmanı araştırıyorsanız senaryonuz göz önünde bulundurun. Genel amaçlı v2 hesabına geçirmek için iki normal kullanıcı senaryosu vardır:

* Varolan genel amaçlı v1 depolama hesabınız ve blob verilerini için doğru depolama katmanı ile bir genel amaçlı v2 depolama hesabı için bir değişiklik değerlendirmek istiyorsunuz.
* Genel amaçlı v2 depolama hesabı kullanın veya zaten varsa ve, blob verilerini sık erişimli veya seyrek erişimli depolama katmanı kullanıp kullanmayacağınızı değerlendirmek istediğiniz karar verdik.

Her iki durumda da birinci öncelik depolanması, erişimi ve bir genel amaçlı v2 depolama hesabında depolanan verileriniz üzerinde işletim maliyetini tahmin etmek ve bu maliyetin mevcut maliyetlerinizle karşılaştırmak için olan.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Geçerli, kullanım desenleri için tahmini maliyetleri

Depolama ve blob verilerini belirli bir katman genel amaçlı v2 depolama hesabındaki erişim maliyetini tahmin etmek için var olan kullanım modelinizi değerlendirmek veya, beklediğiniz kullanım modelini yaklaşık. Genel olarak, şunları bilmek istersiniz:

* Gigabayt dahil olmak üzere, kendi Blob Depolama tüketimi:
    - Depolama hesabınızda ne kadar veri depolanıyor?
    - Aylık temelde veri hacmi nasıl değişiyor; yeni veriler sürekli eski verilerin yerini alıyor mu?
* Birincil erişim düzeni dahil olmak üzere Blob Depolama veri:
    - Ne kadar veri okuma ve depolama hesabına yazılır? 
    - Depolama hesabındaki veriler üzerinde işlemler gerçekleşir ve kaç okuma işlemleri yazma?

Gereksinimleriniz için en iyi erişim katmanına karar vermek için ne kadar kapasitesini belirlemek için blob verilerinizi kullanıyor ve bu verileri nasıl kullanıldığını yararlı olabilir. 

Geçişten önce depolama hesabınız için kullanım verilerini toplamak için depolama hesabı kullanarak izleyebileceğiniz [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure İzleyici, günlüğe kaydetme işlemlerini gerçekleştiren ve Azure depolama gibi Azure Hizmetleri için ölçüm verileri sağlar. 

Depolama hesabınızda bloblar için kullanım verilerini izlemek için kapasite ölçümlerini Azure İzleyicisi'nde etkinleştirin. Kapasite ölçümleri ne kadar depolama hesabınızdaki BLOB, günlük olarak kullanıyorsanız ilgili verileri kaydedin. Kapasite ölçüm verilerini depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir. Blob Depolama kapasite her hesap türü için nasıl fiyatlandırılır bilgi edinmek için [blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).

Blob Depolama için veri erişim desenlerini izlemek için Azure İzleyici'de işlem ölçümlerini etkinleştirin. Her ne sıklıkta adlandırılır tahmin etmek için farklı Azure depolama işlemleri filtreleyebilirsiniz. Bilgi edinmek için farklı türlerdeki işlemlerin bloğu için ücretlendirilir ve ekleme blobları her hesap türü için bkz [blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).  

Azure İzleyici'den ölçümleri toplama hakkında daha fazla bilgi için bkz. [Azure İzleyici'de Azure depolama ölçümleri](storage-metrics-in-azure-monitor.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](storage-quickstart-create-account.md)
- [Azure depolama hesaplarını yönetme](storage-account-manage.md)