---
title: Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum | Microsoft Docs
description: Bu makalede Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum arabirimi kullanma
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: douglasl
ms.openlocfilehash: ff47060ddfee458279c9fed0fd3fcafcf35229d2
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="custom-setup-for-the-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum

Yapılandırma veya (örneğin, ek Windows hizmetlerini başlatmak için) ortamında işletim varsayılan alter Azure SSIS tümleştirmesi çalışma zamanı için özel kurulum arabirimi sağlar veya ek bileşenleri yüklemek (örneğin, derlemeleri, sürücüleri veya Uzantıları), Azure SSIS IR, her bir düğümde Genel olarak, sağlama veya Azure SSIS IR yeniden yapılandırılması sırasında kendi kurulum adımlarını eklemek için bir arabirim sağlar

Bir komut dosyası ve ilişkili dosyalarını hazırlama ve bunları Azure depolama hesabınızdaki blob kapsayıcısı içinde karşıya özel ayarlarınızı yapılandırın. Sağlamak ya da Azure SSIS IR yeniden yapılandırmak için kapsayıcı bir paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) sağlayın Her düğüm, Azure SSIS IR sonra kapsayıcıdan komut dosyası ve ilişkili dosyalarını indirir ve özel kurulumunuzu yükseltilmiş ayrıcalıklarla çalıştırır. Özel Kurulum tamamlandığında, her düğüm yürütme ve diğer günlükler standart çıktısı, kapsayıcıya yükler.

Boş veya lisanssız bileşenleri hem ücretli veya lisanslı bileşenlerini yükleyebilirsiniz. Bir ISV iseniz bkz [geliştirmeyi ücretli veya Azure SSIS IR bileşenleri lisanslı](how-to-develop-azure-ssis-ir-licensed-components.md).


## <a name="current-limitations"></a>Geçerli sınırlamalar

-   Kullanmak istiyorsanız, `gacutil.exe` derlemeleri Genel Derleme Önbelleği'ne (GAC) yüklemek için özel kurulumunun bir parçası olarak sağlamak veya genel Önizleme kapsayıcısında sağlanan kopyayı kullanmak gerekir.

-   Bir sanal ağa, Azure SSIS IR özel kurulum ile katılmak gerekiyorsa, yalnızca Azure Resource Manager Vnet'i desteklenir. Klasik VNet desteklenmiyor.

-   Yönetim paylaşımı üzerinde Azure SSIS IR şu anda desteklenmiyor

-   Özel kurma, bir sürücü için bir dosya paylaşımına eşlemek istiyorsanız `net use` komutu şu anda desteklenmiyor. Sonuç olarak, benzer bir komutu kullanılamaz. `net use d: \\fileshareserver\sharename`. Bunun yerine, kullanın `cmdkey` - Örneğin, komut `cmdkey /add:fileshareserver /user:yyy /pass:zzz` - erişimi `\\fileshareserver\folder` doğrudan paketlerde.

## <a name="prerequisites"></a>Önkoşullar

Azure SSIS IR özelleştirmek için aşağıdakiler gerekir:

