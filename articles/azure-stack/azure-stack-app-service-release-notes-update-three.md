---
title: Azure Stack üzerinde App Service'te güncelleştirme 3 sürüm notları | Microsoft Docs
description: Güncelleştirmede nedir hakkında Azure Stack'te App Service için üç, bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.reviewer: sethm
ms.lastreviewed: 08/20/2018
ms.openlocfilehash: 5ea711d3d4ffff72279e745290c1c8d9d854298e
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447501"
---
# <a name="app-service-on-azure-stack-update-3-release-notes"></a>Güncelleştirme 3 sürüm notları Azure Stack üzerinde App Service'e

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, iyileştirmeler ve düzeltmeler Azure uygulama Hizmeti'nde Azure Stack güncelleştirme 3 ve tüm bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları (yükleme sonrası) yapı ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure Stack tümleşik sisteminize 1807 güncelleştirmesini veya Azure App Service 1.3 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.
>
>

## <a name="build-reference"></a>Yapı Başvurusu

Yapı numarasını güncelleştirme 3'ü Azure Stack üzerinde App Service, **74.0.13698.31**

### <a name="prerequisites"></a>Önkoşullar

Başvurmak [önce Get Started belgeleri](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

1.3 Azure Stack'te Azure App Service'in yükseltmeye başlamadan önce tüm roller hazır olduğundan emin olun Azure Stack Yönetici portalı'nda Azure App Service Yönetim

![App Service rol durumu](media/azure-stack-app-service-release-notes-update-three/image01.png)

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 3'te Azure App Service, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- Kullanımı için SQL Server Always On Azure App Service kaynak sağlayıcısı veritabanları için destek.

- Yeni ortam parametresi hedefleme AAD bölgelere yardımcı olmak için oluşturma AADIdentityApp Yardımcısı komut dosyasına eklendi.

- Güncelleştirmeleri **App Service Kiracı, yönetici, İşlevler portalları ve Kudu Araçları**. Azure Stack portalı SDK sürümü ile tutarlı.

- Güncelleştirmeleri **Azure işlevleri çalışma zamanı** için **v1.0.11820**.

- Güvenilirlik ve sık karşılaşılan sorunları daha kolay tanılanması etkinleştirme hata geliştirmek için çekirdek hizmet güncelleştirmeleri.

- **Aşağıdaki uygulama çerçeveleri ve araçları güncelleştirmeleri**:
  - ASP.NET Core 2.1.2'yi eklendi
  - Eklenen NodeJS 10.0.0
  - Zulu OpenJDK 8.30.0.1 eklendi
  - Eklenen Tomcat 8.5.31 ve 9.0.8
  - Eklenen PHP sürümleri için:
    - 5.6.36
    - 7.0.30
    - 7.1.17
    - 7.2.5
  - Eklenen Wincache 2.0.0.8
  - Güncelleştirilmiş Git için Windows V'ye 2.17.1.2
  - Güncelleştirilmiş Kudu 74.10611.3437 için
  
- **Tüm rollerin temel işletim sistemi güncelleştirmeleri**:
  - [Windows Server 2016 x64 tabanlı sistemleri (KB4132216) için hizmet yığını güncelleştirmesi](https://support.microsoft.com/help/4132216/servicing-stack-update-for-windows-10-1607-may-17-2018)
  - [2018-07-x64 tabanlı sistemleri (KB4338822) için Windows Server 2016 için toplu güncelleştirme](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)

### <a name="post-update-steps-optional"></a>(İsteğe bağlı) sonrası adımlar güncelleştirme

Azure App Service Azure Stack 1.3 Güncelleştirmesi tamamlandıktan sonra Azure Stack dağıtımlarda mevcut Azure App Service'in için kapsanan veritabanı olarak geçirmek isteyen müşteriler için bu adımları uygulayın:

> [!IMPORTANT]
> Bu yordam, yaklaşık 5-10 dakika sürer.  Bu yordam, mevcut veritabanı oturum açma oturumları sonlandırma içerir.  Kapalı kalma süresi geçmek ve Azure Stack geçiş sonrasında Azure App Service doğrulamak için planlama
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

1. Kısmen yer alan için bir veritabanı dönüştürülüyor.  Bu adım, tüm etkin oturumlar sonlandırılan gerektiği kapalı kalma süresi ödenmesini

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

- Çalışanları App Service, var olan bir sanal ağda dağıtılır ve dosya sunucusu yalnızca özel ağda kullanılabilir dosya sunucusuna erişemiyor.  Bu da Azure Stack dağıtım belgeleri üzerinde Azure App Service'te çağırılır.

Mevcut bir sanal ağ ve dosya sunucunuza bağlanmak için bir dahili IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Bunu yapmak için Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
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

Azure Stack 1807 sürüm notlarında belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service'e genel bakış için bkz. [Azure App Service, Azure Stack genel bakış](azure-stack-app-service-overview.md).
- Azure Stack üzerinde App Service'e dağıtmak hazırlanması hakkında daha fazla bilgi için bkz. [Azure Stack'te App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
