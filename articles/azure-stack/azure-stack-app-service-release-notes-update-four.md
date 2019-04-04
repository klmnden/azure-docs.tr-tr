---
title: Azure Stack üzerinde App Service'te güncelleştirme 4 sürüm notları | Microsoft Docs
description: Güncelleştirmede nedir hakkında dört Azure Stack üzerinde App Service'te, bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.reviewer: anwestg
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: c8900e639e6baba29067c50d4ac754d4dbda2dd6
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449232"
---
# <a name="app-service-on-azure-stack-update-4-release-notes"></a>Güncelleştirme 4 sürüm notları Azure Stack üzerinde App Service'e

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, iyileştirmeler ve düzeltmeler Azure uygulama Hizmeti'nde Azure Stack güncelleştirme 4 ve tüm bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları (yükleme sonrası) yapı ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure Stack tümleşik sisteminize 1809 güncelleştirmesini veya Azure App Service 1.4 dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.
>
>

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack Update 4 yapı numarası üzerinde App Service, **78.0.13698.5**

### <a name="prerequisites"></a>Önkoşullar

Başvurmak [önce Get Started belgeleri](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

1.4 Azure Stack'te Azure App Service'in yükseltmeye başlamadan önce:

- Tüm roller hazır olduğundan emin olun Azure Stack Yönetici portalı'nda Azure App Service Yönetim

- App Service ve ana veritabanlarını yedekleme:
  - AppService_Hosting;
  - AppService_Metering;
  - Master

- Kiracı uygulama içerik dosya paylaşımını yedekleme

- Özel betik uzantısı'nın sürümü 1.9 marketten entegratörlerine dağıtın

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 4'te Azure App Service, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- Çözüm için [CVE 2018 8600](https://aka.ms/CVE20188600) arası güvenlik açığı komut Site.

- App Service 2018-02-01 API sürümü için destek eklendi

- Güncelleştirmeleri **App Service Kiracı, yönetici, İşlevler portalları ve Kudu Araçları**. Azure Stack portalı SDK sürümü ile tutarlı.

- Güncelleştirmeleri **Azure işlevleri çalışma zamanı** için **v1.0.11959**.

- Güvenilirlik ve sık karşılaşılan sorunları daha kolay tanılanması etkinleştirme hata geliştirmek için çekirdek hizmet güncelleştirmeleri.

- **Aşağıdaki uygulama çerçeveleri ve araçları güncelleştirmeleri**:
  - Eklenen NodeJS 10.6.0
  - Eklenen NPM 6.1.0
  - Zulu OpenJDK 8.31.0.2 eklendi
  - Eklenen Tomcat 8.5.34 ve 9.0.12
  - Eklenen PHP sürümleri için:
    - 5.6.37
    - 7.0.31
    - 7.1.20
    - 7.2.8
  - Güncelleştirme Python sürümleri için:
    - 2.7.15
    - 3.6.6
  - Güncelleştirilmiş Git için Windows V'ye 2.17.1.2
  - Güncelleştirilmiş Kudu 78.11022.3613 için
  
- **Tüm rollerin temel işletim sistemi güncelleştirmeleri**:
  - [2018-10-x64 tabanlı sistemleri (KB4462928) için Windows Server 2016 için toplu güncelleştirme](https://support.microsoft.com/help/4462928/windows-10-update-kb4462928)

- Wordpress dağıtırken şablonu doğrulama sorunu Çözümlendi; DNN; ve Orchard CMS galeri öğeleri

- Azure Stack, Azure Resource Manager istemci sertifikayı döndürdüğünde yapılandırma sorunu Çözümlendi

- App Service Kiracı Portalı'ndaki kaynak arası kaynak paylaşımı ayarları işlevselliği geri

- Kaynak sağlayıcısı denetim düzlemi yapılandırılan SQL Server örneğine bağlanırken, App Service Yönetim Portalı deneyiminde hata iletisini görüntüler

- Yeni işlev uygulamasında Belirtildiğinde özel depolama bağlantı dizesi içindeki uç nokta belirtildiğinden emin olun

### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]  
> Bir SQL her zaman şirket örneği App Service RP'ye sağladıysanız gerekir [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna ekleme](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve hizmette kaybını önlemek için veritabanlarını eşitleme Olay veritabanı yük devretme.

### <a name="post-update-steps-optional"></a>Güncelleştirme sonrası adımları (isteğe bağlı)

Azure App Service Azure Stack 1.4 güncelleştirmesi tamamlandıktan sonra Azure Stack dağıtımlarda mevcut Azure App Service'in için kapsanan veritabanı olarak geçirmek isteyen müşteriler için bu adımları uygulayın:

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
