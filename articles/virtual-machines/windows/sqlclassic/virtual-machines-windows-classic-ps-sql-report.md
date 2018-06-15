---
title: İle yerel moddaki bir rapor sunucusunda bir VM oluşturmak için PowerShell kullanın | Microsoft Docs
description: 'Bu konuda açıklar ve SQL Server Reporting Services yerel mod rapor sunucusunun bir Azure sanal makine yapılandırma ve dağıtım size yol göstermektedir. '
services: virtual-machines-windows
documentationcenter: na
author: markingmyname
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: maghan
ms.openlocfilehash: edfae3a56bc13e4c41a1676bfc0f4e8cf4cd9d30
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31425087"
---
# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Yerel Mod Rapor Sunucusu ile Azure VM Oluşturmak için PowerShell Kullanma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu konuda açıklar ve SQL Server Reporting Services yerel mod rapor sunucusunun bir Azure sanal makine yapılandırma ve dağıtım size yol göstermektedir. Bu belgede yer alan adımlar, sanal makine ve VM Raporlama Hizmetleri'ni yapılandırmak için Windows PowerShell betiği oluşturmak için adımları el ile birlikte kullanın. HTTP veya HTTPs için bir güvenlik duvarı bağlantı noktası açma yapılandırma komut dosyası içerir.

> [!NOTE]
> Gerekli olmadığı, **HTTPS** rapor sunucusunda **2. adıma geçin**.
> 
> 1. adımda VM oluşturduktan sonra HTTP ve rapor sunucusu yapılandırmak için komut dosyası kullan bölümüne gidin. Sonra komut dosyasını çalıştırmak, rapor sunucusu kullanmak üzere hazırdır.

