---
title: Yerel mod rapor sunucusu ile bir VM oluşturmak için PowerShell kullanma | Microsoft Docs
description: 'Bu konu açıklar ve bir SQL Server Reporting Services yerel mod rapor sunucusu bir Azure sanal makine yapılandırmasına ve dağıtım gösterilmektedir. '
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
ms.openlocfilehash: 6339b49d0bc9c635457f305dad7b1a075327a1dd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60609948"
---
# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Yerel Mod Rapor Sunucusu ile Azure VM Oluşturmak için PowerShell Kullanma
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu konu açıklar ve bir SQL Server Reporting Services yerel mod rapor sunucusu bir Azure sanal makine yapılandırmasına ve dağıtım gösterilmektedir. Bu belgedeki adımlarda, sanal makine ve sanal makinede Raporlama hizmetlerini yapılandırmak için bir Windows PowerShell betiği oluşturmak için el ile yapılacak adımlar bir birleşimini kullanın. HTTP veya HTTPs için bir güvenlik duvarı bağlantı noktası açma yapılandırma betiğini içerir.

> [!NOTE]
> Size gerekli olmayan, **HTTPS** rapor sunucusunda **2. adıma geçin**.
> 
> 1\. adımda sanal Makineyi oluşturduktan sonra HTTP ve rapor sunucusu yapılandırmak için komut dosyası kullan bölümüne gidin. Rapor sunucusu kullanıma hazır, sonra komut dosyasını çalıştırın.

