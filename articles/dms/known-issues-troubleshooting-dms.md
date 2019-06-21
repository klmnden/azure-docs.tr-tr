---
title: Bilinen kullanarak Azure veritabanı geçiş hizmeti ile ilgili yaygın sorunları/hatalarında sorun giderme hakkında daha fazla makale | Microsoft Docs
description: Sık karşılaşılan bilinen sorunları/Azure veritabanı geçiş hizmeti kullanımı ile ilişkili hataları giderme hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/18/2019
ms.openlocfilehash: 1d639a8b1d5c7a5dd2b7bac7c5e020be7c8b1c50
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190955"
---
# <a name="troubleshoot-common-azure-database-migration-service-issues-and-errors"></a>Yaygın Azure veritabanı geçiş hizmeti sorunlarını ve hatalarını giderme

Bu makalede, bazı yaygın sorunlar ve Azure veritabanı geçiş hizmeti kullanıcıları arasında gelebilir hataları açıklanır. Makalede ayrıca, bu sorunları ve hataları çözme hakkında bilgi içerir.

## <a name="migration-activity-in-queued-state"></a>Kuyruğa alınmış durumda geçiş etkinliği

Bir Azure veritabanı geçiş hizmeti projede yeni etkinlikler oluşturun, etkinlikleri bir kuyruğa alınmış durumda kalır.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Azure veritabanı geçiş hizmeti örneği aynı anda çalıştırılması devam eden görevleri için maksimum kapasite sınırına ulaştığında Bu sorun ortaya çıkar. Yeni etkinlik, Kapasite kullanılabilir olana kadar kuyruğa alınır. | Veri geçiş hizmeti örneği projeler arasında etkinlikleri çalıştıran var doğrulayın. Yürütme için sıraya otomatik olarak eklenin yeni etkinlikler oluşturmaya devam edebilirsiniz. Çalışan mevcut etkinliklerden birini tamamlar tamamlamaz, kuyruğa alınmış bir sonraki etkinliğe çalışmaya başlar ve durumu otomatik olarak çalışması için durum değiştirir. Sıralı etkinlik geçişi başlatmak için herhangi bir ek eylemde bulunmanız gerekmez.<br><br> |

## <a name="max-number-of-databases-selected-for-migration"></a>En fazla geçiş için seçilen veritabanı sayısı

Yönetilen örnek için Azure SQL veritabanı veya bir Azure SQL veritabanına taşımak için veritabanı geçiş projeniz için bir etkinlik oluşturma aşağıdaki hata oluşur:

* **Hata**: Geçiş ayarları doğrulama hatası","errorDetail":"'Veritabanları' max'den fazla sayı '4' nesnelerin geçiş için seçildi."

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Dörtten fazla veritabanları için bir tek geçiş etkinliği seçmiş olduğunuz olduğunda bu hata görüntüler. Şu anda, her geçiş etkinliğini dört veritabanlarına sınırlıdır. | Geçiş Etkinlik başına dört veya daha az veritabanı seçin. Dörtten fazla veritabanlarını paralel geçmeniz gerekiyorsa, Azure veritabanı geçiş hizmeti başka bir örneğini sağlayın. Şu anda, her abonelik en fazla iki Azure veritabanı geçiş hizmeti örneği destekler.<br><br> |

## <a name="errors-for-mysql-migration-to-azure-mysql-with-recovery-failures"></a>Kurtarma hatalarıyla Azure MySQL MySQL Geçiş hataları

Azure veritabanı geçiş hizmetini kullanarak MySQL için Azure veritabanı'na Mysql'i geçirdiğinizde, geçiş etkinliğini şu hatayla başarısız olur:

