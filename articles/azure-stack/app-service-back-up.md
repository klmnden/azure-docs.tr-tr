---
title: Azure Stack üzerinde App Service'te yedekleme | Microsoft Docs
description: Ayrıntılı yönergeler için Azure Stack App Service yedekleme.
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
ms.openlocfilehash: 66f1f1389a7e4ceebc38544f2eb9836fad2bd96a
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340555"
---
# <a name="back-up-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service ' yedekleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*  

Bu belge, Azure Stack üzerinde App Service ' yedekleme hakkında yönergeler sağlar.

> [!IMPORTANT]
> Azure Stack üzerinde App Service'te yedeklenmez parçası olarak [Azure Stack altyapısını yedekleme](azure-stack-backup-infrastructure-backup.md). Azure Stack operatör App Service, başarıyla gerekirse kurtarılabilmesini sağlamak için adımları atmanız gerekir.

Azure Stack'te Azure App Service için olağanüstü durum kurtarma planlaması yaparken dikkate alınması gereken dört ana bileşenden oluşur:
1. Kaynak sağlayıcısı altyapı; Sunucu rolleri, çalışan katmanları, vb. 
2. App Service gizli dizileri
3. Uygulama hizmeti SQL Server barındırma ve ölçüm veritabanları
4. App Service dosya paylaşımında depolanabilir App Service kullanıcı iş yükü içerik   

## <a name="back-up-app-service-secrets"></a>App Service gizli diziler ' yedekleme
App Service yedekten kurtarma işlemi gerçekleştirirken, ilk dağıtım tarafından kullanılan App Service anahtarları sağlamanız yeterlidir. Bu bilgiler, App Service başarıyla dağıtılan ve güvenli bir konumda depolanan hemen sonra kaydedilmelidir. Kaynak sağlayıcısı altyapı yapılandırmasını App Service kurtarma PowerShell cmdlet'lerini kullanarak Kurtarma sırasında yedek kopyadan yeniden oluşturulacak.

Aşağıdaki adımları izleyerek uygulama hizmeti gizli anahtarları için Yönetim Portalı'nı kullanın: 

1. Azure Stack yönetim portalında Hizmet Yöneticisi olarak oturum açın.

2. Gözat **App Service'e** -> **gizli dizileri**. 

3. Seçin **gizli dizileri indir**.

   ![Gizli dizileri indir](./media/app-service-back-up/download-secrets.png)

4. Gizli diziler indirme için hazır olduğunuzda, tıklayın **Kaydet** ve App Service gizli dizileri depolamak (**SystemSecrets.JSON**) güvenli bir konumda dosya. 

   ![Gizli dizilerini kaydetme](./media/app-service-back-up/save-secrets.png)

> [!NOTE]
> App Service gizli dizileri Döndürülmüş her zaman bu adımları yineleyin.

## <a name="back-up-the-app-service-databases"></a>App Service veritabanlarını yedekleme
App Service'ı geri yüklemek için ihtiyacınız olacak **Appservice_hosting** ve **Appservice_metering** veritabanı yedeklemeleri. Bu veritabanları yedeklenir ve güvenli bir şekilde düzenli olarak kaydedilen emin olmak için SQL Server bakım planları veya Azure Backup sunucusu kullanmanızı öneririz. Normal SQL yedeklerini sağlama herhangi bir yöntem oluşturulur ancak kullanılabilir.

El ile SQL Server'a her açmışken bu veritabanlarını yedeklemek için aşağıdaki PowerShell komutlarını kullanabilirsiniz:

  ```powershell
  $s = "<SQL Server computer name>"
  $u = "<SQL Server login>" 
  $p = read-host "Provide the SQL admin password"
  sqlcmd -S $s -U $u -P $p -Q "BACKUP DATABASE appservice_hosting TO DISK = '<path>\hosting.bak'"
  sqlcmd -S $s -U $u -P $p -Q "BACKUP DATABASE appservice_metering TO DISK = '<path>\metering.bak'"
  ```

> [!NOTE]
> SQL AlwaysOn veritabanlarını yedeklemek gerekirse izleyin [bu yönergeleri](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server?view=sql-server-2017). 

Tüm veritabanları başarıyla yedeklendi sonra App Service gizli bilgilerle birlikte güvenli bir konuma .bak dosyaları kopyalayın.

## <a name="back-up-the-app-service-file-share"></a>App Service dosya paylaşımını yedekleme
App Service Kiracı uygulama bilgileri depolar Dosya paylaşımındaki. Böylece geri yükleme gerekiyorsa mümkün olduğunca küçük veriler kaybolur bu App Service veritabanlarının yanı sıra düzenli olarak yedeklemelisiniz. 

App Service dosya paylaşımını yedekleme düzenli olarak dosya paylaşımı kopyalamak için Azure Backup sunucusu veya başka bir yöntemi kullanabilirsiniz, içerik için içerik konumu için önceki tüm kurtarma bilgilerini kaydettiğiniz. 

Örneğin, bir Windows PowerShell (PowerShell ISE değil) konsol oturumundan robocopy kullanmak için aşağıdaki adımları kullanabilirsiniz:

```powershell
$source = "<file share location>"
$destination = "<remote backup storage share location>"
net use $destination /user:<account to use to connect to the remote share in the format of domain\username> *
robocopy $source $destination
net use $destination /delete
```

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack üzerinde App Service'e geri yükleme](app-service-recover.md)