## <a name="prerequisites-and-assumptions"></a>Önkoşullar ve varsayımlar
* **Azure aboneliği**: Azure aboneliğinizde kullanılabilir çekirdek sayısını doğrulayın. Önerilen sanal makine boyutu oluşturursanız **A3**, gereksinim duyduğunuz **4** kullanılabilir çekirdek sayısı. Bir VM boyutu kullandığınız **A2**, gereksinim duyduğunuz **2** kullanılabilir çekirdek sayısı.
  
  * Azure portalında, aboneliğinizin çekirdek sınırı doğrulamak için sol bölmedeki ve kullanım'a tıklayın ardından üst menüden ayarları.
  * Çekirdek kotasını artırmak için kişi [Azure Destek](https://azure.microsoft.com/support/options/). VM boyutu için bilgi [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **Windows PowerShell komut dosyası**: Konu, Windows PowerShell temel bilgiye sahip olduğunuzu varsayar. Windows PowerShell kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:
  
  * [Windows Server'da Windows PowerShell'i başlatma](https://docs.microsoft.com/powershell/scripting/setup/starting-windows-powershell)
  * [Windows PowerShell ile çalışmaya başlama](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>1\. adım: Bir Azure sanal makinesi sağlama
1. Azure portalına gidin.
2. Tıklayın **sanal makineler** sol bölmesinde.
   
    ![Microsoft azure sanal makineler](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. **Yeni**’ye tıklayın.
   
    ![Yeni düğmesi](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Tıklayın **galerisinden**.
   
    ![Galeriden yeni vm](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Tıklayın **SQL Server 2014'ten RTM'ye Standard – Windows Server 2012 R2** ve ardından devam etmek için oka tıklayın.
   
    ![ileri](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Reporting Services veri abonelikleri özellik temelli gerekiyorsa seçin **SQL Server 2014'ten RTM'ye Enterprise – Windows Server 2012 R2**. SQL Server sürümleri ve özellik desteği hakkında daha fazla bilgi için bkz. [SQL Server 2012 sürümleri tarafından desteklenen özellikler](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. Üzerinde **sanal makine yapılandırması** sayfasında, aşağıdaki alanları düzenleyin:
   
   * Varsa birden fazla **sürüm yayın tarihi**, en son sürümü seçin.
   * **Sanal makine adı**: Makine adı, sonraki yapılandırma sayfasında varsayılan bulut hizmeti DNS adı olarak da kullanılır. DNS adını Azure hizmeti arasında benzersiz olması gerekir. VM VM ne için kullanıldığını açıklayan bir bilgisayar adıyla yapılandırmayı deneyin. Örneğin ssrsnativecloud.
   * **Katman**: Standart
   * **Boyut: A3** SQL Server iş yükleri için önerilen VM boyutu. Bir sanal makine yalnızca bir rapor sunucusu kullandıysanız, rapor sunucusuna büyük bir iş yükü deneyimleri sürece A2 VM boyutunu yeterli olur. VM fiyatlandırma bilgileri için bkz. [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Yeni kullanıcı adı**: sağladığınız adına bir VM'de yönetici olarak oluşturulur.
   * **Yeni parola** ve **onaylayın**. Bu parola yeni bir yönetici hesabı için kullanılır ve güçlü bir parola kullanmanız önerilir.
   * **İleri**’ye tıklayın. ![Sonraki](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. Sonraki sayfada, aşağıdaki alanları düzenleyin:
   
   * **Bulut hizmeti**: seçin **yeni bir bulut hizmeti oluşturma**.
   * **Bulut hizmeti DNS adı**: Bu VM ile ilişkili olan bulut hizmeti Genel DNS adıdır. VM adı için yazdığınız adı varsayılan addır. Konunun daha sonraki adımlarda güvenilir bir SSL sertifikası oluşturma ve DNS adı değerini ardından kullanılırsa "**verilen**" Sertifika.
   * **Bölge/benzeşim grubu/sanal ağ**: Son kullanıcılarınıza en yakın bölgeyi seçin.
   * **Depolama hesabı**: Otomatik olarak oluşturulan depolama hesabı kullanın.
   * **Kullanılabilirlik kümesi**: Yok.
   * **Uç noktaları** tutmak **Uzak Masaüstü** ve **PowerShell** uç noktaları ve HTTP veya HTTPS uç noktası, ortamınıza bağlı olarak ekleyin.
     
     * **HTTP**: Varsayılan Genel ve özel bağlantı noktaları **80**. Özel bir bağlantı noktası 80 dışında kullanırsanız, değiştirme Not **$HTTPport = 80** http komut.
     * **HTTPS**: Varsayılan Genel ve özel bağlantı noktaları **443**. En iyi güvenlik uygulaması özel bağlantı noktasını değiştirin ve güvenlik duvarını ve rapor sunucusunu özel bağlantı noktasını kullanacak şekilde yapılandırmaktır. Uç noktaları hakkında daha fazla bilgi için bkz. [kümesi ayarlama iletişim bir sanal makine ile nasıl](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Bir bağlantı noktası 443 dışındaki kullanırsanız parametreyi değiştirmeniz gerektiğini unutmayın **$HTTPsport = 443** HTTPS komut.
   * İleri'ye tıklayın. ![ileri](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. Sihirbazın son sayfasında, varsayılan tutun **VM aracısını yükleyin** seçili. Bu konu başlığındaki adımları VM Aracısı yazılımınız değil ancak bu VM devam etmeyi planlıyorsanız, VM aracısı ve uzantıları, kendisinin CM geliştirmek izin verir.  VM Aracısı hakkında daha fazla bilgi için bkz. [VM aracısı ve uzantılar – bölüm 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Bir çalışan varsayılan yüklü uzantıları ad VM masaüstüne, iç IP ve boş disk alanı gibi sistem bilgilerini görüntüleyen "BGINFO" uzantısıdır.
9. Tam tıklayın. ![Tamam](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. **Durumu** sanal makinenin görüntüler olarak **başlangıç (hazırlama)** olarak sonra görüntüler ve sağlama işlemi sırasında **çalıştıran** VM'nin, sağlanmış ve kullanıma hazır olduğunda.

## <a name="step-2-create-a-server-certificate"></a>2\. adım: Bir sunucu sertifikası oluşturma
> [!NOTE]
> Rapor sunucusunda HTTPS'yi gerektirmez varsa **2. adıma geçin** ve bölüme gitmek **HTTP ve rapor sunucusu yapılandırmak için komut dosyası kullanma**. HTTP betiği hızlı bir şekilde rapor sunucusunu yapılandırmak ve rapor sunucusu için kullanılmaya hazır olacaktır.

VM'de HTTPS kullanmak için güvenilir bir SSL sertifikası gerekir. Senaryonuza bağlı olarak, aşağıdaki iki yöntemden birini kullanabilirsiniz:

* Geçerli bir SSL sertifikası bir sertifika yetkilisi (CA) tarafından verilen ve Microsoft tarafından güvenilir. CA kök sertifikaları Microsoft kök sertifikası programı dağıtılacak gereklidir. Bu program hakkında daha fazla bilgi için bkz. [Windows ve Windows Phone 8 SSL kök sertifika programı (üye CAs)](https://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) ve [Microsoft kök sertifika programı giriş](https://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Kendinden imzalı bir sertifika. Otomatik olarak imzalanan sertifikaları üretim ortamları için önerilmez.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Bir güvenilen sertifika yetkilisi (CA) tarafından oluşturulan bir sertifika kullanmak için
1. **Web sitesi için bir sunucu sertifikası bir sertifika yetkilisinden sertifika**. 
   
    Bir sertifika yetkilisine göndermek bir sertifika isteği dosyasını (Certreq.txt) oluşturmak veya bir çevrimiçi sertifika yetkilisi için bir istek oluşturun ve Web sunucusu sertifikası sihirbazını kullanabilirsiniz. Microsoft Sertifika Hizmetlerini Windows Server 2012 gibi. Sunucu sertifikası tarafından sunulan kimlik güvence düzeyine bağlı olarak buna birkaç ay isteğinizi onaylamanız ve bir sertifika dosyası göndermek sertifika yetkilisi için birkaç gün var. 
   
    Bir sunucu sertifikası isteme hakkında daha fazla bilgi için aşağıdakilere bakın: 
   
   * Kullanım [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Windows Server 2012 yönetimi için Güvenlik Araçları'nı tıklatın.
     
     [Windows Server 2012 yönetimi için güvenlik araçları](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > **Verilen** alana güvenilir SSL sertifikası ile aynı olmalıdır **bulut hizmeti DNS adı** yeni VM için kullanılır.

2. **Web sunucusunda sunucu sertifikasının yüklenmesine**. Web sunucusu bu durumda rapor sunucusunu barındıran sanal makine olduğu ve Reporting Services yapılandırıldığında Web sitesi sonraki adımlarda oluşturulur. Sunucu sertifikasının sertifika MMC ek bileşenini kullanarak Web sunucusu yükleme hakkında daha fazla bilgi için bkz. [sunucu sertifikasını yüklemeniz](https://technet.microsoft.com/library/cc740068).
   
    Rapor sunucusu, sertifikaları değerini yapılandırmak için bu konu ile eklenen komut dosyası kullanmak istiyorsanız **parmak izi** betiğin bir parametre olarak gereklidir. Sertifikanın parmak izini alma konusunda ayrıntılar için sonraki bölüme bakın.
3. Sunucu sertifikası rapor sunucusuna atayın. Rapor sunucusu yapılandırırsanız, sonraki bölümde atama tamamlandı.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Sanal makineler otomatik olarak imzalanan sertifika kullanmak için
Sanal makine sağlanırken VM'de otomatik olarak imzalanan bir sertifika oluşturulur. Sertifika, sanal makine DNS adı ile aynı ada sahiptir. Sertifika hataları önlemek için gerekli sertifikayı VM üzerinde ve tüm site kullanıcıları tarafından güveniliyor.

1. Yerel sanal makine üzerinde sertifikanın kök CA'ya güvenmek üzere sertifikaya eklemek **güvenilen kök sertifika yetkilileri**. Gerekli adımlar özetini verilmiştir. CA güven konusunda ayrıntılı adımlar için bkz: [sunucu sertifikasını yüklemeniz](https://technet.microsoft.com/library/cc740068).
   
   1. Azure portalından sanal Makineyi seçin ve Bağlan'a tıklayın. Tarayıcı yapılandırmanıza bağlı olarak, VM'ye bağlanmak için bir .rdp dosyası kaydetmek için istenebilir.
      
       ![Azure sanal makinesine bağlanın](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kullanıcı VM adı, kullanıcı adı ve sanal Makineyi oluştururken yapılandırdığınız parolayı kullanın. 
      
       Örneğin, aşağıdaki görüntüde, VM adıdır **ssrsnativecloud** ve kullanıcı adı **testuser**.
      
       ![oturum açma vm adını içerir.](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. MMC.exe çalıştırın. Daha fazla bilgi için [nasıl yapılır: MMC ek bileşeni ile sertifikaları görüntüleme](https://msdn.microsoft.com/library/ms788967.aspx).
   3. Konsol uygulamasında **dosya** menüsünde Ekle **sertifikaları** ek bileşeninde, select **bilgisayar hesabı** istenir ve ardından **sonraki**.
   4. Seçin **yerel bilgisayar** yönetmek ve ardından **son**.
   5. Tıklayın **Tamam** ve ardından **sertifikaları - Kişisel** düğümleri ve ardından **sertifikaları**. Sertifika sonra sanal makinenin DNS adı olarak adlandırılır ve ile sona eren **cloudapp.net**. Sertifika adını sağ tıklatıp **kopyalama**.
   6. Genişletin **güvenilen kök sertifika yetkilileri** düğümünü ve ardından sağ **sertifikaları** ve ardından **Yapıştır**.
   7. Doğrulamak için çift sertifika adı altında tıklayın **güvenilen kök sertifika yetkilileri** ve hiçbir hata olmadığından ve sertifikanızı gördüğünüz doğrulayın. Rapor sunucusu, sertifikaları değerini yapılandırmak için bu konu ile dahil HTTPS betik kullanmak istiyorsanız **parmak izi** betiğin bir parametre olarak gereklidir. **Parmak izi değerini almak için**, aşağıdaki adımları tamamlayın. Parmak izi bölümünde almak için bir PowerShell örneğini de mevcuttur [HTTPS ve rapor sunucusu yapılandırmak için komut dosyası kullanma](#use-script-to-configure-the-report-server-and-https).
      
      1. Sertifika, örneğin ssrsnativecloud.cloudapp.net adına çift tıklayın.
      2. **Ayrıntılar** sekmesine tıklayın.
      3. Tıklayın **parmak izi**. Parmak izi değerini örneğin a6 Ayrıntılar alanında görüntülenen 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
      4. Parmak izini kopyalayın ve değeri daha sonra kullanmak üzere kaydetmek veya artık betiği düzenleyin.
      5. (*) Çalıştırmadan önce betiğin, değer çiftlerini arasındaki boşlukları kaldırın. Örneğin önce belirtilen parmak izi artık a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f olacaktır.
      6. Sunucu sertifikası rapor sunucusuna atayın. Rapor sunucusu yapılandırırsanız, sonraki bölümde atama tamamlandı.

Otomatik olarak imzalanan bir SSL sertifikası kullanıyorsanız sertifika adına VM konak adı zaten eşleşir. Bu nedenle, makinenin DNS genel olarak kaydedilmiş ve herhangi bir istemciden erişilebilir.

## <a name="step-3-configure-the-report-server"></a>3\. adım: Rapor sunucusunu yapılandırma
Bu bölümde, sanal Makinenin bir Reporting Services yerel mod rapor sunucusu nasıl yapılandıracağınız anlatılmaktadır. Rapor sunucusunu yapılandırmak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Rapor sunucusunu yapılandırmak için komut dosyası kullan
* Rapor sunucusunu yapılandırmak için Configuration Manager'ı kullanın.

Daha ayrıntılı adımları için konudaki [sanal makineye bağlanın ve Raporlama Hizmetleri Yapılandırma Yöneticisi'ni Başlat](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Kimlik doğrulaması Not:** Windows kimlik doğrulaması önerilen kimlik doğrulama yöntemidir ve varsayılan Reporting Services kimlik doğrulamasıdır. VM üzerinde yapılandırılan kullanıcılar için Reporting Services rolleri atanmış ve Reporting Services erişebilirsiniz.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Rapor sunucusu ve HTTP yapılandırmak için komut dosyası kullan
Rapor sunucusunu yapılandırmak için Windows PowerShell Betiği kullanmak için aşağıdaki adımları tamamlayın. Yapılandırma, HTTP, HTTPS değil içerir:

1. Azure portalından sanal Makineyi seçin ve Bağlan'a tıklayın. Tarayıcı yapılandırmanıza bağlı olarak, VM'ye bağlanmak için bir .rdp dosyası kaydetmek için istenebilir.
   
    ![Azure sanal makinesine bağlanın](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kullanıcı VM adı, kullanıcı adı ve sanal Makineyi oluştururken yapılandırdığınız parolayı kullanın. 
   
    Örneğin, aşağıdaki görüntüde, VM adıdır **ssrsnativecloud** ve kullanıcı adı **testuser**.
   
    ![oturum açma vm adını içerir.](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Sanal makinede açın **Windows PowerShell ISE** yönetici ayrıcalıklarına sahip. PowerShell ISE, Windows server 2012'de varsayılan olarak yüklenir. Bu işlem, betik ISE'ye yapıştırın, komut dosyasını değiştirin ve ardından komut dosyasını çalıştırmak yerine standart bir Windows PowerShell penceresi ISE kullanmanız önerilir.
3. Windows PowerShell ISE'de tıklayın **görünümü** menüsünü seçin ve ardından **betik bölmesini göster**.
4. Aşağıdaki betiği kopyalayın ve betiği Windows PowerShell ISE betik bölmesine yapıştırın.
   
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
5. Bir HTTP bağlantı noktası 80'den farklı bir VM oluşturduysanız $HTTPport parametre değiştirme = 80.
6. Betik, şu anda Reporting Services için yapılandırılır. Raporlama Hizmetleri için komut dosyasını çalıştırmak istiyorsanız, ad alanına "v11" Get-WmiObject deyimindeki yolunu sürümü bölümünü değiştirin.
7. Betiği çalıştırın.

**Doğrulama**: Temel rapor sunucusu işlevselliği çalıştığını doğrulamak için bkz: [yapılandırmasını doğrulama](#verify-the-configuration) bu konunun ilerleyen bölümlerinde.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Rapor sunucusu ve HTTPS yapılandırmak için komut dosyası kullan
Rapor sunucusunu yapılandırmak için Windows PowerShell kullanmak için aşağıdaki adımları tamamlayın. Yapılandırma, HTTPS değil HTTP içerir.

1. Azure portalından sanal Makineyi seçin ve Bağlan'a tıklayın. Tarayıcı yapılandırmanıza bağlı olarak, VM'ye bağlanmak için bir .rdp dosyası kaydetmek için istenebilir.
   
    ![Azure sanal makinesine bağlanın](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Kullanıcı VM adı, kullanıcı adı ve sanal Makineyi oluştururken yapılandırdığınız parolayı kullanın. 
   
    Örneğin, aşağıdaki görüntüde, VM adıdır **ssrsnativecloud** ve kullanıcı adı **testuser**.
   
    ![oturum açma vm adını içerir.](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Sanal makinede açın **Windows PowerShell ISE** yönetici ayrıcalıklarına sahip. PowerShell ISE, Windows server 2012'de varsayılan olarak yüklenir. Bu işlem, betik ISE'ye yapıştırın, komut dosyasını değiştirin ve ardından komut dosyasını çalıştırmak yerine standart bir Windows PowerShell penceresi ISE kullanmanız önerilir.
3. Betiklerin çalıştırılmasını etkinleştirmek için aşağıdaki Windows PowerShell komutunu çalıştırın:
   
        Set-ExecutionPolicy RemoteSigned
   
    Ardından, ilkeyi doğrulamak için aşağıdaki çalıştırabilirsiniz:
   
        Get-ExecutionPolicy
4. İçinde **Windows PowerShell ISE**, tıklayın **görünümü** menüsünü seçin ve ardından **betik bölmesini göster**.
5. Aşağıdaki betiği kopyalayın ve Windows PowerShell ISE betik bölmesine yapıştırın.
   
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
   
   * Bu bir **gerekli** parametresi. Certificate değerini önceki adımlardan kaydetmediyseniz, sertifika parmak izini Sertifika Karması değeri kopyalamak için aşağıdaki yöntemlerden birini kullanın.:
     
       Sanal makinede, Windows PowerShell ISE'yi açın ve aşağıdaki komutu çalıştırın:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       Çıktı aşağıdakine benzer olacaktır. Komut dosyası boş bir satır döndürürse, VM örnek yapılandırılmış bir sertifikanın yok, bakın [sanal makineleri otomatik olarak imzalanan sertifika kullanmaya](#to-use-the-virtual-machines-self-signed-certificate).
     
     OR
   * Mmc.exe VM'de çalıştırmak ve ardından eklemek **sertifikaları** ek.
   * Altında **güvenilen kök sertifika yetkilileri** düğüme çift tıklayın, sertifika adı. Sanal makinenin otomatik olarak imzalanan sertifika kullanıyorsanız, sertifikayı sonra sanal makinenin DNS adı olarak adlandırılır ve ile sona erer **cloudapp.net**.
   * **Ayrıntılar** sekmesine tıklayın.
   * Tıklayın **parmak izi**. Parmak izi değerini örneğin af 11 60 b6 4b 28 8 d 89 0a Ayrıntılar alanında görüntülenen 82 12 ff 6b a9 c3 66 4f 31 90 48
   * **Betiği çalıştırmadan önce**, değer çiftlerini arasındaki boşlukları Kaldır. Örneğin af1160b64b288d890a8212ff6ba9c3664f319048
7. Değiştirme **$httpsport** parametresi: 
   
   * Ardından için HTTPS uç noktasının bağlantı noktası 443'ü kullandıysanız, betikteki Bu parametre güncelleştirmek gerekmez. Aksi takdirde sanal makinede HTTPS özel uç nokta yapılandırdığınızda, seçtiğiniz bağlantı noktası değeri kullanın.
8. Değiştirme **$DNSName** parametresi: 
   
   * Betik bir joker sertifikası $DNSName yapılandırılmış = "+". Bir joker karakter sertifika bağlamasını yapılandırma, out $DNSName açıklama istiyorsanız bunu yaparsanız ="+"ve aşağıdaki satırı tam $DNSNAme başvurusu, ## $DNSName="$server.cloudapp.net etkinleştir".
     
       Reporting Services için sanal makinenin DNS adı kullanmak istemiyorsanız $DNSName değerini değiştirin. Parametreyi kullanırsanız, sertifika Ayrıca bu adını kullanmanız gerekir ve bir DNS sunucusu adına genel olarak kaydedin.
9. Betik, şu anda Reporting Services için yapılandırılır. Raporlama Hizmetleri için komut dosyasını çalıştırmak istiyorsanız, ad alanına "v11" Get-WmiObject deyimindeki yolunu sürümü bölümünü değiştirin.
10. Betiği çalıştırın.

**Doğrulama**: Temel rapor sunucusu işlevselliği çalışıp çalışmadığını doğrulamak için bu konunun ilerleyen bölümlerinde yapılandırma bölümü doğrula bakın. Sertifikayı doğrulamak için bağlama yönetici ayrıcalıklarıyla bir komut istemi açın ve ardından aşağıdaki komutu çalıştırın:

    netsh http show sslcert

Sonuç şunları içerir:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Rapor sunucusunu yapılandırmak için Yapılandırma Yöneticisi'ni kullanın
Rapor sunucusunu yapılandırmak için PowerShell betiğini çalıştırmak istemiyorsanız, yerel mod Reporting Services configuration manager rapor sunucusu yapılandırmak için bu bölümdeki adımları izleyin.

1. Azure portalından sanal Makineyi seçin ve Bağlan'a tıklayın. Kullanıcı adı ve sanal Makineyi oluştururken yapılandırdığınız parolayı kullanın.
   
    ![Azure sanal makinesine bağlanın](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Windows Update'i çalıştırın ve güncelleştirmelerin VM'ye yükleyin. Sanal Makinenin yeniden başlatılması gerekiyorsa, VM'yi yeniden başlatın ve sanal Makineye Azure portalından yeniden.
3. VM Başlat menüsünden yazın **Reporting Services** açın **Reporting Services Yapılandırma Yöneticisi**.
4. Varsayılan değerleri bırakın **sunucu adı** ve **rapor sunucusu örneği**. **Bağlan**'a tıklayın.
5. Sol bölmede **Web hizmeti URL'si**.
6. Varsayılan olarak, "Tüm atanan" ıp'li HTTP bağlantı noktası 80 için RS yapılandırılır. HTTPS eklemek için:
   
   1. İçinde **SSL sertifikası**:, [VM adı] örneğin kullanmak istediğiniz sertifikayı seçin. cloudapp.net. Hiçbir sertifika listelenmiyorsa, bölümüne bakın. **2. adım: Bir sunucu sertifikası oluşturma** yükleme ve sanal makinedeki sertifikaya güvenmek hakkında bilgi için.
   2. Altında **SSL bağlantı noktası**: 443'ü seçin. Sanal makine farklı bir özel bağlantı noktası ile HTTPS özel uç nokta yapılandırdıysanız, bu değeri burada kullanın.
   3. Tıklayın **Uygula** ve işlemin tamamlanmasını bekleyin.
7. Sol bölmede **veritabanı**.
   
   1. Tıklayın **değiştirme veritabanı**e.
   2. Tıklayın **yeni bir rapor sunucusu veritabanı oluşturmanız** ve ardından **sonraki**.
   3. Varsayılan değeri bırakın **sunucu adı**: VM olarak adlandırın ve varsayılan değeri bırakın **kimlik doğrulama türü** olarak **geçerli kullanıcının** – **tümleşik güvenlik**. **İleri**’ye tıklayın.
   4. Varsayılan değeri bırakın **veritabanı adı** olarak **ReportServer** tıklatıp **sonraki**.
   5. Varsayılan değeri bırakın **kimlik doğrulama türü** olarak **hizmet kimlik bilgilerini** tıklatıp **sonraki**.
   6. Tıklayın **sonraki** üzerinde **özeti** sayfası.
   7. Yapılandırma tamamlandığında tıklayın **son**.
8. Sol bölmede **Rapor Yöneticisi URL'si**. Varsayılan değeri bırakın **sanal dizin** olarak **raporları** tıklatıp **Uygula**.
9. Tıklayın **çıkış** Reporting Services Yapılandırma Yöneticisi'ni kapatın.

## <a name="step-4-open-windows-firewall-port"></a>4\. Adım: Windows Güvenlik Duvarı bağlantı noktası Aç
> [!NOTE]
> Komut dosyalarından birini rapor sunucusunu yapılandırmak için kullandıysanız, bu bölümü atlayabilirsiniz. Betik, güvenlik duvarı bağlantı noktasını açmak için bir adım dahil. Varsayılan bağlantı noktası HTTP için 80 ve HTTPS için 443 numaralı bağlantı noktasını oluştu.
> 
> 

Rapor Yöneticisi'ni veya rapor sunucusu sanal makinede uzaktan bağlanmak için VM üzerinde bir TCP uç noktası gereklidir. Sanal makinenin güvenlik duvarında aynı bağlantı noktasını açmak için gereklidir. Sanal makine sağlanırken uç noktası oluşturuldu.

Bu bölümde, güvenlik duvarı bağlantı noktası açma hakkında temel bilgiler sağlar. Daha fazla bilgi için [rapor sunucusu erişimi için bir güvenlik duvarı yapılandırma](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Rapor sunucusunu yapılandırmak için komut dosyası kullandıysanız, bu bölümü atlayabilirsiniz. Betik, güvenlik duvarı bağlantı noktasını açmak için bir adım dahil.
> 
> 

Özel bir bağlantı noktası HTTPS için 443 dışındaki yapılandırdıysanız, aşağıdaki komut dosyasını uygun şekilde değiştirin. Bağlantı noktasını açmanız **443** Windows Güvenlik Duvarı üzerinde aşağıdaki adımları tamamlayın:

1. Yönetici ayrıcalıklarıyla bir Windows PowerShell penceresi açın.
2. VM üzerinde bir HTTPS uç noktası yapılandırıldığında bir bağlantı noktası 443 dışındaki kullandıysanız aşağıdaki komutta bağlantı noktasını güncelleştirin ve ardından komutu çalıştırın:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. Komut tamamlandığında, **Tamam** komut isteminde görüntülenir.

Bağlantı noktası açık olduğunu doğrulamak için bir Windows PowerShell penceresi açın ve aşağıdaki komutu çalıştırın:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Temel rapor sunucusu işlevselliği artık çalışır durumda olduğunu doğrulamak için tarayıcınızı yönetici ayrıcalıklarıyla açın ve ardından aşağıdaki rapor sunucusu ad Rapor Yöneticisi için URL'leri göz atın:

* Sanal makinede rapor sunucusu URL'sine gidin:
  
        http://localhost/reportserver
* Sanal makinede Rapor Yöneticisi'ni URL'sine gidin:
  
        http://localhost/Reports
* Yerel bilgisayarınızdan göz atın **uzak** VM'de Manager rapor. DNS adı aşağıdaki örnekte uygun şekilde güncelleştirin. Parola istendiğinde, sanal makine sağlanırken oluşturduğunuz yönetici kimlik bilgileri kullanın. [Domain] kullanıcı adı.\[kullanıcı adı] biçimi, VM bilgisayar adı, örneğin ssrsnativecloud\testuser olduğu etki alanı. HTTP kullanmıyorsanız**S**, kaldırma **s** URL. Ek kullanıcılar VM oluşturma ile ilgili bilgi için sonraki bölüme bakın.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* Yerel bilgisayarınızdan uzak rapor sunucusu URL'sine gidin. DNS adı aşağıdaki örnekte uygun şekilde güncelleştirin. HTTPS kullanmıyorsanız, URL'de kaldırın.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Kullanıcı oluşturma ve rol atama
Yapılandırma ve rapor sunucusu doğruladıktan sonra genel yönetici bir veya daha fazla kullanıcı oluşturmak ve kullanıcılar için Reporting Services rolleri atamak için bir görevdir. Daha fazla bilgi için, aşağıdakilere bakın:

* [Bir yerel kullanıcı hesabı oluşturun](https://technet.microsoft.com/library/cc770642.aspx)
* [Rapor sunucusu (Rapor Yöneticisi) verme kullanıcı erişimini](https://msdn.microsoft.com/library/ms156034.aspx))
* [Oluşturma ve rol atamalarını yönetme](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Oluşturma ve raporları Azure sanal makine yayımlama
Aşağıdaki tabloda bazı Microsoft Azure sanal makinede barındırılan bir rapor sunucusu için bir şirket içi bilgisayardan varolan raporları yayımlamak kullanılabilir seçenekler özetlenmektedir:

* **RS.exe betik**: Rapor öğeleri ve mevcut rapor sunucusu, Microsoft Azure sanal makinenize kopyalamak için RS.exe betiğini kullanın. Daha fazla bilgi için bkz: ' % s'bölümü "Yerel mod – Microsoft Azure sanal makinesinde yerel moda" [rapor sunucuları arasında içerik geçişi için betik örnek Raporlama Hizmetleri rs.exe](https://msdn.microsoft.com/library/dn531017.aspx).
* **Rapor Oluşturucu**: Tıklayarak sanal makineyi içeren-Microsoft SQL Server Rapor Oluşturucusu sürümünü bir kez. Rapor Oluşturucusu'nu ilk zamanı sanal makineyi başlatmak için:
  
  1. Tarayıcınız, yönetici ayrıcalıklarıyla başlatın.
  2. Sanal makinede Rapor Yöneticisi'ni bulun ve tıklayın **Rapor Oluşturucusu'nu** Şeritte.
     
     Daha fazla bilgi için [yükleme, kaldırma ve destekleyici Rapor Oluşturucusu'nu](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server veri araçları: VM**:  SQL Server 2012 ile VM oluşturduğunuz sonra SQL Server veri araçları sanal makineye yüklenir ve oluşturmak için kullanılan **rapor sunucusu projeleri** ve sanal makine üzerindeki raporlar. SQL Server veri araçları, sanal makinede rapor sunucusu raporları yayımlayabilirsiniz.
  
    SQL server 2014 ile bir VM oluşturduysanız, SQL Server veri araçları - BI visual Studio için yükleyebilirsiniz. Daha fazla bilgi için, aşağıdakilere bakın:
  
  * [Microsoft SQL Server veri araçları - Visual Studio 2013 için iş zekası](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Microsoft SQL Server veri araçları - Visual Studio 2012 için iş zekası](https://www.microsoft.com/download/details.aspx?id=36843)
  * [SQL Server veri araçları ve SQL Server İş Zekası'nı (SSDT-BI)](https://docs.microsoft.com/sql/ssdt/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi)
* **SQL Server veri araçları: Uzak**:  Yerel bilgisayarınızda SQL Server veri araçları Raporlama Hizmetleri raporlarını içeren bir Reporting Services projesi oluşturun. Projenin web hizmetinin URL'SİYLE bağlanmak için yapılandırın.
  
    ![SSRS projesi için SSDT proje özellikleri](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Komut dosyası kullanma**: Rapor sunucusu içeriğini kopyalamak için komut dosyası kullanın. Daha fazla bilgi için [rapor sunucuları arasında içerik geçişi için betik örnek Raporlama Hizmetleri rs.exe](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Sanal makine kullanmıyorsanız maliyeti en aza indir
> [!NOTE]
> İçin Azure Virtual Machines'inizi kullanımda olmadığında ücretleri en aza indirmek için Azure portalında VM'yi kapatın. Sanal makineyi bir VM içinde Windows güç seçeneklerini kullanırsanız, aynı VM için yine de ücretlendirilir. Giderlerini azaltmak için Azure portalında VM'yi kapatmanız gerekir. VM artık ihtiyacınız yoksa, depolama ücretleri önlemek için VM ve ilişkili .vhd dosyaları silmeyi unutmayın. Daha fazla bilgi için SSS bölümüne bakın. [sanal makineler fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Daha Fazla Bilgi
### <a name="resources"></a>Kaynaklar
* SQL Server İş Zekası ve SharePoint 2013'ün tek sunuculu dağıtımına benzer içerikler için bkz: [bir SQL Server BI ile Azure VM ve SharePoint 2013'ü oluşturmak için Windows PowerShell kullanarak](https://blogs.technet.microsoft.com/ptsblog/2013/10/24/use-powershell-to-create-a-windows-azure-vm-with-sql-server-bi-and-sharepoint-2013/).
* SQL Server İş Zekası Azure sanal Makineler'de dağıtımları ilgili genel bilgi için bkz: [SQL Server İş Zekası Azure sanal Makineler'de](virtual-machines-windows-classic-ps-sql-bi.md).
* Azure işlem ücretleri maliyeti hakkında daha fazla bilgi için bkz, sanal makineler sekmesinden [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Topluluk içeriği
* Komut dosyası kullanmadan bir Reporting Services yerel mod rapor sunucusu oluşturma konusunda adım adım yönergeler için bkz. [barındırma SQL Raporlama hizmeti Azure sanal makine üzerinde](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Azure Vm'lerde SQL Server için diğer kaynaklara bağlantılar
[SQL Server Azure sanal makinelerine genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