* **Hata**: Veritabanı geçiş hatası - görev 'TaskID' [n] kurtarma art arda hatalar nedeniyle askıya alındı.

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Geçiş yapan kullanıcı ReplicationAdmin rolüne ve/veya çoğaltma istemci, çoğaltma ve Süper (MySQL 5.6.6 öncesi'den önceki sürümler) ayrıcalıkları eksik olduğunda bu hata oluşabilir.<br><br><br><br><br><br><br><br><br><br><br><br><br> | Emin [önkoşul ayrıcalıkları](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#prerequisites) kullanıcı için hesap yapılandırılmış olan doğru MySQL örneği için Azure veritabanı. Örneğin, gerekli ayrıcalıklara sahip ' migrateuser' adlı bir kullanıcı oluşturmak için aşağıdaki adımlar izlenebilir:<br>1. Kullanıcı oluştur migrateuser@'%' tarafından TANIMLANAN 'gizli'; <br>2. Db_name.* '' gizli'; tarafından tanımlanan migrateuser'@'%' için tüm ayrıcalıkları verin Daha fazla veritabanı erişimi vermek için bu adımı yineleyin <br>3. GRANT çoğaltma ikincil üzerinde *.* '' gizli'; tarafından tanımlanan migrateuser'@'%' için<br>4. GRANT çoğaltma istemcide *.* '' gizli'; tarafından tanımlanan migrateuser'@'%' için<br>5. Flush ayrıcalıkları; |

## <a name="error-when-attempting-to-stop-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti durdurulmaya çalışılırken hata oluştu

Azure veritabanı geçiş hizmeti örneği durdururken, şu hatayı alırsınız:

* **Hata**: Hizmet için durdurma başarısız oldu. Hata: {'error': {'code': 'InvalidRequest', 'message':' bir veya daha fazla etkinlik şu anda çalışıyor. Hizmeti durdurmak için etkinlikleri tamamlamış olmanız veya bu etkinlikleri el ile durdurun ve yeniden deneyin kadar bekleyin.'}}

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Durdurma işlemi deneniyor hizmet örneği, hala çalışmakta olan veya mevcut etkinlikler geçiş projeleri içerir. Bu hata görüntüler. <br><br><br><br><br><br> | Azure veritabanı geçiş Hizmeti Durdur çalıştığınız örneğinde çalışan hiçbir etkinlik olduğundan emin olun. Hizmeti durdurmak denemeden önce etkinlikler veya projeleri de silebilirsiniz. Aşağıdaki adımlar, tüm çalışan görevlerin silerek geçiş hizmeti örneği temizlemek için projeleri kaldırma göstermektedir:<br>1. Install-Module-AzureRM.DataMigration adı <br>2. Login-AzureRmAccount <br>3. Select-AzureRmSubscription - SubscriptionName "<subName>" <br> 4. Remove-AzureRmDataMigrationProject-adı <projectName> - ResourceGroupName <rgName> - ServiceName <serviceName> - DeleteRunningTask |

## <a name="error-when-attempting-to-start-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti başlatılmaya çalışılırken bir hata oluştu

Azure veritabanı geçiş hizmeti örneği başlatma sırasında şu hatayı alırsınız:

* **Hata**: Hizmet başlatma başarısız. Hata: {'errorDetail': 'hizmet başarısız başlatmak için lütfen Microsoft desteğine başvurun'}

| Nedeni         | Çözüm |
| ------------- | ------------- |
| Önceki örneği dahili olarak başarısız olduğunda bu hata görüntüler. Bu hata nadiren oluşur ve mühendislik ekibi bu durumun farkında olur. <br> | Olamaz başlatın ve sonra değiştirmek için yeni bir tane sağlayın, hizmet örneğini silin. |

## <a name="error-restoring-database-while-migrating-sql-to-azure-sql-db-managed-instance"></a>Azure SQL DB'ye geçirme SQL örneği tarafından yönetilen veritabanı geri yüklenirken hata oluştu

SQL Server'dan Azure SQL veritabanı yönetilen örneği için çevrimiçi bir geçiş gerçekleştirdiğinizde, şu hata ile tam geçiş başarısız olur:

* **Hata**: Geri yükleme işlemi için işlem kimliği 'Operationıd' başarısız oldu. Kod 'AuthorizationFailed' Message ' istemci 'objectID' nesne kimliğine sahip ' ClientID' kapsamı üzerinde 'Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/read' işlemini gerçekleştirme yetkisi yok. ' /subscriptions/ Subscriptionıd '.'.

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Bu hata, SQL Server'dan bir Azure SQL veritabanı yönetilen örneği'ne kadar çevrimiçi geçiş için kullanılan uygulama sorumlusu, abonelikte izin katkıda sahip olmayan gösterir. Şu anda yönetilen örneği ile belirli API çağrıları geri yükleme işlemi için abonelik bu izni gerektirir. <br><br><br><br><br><br><br><br><br><br><br><br><br><br> | Kullanım `Get-AzureADServicePrincipal` PowerShell cmdlet'iyle `-ObjectId` kullanılan uygulama kimliği görünen adını listelemek için hata iletisindeki kullanılabilir.<br><br> Bu uygulama izinleri doğrulayın ve var. olmak [katkıda bulunan rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) abonelik düzeyinde. <br><br> Gerekli kısıtlamak için Azure veritabanı geçiş hizmeti mühendislik ekibi çalışma geçerli erişim rolü abonelik üzerinde katkıda bulunan. Kullanılmasına izin vermeyen bir iş gereksinimi varsa katkıda bulunan rolü, Ek Yardım için Azure desteğine başvurun. |

## <a name="error-when-deleting-nic-associated-with-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti ile ilişkili NIC silme hatası

Azure veritabanı geçiş hizmeti ile ilişkili bir ağ arabirimi kartı silmeye çalıştığınızda, silme girişimi şu hatayla başarısız olur:

* **Hata**: Azure veritabanı geçiş hizmeti için DMS hizmetini kullanarak NIC nedeniyle ilişkili NIC silinemiyor

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Azure veritabanı geçiş hizmeti örneği hala var olabilir, bu sorun ortaya çıkar ve NIC kullanma <br><br><br><br><br><br><br><br> | Bu NIC silmek için hizmet tarafından kullanılan NIC, otomatik olarak siler DMS hizmeti örneğini silin.<br><br> **Önemli**: Çalışan etkinlik silinmesini Azure veritabanı geçiş hizmeti örneği olduğundan emin olun.<br><br> Tüm projeler ve etkinlikler için Azure veritabanı geçiş hizmeti örneği ilişkili silindikten sonra hizmet örneği silebilirsiniz. Hizmet örneği tarafından kullanılan NIC, hizmet silme işleminin bir parçası olarak otomatik olarak temizlenir. |

## <a name="connection-error-when-using-expressroute"></a>Expressroute'u kullanırken bağlantı hatası

Azure veritabanı geçişi hizmeti projesi Sihirbazı kaynağına bağlanmaya çalışırken bağlantı verilerinizi uzun süren zaman aşımından sonra kaynak bağlantısı için ExpressRoute kullanıyorsanız başarısız olur.

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Kullanırken [ExpressRoute](https://azure.microsoft.com/services/expressroute/), Azure veritabanı geçiş hizmeti [gerektirir](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) sağlama hizmeti ile ilişkilendirilen sanal ağ alt ağda üç hizmet uç noktaları:<br> --Service Bus uç noktası<br> --Depolama uç noktası<br> --Hedef veritabanı uç noktası (örneğin, SQL uç noktası, Cosmos DB uç noktası)<br><br><br><br><br> | [Etkinleştirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) gerekli hizmet uç noktaları için Azure veritabanı geçiş hizmeti ile kaynak arasında ExpressRoute bağlantısı. <br><br><br><br><br><br><br><br> |

## <a name="timeout-error-when-migrating-a-mysql-database-to-azure-mysql"></a>Bir MySQL veritabanı için Azure MySQL geçişi sırasında zaman aşımı hatası

Bir MySQL veritabanı için Azure veritabanı geçiş hizmeti aracılığıyla MySQL örneği için Azure veritabanı geçiş yaptığınızda, geçiş işlemi aşağıdaki zaman aşımı hatası ile başarısız olur:

* **Hata**: Veritabanı geçişi hata - dosya - yüklenemedi dosyası için yükleme işlemi başlatılamadı 'n' RetCode: SQL_ERROR hatası SqlState: HY000 NativeError: 1205 iletisi: [MySQL] [ODBC sürücüsü] [mysqld] kilit zaman aşımı aşıldı; bekleyin İşlem yeniden başlatmayı deneyin

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Geçişi, geçiş sırasında kilit bekleme zaman aşımı nedeniyle başarısız olduğunda bu hata oluşur. | Sunucu parametresinin değerini artırmayı **'innodb_lock_wait_timeout'** . İzin verilen en yüksek değer 1073741824 ' dir. |

## <a name="error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance"></a>Kaynak SQL Server dinamik bağlantı noktası kullanırken bağlanma veya adlandırılmış örneğine hata

Adlandırılmış örnek veya dinamik bir bağlantı noktası üzerinde çalışan SQL Server kaynak için Azure veritabanı geçiş hizmeti bağlanmaya çalıştığınızda, bağlantı şu hatayla başarısız olur:

* **Hata**: -1 - SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığını doğrulayın. (sağlayıcısı: SQL ağ arabirimleri, hata: 26 - Server/örneği belirtilen bulma hatası)

| Nedeni         | Çözüm    |
| ------------- | ------------- |
| Bağlanmak için Azure veritabanı geçiş Hizmeti'nin çalıştığı kaynak SQL Server örneğine dinamik bir bağlantı noktası veya adlandırılmış bir örnek kullanarak bu sorun ortaya çıkar. SQL Server Browser hizmetini UDP bağlantı noktası 1434 gelen bağlantılar için adlandırılmış bir örnek veya dinamik bir bağlantı noktası kullanırken dinler. Dinamik bağlantı noktası, SQL Server hizmeti her başladığında değişebilir. Bir örneği SQL Server Yapılandırma Yöneticisi'ndeki ağ yapılandırması ile atanan dinamik bağlantı noktası kontrol edebilirsiniz.<br><br><br> |Kaynak SQL Server Browser hizmetini UDP bağlantı noktası 1434'e ve uygunsa dinamik olarak atanan TCP bağlantı noktası üzerinden SQL Server örneği için Azure veritabanı geçiş hizmeti bağlanabildiğinizi doğrulayın. |

## <a name="additional-known-issues"></a>Bilinen diğer sorunlar

* [Bilinen sorunları/geçiş sınırlamalarıyla birlikte Azure SQL veritabanı çevrimiçi geçişleri](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [MySQL için Azure veritabanı çevrimiçi geçişleri ile bilinen sorunları/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [PostgreSQL için Azure veritabanı çevrimiçi geçişleri ile bilinen sorunları/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Sonraki adımlar

* Makaleyi görüntüleyin [Azure veritabanı geçiş hizmeti PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Makaleyi görüntüleyin [nasıl MySQL için Azure veritabanı'nda Azure portalını kullanarak sunucu parametrelerini yapılandırma](https://docs.microsoft.com/azure/mysql/howto-server-parameters).
* Makaleyi görüntüleyin [için Azure veritabanı geçiş hizmetini kullanarak önkoşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs).
* Bkz: [kullanarak Azure veritabanı geçiş hizmeti hakkında SSS](https://docs.microsoft.com/azure/dms/faq).
