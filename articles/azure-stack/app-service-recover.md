---
title: Azure Stack üzerinde App Service'te kurtarma | Microsoft Docs
description: Azure Stack App Service olağanüstü durum kurtarma için ayrıntılı kılavuz
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: jeffgilb
ms.reviewer: apwestgarth
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: b034259dde817c863d976384b08da17983ed9de7
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340248"
---
# <a name="recovery-of-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service'nın kurtarma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*  

Bu belge, App Service olağanüstü durum kurtarma için yapılacak eylemler hakkında yönergeler sağlar.

Azure Stack üzerinde App Service'te yedeklemeden kurtarmak için aşağıdaki eylemleri atılmalıdır:
1.  App Service veritabanlarını geri yükleme
2.  Dosya sunucusu paylaşımı içeriğini geri yükleme
3.  App Service rolleri ve Hizmetleri geri yükleme

Azure Stack depolama için işlev uygulamaları depolama kullandıysanız, işlev uygulamaları geri yüklemek için adımları da almalıdır.

## <a name="restore-the-app-service-databases"></a>App Service veritabanlarını geri yükleme
Uygulama hizmeti SQL Server veritabanlarını bir üretime hazır SQL Server örneğinde geri yüklenmelidir. 

Sonra [SQL Server örneği hazırlama](azure-stack-app-service-before-you-get-started.md#prepare-the-sql-server-instance) App Service veritabanlarını barındırmak için veritabanlarını yedekten geri yüklemek için aşağıdaki adımları kullanın:

1. Yönetici izinlerine sahip kurtarılan App Service veritabanlarını barındıracak SQL Server için oturum açın.
2. Yönetici izinlerine sahip çalışan bir komut isteminden App Service veritabanlarını geri yüklemek için aşağıdaki komutları kullanın:
    ```dos
    sqlcmd -U <SQL admin login> -P <SQL admin password> -Q "RESTORE DATABASE appservice_hosting FROM DISK='<full path to backup>' WITH REPLACE"
    sqlcmd -U <SQL admin login> -P <SQL admin password> -Q "RESTORE DATABASE appservice_metering FROM DISK='<full path to backup>' WITH REPLACE"
    ```
3. Her iki App Service veritabanı başarıyla geri yüklendi ve SQL Server Management Studio'dan çıkın olduğunu doğrulayın.

> [!NOTE]
> Bir yük devretme kümesi örneği hatadan kurtarmak için [bu yönergeleri](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure?view=sql-server-2017). 

## <a name="restore-the-app-service-file-share-content"></a>App Service dosya paylaşımının içeriğini geri yükleme
Sonra [dosya sunucusu hazırlama](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server) App Service dosya paylaşımı barındırmak için Kiracı dosya paylaşımının içeriğini geri yüklemeniz gerekir. Hangi yöntemi kullanabileceğiniz yeni oluşturulan App Service dosya paylaşım konumuna dosyaları kopyalamak kullanılabilir olan. Bu örnek dosya sunucusunda çalışan PowerShell ve robocopy uzak bir paylaşıma bağlanabilir ve dosyaları paylaşıma kopyalayın üzere kullanır:

```powershell
$source = "<remote backup storage share location>"
$destination = "<local file share location>"
net use $source /user:<account to use to connect to the remote share in the format of domain\username> *
robocopy /E $source $destination
net use $source /delete
```

Dosya paylaşımının içeriğini kopyalamaya ek olarak Dosya paylaşımındaki izinlerin da sıfırlamanız gerekir. Bunu yapmak için dosya sunucusu bilgisayarında bir yönetici komut istemi açın ve çalıştırın **ReACL.cmd** dosya. **ReACL.cmd** dosya App Service yükleme dosyalarında bulunan **BCDR** dizin.

## <a name="restore-app-service-roles-and-services"></a>App Service rolleri ve Hizmetleri geri yükleme
Dosya paylaşımının içeriğini ve App Service veritabanları geri yükledikten sonra sonraki App Service rolleri ve Hizmetleri geri yükleme için PowerShell kullanmanız gerekir. Bu adımlar, App Service gizli dizileri ve hizmet yapılandırması geri yükler.  

1. App Service Denetleyicisi oturum **CN0 VM** VM olarak **roleadmin** kullanarak App Service yüklemesi sırasında belirttiğiniz parola. 
    > [!TIP]
    > Sanal makinenin ağ güvenlik grubu RDP bağlantılarına izin verecek şekilde değiştirmeniz gerekir. 
2. Kopyalama **SystemSecrets.JSON** dosyasını yerel olarak denetleyiciye VM. Bu dosyanın şu şekilde yolunu sağlamanız gerekecektir `$pathToExportedSecretFile` sonraki adımda parametre. 
3. App Service rolleri ve Hizmetleri geri yüklemek için yükseltilmiş bir PowerShell konsol penceresinde aşağıdaki komutları kullanın:

    ```powershell
    # Stop App Service services on the primary controller VM
    net stop WebFarmService
    net stop ResourceMetering
    net stop HostingVssService # This service was deprecated in the App Service 1.5 release and is not required after the App Service 1.4 release.

    # Restore App Service secrets. Provide the path to the App Service secrets file copied from backup. For example, C:\temp\SystemSecrets.json.
    # Press ENTER when prompted to reconfigure App Service from backup 

    # If necessary, use -OverrideDatabaseServer <restored server> with Restore-AppServiceStamp when the restored database server has a different address than backed-up deployment.
    # If necessary, use -OverrideContentShare <restored file share path> with Restore-AppServiceStamp when the restored file share has a different path from backed-up deployment.
    Restore-AppServiceStamp -FilePath $pathToExportedSecretFile 

    # Restore App Service roles
    Restore-AppServiceRoles

    # Restart App Service services
    net start WebFarmService
    net start ResourceMetering
    net start HostingVssService  # This service was deprecated in the App Service 1.5 release and is not required after the App Service 1.4 release.

    # After App Service has successfully restarted, and at least one management server is in ready state, synchronize App Service objects to complete the restore
    # Enter Y when prompted to get all sites and again for all ServerFarm entities.
    Get-AppServiceSite | Sync-AppServiceObject
    Get-AppServiceServerFarm | Sync-AppServiceObject
    ```

> [!TIP]
> Komut tamamlandığında bu PowerShell oturumu kapatmanız önerilir.

## <a name="restore-function-apps"></a>İşlev uygulamaları geri yükleme 
Azure Stack için App Service Kiracı kullanıcı uygulamaları veya dosya paylaşımı içeriği dışında veri geri yüklemeyi desteklemez. Bu nedenle, bu gerekir yedeklenebilir ve App Service dışında kurtarılan yedekleme ve geri yükleme işlemleri. Azure Stack depolama için işlev uygulamaları depolama kullandıysanız, kayıp veri kurtarmak için aşağıdaki adımları gerçekleştirilmelidir:

1. İşlev uygulaması için kullanılacak yeni bir depolama hesabı oluşturun. Bu depolama, Azure Stack depolama, Azure depolama veya herhangi bir uyumlu depolama olabilir.
2. Depolama bağlantı dizesini alın.
3. İşlev portalını açın, işlev uygulamasına göz atın.
4. Gözat **Platform özellikleri** sekmesine **uygulama ayarları**.
5. Değişiklik **AzureWebJobsDashboard** ve **AzureWebJobsStorage** tıklayın ve yeni bağlantı dizesi için **Kaydet**.
6. Geçiş **genel bakış**.
7. Uygulamayı yeniden başlatın. Tüm hataları temizlemek için birkaç deneme zaman alabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack’te App Service’a genel bakış](azure-stack-app-service-overview.md)