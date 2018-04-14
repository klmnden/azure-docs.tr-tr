---
title: "Premium depolama alanına var olan Azure veri ambarınız geçiş | Microsoft Docs"
description: "Mevcut bir veri ambarı premium depolama alanına geçirmek için yönergeler"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 03/15/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 3b43bc17b7f9cf80a9520c5c573be3a48d82e4e7
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a>Premium depolama alanına, veri ambarı geçiş
En yeni Azure SQL Data Warehouse [büyük performans öngörülebilirlik için premium depolama][premium storage for greater performance predictability]. Standart depolama üzerinde şu anda mevcut veri ambarlarında artık premium depolama alanına geçirilemez. Otomatik geçiş yararlanabilir veya geçirmek denetlemek tercih ederseniz (hangi içeren miktar kapalı kalma süresi), kendiniz geçiş yapabilirsiniz.

Birden fazla veri ambarı varsa, [otomatik geçiş zamanlama] [ automatic migration schedule] zaman da geçişi belirlemek için.

## <a name="determine-storage-type"></a>Depolama türü belirlenemiyor
Standart depolama kullanmakta olduğunuz aşağıdaki tarihlerden önce bir veri ambarı oluşturduysanız, gerekir.

| **Bölge** | **Bu tarihten önce oluşturulan veri ambarı** |
|:--- |:--- |
| Avustralya Doğu |1 Ocak 2018 |
| Çin Doğu |1 Kasım 2016 |
| Çin Kuzey |1 Kasım 2016 |
| Almanya Orta |1 Kasım 2016 |
| Almanya Kuzeydoğu |1 Kasım 2016 |
| Hindistan Batı |1 Şubat 2018 |
| Japonya Batı |1 Şubat 2018 |
| Orta Kuzey ABD |10 Kasım 2016 |

## <a name="automatic-migration-details"></a>Otomatik geçiş ayrıntıları
Varsayılan olarak, biz veritabanınız için 6:00 ile 6: 00'da, bölgenin yerel saatle sırasında arasında geçiş yapacağınız [otomatik geçiş zamanlama][automatic migration schedule]. Mevcut veri ambarınız geçiş sırasında kullanılamaz. Geçiş depolama terabayt başına yaklaşık bir saat veri ambarına olur. Otomatik geçiş herhangi bir kısmının sırasında ücret alınmaz.

> [!NOTE]
> Geçiş tamamlandığında, veri Ambarınızı tekrar çevrimiçi ve kullanılabilir olacaktır.
>
>

Microsoft (bunlar herhangi katılımı yapmanıza gerek yoktur) geçiş işlemini tamamlamak için aşağıdaki adımları sürüyor. Bu örnekte, var olan veri ambarınız standart depolama üzerinde şu anda "MyDW." adlı düşünün

1. Microsoft "MyDW_DO_NOT_USE_ [zaman damgası]." için "MyDW" yeniden adlandırır.
2. Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].” Bu süre boyunca, bir yedekleme gerçekleştirilir. Biz bu işlem sırasında herhangi bir sorunla karşılaşırsanız, birden çok duraklatır ve sürdürür görebilirsiniz.
3. Microsoft, premium depolama 2. adımda alınan yedekten "MyDW" adlı yeni bir veri ambarını oluşturur. Geri yükleme tamamlandıktan sonra "MyDW" kadar görünmez.
4. Olan geçişten önce oluştu. geri yükleme tamamlandığında, "MyDW" aynı data warehouse birimleri ve durumu (duraklatılmış ya da etkin) döndürür.
5. Geçiş işlemi tamamlandıktan sonra Microsoft "MyDW_DO_NOT_USE_ [zaman damgası]" siler.

> [!NOTE]
> Aşağıdaki ayarlar geçişin bir parçası taşınmaz:
>
> * Veritabanı düzeyinde denetimi yeniden etkinleştirilmesi gerekir.
> * Veritabanı düzeyinde güvenlik duvarı kuralları yeniden eklenmesi gerekir. Sunucu düzeyinde güvenlik duvarı kuralları etkilenmez.
>
>

### <a name="automatic-migration-schedule"></a>Otomatik geçiş zamanlama
6:00 ile 6: 00'da (her bölge yerel saati) aşağıdaki kesinti zamanlama sırasında arasında otomatik geçiş oluşur.