## <a name="prerequisites-and-assumptions"></a>Önkoşullar ve varsayımlar
* **Azure aboneliği**: Azure aboneliğinizde kullanılabilir çekirdek sayısı doğrulayın. Önerilen VM boyutu oluşturursanız **A3**, gereksinim duyduğunuz **4** kullanılabilir çekirdekler. Bir VM boyutu kullanırsanız **A2**, gereksinim duyduğunuz **2** kullanılabilir çekirdekler.
  
  * Azure portalında aboneliğinizin çekirdek sınırına doğrulamak için üst menüde sol bölmesinde sonra tıklatın kullanım ayarları tıklatın.
  * Çekirdek Kotayı artırmak için ilgili kişi [Azure Destek](https://azure.microsoft.com/support/options/). VM boyutu bilgi için bkz: [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **Windows PowerShell komut dosyası**: konu Windows PowerShell temel bilgiye sahip olduğunuzu varsayar. Windows PowerShell'i kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:
  
  * [Windows Server'da Windows PowerShell'i başlatma](https://technet.microsoft.com/library/hh847814.aspx)
  * [Windows PowerShell ile çalışmaya başlama](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>1. adım: bir Azure sanal makine sağlama
1. Azure Portalı'na göz atın.
2. Tıklatın **sanal makineleri** sol bölmede.
   
    ![Microsoft azure sanal makineler](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. **Yeni**’ye tıklayın.
   
    ![Yeni düğmesi](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Tıklatın **galerisinden**.
   
    ![Galeriden yeni vm](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Tıklatın **SQL Server 2014 RTM Standard – Windows Server 2012 R2** ve ardından devam etmek için oka tıklayın.
   
    ![ileri](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Raporlama Hizmetleri veri abonelikleri özelliği tabanlı gerek duyuyorsanız, **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. SQL Server sürümleri ve özellik desteği hakkında daha fazla bilgi için bkz: [SQL Server 2012 sürümleri tarafından desteklenen özellikler](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. Üzerinde **sanal makine yapılandırması** sayfasında, aşağıdaki alanları düzenleyin:
   
   * Varsa birden fazla **sürüm yayın tarihi**, en son sürümünü seçin.
   * **Sanal makine adı**: makine adı, ayrıca sonraki yapılandırma sayfasında varsayılan bulut hizmeti DNS adı olarak kullanılır. DNS adı Azure hizmeti arasında benzersiz olması gerekir. VM VM için kullanıldığını açıklar bir bilgisayar adıyla yapılandırmayı göz önünde bulundurun. Örneğin ssrsnativecloud.
   * **Katman**: standart
   * **Boyutu: A3** SQL Server iş yükleri için önerilen VM boyutu. Bir VM yalnızca bir rapor sunucusu olarak kullanılıyorsa, rapor sunucusu büyük bir iş yükü karşılaştığında sürece, A2 VM boyutunu yeterli olur. VM fiyatlandırma bilgileri için bkz: [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Yeni bir kullanıcı adı**: sağladığınız ad VM üzerinde bir yönetici olarak oluşturulur.
   * **Yeni parola** ve **onaylayın**. Bu parola yeni yönetici hesabı için kullanılır ve güçlü bir parola kullanmanız önerilir.
   * **İleri**’ye tıklayın. ![Sonraki](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. Sonraki sayfada aşağıdaki alanları düzenleyin:
   
   * **Bulut hizmeti**: seçin **yeni bir bulut hizmeti oluşturma**.
   * **Bulut hizmeti DNS adı**: VM ile ilişkili bulut hizmetinin Genel DNS adı budur. Varsayılan adı VM adı için yazdığınız adıdır. Güvenilir bir SSL sertifikası oluşturma konusunda daha sonraki adımlarda ve daha sonra DNS adı değerini kullanılıyorsa "**verilen**" sertifikanın.
   * **Bölge/benzeşim grubu/sanal ağ**: son kullanıcılarınız için en yakın bölgeyi seçin.
   * **Depolama hesabı**: otomatik olarak oluşturulan depolama hesabı kullanın.
   * **Kullanılabilirlik kümesi**: yok.
   * **Uç noktaları** tutmak **Uzak Masaüstü** ve **PowerShell** uç noktaları ve HTTP veya HTTPS uç noktası, ortamınıza bağlı olarak ekleyin.
     
     * **HTTP**: varsayılan genel ve özel bağlantı noktaları **80**. Özel bir bağlantı noktası 80 dışında kullanırsanız değiştirme Not **$HTTPport = 80** http komut.
     * **HTTPS**: varsayılan genel ve özel bağlantı noktaları **443**. En iyi güvenlik uygulaması, güvenlik duvarını ve özel bağlantı noktası kullanmak üzere rapor sunucusuna özel bağlantı noktasını değiştirin ve yapılandırmaktır. Uç noktalar hakkında daha fazla bilgi için bkz: [ayarlamak yukarı iletişimi bir sanal makineyle nasıl](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Bir bağlantı noktası 443 dışındaki kullanıyorsanız, parametre değiştirme Not **$HTTPsport = 443** HTTPS komut.
   * İleri'yi tıklatın. ![ileri](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. Sihirbazın son sayfasında varsayılan bırakın **VM Aracısı yükleme** seçili. Bu konudaki adımları VM Aracısı'nı kullanan değil ancak bu VM tutmak planlıyorsanız, VM aracısı ve uzantılar geliştirmek kendisinin CM olanak sağlar.  VM Aracısı hakkında daha fazla bilgi için bkz: [VM aracısı ve uzantılar – Kısım 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Çalıştıran varsayılan yüklü uzantıları ad birini görüntüleyen VM Masaüstü, iç IP ve boş disk alanı gibi sistem bilgisi "BGINFO" uzantısıdır.
9. Tamamlandı'yi tıklatın. ![tamam](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. **Durum** VM görüntüler olarak **başlangıç (hazırlama)** olarak sonra görüntülenir ve sağlama işlemi sırasında **çalıştıran** VM sağlanan ve hazır olduğunda.

## <a name="step-2-create-a-server-certificate"></a>2. adım: bir sunucu sertifikası oluşturma
> [!NOTE]
> Rapor sunucusunda HTTPS ihtiyacınız yoksa, şunları yapabilirsiniz **2. adıma geçin** ve bölümüne gidin **HTTP ve rapor sunucusu yapılandırmak için komut dosyası kullanma**. Rapor sunucusu hızlı bir şekilde yapılandırmak için HTTP komut dosyasını kullanın ve rapor sunucusu kullanıma hazır olacaktır.

VM HTTPS kullanmak için güvenilir bir SSL sertifikası gerekir. Senaryonuza bağlı olarak, aşağıdaki iki yöntemden birini kullanabilirsiniz:

* Geçerli bir SSL sertifikası bir sertifika yetkilisi (CA) tarafından verilen ve Microsoft tarafından güvenilen. CA kök sertifikaları Microsoft kök sertifikası programı dağıtılacak gereklidir. Bu program hakkında daha fazla bilgi için bkz: [Windows ve Windows Phone 8 SSL kök sertifika programı (üye CA'lar)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) ve [Microsoft kök sertifikası programı giriş](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Kendinden imzalı bir sertifika. Otomatik olarak imzalanan sertifikalar üretim ortamları için önerilmez.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Bir güvenilen sertifika yetkilisi (CA) tarafından oluşturulan bir sertifika kullanmak için
1. **Web sitesi için bir sunucu sertifikası bir sertifika yetkilisinden sertifika istemek**. 
   
    Bir sertifikalandırma yetkilisine göndermek bir sertifika isteği dosyasını (Certreq.txt) oluşturmak veya bir çevrimiçi sertifika yetkilisi için bir istek oluşturun, Web Sunucusu Sertifika Sihirbazı'nı kullanabilirsiniz. Örneğin, Microsoft Sertifika Hizmetleri Windows Server 2012'de. Sunucu sertifikanızı tarafından sunulan kimlik güvence düzeyine bağlı olarak, birkaç isteğinizi onaylamak ve bir sertifika dosyası göndermesi sertifika yetkilisi için birkaç ay gündür. 
   
    Sunucu sertifikaları isteme hakkında daha fazla bilgi için aşağıdakilere bakın: 
   
   * Kullanım [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Windows Server 2012 yönetimi için Güvenlik Araçları'nı tıklatın.
     
     [Windows Server 2012 yönetimi için güvenlik araçları](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > **Verilen** güvenilir SSL sertifikası alanı ile aynı olmalıdır **bulut hizmeti DNS adı** yeni VM için kullanılır.

2. **Web sunucusunda sunucu sertifikasının yüklenmesine**. Web sunucusu bu durumda olduğundan rapor sunucusunu barındıran VM ve Reporting Services yapılandırdığınızda, sonraki adımlarda Web sitesi oluşturulur. Sunucu sertifikasının sertifika MMC ek bileşenini kullanarak Web sunucusu üzerinde yükleme hakkında daha fazla bilgi için bkz: [sunucu sertifikasını yüklemeniz](https://technet.microsoft.com/library/cc740068).
   
    Rapor sunucusu, sertifikaları değerini yapılandırmak için bu konu ile dahil betik kullanmak isteyip istemediğinizi **parmak izi** bir komut parametresi olarak gereklidir. Sertifikanın parmak izini alma konusunda ayrıntılar için sonraki bölüme bakın.
3. Sunucu sertifikası rapor sunucusuna atayın. Rapor sunucusu yapılandırdığınızda atama sonraki bölümde tamamlandı.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Sanal makineler otomatik olarak imzalanan sertifika kullanmak için
VM hazırlandığında kendinden imzalı bir sertifika VM oluşturuldu. Sertifika VM DNS adı ile aynı ada sahiptir. Sertifika hataları önlemek için bu değer sertifika VM üzerinde ve sitenin tüm kullanıcılar tarafından güvenilir olduğunu.

1. Sertifikayı yerel VM sertifikadaki kök CA'ya güvenmek ekleyin **güvenilen kök sertifika yetkilileri**. Gerekli adımlar bir özeti verilmiştir. CA güven konusunda ayrıntılı adımlar için bkz: [sunucu sertifikasını yüklemeniz](https://technet.microsoft.com/library/cc740068).
   
   1. Azure portalından VM seçin ve Bağlan'a tıklayın. Tarayıcı yapılandırmanıza bağlı olarak, VM'ye bağlanmak için bir .rdp dosyası kaydetmek için istenebilir.
      
       ![Azure sanal makineye bağlanma](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kullanıcı VM adı, kullanıcı adı ve parola VM oluştururken yapılandırılmış kullanın. 
      
       Örneğin, aşağıdaki görüntüde, VM adıdır **ssrsnativecloud** ve kullanıcı adı **testuser**.
      
       ![oturum açma bilgileri vm adını içerir](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. MMC.exe çalıştırın. Daha fazla bilgi için bkz: [nasıl yapılır: MMC ek bileşeni ile sertifikaları görüntüleme](https://msdn.microsoft.com/library/ms788967.aspx).
   3. Konsol uygulamasındaki **dosya** menüsünde eklemek **sertifikaları** ek bileşeninde, select **bilgisayar hesabı** istenir ve ardından **sonraki**.
   4. Seçin **yerel bilgisayar** yönetmek ve ardından **son**.
   5. Tıklatın **Tamam** genişletin ve ardından **sertifikalar - kişisel** düğümleri ve ardından **Sertifikalar**. Sertifika VM DNS adından sonra adlandırılır ve ile biten **cloudapp.net**. Sertifika adını sağ tıklatıp **kopya**.
   6. Genişletme **güvenilen kök sertifika yetkilileri** düğümünü ve ardından sağ tıklayarak **sertifikaları** ve ardından **Yapıştır**.
   7. Doğrulamak için çift sertifika adı altında tıklatın **güvenilen kök sertifika yetkilileri** ve hatalar yoktur ve sertifikanızı gördüğünüz doğrulayın. Rapor sunucusu, sertifikaları değerini yapılandırmak için bu konu ile dahil HTTPS betik kullanmak isteyip istemediğinizi **parmak izi** bir komut parametresi olarak gereklidir. **Parmak izi değerini almak için**, aşağıdaki adımları tamamlayın. Parmak izi bölümünde almak için bir PowerShell örnek bulunmaktadır [HTTPS ve rapor sunucusu yapılandırmak için komut dosyası kullanma](#use-script-to-configure-the-report-server-and-HTTPS).
      
      1. Sertifika, örneğin ssrsnativecloud.cloudapp.net adına çift tıklayın.
      2. Tıklatın **ayrıntıları** sekmesi.
      3. Tıklatın **parmak izi**. Parmak izi değerini örneğin a6 Ayrıntılar alanında görüntülenen 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
      4. Parmak izini kopyalayın ve değer daha sonra kullanmak üzere kaydedin veya komut dosyası artık düzenleyin.
      5. (*) Komut dosyası çalıştırılmadan önce değer çiftlerini Between boşlukları kaldırın. Örneğin önce not ettiğiniz parmak izi şimdi a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f olur.
      6. Sunucu sertifikası rapor sunucusuna atayın. Rapor sunucusu yapılandırdığınızda atama sonraki bölümde tamamlandı.

Otomatik olarak imzalanan bir SSL sertifikası kullanıyorsanız, sertifikadaki adı zaten VM konak adı ile eşleşir. Bu nedenle, makinenin DNS zaten genel olarak kaydedilir ve herhangi bir istemciden erişilebilir.

## <a name="step-3-configure-the-report-server"></a>3. adım: rapor sunucusu yapılandırma
Bu bölümde bir Reporting Services yerel mod rapor sunucusu olarak VM nasıl yapılandıracağınız anlatılmaktadır. Rapor sunucusu yapılandırmak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Rapor sunucusu yapılandırmak için komut dosyası kullan
* Rapor sunucusu yapılandırmak için Yapılandırma Yöneticisi'ni kullanın.

Daha ayrıntılı adımları için bkz [sanal makineye bağlanın ve Raporlama Hizmetleri Yapılandırma Yöneticisi'ni Başlat](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Kimlik doğrulama Not:** Windows kimlik doğrulaması önerilen kimlik doğrulama yöntemi ve varsayılan Reporting Services kimlik doğrulaması olur. VM üzerinde yapılandırılmış olan kullanıcılar Reporting Services erişebilir ve Reporting Services rollerine atanmış.

### <a name="use-script-to-configure-the-report-server-and-http"></a>HTTP ve rapor sunucusu yapılandırmak için komut dosyası kullan
Rapor sunucusu yapılandırmak için Windows PowerShell Betiği kullanmak için aşağıdaki adımları tamamlayın. Yapılandırma, HTTP, HTTPS değil içerir:

1. Azure portalından VM seçin ve Bağlan'a tıklayın. Tarayıcı yapılandırmanıza bağlı olarak, VM'ye bağlanmak için bir .rdp dosyası kaydetmek için istenebilir.
   
    ![Azure sanal makineye bağlanma](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kullanıcı VM adı, kullanıcı adı ve parola VM oluştururken yapılandırılmış kullanın. 
   
    Örneğin, aşağıdaki görüntüde, VM adıdır **ssrsnativecloud** ve kullanıcı adı **testuser**.
   
    ![oturum açma bilgileri vm adını içerir](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. VM üzerinde açmak **Windows PowerShell ISE** yönetici ayrıcalıklarına sahip. PowerShell ISE, Windows server 2012'de varsayılan olarak yüklenir. Böylece işe betiğini yapıştırın, komut dosyasını değiştirin ve sonra komut dosyasını çalıştırmak yerine standart bir Windows PowerShell penceresi ISE kullanmanız önerilir.
3. Windows PowerShell ISE'de tıklatın **Görünüm** menüsünü seçin ve ardından **betik bölmesini göster**.
4. Aşağıdaki komut dosyasını kopyalayıp komut dosyasını Windows PowerShell ISE betik bölmesine yapıştırın.
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
   
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. Bir HTTP bağlantı noktası 80 dışında VM oluşturduysanız $HTTPport parametre değiştirme = 80.
6. Komut dosyası şu anda Raporlama Hizmetleri için yapılandırılır. Raporlama Hizmetleri için komut dosyasını çalıştırmak istiyorsanız, ad alanına "v11" Get-WmiObject deyiminde yoluna sürüm kısmı değiştirin.
7. Betiği çalıştırın.

**Doğrulama**: temel rapor sunucusu işlevselliği çalıştığını doğrulamak için bkz: [yapılandırmasını doğrulama](#verify-the-configuration) bu konunun ilerleyen bölümlerinde.

### <a name="use-script-to-configure-the-report-server-and-https"></a>HTTPS ve rapor sunucusu yapılandırmak için komut dosyası kullan
Rapor sunucusu yapılandırmak için Windows PowerShell kullanmak için aşağıdaki adımları tamamlayın. Yapılandırma, HTTPS değil HTTP içerir.

1. Azure portalından VM seçin ve Bağlan'a tıklayın. Tarayıcı yapılandırmanıza bağlı olarak, VM'ye bağlanmak için bir .rdp dosyası kaydetmek için istenebilir.
   
    ![Azure sanal makineye bağlanma](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kullanıcı VM adı, kullanıcı adı ve parola VM oluştururken yapılandırılmış kullanın. 
   
    Örneğin, aşağıdaki görüntüde, VM adıdır **ssrsnativecloud** ve kullanıcı adı **testuser**.
   
    ![oturum açma bilgileri vm adını içerir](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. VM üzerinde açmak **Windows PowerShell ISE** yönetici ayrıcalıklarına sahip. PowerShell ISE, Windows server 2012'de varsayılan olarak yüklenir. Böylece işe betiğini yapıştırın, komut dosyasını değiştirin ve sonra komut dosyasını çalıştırmak yerine standart bir Windows PowerShell penceresi ISE kullanmanız önerilir.
3. Çalışan betikleri etkinleştirmek için aşağıdaki Windows PowerShell komutunu çalıştırın:
   
        Set-ExecutionPolicy RemoteSigned
   
    Ardından, ilke doğrulamak için aşağıdaki çalıştırabilirsiniz:
   
        Get-ExecutionPolicy
4. İçinde **Windows PowerShell ISE**, tıklatın **Görünüm** menüsünü seçin ve ardından **betik bölmesini göster**.
5. Aşağıdaki komut dosyasını kopyalayıp Windows PowerShell ISE betik bölmesine yapıştırın.
   
        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
   
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. Değiştirme **$certificatehash** komut parametresi:
   
   * Bu bir **gerekli** parametresi. Önceki adımları sertifika değeri kaydetmediyseniz, sertifika parmak izini sertifika karma değerini kopyalamak için aşağıdaki yöntemlerden birini kullanın.:
     
       VM Windows PowerShell ISE açın ve aşağıdaki komutu çalıştırın:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       Çıktı aşağıdakine benzer görünecektir. Komut dosyası boş bir satır döndürürse, VM örneğin yapılandırılmış bir sertifikaya sahip değilse, bölümüne bakın [sanal makineleri otomatik olarak imzalanan sertifikayı kullanmak üzere](#to-use-the-virtual-machines-self-signed-certificate).
     
     OR
   * VM mmc.exe çalıştırın ve ardından ekleyin **sertifikaları** ek bileşenini.
   * Altında **güvenilen kök sertifika yetkilileri** düğüme çift tıklayın, sertifika adı. VM otomatik olarak imzalanan sertifika kullanıyorsanız, sertifika VM DNS adından sonra adlandırılır ve ile biten **cloudapp.net**.
   * Tıklatın **ayrıntıları** sekmesi.
   * Tıklatın **parmak izi**. Parmak izi değerini örneğin af 11 60 b6 4b 28 8 d 89 0a Ayrıntılar alanında görüntülenen 82 12 ff 6b a9 c3 66 4f 31 90 48
   * **Komut dosyasını çalıştırmadan önce**, değer çiftlerini Between boşlukları Kaldır. Örneğin af1160b64b288d890a8212ff6ba9c3664f319048
7. Değiştirme **$httpsport** parametre: 
   
   * Ardından HTTPS uç noktası için 443 numaralı bağlantı noktasını kullandıysanız, komut dosyası bu parametrede güncelleştirmek gerekmez. Aksi durumda VM HTTPS özel uç noktası yapılandırıldığında, seçtiğiniz bağlantı noktası değeri kullanın.
8. Değiştirme **$DNSName** parametre: 
   
   * Komut dosyasını bir joker sertifika $DNSName yapılandırılmış = "+". Bir joker karakter sertifika bağını yapılandırmak, $DNSName açıklama istiyorsanız bunu yaparsanız ="+"ve aşağıdaki satırı, tam $DNSNAme başvurusu, ## $DNSName="$server.cloudapp.net etkinleştir".
     
       Raporlama Hizmetleri için sanal makinenin DNS adı kullanmasını istemiyorsanız $DNSName değerini değiştirin. Parametreyi kullanırsanız, sertifika bu adı kullanmanız gerekir ve bir DNS sunucusu adına genel olarak kaydedin.
9. Komut dosyası şu anda Raporlama Hizmetleri için yapılandırılır. Raporlama Hizmetleri için komut dosyasını çalıştırmak istiyorsanız, ad alanına "v11" Get-WmiObject deyiminde yoluna sürüm kısmı değiştirin.
10. Betiği çalıştırın.

**Doğrulama**: temel rapor sunucusu işlevselliği çalıştığını doğrulamak için bkz: [yapılandırmasını doğrulama](#verify-the-connection) bu konunun ilerleyen bölümlerinde. Sertifikayı doğrulamak için bağlama yönetici ayrıcalıklarıyla bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    netsh http show sslcert

Sonuç aşağıdakileri içerir:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Rapor sunucusu yapılandırmak için Yapılandırma Yöneticisi'ni kullanın
Rapor sunucusu yapılandırmak için PowerShell betiğini çalıştırmak istemiyorsanız, rapor sunucusu yapılandırmak için Reporting Services yerel mod Yapılandırma Yöneticisi'ni kullanmak için bu bölümdeki adımları izleyin.

1. Azure portalından VM seçin ve Bağlan'a tıklayın. Kullanıcı adı ve parola VM oluştururken yapılandırılmış kullanın.
   
    ![Azure sanal makineye bağlanma](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Windows Update'i çalıştırın ve VM güncelleştirmeleri yükleyin. VM yeniden başlatma gerekiyorsa, VM'yi yeniden başlatın ve Azure portalından VM yeniden.
3. VM Başlat menüsünden yazın **Reporting Services** açarak **Reporting Services Configuration Manager**.
4. Varsayılan değerleri bırakın **sunucu adı** ve **rapor sunucusu örneği**. **Bağlan**'a tıklayın.
5. Sol bölmede **Web hizmeti URL'si**.
6. Varsayılan olarak, RS IP "Tüm atanan" HTTP bağlantı noktası 80 için yapılandırılır. HTTPS eklemek için:
   
   1. İçinde **SSL sertifikası**: [VM adı] Örneğin, kullanmak istediğiniz sertifikayı seçin. cloudapp.net. Hiçbir sertifika listelenmiyorsa, bölümüne bakın. **2. adım: bir sunucu sertifikası oluşturma** nasıl yükleneceği ve VM sertifikadaki güven hakkında bilgi için.
   2. Altında **SSL bağlantı noktası**: 443'ü seçin. Farklı bir özel bağlantı noktası ile sanal HTTPS özel uç noktasını yapılandırdıysanız, bu değer burada kullanın.
   3. Tıklatın **Uygula** ve işlemin tamamlanmasını bekleyin.
7. Sol bölmede **veritabanı**.
   
   1. Tıklatın **değiştirme Databas**e.
   2. Tıklatın **yeni bir rapor sunucusu veritabanı oluşturun** ve ardından **sonraki**.
   3. Varsayılan adı bırakın **sunucu adı**: VM olarak adlandırın ve varsayılan değeri bırakın **kimlik doğrulama türü** olarak **geçerli kullanıcı** – **tümleşik güvenlik**. **İleri**’ye tıklayın.
   4. Varsayılan adı bırakın **veritabanı adı** olarak **ReportServer** tıklatıp **sonraki**.
   5. Varsayılan adı bırakın **kimlik doğrulama türü** olarak **hizmeti kimlik bilgileri** tıklatıp **sonraki**.
   6. Tıklatın **sonraki** üzerinde **Özet** sayfası.
   7. Yapılandırma tamamlandığında tıklatın **son**.
8. Sol bölmede **Rapor Yöneticisi URL'si**. Varsayılan adı bırakın **sanal dizin** olarak **raporları** tıklatıp **Uygula**.
9. Tıklatın **çıkış** Raporlama Hizmetleri Yapılandırma Yöneticisi'ni kapatın.

## <a name="step-4-open-windows-firewall-port"></a>4. adım: Windows Güvenlik Duvarı bağlantı noktası Aç
> [!NOTE]
> Rapor sunucusu yapılandırmak için komut dosyalarından birini kullanılan, bu bölümü atlayabilirsiniz. Komut dosyası, güvenlik duvarı bağlantı noktasını açmak için bir adım dahil. Varsayılan bağlantı noktası HTTP için 80 ve HTTPS için 443 numaralı bağlantı noktasını oluştu.
> 
> 

Rapor Yöneticisi'ni veya sanal makine rapor sunucusunda uzaktan bağlanmak için bir TCP uç noktası VM gereklidir. VM'ın Güvenlik Duvarı'nda aynı bağlantı noktasını açmak için gereklidir. VM hazırlandığında uç noktası oluşturuldu.

Bu bölümde, güvenlik duvarı bağlantı noktasını açma hakkında temel bilgiler sağlar. Daha fazla bilgi için bkz: [güvenlik duvarını rapor sunucusu erişimi için yapılandırma](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Rapor sunucusu yapılandırmak için komut dosyası kullandıysanız, bu bölümü atlayabilirsiniz. Komut dosyası, güvenlik duvarı bağlantı noktasını açmak için bir adım dahil.
> 
> 

Aşağıdaki betik, özel bir bağlantı noktası HTTPS için 443 dışındaki yapılandırdıysanız, uygun şekilde değiştirin. Bağlantı noktasını açmak için **443** Windows Güvenlik Duvarı üzerinde aşağıdaki adımları tamamlayın:

1. Yönetici ayrıcalıklarıyla bir Windows PowerShell penceresi açın.
2. VM HTTPS uç noktası yapılandırıldığında bir bağlantı noktası 443 dışındaki kullandıysanız, aşağıdaki komut bağlantı noktasını güncelleştirin ve ardından komutu çalıştırın:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. Komut tamamlandığında **Tamam** Komut İstemi'nde görüntülenir.

Bağlantı noktasının açık olduğunu doğrulamak için bir Windows PowerShell penceresi açın ve aşağıdaki komutu çalıştırın:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Temel rapor sunucusu işlevselliği artık çalıştığını doğrulamak için tarayıcınızı yönetici ayrıcalıklarıyla açın ve aşağıdaki rapor sunucusu ad Rapor Yöneticisi için URL'leri göz atın:

* VM, rapor sunucusu URL'sine gidin:
  
        http://localhost/reportserver
* VM, Rapor Yöneticisi URL'si göz atın:
  
        http://localhost/Reports
* Yerel bilgisayarınızdan Gözat **uzak** VM Manager rapor. Aşağıdaki örnekte uygun şekilde DNS adını güncelleştirin. İçin bir parola istendiğinde, VM hazırlandığında oluşturduğunuz yönetici kimlik bilgileri kullanın. Kullanıcı adı [etkialanı].\[kullanıcı adı] Burada etki alanı, örneğin ssrsnativecloud\testuser VM bilgisayar adı biçimi. HTTP kullanmıyorsanız**S**, kaldırma **s** URL. VM ek kullanıcılar oluşturma hakkında bilgi için sonraki bölüme bakın.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* Yerel bilgisayarınızdan uzak rapor sunucusu URL'si için göz atın. Aşağıdaki örnekte uygun şekilde DNS adını güncelleştirin. HTTPS kullanmıyorsanız s URL'de kaldırın.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Kullanıcılar oluşturma ve rolleri atama
Yapılandırma ve rapor sunucusu doğruladıktan sonra bir ortak yönetici bir veya daha fazla kullanıcı oluşturmak ve kullanıcılar için Raporlama Hizmetleri rolleri atamak için bir görevdir. Daha fazla bilgi için aşağıdakilere bakın:

* [Bir yerel kullanıcı hesabı oluşturun](https://technet.microsoft.com/library/cc770642.aspx)
* [Bir rapor sunucusu (Rapor Yöneticisi) GRANT kullanıcı erişimini](https://msdn.microsoft.com/library/ms156034.aspx))
* [Oluşturma ve rol atamalarını yönetme](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Oluşturma ve Azure sanal makinesi raporları yayımlamak için
Aşağıdaki tabloda Microsoft Azure sanal makine üzerinde barındırılan bir rapor sunucusu için bir şirket içi bilgisayardan varolan raporları yayınlamak için kullanılabilir seçenekleri özetlenmektedir:

* **RS.exe betik**: Microsoft Azure sanal makinenize rapor öğeleri ve varolan rapor sunucusuna kopyalamak için kullanım RS.exe komut dosyası. Daha fazla bilgi için "Yerel mod – Microsoft Azure sanal makinesinde yerel moda" bölümüne bakın [örnek Raporlama Hizmetleri rs.exe içerik rapor sunucuları arasında geçirmek için komut dosyası](https://msdn.microsoft.com/library/dn531017.aspx).
* **Rapor Oluşturucu**: sanal makine tıklatın içerir-Microsoft SQL Server Rapor Oluşturucusu sürümünü bir kez. Rapor Oluşturucusu'nu ilk zamanı sanal makineyi başlatmak için:
  
  1. Tarayıcınız yönetici ayrıcalıklarıyla başlatın.
  2. Sanal makinede Rapor Yöneticisi'ni bulun ve tıklatın **Rapor Oluşturucu** Şeritte.
     
     Daha fazla bilgi için bkz: [yükleme, kaldırma ve destekleyici Rapor Oluşturucusu'nu](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server veri araçları: VM**: SQL Server 2012 VM oluşturduğunuz sonra SQL Server veri araçları sanal makinede yüklü olduğundan ve oluşturmak için kullanılan **rapor sunucusu projeleri** ve sanal makinede raporlar. SQL Server veri araçları raporları, rapor sunucusu üzerinde sanal makine için yayımlayabilirsiniz.
  
    SQL server 2014 ile VM oluşturduysanız, SQL Server veri araçları - BI visual Studio için yükleyebilirsiniz. Daha fazla bilgi için aşağıdakilere bakın:
  
  * [Microsoft SQL Server veri araçları - Visual Studio 2013 iş zekası](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Microsoft SQL Server veri araçları - Visual Studio 2012 için iş zekası](https://www.microsoft.com/download/details.aspx?id=36843)
  * [SQL Server veri araçları ve SQL Server Business Intelligence (BI SSDT)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* **SQL Server veri araçları: Uzaktan**: yerel bilgisayarınızda SQL Server veri araçları Raporlama Hizmetleri raporlarını içeren bir Reporting Services projesi oluşturun. Proje için web hizmeti URL'si bağlanmak için yapılandırın.
  
    ![SSRS projesi için SSDT proje özellikleri](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Komut dosyası kullanma**: rapor sunucusu içeriği kopyalamak için komut dosyası kullanın. Daha fazla bilgi için bkz: [örnek Raporlama Hizmetleri rs.exe içerik rapor sunucuları arasında geçirmek için komut dosyası](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>VM kullanmıyorsanız maliyeti en aza indir
> [!NOTE]
> İçin Azure sanal makinelerinizi kullanılmadığında ücretleri en aza indirmek için Azure portalından VM'yi kapatın. VM kapatma için bir VM içinde Windows güç seçenekleri kullanırsanız, hala aynı VM için ücretlendirilirsiniz. Giderlerini azaltmak için Azure portalında VM kapatmanız gerekir. Depolama ücretleri önlemek için VM ve ilişkili .vhd dosyaları silmek VM artık ihtiyacınız varsa, unutmayın. Daha fazla bilgi için SSS bölümüne bakın [sanal makineler fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Daha Fazla Bilgi
### <a name="resources"></a>Kaynaklar
* SQL Server Business Intelligence ve SharePoint 2013 tek sunucu dağıtımı için ilgili benzer içerik için bkz: [bir SQL Server BI ile Azure VM ve SharePoint 2013 oluşturmak için Windows PowerShell'i kullanın](https://msdn.microsoft.com/library/azure/dn385843.aspx).
* SQL Server Business Intelligence ve SharePoint 2013'ün birden çok sunucu dağıtımı için ilgili benzer içerik için bkz: [dağıtmak SQL Server Business Intelligence Azure Virtual Machines'de](https://msdn.microsoft.com/library/dn321998.aspx).
* SQL Server Business Intelligence Azure Virtual Machines'de dağıtımları ilgili genel bilgiler için bkz: [SQL Server Business Intelligence Azure Virtual Machines'de](virtual-machines-windows-classic-ps-sql-bi.md).
* Sanal makineler sekmesinde Azure işlem ücretleri maliyeti hakkında daha fazla bilgi için bkz: [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Topluluk içeriği
* Raporlama Hizmetleri yerel moddaki bir rapor sunucusunda komut dosyasını kullanmadan oluşturma hakkında adım adım yönergeler için bkz: [barındırma SQL Raporlama hizmeti Azure sanal makine üzerinde](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Azure vm'lerinde SQL Server için diğer kaynakların bağlantılarını
[SQL Server üzerinde Azure sanal makinelere genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

