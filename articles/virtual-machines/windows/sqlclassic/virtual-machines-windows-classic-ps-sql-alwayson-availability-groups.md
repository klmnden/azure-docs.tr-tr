---
title: PowerShell kullanarak bir Azure sanal makinesinde Always On kullanılabilirlik grubu yapılandırma | Microsoft Docs
description: Bu öğreticide Klasik dağıtım modeliyle oluşturulan kaynaklar kullanılmaktadır. Azure'da bir Always On kullanılabilirlik grubu oluşturmak için PowerShell kullanın.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: c089d54544217cf72f81a2535ceede50d25b9b61
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362195"
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a>Azure VM'de PowerShell ile Always On kullanılabilirlik grubu yapılandırma
> [!div class="op_single_selector"]
> * [Klasik: KULLANICI ARABİRİMİ](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasik: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Başlamadan önce artık Azure resource manager modelinde bu görevi tamamlayabilirsiniz göz önünde bulundurun. Yeni dağıtımlar için Azure resource manager modelini öneririz. Bkz: [SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> En yeni dağıtımların Resource Manager modelini kullanmasını öneririz. Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir.

Azure sanal makineleri (VM'ler), Veritabanı yöneticileri, yüksek oranda kullanılabilir SQL Server Sistem maliyetlerini düşürmeye yardımcı olabilir. Bu öğreticide SQL Server Always On için uçtan uca bir Azure ortamı içinde bir kullanılabilirlik grubuna uygulamak gösterilmektedir. Öğreticinin sonunda, SQL Server Always On azure'da çözümünüzü aşağıdaki öğelerden oluşur:

* Bir ön uç dahil olmak üzere birden çok alt ağa ve arka uç alt ağı içeren bir sanal ağ.
* Bir Active Directory etki alanı ile etki alanı denetleyicisi.
* İki SQL Server arka uç alt ağına dağıtılır ve Active Directory etki alanına katılmış sanal makineleri.
* Düğüm çoğunluğu çekirdek modeli ile üç düğümlü bir Windows Yük devretme kümesi.
* Bir kullanılabilirlik veritabanının bir kullanılabilirlik grubu ile iki synchronous-commit çoğaltmalarından.

Bu senaryo, Hesaplı maliyet veya başka faktörlerin için değil, Azure üzerinde Basitlik için iyi bir seçim ' dir. Örneğin, bir iki düğümlü yük devretme kümesinde çekirdek dosya paylaşım tanığı olarak etki alanı denetleyicisi kullanarak azure'da işlem saatleri kaydetmek bir çoğaltma iki kullanılabilirlik grubu için VM sayısını en aza indirebilirsiniz. Bu yöntem, yukarıdaki yapılandırma diğerine göre VM sayısını azaltır.

Bu öğretici her adımın ayrıntıları elaborating olmadan, yukarıda açıklandığı gibi çözüm ayarlamak için gerekli olan adımları göstermek için tasarlanmıştır. Bu nedenle, GUI yapılandırma adımları sağlamak yerine, her adımda hızlı bir şekilde yararlanmak için betik oluşturma PowerShell kullanır. Bu öğreticide aşağıdaki varsayılır:

* Sanal makine aboneliği ile bir Azure hesabı zaten var.
* Yüklediğiniz [Azure PowerShell cmdlet'lerini](/powershell/azure/overview).
* Always On kullanılabilirlik grupları şirket içi çözümler için düz bir anlayış zaten var. Daha fazla bilgi için [Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Azure aboneliğinize bağlanın ve sanal ağ oluşturma
1. Bir PowerShell penceresinde, yerel bilgisayarınızda Azure modülünü içeri aktarın, yayımlama ayarları dosyası makinenize indirmek ve PowerShell oturumunuzda, indirilen yayımlama ayarlarını içeri aktararak Azure aboneliğinize bağlanın.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    **Get-AzurePublishSettingsFile** komut otomatik olarak Azure ile bir yönetim sertifikası oluşturur ve makinenize indirir. Bir tarayıcı otomatik olarak açılır ve Azure aboneliğiniz için Microsoft hesap kimlik bilgilerini girmeniz istenir. İndirilen **.publishsettings** dosya, Azure aboneliğinizi yönetmek için ihtiyacınız olan tüm bilgileri içerir. Bu dosyayı yerel bir dizine kaydettikten sonra bunu kullanarak içeri **Import-AzurePublishSettingsFile** komutu.

   > [!NOTE]
   > .Publishsettings dosyasını Azure abonelik ve hizmetleri yönetmek için kullanılan (kodlanmamış), kimlik bilgilerini içerir. Bu dosya için en iyi güvenlik uygulaması, kaynak dizinleri (örneğin, Libraries\Documents klasöründe) dışında geçici olarak depolar ve içeri aktarma tamamlandıktan sonra Sil sağlamaktır. .Publishsettings dosyasını erişim kazanır kötü niyetli bir kullanıcı düzenleyin, oluşturun ve Azure hizmetlerinizi silin.

2. Bir dizi bulut BT altyapısı oluşturmak için kullanacağınız değişkenleri tanımlayın.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Komutlarınızın daha sonra başarılı olmak için aşağıdakilere dikkat edin:

   * Değişkenleri **$storageAccountName** ve **$dcServiceName** , bulut depolama hesabı ve bulut sunucusu, sırasıyla, Internet'te tanımlamak için kullanıldığından benzersiz olması gerekir.
   * Değişkenler için belirttiğiniz adları **$affinityGroupName** ve **$virtualNetworkName** daha sonra kullanacağınız sanal ağ yapılandırma belgede yapılandırılır.
   * **$sqlImageName** güncelleştirilmiş SQL Server 2012 Service Pack 1 Enterprise Edition içeren VM görüntü adını belirtir.
   * Kolaylık olması için **Contoso! 000** tüm öğretici boyunca kullanılan paroladır.

3. Bir benzeşim grubu oluşturun.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Bir yapılandırma dosyasını içeri aktararak sanal ağ oluşturun.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Yapılandırma dosyası, aşağıdaki XML belgesi içeriyor. Kısaca, bu adlı bir sanal ağ belirtir **ContosoNET** adlı benzeşim grubunda **ContosoAG**. Adres alanı sahip **10.10.0.0/16** ve iki alt ağa sahip **10.10.1.0/24** ve **10.10.2.0/24**, olan ön alt ağı ve geri alt ağ, sırasıyla. Burada, Microsoft SharePoint gibi istemci uygulamalarını yerleştirebilirsiniz ön alt yer. Geri alt ağ, SQL Server Vm'leri yerleştirdiğiniz ' dir. Değiştirirseniz **$affinityGroupName** ve **$virtualNetworkName** değişkenleri daha önce de aşağıdaki karşılık gelen adlarını değiştirmeniz gerekir.

        <NetworkConfiguration xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. Oluşturduğunuz ve aboneliğinizde geçerli bir depolama hesabı olarak ayarlanmış bir benzeşim grubu ile ilişkili bir depolama hesabı oluşturun.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Etki alanı denetleyicisi sunucusuna yeni bulut hizmeti ve kullanılabilirlik kümesindeki oluşturun.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Bu piped komutlar, şunları yapın:

   * **Yeni AzureVMConfig** bir VM yapılandırması oluşturur.
   * **Ekle-AzureProvisioningConfig** tek başına Windows server'ın yapılandırma parametrelerini sağlar.
   * **Ekleme AzureDataDisk** hiçbiri olarak ayarlamak önbelleğe alma seçeneği ile Active Directory verilerini depolamak için kullanacağınız veri diski ekler.
   * **Yeni-AzureVM** yeni bir bulut hizmeti ve yeni bulut hizmetinde yeni bir Azure VM oluşturur.

7. Yeni VM tam olarak hazırlanmasını bekleyin ve çalışma dizininizin Uzak Masaüstü dosyası indirmek. Yeni bir Azure VM sağlamak, uzun zaman aldığından `while` kullanıma hazır olana kadar yeni VM Yoklama döngüsü devam eder.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Şimdi, etki alanı denetleyicisi sunucusuna başarıyla hazırlandı. Ardından, bu etki alanı denetleyicisi sunucusuna Active Directory etki alanı yapılandıracaksınız. Yerel bilgisayarınızda PowerShell penceresini kapatmayın. İki SQL Server Vm'leri oluşturmak için daha sonra tekrar kullanacaksınız.

## <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisi yapılandırma
1. Uzak Masaüstü dosyası başlatarak etki alanı denetleyicisi sunucusuna bağlanın. Makine yöneticisinin AzureAdmin kullanıcı adı ve parolası kullanmak **Contoso! 000**, yeni sanal makine oluştururken belirttiğiniz.
2. Yönetici modunda bir PowerShell penceresi açın.
3. Aşağıdaki komutu çalıştırın **DCPROMO. EXE** ayarlamak için komut **corp.contoso.com** M sürücüde veri dizinlerle etki alanı.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Komut bittikten sonra VM otomatik olarak yeniden başlatır.

4. Uzak Masaüstü dosyası başlatarak etki alanı denetleyicisi sunucusuna yeniden bağlanın. Bu süre olarak oturum **CORP\Administrator**.
5. Yönetici modunda bir PowerShell penceresi açın ve aşağıdaki komutu kullanarak Active Directory PowerShell modülü içeri aktarın:

        Import-Module ActiveDirectory

6. Üç kullanıcı etki alanına eklemek için aşağıdaki komutları çalıştırın.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** SQL Sunucu hizmeti örneği, yük devretme kümesinin ve kullanılabilirlik grubu için ilgili her şeyi yapılandırmak için kullanılır. **CORP\SQLSvc1** ve **CORP\SQLSvc2** SQL Server hizmet hesabı olarak iki SQL Server Vm'leri için kullanılır.
7. Ardından, vermek için aşağıdaki komutları çalıştırın **CORP\Install** etki alanında bilgisayar nesneleri oluşturma izni.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Yukarıda belirtilen GUID için bilgisayar nesnesi türünü GUID'dir. **CORP\Install** hesap gereksinimlerini **tüm özellikleri oku** ve **bilgisayar nesneleri oluşturma** yük devretme kümesi için etkin doğrudan nesneleri oluşturma izni. **Tüm özellikleri oku** izni zaten verildiğinde CORP\Install için varsayılan olarak, bu nedenle açıkça vermeniz gerekmez. Yük devretme kümesini oluşturmak için gereken izinler hakkında daha fazla bilgi için bkz. [yük devretme kümesi adım adım Kılavuzu: Active Directory'de hesapları yapılandırma](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Active Directory ve kullanıcı nesnelerinin yapılandırma bitirdikten sonra iki SQL Server Vm'leri oluşturacak ve bunları bu etki alanına ekleyin.

## <a name="create-the-sql-server-vms"></a>SQL Server Vm'leri oluşturma
1. Yerel bilgisayarınızda açık olan bir PowerShell penceresi kullanmaya devam edin. Aşağıdaki ek değişkenleri tanımlayın:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP adresi **10.10.0.4** genellikle oluşturduğunuz ilk VM'ye atanmış **10.10.0.0/16** Azure sanal ağınızın alt ağ. Bu etki alanı denetleyicisi sunucunuzun adresini çalıştırarak olduğunu doğrulamalısınız **IPCONFIG**.
2. Aşağıdaki yöneltilen adlı yük devretme kümesinde ilk VM oluşturmak için komutları çalıştırma **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Yukarıdaki komut ilişkin aşağıdakileri unutmayın:

   * **Yeni AzureVMConfig** istenen kullanılabilirlik kümesi adı ile bir sanal makine yapılandırması oluşturur. Sonraki VM'lerin aynı kullanılabilirlik kümesi adı ile aynı kullanılabilirlik kümesine katılır oluşturulan.
   * **Ekle-AzureProvisioningConfig** VM oluşturduğunuz Active Directory etki alanına katılır.
   * **Set-AzureSubnet** geri alt ağda VM yerleştirir.
   * **Yeni-AzureVM** yeni bir bulut hizmeti ve yeni bulut hizmetinde yeni bir Azure VM oluşturur. **DnsSettings** parametresinin belirttiği yeni bir bulut hizmeti sunucuları için DNS sunucusunun IP adresine sahiptir **10.10.0.4**. Etki alanı denetleyicisi sunucunun IP adresidir. Bu parametre, bulut hizmetine başarıyla Active Directory etki alanına yeni VM'lerin etkinleştirmek için gereklidir. Bu parametre olmadan, IPv4 ayarları VM sağlandıktan sonra etki alanı denetleyicisi sunucusuna birincil DNS sunucusu olarak kullanmak için VM'yi el ile ayarlamanız ve ardından VM Active Directory etki alanına gerekir.
3. Aşağıdaki yöneltilen adlı SQL Server Vm'leri oluşturmak için komutları çalıştırma **ContosoSQL1** ve **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Yukarıdaki komutlar ilişkin aşağıdakileri unutmayın:

   * **Yeni AzureVMConfig** aynı kullanılabilirlik kümesi adı etki alanı denetleyicisi sunucusu olarak ve sanal makine galerisinde SQL Server 2012 Service Pack 1 Enterprise Edition görüntüyü kullanır. Ayrıca okuma yalnızca önbelleğe alma için işletim sistemi diski ayarlar (hiçbir yazma önbelleğe alma). Veritabanı dosyalarını ayrı veri diski sanal Makineye eklediğiniz geçirme ve hiçbir okuma veya yazma önbelleği ile yapılandırma öneririz. Ancak, bir sonraki en iyi şey üzerinde işletim sistemi diski okuma önbelleğini kaldıramayacağınız, işletim sistemi disk üzerinde yazma önbelleği kaldırmaktır.
   * **Ekle-AzureProvisioningConfig** VM oluşturduğunuz Active Directory etki alanına katılır.
   * **Set-AzureSubnet** geri alt ağda VM yerleştirir.
   * **Ekle-AzureEndpoint** erişim uç noktaları ekler; böylece bu SQL Server Hizmetleri örnekleri Internet üzerindeki istemci uygulamalara erişebilirsiniz. Farklı bağlantı noktalarını ContosoSQL1 ve ContosoSQL2 verilir.
   * **Yeni-AzureVM** ContosoQuorum aynı bulut hizmetinde yeni SQL Server VM oluşturur. Aynı kullanılabilirlik kümesinde olmasını istiyorsanız aynı bulut hizmetinde VM'lerin yerleştirmeniz gerekir.
4. Tam olarak hazırlanmasını her VM için ve çalışma dizininizin kendi Uzak Masaüstü dosyası indirmek her bir VM için bekleyin. `for` Döngü döngüleri üç yeni VM'ler ve bunların her biri için en üst düzey süslü ayraçlar içindeki komutları yürütür.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    SQL Server Vm'leri artık sağlanır ve çalışıyor, ancak varsayılan seçeneklerle SQL Server ile birlikte yüklenen.

## <a name="initialize-the-failover-cluster-vms"></a>Yük devretme kümesi sanal makineleri başlatma
Bu bölümde, yük devretme kümesi ve SQL Server yüklemesi kullanacağınız üç sunucu değiştirmeniz gerekir. Bu avantajlar şunlardır:

* Tüm sunucular için: Yüklemeniz gereken **Yük Devretme Kümelemesi** özelliği.
* Tüm sunucular için: Eklemenize gerek **CORP\Install** makineyle **yönetici**.
* ContosoSQL1 ve yalnızca ContosoSQL2: Eklemenize gerek **CORP\Install** olarak bir **sysadmin** varsayılan veritabanı rolü.
* ContosoSQL1 ve yalnızca ContosoSQL2: Eklemenize gerek **NT AUTHORITY\SYSTEM** bir oturum açma aşağıdaki izinlere sahip olarak:

  * Herhangi bir kullanılabilirlik grubu Değiştir
  * SQL bağlanma
  * VIEW server state
* ContosoSQL1 ve yalnızca ContosoSQL2: **TCP** protokolünün SQL Server VM üzerinde zaten etkin. Ancak, yine de SQL Server'ın uzaktan erişim için güvenlik duvarını açmanız gerekir.

Şimdi başlamak hazırsınız. İle başlayarak **ContosoQuorum**, aşağıdaki adımları izleyin:

1. Bağlanma **ContosoQuorum** açarak Uzak Masaüstü dosyası. Makine yöneticisinin kullanıcı adı kullanma **AzureAdmin** ve parola **Contoso! 000**, Vm'leri oluştururken belirttiğiniz.
2. Bilgisayarlar başarıyla için alanına olduğunu doğrulayın **corp.contoso.com**.
3. SQL Server yüklemesi devam etmeden önce otomatik başlatma görevlerin tamamlanmasını bekleyin.
4. Yönetici modunda bir PowerShell penceresi açın.
5. Windows Yük Devretme Kümelemesi özelliğini yükleyin.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Ekleme **CORP\Install** yerel bir yönetici olarak.

        net localgroup administrators "CORP\Install" /Add
7. ContosoQuorum dışında oturum açın. Bu sunucu ile artık hazırsınız.

        logoff.exe

Ardından, başlatma **ContosoSQL1** ve **ContosoSQL2**. Aşağıdaki adımları izleyin, hem SQL Server Vm'leri için aynıdır.

1. Uzak Masaüstü dosyası başlatarak iki SQL Server Vm'leri için bağlanın. Makine yöneticisinin kullanıcı adı kullanma **AzureAdmin** ve parola **Contoso! 000**, Vm'leri oluştururken belirttiğiniz.
2. Bilgisayarlar başarıyla için alanına olduğunu doğrulayın **corp.contoso.com**.
3. SQL Server yüklemesi devam etmeden önce otomatik başlatma görevlerin tamamlanmasını bekleyin.
4. Yönetici modunda bir PowerShell penceresi açın.
5. Windows Yük Devretme Kümelemesi özelliğini yükleyin.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Ekleme **CORP\Install** yerel bir yönetici olarak.

        net localgroup administrators "CORP\Install" /Add
7. SQL Server PowerShell sağlayıcısını içeri aktarın.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Ekleme **CORP\Install** için varsayılan SQL Server örneğinde sysadmin rolüne olarak.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. Ekleme **NT AUTHORITY\SYSTEM** olarak bir oturum açma yukarıda açıklanan üç izinlere sahip.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. SQL Server'ın uzaktan erişim için Güvenlik Duvarı'nı açın.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Her iki VM dışında oturum açın.

         logoff.exe

Son olarak, kullanılabilirlik grubu yapılandırma hazırsınız. SQL Server PowerShell sağlayıcısını tüm işlemleri gerçekleştirmek için kullanacağınız **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>Kullanılabilirlik grubu yapılandırma
1. Bağlanma **ContosoSQL1** yeniden başlatarak Uzak Masaüstü dosyası tarafından. Yerine makine hesabını kullanarak oturum açma kullanarak oturum açın **CORP\Install**.
2. Yönetici modunda bir PowerShell penceresi açın.
3. Aşağıdaki değişkenleri tanımlayın:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. SQL Server PowerShell sağlayıcısını içeri aktarın.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. SQL Server hizmet hesabı için ContosoSQL1 CORP\SQLSvc1 için değiştirin.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. SQL Server hizmet hesabı için ContosoSQL2 CORP\SQLSvc2 için değiştirin.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. İndirme **CreateAzureFailoverCluster.ps1** gelen [Always On kullanılabilirlik gruplarının Azure VM'de yük devretme kümesi oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) için yerel çalışma dizini. İşlevsel bir yük devretme kümesi oluşturmanıza yardımcı olması için bu betiği kullanacaksınız. Windows Yük Devretme Kümelemesi Azure ağı ile nasıl etkileştiğini önemli bilgiler için bkz: [Azure sanal Makineler'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Çalışma dizinine ve indirilen komut dosyası ile yük devretme kümesi oluşturun.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. Always On kullanılabilirlik grupları için varsayılan SQL Server örneklerini etkinleştirileceği **ContosoSQL1** ve **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. Yedekleme dizini oluşturma ve SQL Server hizmet hesapları için izinleri verin. Bu dizin, bir kullanılabilirlik veritabanı ikincil Çoğaltmada hazırlamak için kullanacaksınız.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Bir veritabanı oluşturma **ContosoSQL1** adlı **MyDB1**, hem tam yedekleme hem de bir günlük yedeklemesi alın ve bunları geri **ContosoSQL2** ile **WITH NORECOVERY**  seçeneği.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. Kullanılabilirlik grubu uç noktaları üzerinde SQL Server Vm'leri oluşturma ve uç noktalara uygun izinleri ayarlayın.

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2
13. Kullanılabilirlik çoğaltmaları oluşturun.

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. Son olarak, kullanılabilirlik grubunu oluşturma ve ikincil çoğaltma kullanılabilirlik grubuna katılın.

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a>Sonraki adımlar
Artık başarıyla SQL Server Always On kullanılabilirlik grubu Azure'da oluşturarak uyguladık. Bu kullanılabilirlik grubu için bir dinleyici yapılandırma için bkz: [Azure'da AlwaysOn Kullanılabilirlik grupları için ILB dinleyicisi yapılandırma](../classic/ps-sql-int-listener.md).

Azure'da SQL Server'ı kullanma hakkında diğer bilgiler için bkz. [Azure sanal makineler'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
