---
title: Azure Stack üzerinde App Service'te güncelleştirme 5 sürüm notları | Microsoft Docs
description: Beş Azure Stack'te App Service için bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu güncelleştirmesi ne olduğu hakkında öğrenin.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: 192ac256f013498e57ecf7939d29796af073b948
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59260570"
---
# <a name="app-service-on-azure-stack-update-5-release-notes"></a>Güncelleştirme 5 sürüm notları Azure Stack üzerinde App Service'e

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, iyileştirmeler ve düzeltmeler Azure uygulama Hizmeti'nde Azure Stack güncelleştirme 5 ve tüm bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları (yükleme sonrası) yapı ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure Stack tümleşik sisteminize 1901 güncelleştirmesini veya Azure App Service 1.5 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.


## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 5 derleme numarası üzerinde App Service, **80.0.2.15**

### <a name="prerequisites"></a>Önkoşullar

Başvurmak [önce Get Started belgeleri](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

1.5 Azure Stack'te Azure App Service'in yükseltmeye başlamadan önce:

- Tüm roller hazır olduğundan emin olun Azure Stack Yönetici portalı'nda Azure App Service Yönetim

- App Service ve ana veritabanlarını yedekleme:
  - AppService_Hosting;
  - AppService_Metering;
  - Master

- Kiracı uygulama içerik dosya paylaşımını yedekleme

- Alanınızdaki **özel betik uzantısı** sürüm **1.9.1** marketten

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 5 üzerinde Azure App Service, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- Güncelleştirmeleri **App Service Kiracı, yönetici, İşlevler portalları ve Kudu Araçları**. Azure Stack portalı SDK sürümü ile tutarlı.

- Güncelleştirmeleri **Azure işlevleri çalışma zamanı** için **v1.0.12205**.

- Güncelleştirmeleri **Kudu Araçları** işletim müşteriler için stil ve işlevselliği ile ilgili sorunları gidermek için **bağlantısı kesildi** Azure Stack. 

- Güvenilirlik ve sık karşılaşılan sorunları daha kolay tanılanması etkinleştirme hata geliştirmek için çekirdek hizmet güncelleştirmeleri.

- **Aşağıdaki uygulama çerçeveleri ve araçları güncelleştirmeleri**:
  - Ek ASP.NET Core 2.1.6 ve 2.2.0
  - Eklenen NodeJS 10.14.1
  - Eklenen NPM 6.4.1
  - Güncelleştirilmiş Kudu 79.20129.3767 için
  
- **Tüm rollerin temel işletim sistemi güncelleştirmeleri**:
  - [2019-02-x64 tabanlı sistemleri (KB4487006) için Windows Server 2016 için toplu güncelleştirme](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006)

### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]  
> Bir SQL her zaman şirket örneği ile App Service kaynak sağlayıcısı sağlamışsanız gerekir [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna ekleme](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve kaybını önlemek için veritabanlarını eşitleme Hizmet veritabanı yük devretme durumunda.

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

Azure App Service Azure Stack 1.5 güncelleştirmesi tamamlandıktan sonra Azure Stack dağıtımlarda mevcut Azure App Service'in için kapsanan veritabanı olarak geçirmek isteyen müşteriler için bu adımları uygulayın:

> [!IMPORTANT]
> Geçiş yordamı, yaklaşık 5-10 dakika sürer.  Yordamı, mevcut veritabanı oturum açma oturumları sonlandırma içerir.  Kapalı kalma süresi geçmek ve Azure Stack geçiş sonrasında Azure App Service doğrulamak için planlayın.  Azure Stack 1.3 üzerinde Azure App Service'e güncelleştirdikten sonra aşağıdaki adımları tamamladıysanız Bu adım gerekli değildir.
>
>

1. Ekleme [bir kullanılabilirlik grubuna AppService veritabanları (appservice_hosting ve appservice_metering)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)

1. Veritabanını bulunan etkinleştir
    ```sql

        sp_configure 'contained database authentication', 1;
        GO
        RECONFIGURE;
            GO
    ```

1. Tüm etkin oturumlar sonlandırılan gerektiği bir veritabanı, kısmen içerdiği için dönüştürme, dönüştürme kapalı kalma süresi ödenmesini gerektirir

    ```sql
        /******** [appservice_metering] Migration Start********/
            USE [master];

            -- kill all active sessions
            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_metering')

            EXEC(@kill);

            USE [master]  
            GO  
            ALTER DATABASE [appservice_metering] SET CONTAINMENT = PARTIAL  
            GO  

        /********[appservice_metering] Migration End********/

        /********[appservice_hosting] Migration Start********/

            -- kill all active sessions
            USE [master];

            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_hosting')

            EXEC(@kill);

            -- Convert database to contained
            USE [master]  
            GO  
            ALTER DATABASE [appservice_hosting] SET CONTAINMENT = PARTIAL  
            GO  

            /********[appservice_hosting] Migration End********/
    '''

1. Migrate Logins to Contained Database Users

    ```sql
        IF EXISTS(SELECT * FROM sys.databases WHERE Name=DB_NAME() AND containment = 1)
        BEGIN
        DECLARE @username sysname ;  
        DECLARE user_cursor CURSOR  
        FOR
            SELECT dp.name
            FROM sys.database_principals AS dp  
            JOIN sys.server_principals AS sp
                ON dp.sid = sp.sid  
                WHERE dp.authentication_type = 1 AND dp.name NOT IN ('dbo','sys','guest','INFORMATION_SCHEMA');
            OPEN user_cursor  
            FETCH NEXT FROM user_cursor INTO @username  
                WHILE @@FETCH_STATUS = 0  
                BEGIN  
                    EXECUTE sp_migrate_user_to_contained
                    @username = @username,  
                    @rename = N'copy_login_name',  
                    @disablelogin = N'do_not_disable_login';  
                FETCH NEXT FROM user_cursor INTO @username  
            END  
            CLOSE user_cursor ;  
            DEALLOCATE user_cursor ;
            END
        GO
    ```

Doğrulama

1. SQL Server'ın etkin bir kapsama sahip olup olmadığını denetleyin

    ```sql
        sp_configure  @configname='contained database authentication'
    ```

1. Varolan kapsanan davranışını denetleme
    ```sql
        SELECT containment FROM sys.databases WHERE NAME LIKE (SELECT DB_NAME())
    ```

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Çalışanları App Service, var olan bir sanal ağda dağıtılır ve dosya sunucusu yalnızca Azure Stack dağıtım belgeleri üzerinde Azure App Service'te adlandırıldığı gibi özel ağda kullanılabilir dosya sunucusuna erişemiyor.

Mevcut bir sanal ağ ve dosya sunucunuza bağlanmak için bir dahili IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
 * Kaynak: Herhangi biri
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP Adresleri
 * Hedef IP adresi aralığı: Dosya sunucusu için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: İzin Ver
 * Önceliği: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Azure App Service'te çalışan bulut yöneticileri için bilinen sorunlar

Belgeye başvurun [Azure Stack 1809 sürüm notları](azure-stack-update-1809.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service'e genel bakış için bkz. [Azure App Service, Azure Stack genel bakış](azure-stack-app-service-overview.md).
- Azure Stack üzerinde App Service'e dağıtmak hazırlanması hakkında daha fazla bilgi için bkz. [Azure Stack'te App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
