---
title: Azure-SSIS tümleştirme çalışma zamanı Kurulum özelleştirme | Microsoft Docs
description: Bu makalede, Azure-SSIS tümleştirme çalışma zamanı için özel kurulum arabirimi ayarlarını değiştirmek veya ek bileşenleri yüklemek için kullanmayı açıklar
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 1/25/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: cfa9d6a1a287281bec91facf04c73506db81f84a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64711561"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı Kurulum özelleştirme

Azure-SSIS tümleştirme çalışma zamanı için özel kurulum arabirimi sağlama veya yeniden yapılandırılması, Azure-SSIS IR'yi sırasında kendi kurulum adımlarını eklemek için bir arabirim sağlar. Özel Kurulum (örneğin, derlemeleri, sürücüleri veya Uzantılar) ek bileşenleri yüklemek ya da yapılandırma veya ortama (örneğin, ek Windows hizmetlerini başlatın veya dosya paylaşımları için erişim kimlik bilgilerini kalıcı hale getirmek için) işletim varsayılan alter olanak tanır. Azure-SSIS IR, her bir düğümde

Bir komut dosyası ve ilişkili dosyaları hazırlama ve bir Azure depolama hesabınızdaki blob kapsayıcısına karşıya yükleyerek özel kurulumunuzu yapılandırın. Sağlama veya, Azure-SSIS IR'yi yeniden kapsayıcınız için bir paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) sağlayın Azure-SSIS IR her düğüm, kapsayıcıdan komut dosyası ve ilişkili dosyalarını indirir ve yükseltilmiş ayrıcalıklara sahip özel kurulumunuzu çalıştırır. Özel Kurulum tamamlandığında, her düğüm, yürütme ve diğer günlükler standart çıktısı, kapsayıcıya yükler.

