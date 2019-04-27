---
title: SQL Server Integration Services paketlerini Azure'a geçirme | Microsoft Docs
description: SQL Server Integration Services paketlerini Azure'a geçirmeyi öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 884af4624c1e92ee765353c90fd189220664381d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532355"
---
# <a name="migrate-sql-server-integration-services-packages-to-azure"></a>SQL Server Integration Services paketlerini Azure'a geçirme
SQL Server Integration Services (SSIS) kullanın ve Azure SQL veritabanı sunucusu veya Azure SQL veritabanı yönetilen örneği tarafından barındırılan SSISDB hedef SQL Server tarafından barındırılan SSISDB kaynağından SSIS projelerini/paketlerini geçiş yapmak istiyorsanız, bunları yeniden dağıtabilirsiniz Tümleştirme hizmetleri Dağıtım Sihirbazı'nı kullanarak. SQL Server Management Studio (SSMS) içinde sihirbazından başlatabilirsiniz.

SSIS kullandığınız sürümü önceyse dağıtarak, SSIS projelerini/proje dağıtım modeline paketlerini önce 2012'den önce tümleştirme hizmetleri proje dönüştürme SSMS başlatılabilir Sihirbazı'nı kullanarak bunları dönüştürmeniz gerekir. Daha fazla bilgi için bkz [projeleri için proje dağıtım modeli dönüştürme](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure veritabanı geçiş hizmeti (DMS) geçişi SSISDB kaynağı şu anda desteklememektedir ancak SSIS projelerini/aşağıdaki işlemi kullanarak paketlerinizi yeniden dağıtabilirsiniz. 

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Kaynak SSIS projelerini/paketlerini değerlendirin.
> * SSIS projelerini/paketlerini Azure'a geçirin.

## <a name="prerequisites"></a>Önkoşullar
Bu adımları tamamlamak için ihtiyacınız vardır:

- SSMS 17.2 veya sonraki bir sürümü.
- Hedef veritabanı sunucunuzun SSISDB'yi barındırmak için bir örneği.
 
  Zaten yoksa:
    - Azure SQL veritabanı için SQL Server (yalnızca mantıksal sunucu) giderek Azure portalını kullanarak Azure SQL veritabanı sunucusu (olmadan, bir veritabanı) oluşturma [form](https://ms.portal.azure.com/#create/Microsoft.SQLServer).
    - Azure SQL veritabanı yönetilen örneği için ayrıntılı makaleyi izleyin [bir Azure SQL veritabanı yönetilen örneği oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).

- SSIS, Azure Data Factory (' % s'hedef Azure SQL veritabanı sunucusu örneği tarafından barındırılan SSISDB ile Azure-SSIS Integration Runtime (IR) içeren ADF) sağlanmalıdır (makalesinde açıklandığı şekilde [Azure-SSIS tümleştirme sağlayın Azure Data factory'de çalışma zamanı](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)) veya Azure SQL veritabanı yönetilen örneği (makalesinde açıklandığı şekilde [Azure-SSIS tümleştirme çalışma zamanının Azure veri fabrikasında](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)). 

## <a name="assess-source-ssis-projectspackages"></a>Kaynak SSIS projelerini/paketlerini değerlendirmek
Veritabanı geçiş Yardımcısı (DMA) ya da Azure veritabanı geçiş hizmeti (DMS) değerlendirmesi SSISDB kaynağının henüz tümleşikleştirilmemiştir karşın, SSIS projelerini/paketlerini değerlendirilen/barındırılan SSISDB hedefe imzalanmasını olarak doğrulanması Azure SQL veritabanı sunucusu veya Azure SQL veritabanı yönetilen örneği.

## <a name="migrate-ssis-projectspackages"></a>SSIS projelerini/paketlerini geçirme
SSIS projelerini/paketlerini Azure SQL veritabanı sunucusu veya Azure SQL veritabanı yönetilen örneğine geçirmek için aşağıdaki adımları gerçekleştirin.

1.  SSMS'yi açın ve ardından **seçenekleri** görüntülenecek **sunucuya Bağlan** iletişim kutusu.

2.  Üzerinde **oturum açma** sekmesinde, Azure SQL veritabanı sunucusu veya Azure SQL veritabanı yönetilen ' % s'hedef SSISDB barındırma örneği bağlanmak gereken bilgileri belirtin.

    ![SSIS oturum açma sekmesi](media/how-to-migrate-ssis-packages/dms-ssis-login-tab.png)
 
3.  Üzerinde **bağlantı özellikleri** sekmesinde **veritabanına bağlan** metin kutusunu seçin veya girin **SSISDB**ve ardından **Connect**.

    ![SSIS bağlantı özellikleri sekmesi](media/how-to-migrate-ssis-packages/dms-ssis-conncetion-properties-tab.png)

4.  SSMS nesne Gezgini'nde **tümleştirme hizmetleri katalogları** düğümünü genişletin **SSISDB**, hiçbir var olan bir klasör varsa, sonra sağ tıklayın ve **SSISDB** ve Yeni bir klasör oluşturun.

5.  Altında **SSISDB**, herhangi bir klasörü genişletin, sağ **projeleri**ve ardından **projesini Dağıt**.

    ![Genişletilmiş SSIS SSISDB düğümü](media/how-to-migrate-ssis-packages/dms-ssis-ssisdb-node-expanded.png)

6.  Tümleştirme hizmetleri Dağıtım Sihirbazı'nda üzerinde **giriş** sayfasında bilgileri gözden geçirin ve ardından **sonraki**.

    ![Dağıtım Sihirbazı giriş sayfası](media/how-to-migrate-ssis-packages/dms-deployment-wizard-introduction-page.png)

7.  Üzerinde **Kaynağı Seç** sayfasında, dağıtmak istediğiniz var olan SSIS projenin belirtin.

    SSMS Ayrıca kaynak SSISDB barındıran SQL Server'a bağlıysa, seçin **Integration Services Kataloğu**ve ardından kataloğunuzda projenizi doğrudan dağıtmak için sunucu adını ve proje yolunu girin.

    Alternatif olarak, seçin **proje dağıtım dosyası**ve ardından projenizi dağıtmak için mevcut bir proje dağıtım dosyasını (.ispac) yolunu belirtin.

    ![Dağıtım Sihirbazı Kaynağı Seç sayfası](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-source-page.png)
 
8.  **İleri**’yi seçin.
9.  Üzerinde **hedef Seç** sayfasında, projeniz için bir hedef belirtin.

       a. Sunucu adı metin kutusunda tam Azure SQL veritabanı sunucu adı girin. (< sunucu_adı >. database.windows.net) veya Azure SQL veritabanı yönetilen örneği adı (< server_name.dns_prefix >. database.windows.net).
 
       b. Kimlik doğrulama bilgileri sağlayın ve ardından **Connect**.
    
       ![Dağıtım Sihirbazı Hedef Seç sayfası](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-destination-page.png)

    c. Hedef klasör SSISDB belirtmek için göz atmayı seçin ve ardından İleri'yi seçin.

    > [!NOTE]
    > **Sonraki** düğmesi yalnızca seçtikten sonra etkin **Connect**. 

10. Üzerinde **doğrulama** sayfasında, tüm hataları/uyarıları görüntüleyin ve ardından gerekirse paketlerinizi uygun şekilde değiştirin.

    ![Dağıtımı Doğrulama Sihirbazı sayfası](media/how-to-migrate-ssis-packages/dms-deployment-wizard-validate-page.png)

11. **İleri**’yi seçin.

12. Üzerinde **gözden** sayfasında, dağıtım ayarlarınızı gözden geçirin.

    > [!NOTE]
    > Ayarlarınızı önceki seçerek ya da adım bağlantılardan herhangi birine sol bölmede seçerek değiştirebilirsiniz.

13. Seçin **Dağıt** dağıtım işlemini başlatmak için.

14. Dağıtım işlemi tamamlandıktan sonra başarı veya başarısızlık her dağıtım eyleminin görüntüler sonuçları sayfasında görüntüleyebilirsiniz.
    a. Herhangi bir işlem de başarısız **sonucu** sütunundaki **başarısız** hata açıklaması görüntülenecek.
    b. İsteğe bağlı olarak **Raporu Kaydet** sonuçlarını bir XML dosyasına kaydetmek için.

15. Seçin **Kapat** tümleştirme hizmetleri Dağıtım Sihirbazı'ndan çıkmak için.

Dağıtım projenizin hatasız başarılı olursa, Azure-SSIS IR'yi üzerinde çalıştırmak için içerdiği tüm paketler seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft Geçiş Kılavuzu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).