-   [Azure aboneliği](https://azure.microsoft.com/)

-   [Bir Azure SQL veritabanı veya örnek yönetilen sunucu](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Azure SSIS IR sağlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Bir Azure Storage hesabı](https://azure.microsoft.com/services/storage/). Özel Kurulum için karşıya yükleme ve bir blob kapsayıcısında özel kurulum komut dosyası ve ilişkili dosyalarını depolar. Özel Kurulum işlemi, yürütme günlüklerini blob kapsayıcıya de yükler.

## <a name="instructions"></a>Yönergeler

2.  İndirme ve yükleme [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) (sürüm 5.4 veya üstü).

3.  Özel Kurulum komut dosyası ve ilişkili dosyalarını (örneğin, .bat, .cmd, .exe, .dll, .msi veya .ps1 dosyaları) hazırlayın.

    1.  Adlı bir komut dosyası olmalıdır `main.cmd`, özel kurulumunuzu giriş noktasını olduğu.

    2.  Diğer araçları tarafından oluşturulan ek günlükleri istiyorsanız (örneğin, `msiexec.exe`), kapsayıcıya yüklenebilmesi için önceden tanımlanmış ortam değişkeni belirtin `CUSTOM_SETUP_SCRIPT_LOG_DIR` komut dosyalarınızı günlük klasöründe olarak (örneğin, `msiexec /i xxx.msi /quiet
        /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

4.  İndirme, yükleme ve başlatma [Azure Storage Gezgini](http://storageexplorer.com/).

    1.  Altında **(yerel ve iliştirildiği)**, sağ select **depolama hesapları** seçip **Azure depolama Bağlan**.

       ![Azure depolamaya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

    2.  Seçin **bir depolama hesabı adı ve anahtar kullanmak** seçip **sonraki**.

       ![Depolama hesabı adını ve anahtarını kullanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

    3.  Anahtar, seçin ve Azure depolama hesabı adı girin **sonraki**ve ardından **Bağlan**.

       ![Depolama hesap adı ve anahtar belirtin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

    4.  Bağlı Azure Storage hesabınız altında sağ tıklayın **Blob kapsayıcıları**seçin **Blob kapsayıcısı oluşturmak**ve yeni kapsayıcı adı.

       ![Blob kapsayıcısı oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

    5.  Yeni kapsayıcıyı seçin ve özel kurulum komut dosyası ve ilişkili dosyalarını yükleyin. Karşıya yüklediğiniz emin olun `main.cmd` en üst düzeyinde kapsayıcının herhangi bir klasör içinde değil. 

       ![Blob kapsayıcısına dosyaları karşıya yükleme](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

    6.  Kapsayıcıya sağ tıklayın ve seçin **paylaşılan erişim imzası Al**.

       ![İçin kapsayıcı paylaşılan erişim imzası Al](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

    7.  Okuma ve yeterince uzun süre sonu zamanı ile kapsayıcı SAS URI'sini Oluştur + yazma + listesinde izinleri. Karşıdan yükle ve Azure SSIS IR herhangi bir düğümün yeniden her özel kurulum komut dosyası ve ilişkili dosyalarını çalıştırmak için SAS URI'sini gerekir. Kurulum yürütme günlüklerini karşıya yükleme izni yazmanız gerekir.

       ![Kapsayıcı paylaşılan erişim imzası oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

    8.  Kopyalayın ve, kapsayıcı SAS URI'sini kaydedin.

       ![Kopyalayın ve paylaşılan erişim imzası kaydedin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

    9.  Sağlamak ya da Azure SSIS IR başlamadan önce Azure SSIS IR PowerShell ile yeniden Çalıştır `Set-AzureRmDataFactoryV2IntegrationRuntime` için yeni değer olarak, kapsayıcı SAS URI'sini cmdlet'iyle `SetupScriptContainerSasUri` parametresi. Örneğin:

       ```powershell
       Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName `
                                                  -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

       Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                    -Name $MyAzureSsisIrName `
                                                    -ResourceGroupName $MyResourceGroupName
       ```

    10.  Özel Kurulum tamamlandıktan ve Azure SSIS IR başlatır ve standart çıktısı bulabilirsiniz sonra `main.cmd` ve diğer yürütme günlüklerini `main.cmd.log` depolama kapsayıcısının klasör.

2.  Diğer özel kurulum örnekleri görmek için Azure Storage Gezgini ile genel Önizleme kapsayıcısına bağlayın.

    a.  Altında **(yerel ve iliştirildiği)**, sağ **depolama hesapları**seçin **Azure depolama Bağlan**seçin **bir bağlantı dizesi veya bir paylaşılan erişim kullanın İmza URI**ve ardından **sonraki**.

       ![Azure storage paylaşılan erişim imzası ile bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

    b.  Seçin **SAS URI'sini kullanmak** ve genel Önizleme kapsayıcısı aşağıdaki SAS URI'sini girin. Seçin **sonraki**, ardından **Bağlan**.

       `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

       ![Paylaşılan erişim imzası için kapsayıcı sağlar.](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

    c. Bağlı genel Önizleme kapsayıcıyı seçin ve çift `CustomSetupScript` klasör. Bu klasörde şu öğeler şunlardır:

       1. A `Sample` Azure SSIS IR her bir düğümde temel görev yüklemek için özel kurulum içeren klasör Görev birkaç saniye için uyku ancak hiçbir şey yapmaz. Klasörü de içeren bir `gacutil` içeren klasör `gacutil.exe`.

       2. A `UserScenarios` gerçek kullanıcı senaryoları için birkaç özel ayarlar içeren klasör.

    ![Genel Önizleme kapsayıcısının içeriği](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

    d. Çift `UserScenarios` klasör. Bu klasörde şu öğeler şunlardır:

       1. A `.NET FRAMEWORK 3.5` Azure SSIS IR, her bir düğümde özel bileşenler için gerekebilecek .NET Framework'ün önceki bir sürümünü yüklemek için özel kurulum içeren klasör

       2. A `BCP` SQL Server komut satırı yardımcı programları yüklemek için özel kurulum içeren klasör (`MsSqlCmdLnUtils.msi`), toplu kopyalama programı da dahil olmak üzere (`bcp`), Azure SSIS IR, her bir düğümde

       3. Bir `EXCEL` açık kaynak derlemelerini yüklemek için özel kurulum içeren klasör (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, ve `ExcelDataReader.dll`), Azure SSIS IR, her bir düğümde

       4. Bir `MSDTC` ağ ve güvenlik yapılandırmaları her düğüme Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) örneği için Azure SSIS IR değiştirmek için özel kurulum içeren klasör

       5. Bir `ORACLE ENTERPRISE` özel kurulum komut dosyasını içeren klasöre (`main.cmd`) ve sessiz yükleme yapılandırma dosyası (`client.rsp`), Azure SSIS IR Enterprise Edition her bir düğümde Oracle OCI sürücüyü yüklemek için. Bu kurulum, Oracle Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. İlk olarak, en son Oracle istemcisi - Örneğin, karşıdan `winx64_12102_client.zip` - aşağı [Oracle](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) ve ile birlikte karşıya `main.cmd` ve `client.rsp` , kapsayıcı içine. Oracle için bağlanmak için TNS kullanıyorsanız, aynı zamanda karşıdan yüklemeniz `tnsnames.ora`, düzenlemek ve Kurulum sırasında Oracle yükleme klasörüne kopyalanabilmesi için kapsayıcıya yükleyin.

       6. Bir `ORACLE STANDARD` özel kurulum komut dosyasını içeren klasöre (`main.cmd`), Azure SSIS IR her bir düğümde Oracle ODP.NET sürücüyü yüklemek için Bu kurulum, ADO.NET Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. İlk olarak, en son Oracle ODP.NET sürücüsü - Örneğin, karşıdan `ODP.NET_Managed_ODAC122cR1.zip` - aşağı [Oracle](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)ve ile birlikte karşıya `main.cmd` , kapsayıcı içine.

       7. Bir `SAP BW` özel kurulum komut dosyasını içeren klasöre (`main.cmd`) SAP .NET bağlayıcı derlemesini yüklemek için (`librfc32.dll`), Azure SSIS IR Enterprise Edition'ın her bir düğümde. Bu kurulum, SAP BW Bağlantı Yöneticisi, kaynak ve hedef kullanmanızı sağlar. İlk olarak, 64 bit veya 32-bit sürümünü karşıya `librfc32.dll` ile birlikte, kapsayıcı SAP yükleme klasöründen `main.cmd`. Komut dosyası sonra SAP derlemeye kopyalar `%windir%\SysWow64` veya `%windir%\System32` Kurulum sırasında klasör.

       8. A `STORAGE` Azure SSIS IR her bir düğümde Azure PowerShell'i yüklemek için özel kurulum içeren klasör Bu kurulum dağıtmanıza olanak tanır ve çalışma SSIS paketleri çalıştıran [Azure Storage hesabınızı yönetmek için PowerShell komut dosyalarını](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Kopya `main.cmd`, bir örnek `AzurePowerShell.msi` (veya en son sürümü yükleyin), ve `storage.ps1` , kapsayıcısı. PowerShell.dtsx paketlerinizin şablon olarak kullanın. Paket şablonu birleştirir bir [Azure Blob indirme görev](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), hangi yüklemeleri `storage.ps1` değiştirilebilir bir PowerShell Betiği olarak ve bir [yürütme işlemi görev](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) , komut dosyası her düğüm üzerinde yürütülür.

       9. A `TERADATA` özel kurulum komut dosyasını içeren klasöre (`main.cmd)`, ilişkili dosya (`install.cmd`) ve yükleyici paketleri (`.msi`). Bu dosyalar Azure SSIS IR Enterprise Edition her bir düğümde Teradata bağlayıcılar, birleştirilmiş TPT API ve ODBC sürücüsü yükleyin. Bu kurulum Teradata Bağlantı Yöneticisi, kaynak ve hedef kullanmanıza olanak sağlar. İlk olarak, Teradata Araçlar ve yardımcı programları (TTU) 15.x zip dosyasını indirin (örneğin, `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) gelen [Teradata](http://partnerintelligence.teradata.com)ve ardından yukarıdaki ile birlikte karşıya `.cmd` ve `.msi` , kapsayıcı dosyalarıyla.

    ![Kullanıcı senaryoları klasöründeki klasörleri](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

    e. Bu özel kurulum örnekleri denemeye kopyalayın ve seçilen klasör içeriği, kapsayıcıya yapıştırın. Sağlamak ya da, Azure SSIS IR PowerShell ile yeniden Çalıştır `Set-AzureRmDataFactoryV2IntegrationRuntime` için yeni değer olarak, kapsayıcı SAS URI'sini cmdlet'iyle `SetupScriptContainerSasUri` parametresi.

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure SSIS Integration zamanının Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Geliştirmeyi ücretli veya Azure SSIS tümleştirmesi çalışma zamanı için özel bileşenler lisanslı](how-to-develop-azure-ssis-ir-licensed-components.md)