| **Bölge** | **Tahmini Başlangıç tarihi** | **Tahmini Bitiş tarihi** |
|:--- |:--- |:--- |
| Avustralya Doğu |19 Mart 2018 |20 Mart 2018 |
| Çin Doğu |Zaten geçişi |Zaten geçişi |
| Çin Kuzey |Zaten geçişi |Zaten geçişi |
| Almanya Orta |Zaten geçişi |Zaten geçişi |
| Almanya Kuzeydoğu |Zaten geçişi |Zaten geçişi |
| Hindistan Batı |19 Mart 2018 |20 Mart 2018 |
| Japonya Batı |19 Mart 2018 |20 Mart 2018 |
| Orta Kuzey ABD |Zaten geçişi |Zaten geçişi |

## <a name="self-migration-to-premium-storage"></a>Premium depolama için kendi kendine geçiş
Kapalı kalma süresi ortaya çıktığında denetlemek istiyorsanız, mevcut bir veri ambarı standart depolama premium depolama alanına geçirmek için aşağıdaki adımları kullanabilirsiniz. Bu seçeneği seçerseniz, bu bölgede otomatik geçiş başlamadan önce kendi kendine geçiş tamamlamanız gerekir. Bu herhangi bir çakışmasına neden otomatik geçiş riskini önlemek sağlar (bkz [otomatik geçiş zamanlama][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Kendi kendine geçiş yönergeleri
Veri ambarınız kendiniz geçirmek için yedekleme ve geri yükleme özellikleri. Geçişi geri yükleme bölümü depolama terabayt başına yaklaşık bir saat veri ambarına yapılacak beklenir. Geçiş tamamlandıktan sonra aynı adı tutmak istiyorsanız, izleyin [geçiş sırasında yeniden adlandırma adımlarını][steps to rename during migration].

1. [Duraklatma] [ Pause] , veri ambarı. 
2. [Geri yükleme] [ Restore] en son anlık.
3. Mevcut veri ambarınız standart depolama silin. **Bu adım başarısız olursa, her iki veri ambarlarında için sizden ücret alınır.**

> [!NOTE]
>
> Veri ambarınız geri yüklerken, veri Ambarınızı duraklatıldı sonra kullanılabilir en son geri yükleme noktası oluşur doğrulayın.
>
> Aşağıdaki ayarlar geçişin bir parçası taşınmaz:
>
> * Veritabanı düzeyinde denetimi yeniden etkinleştirilmesi gerekir.
> * Veritabanı düzeyinde güvenlik duvarı kuralları yeniden eklenmesi gerekir. Sunucu düzeyinde güvenlik duvarı kuralları etkilenmez.
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a>Veri ambarı (isteğe bağlı) geçiş sırasında yeniden adlandırma
İki veritabanı aynı mantıksal sunucu üzerinde aynı ada sahip olamaz. SQL veri ambarı artık bir veri ambarı yeniden adlandırmak için özelliğini destekler.

Bu örnekte, var olan veri ambarınız standart depolama üzerinde şu anda "MyDW." adlı düşünün

1. "MyDW" aşağıdaki ALTER DATABASE komutunu kullanarak yeniden adlandırın. (Bu örnekte, biz "MyDW_BeforeMigration." yeniden adlandırmadan)  Bu komut, tüm mevcut işlemleri durdurur ve başarılı olması için ana veritabanında gerçekleştirilmelidir.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Pause][Pause] "MyDW_BeforeMigration." 
3. [Geri yükleme] [ Restore] en son kullanılan (örneğin, "MyDW") olacak şekilde ada sahip yeni bir veritabanı anlık görüntüsü.
4. "MyDW_BeforeMigration." Sil **Bu adım başarısız olursa, her iki veri ambarlarında için sizden ücret alınır.**


## <a name="next-steps"></a>Sonraki adımlar
Premium depolama alanına değişiklikle Ayrıca, veri ambarı temel alınan mimarisinde veritabanı blob dosyaları sayısının artması vardır. Bu değişiklik performans yararlarını en üst düzeye çıkarmak için aşağıdaki komut dosyasını kullanarak, kümelenmiş columnstore dizinleri yeniden oluşturun. Komut dosyasını varolan verilerinizden bazıları ek BLOB'larını zorlayarak çalışır. Hiçbir eylemde bulunmamanız halinde, tablolara daha fazla veri yükleme gibi veri zamanla doğal olarak dağıtan.

Veri ambarınız ile herhangi bir sorunla karşılaştığınızda [destek bileti oluşturma] [ create a support ticket] ve başvuru "geçişi premium depolama alanına" olası neden olarak.

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: quickstart-scale-compute-portal.md
[mediumrc role]: resource-classes-for-workload-management.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