Ücretsiz veya lisanssız bileşenleri hem Ücretli ya da lisanslı bileşenleri yükleyebilirsiniz. Bir ISV iseniz bkz [nasıl geliştirileceğini ücretli veya lisanslı bileşenler için Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> Dv2 serisi düğümler Azure-SSIS IR, özel kurulum için uygun değildir. Bu nedenle Lütfen v3 serisi düğümler yerine kullanın.  Dv2 serisi düğümler zaten kullanıyorsanız, v3 serisi düğümler olabildiğince çabuk kullanmak için lütfen geçiş yapın.

## <a name="current-limitations"></a>Geçerli sınırlamalar

-   Kullanmak istiyorsanız `gacutil.exe` derlemeleri Genel Derleme Önbelleği'ne (GAC) yüklemek için sağlamanız gerekir `gacutil.exe` parçası olarak özel kurulum veya genel Önizleme kapsayıcıda sağlanan kopyayı kullanın.

-   Bir alt komut dosyanızdaki başvurmak istiyorsanız `msiexec.exe` desteklemediği `.\` kök klasörü başvurmak için gösterimi. Gibi bir komut kullanın `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` yerine `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

-   Azure-SSIS IR özel kurulum ile bir sanal ağa eklemeniz gerekiyorsa, yalnızca Azure Resource Manager sanal ağı desteklenir. Klasik sanal ağ desteklenmiyor.

-   Yönetim paylaşımı üzerinde Azure-SSIS IR'yi şu anda desteklenmiyor

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS IR özelleştirmek için aşağıdakiler gerekir:

-   [Azure aboneliği](https://azure.microsoft.com/)

-   [Bir Azure SQL veritabanı veya yönetilen örnek sunucusu](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Azure-SSIS IR sağlamak](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Bir Azure depolama hesabı](https://azure.microsoft.com/services/storage/). Özel Kurulum için karşıya yükleme ve özel kurulum komut dosyanızı ve ilişkili dosyalarını bir blob kapsayıcısı içinde saklamak gerekir. Özel Kurulum işlemi, ayrıca, yürütme günlüklerini aynı blob kapsayıcısına yükler.

## <a name="instructions"></a>Yönergeler

1. İndirme ve yükleme [Azure PowerShell](/powershell/azure/install-az-ps).

1. Özel Kurulum komut dosyanızı ve ilişkili dosyaları (örneğin, .bat, .cmd, .exe, .dll, .msi veya .ps1 dosyaları) hazırlayın.

   1.  Adlı bir komut dosyası olmalıdır `main.cmd`, özel kurulumunuzu giriş noktası olduğu.

   1.  Diğer araçları tarafından oluşturulan ek günlükler istiyorsanız (örneğin, `msiexec.exe`), kapsayıcıya yüklenmek üzere önceden tanımlanmış ortam değişkenini belirtin `CUSTOM_SETUP_SCRIPT_LOG_DIR` betiklerinizi günlük klasöründe olarak (örneğin, `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1. İndirme, yükleme ve başlatma [Azure Depolama Gezgini](https://storageexplorer.com/).

   1. Altında **(yerel ve eklenmiş)** , sağ tıklayarak seçin **depolama hesapları** seçip **Azure Storage'a Bağlan**.

      ![Azure depolamaya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. Seçin **bir depolama hesabı adı ve anahtarı kullan** seçip **sonraki**.

      ![Depolama hesabı adını ve anahtarını kullanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Azure depolama hesabı adı ve anahtarı, select girin **sonraki**ve ardından **Connect**.

      ![Depolama hesabı adı ve anahtarı sağlayın](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. Bağlı Azure depolama hesabınızın altında sağ **Blob kapsayıcıları**seçin **Blob kapsayıcısı Oluştur**ve yeni kapsayıcı adı.

      ![Blob kapsayıcısı oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Yeni kapsayıcıyı seçin ve özel kurulum komut dosyanızı ve ilişkili dosyalarını karşıya yükleyin. Karşıya yüklediğiniz emin `main.cmd` en üst düzeyde kapsayıcınızın herhangi bir klasörü içinde değil. Ayrıca olun kapsayıcınızı yalnızca gerekli özel kurulum dosyaları içerir, böylece bunları Azure-SSIS IR daha sonra indirme uzun olmaz. Özel Kurulum için en uzun süresi şu anda bu, kapsayıcıyı tüm dosyaları indirmek ve bunları Azure-SSIS IR'yi üzerinde yüklemek için zaman içerir ve zaman aşımına uğramadan önce 45 dakika ayarlandıysa Daha uzun bir süre gerekiyorsa, Lütfen bir destek bileti yükseltin.

      ![Dosyaları blob kapsayıcısına yükleyin.](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Kapsayıcıya sağ tıklayıp **paylaşılan erişim imzası Al**.

      ![İçin kapsayıcı paylaşılan erişim imzası Al](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. Okuma ve yeterince uzun süre sonu zamanı ile kapsayıcınız için SAS URI'si oluşturma + yazma + listesinde izinleri. SAS URI'sini indirip herhangi bir Azure-SSIS IR düğüm başlatıldığında/yeniden olduğunda özel kurulum komut dosyanızı ve ilişkili dosyalarını çalıştırmak için ihtiyacınız vardır. Kurulum yürütme günlüklerini karşıya yükleme izni yazmanız.

      > [!IMPORTANT]
      > SAS URI'sini dolmaz ve özel kurulum kaynakları her zaman özellikle düzenli olarak durdurup Azure-SSIS IR bu süre boyunca, Azure-SSIS IR, oluşturma, silme için tüm yaşam döngüsü boyunca kullanılabilir. Lütfen emin olun.

      ![Kapsayıcı paylaşılan erişim imzası oluşturma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Kopyalayın ve kaydedin, kapsayıcı SAS URI'si.

      ![Kopyalayın ve paylaşılan erişim imzası kaydedin](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

   1. Sağlayın ya da Azure-SSIS IR başlamadan önce Azure-SSIS IR ile Data Factory kullanıcı Arabirimi, yeniden uygun alanına, kapsayıcı SAS URI'sini girin **Gelişmiş ayarlar** paneli:

      ![Paylaşılan erişim imzası girin](media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

      Sağlama veya PowerShell ile Azure-SSIS IR, Azure-SSIS IR başlamadan önce yeniden çalıştırma `Set-AzDataFactoryV2IntegrationRuntime` değeri olarak yeni kapsayıcı SAS URI'sini cmdlet'iyle `SetupScriptContainerSasUri` parametresi. Örneğin:

      ```powershell
      Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                -Name $MyAzureSsisIrName `
                                                -ResourceGroupName $MyResourceGroupName `
                                                -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

      Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName
      ```

   1. Özel kurulumunuz tamamlandıktan ve Azure-SSIS IR başlatır, standart çıktısı bulabilirsiniz sonra `main.cmd` ve diğer yürütme'de oturum açması `main.cmd.log` depolama kapsayıcınızda klasörü.

1. Diğer özel kurulum örneklerini görmek için Azure Depolama Gezgini ile genel Önizleme kapsayıcıya bağlanın.

   a.  Altında **(yerel ve eklenmiş)** , sağ **depolama hesapları**seçin **Azure Storage'a Bağlan**seçin **bir bağlantı dizesi veya paylaşılan erişim imzası URI'si**ve ardından **sonraki**.

      ![Paylaşılan erişim imzası ile Azure Depolama'ya bağlanma](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b.  Seçin **SAS URI'si kullanma** ve genel Önizleme kapsayıcısı aşağıdaki SAS URI'sini girin. Seçin **sonraki**, ardından **Connect**.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![Paylaşılan erişim imzası için kapsayıcı sağlar.](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. Bağlı genel Önizleme kapsayıcıyı seçin ve çift `CustomSetupScript` klasör. Bu klasörde aşağıdaki öğeler şunlardır:

      1. A `Sample` içeren temel bir görev, Azure-SSIS IR'yi her düğümde yüklemek için özel bir kurulum klasörü Görev birkaç saniye için uyku ancak hiçbir şey yapmaz. Klasörde ayrıca bir `gacutil` klasörü, tüm içeriğini (`gacutil.exe`, `gacutil.exe.config`, ve `1033\gacutlrc.dll`), kapsayıcıya olarak kopyalanabilir. Ayrıca, `main.cmd` dosya paylaşımları için erişim kimlik bilgilerini kalıcı hale getirmek için açıklamalarını içerir.

      1. A `UserScenarios` gerçek kullanıcı senaryoları için çeşitli özel ayarlar içeren klasör.

   ![Genel Önizleme kapsayıcı içeriğini](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   d. Çift `UserScenarios` klasör. Bu klasörde aşağıdaki öğeler şunlardır:

      1. A `.NET FRAMEWORK 3.5` her düğümde, Azure-SSIS IR, özel bileşenler için gerekli olabilecek .NET Framework'ün önceki bir sürümünü yüklemek için özel bir kurulum içeren klasör

      1. A `BCP` SQL Server komut satırı yardımcı programlarını yüklemek için özel bir kurulum içeren klasörü (`MsSqlCmdLnUtils.msi`), toplu kopyalama programı da dahil olmak üzere (`bcp`), her düğüme, Azure-SSIS IR

      1. Bir `EXCEL` açık kaynaklı derlemeleri yüklemek için özel bir kurulum içeren klasörü (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, ve `ExcelDataReader.dll`), Azure-SSIS IR, her bir düğümde

      1. Bir `ORACLE ENTERPRISE` özel kurulum betiği içeren klasörü (`main.cmd`) ve sessiz yükleme yapılandırma dosyası (`client.rsp`) Oracle bağlayıcılar ve OCI sürücü, Azure-SSIS IR Enterprise Edition'ın her düğüme yüklenecek. Bu kurulum Oracle Bağlantı Yöneticisi, kaynak ve hedef kullanmanıza olanak sağlar. İlk olarak, Oracle için Microsoft Connectors v5.0 indirin (`AttunitySSISOraAdaptersSetup.msi` ve `AttunitySSISOraAdaptersSetup64.msi`) öğesinden [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55179) ve en son Oracle istemcisini - Örneğin, `winx64_12102_client.zip` - [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html), bunların tümünü birlikte karşıya `main.cmd` ve `client.rsp` kapsayıcınıza. Oracle için bağlanmak için kullandığınız TNS de indirmek gerekirse `tnsnames.ora`, düzenlemek ve böylece Kurulum sırasında Oracle yükleme klasörüne kopyalanabilir, kapsayıcıya yükleyin.

      1. Bir `ORACLE STANDARD ADO.NET` özel kurulum betiği içeren klasörü (`main.cmd`), Azure-SSIS IR'yi her düğümde Oracle ODP.NET sürücüsünü yüklenecek Bu kurulum ADO.NET Bağlantı Yöneticisi, kaynak ve hedef kullanmanıza olanak sağlar. İlk olarak, en son Oracle ODP.NET sürücüsünü - Örneğin, indirme `ODP.NET_Managed_ODAC122cR1.zip` - [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)ve ardından ile birlikte karşıya `main.cmd` kapsayıcınıza.
       
      1. Bir `ORACLE STANDARD ODBC` özel kurulum betiği içeren klasörü (`main.cmd`) Oracle ODBC sürücüsünü yüklemek ve Azure-SSIS IR, her bir düğümde DSN yapılandırmak için Bu kurulum ODBC Bağlantı Yöneticisi/kaynak/hedef veya Power Query Bağlantı Yöneticisi/kaynak Oracle sunucusuna bağlanmak için ODBC veri kaynağı türü ile kullanmanıza olanak sağlar. İlk olarak, en son Oracle anlık istemci (temel paket veya temel Lite paket) ve ODBC Package - Örneğin, 64-bit paketleri indirme [burada](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (temel paket: `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`, temel Lite paket: `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`, ODBC paketini : `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`) veya 32-bit paketleri [burada](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (temel paket: `instantclient-basic-nt-18.3.0.0.0dbru.zip`, temel Lite paket: `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`, ODBC paket: `instantclient-odbc-nt-18.3.0.0.0dbru.zip`) ve ardından bunları ile birlikte yüklemek `main.cmd` içine kapsayıcı.

      1. Bir `SAP BW` özel kurulum betiği içeren klasörü (`main.cmd`) SAP .NET bağlayıcı derlemesini yüklemek için (`librfc32.dll`), Azure-SSIS IR Enterprise Edition'ın her bir düğümde. Bu kurulum SAP BW Bağlantı Yöneticisi, kaynak ve hedef kullanmanıza olanak sağlar. İlk olarak, 64-bit veya 32-bit sürümünü karşıya `librfc32.dll` kapsayıcınızı, birlikte SAP yükleme klasöründen `main.cmd`. Betik daha sonra SAP bütünleştirilmiş kod içine kopyalar `%windir%\SysWow64` veya `%windir%\System32` Kurulum sırasında klasör.

      1. A `STORAGE` , Azure-SSIS IR'yi her düğümde Azure PowerShell'i yüklemek için özel bir kurulum içeren klasör Bu kurulum dağıtmanıza olanak tanır ve çalışma SSIS paketleri çalıştıran [Azure depolama hesabınızı yönetmek için PowerShell betikleri](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Kopyalama `main.cmd`, bir örnek `AzurePowerShell.msi` (veya en son sürümü yükleyin) ve `storage.ps1` kapsayıcınız için. PowerShell.dtsx paketleriniz için şablon olarak kullanın. Paket şablonu birleştirir bir [Azure Blob indirme görev](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), hangi yüklemeleri `storage.ps1` değiştirilebilir bir PowerShell Betiği olarak ve bir [yürütme işlemi görevi](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) , betiği her düğüm üzerinde yürütülür.

      1. A `TERADATA` özel kurulum betiği içeren klasörü (`main.cmd`), kendi ilişkili dosya (`install.cmd`) ve yükleyici paketleri (`.msi`). Bu dosyalar, Teradata bağlayıcılar, TPT API ve ODBC sürücüsü, Azure-SSIS IR Enterprise Edition'ın her düğüme yükleyin. Bu kurulum Teradata Bağlantı Yöneticisi, kaynak ve hedef kullanmanıza olanak sağlar. İlk olarak, Teradata Araçlar ve yardımcı programlar (TTU) 15.x zip dosyasını indirin (örneğin, `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) öğesinden [Teradata](http://partnerintelligence.teradata.com)ve ardından yukarıdaki birlikte karşıya `.cmd` ve `.msi` dosyaların kapsayıcınıza.

   ![Kullanıcı senaryoları klasöründeki klasörleri](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   e. Bu özel kurulum örnekleri denemeye kopyalayın ve seçilen klasördeki içeriği, kapsayıcıya yapıştırın. Sağlama veya PowerShell ile Azure-SSIS IR yeniden çalıştırma `Set-AzDataFactoryV2IntegrationRuntime` değeri olarak yeni kapsayıcı SAS URI'sini cmdlet'iyle `SetupScriptContainerSasUri` parametresi.

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure-SSIS Integration Runtime'nın Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Geliştirme konusunda ücretli veya özel bileşenler Azure-SSIS tümleştirme çalışma zamanı için lisanslı](how-to-develop-azure-ssis-ir-licensed-components.md)
