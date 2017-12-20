---
title: "PowerShell kullanarak Azure VM temelinde Always On kullanılabilirlik grubu yapılandırma | Microsoft Docs"
description: "Bu öğretici Klasik dağıtım modeliyle oluşturulan kaynakları kullanır. Always On kullanılabilirlik grubu oluşturmak için PowerShell kullanın."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b99cf767fb931d3f7fe14fcbe7990126244613ed
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a>PowerShell ile Azure VM'de Always On kullanılabilirlik grubu yapılandırma
> [!div class="op_single_selector"]
> * [Klasik: kullanıcı Arabirimi](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasik: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Başlamadan önce Azure resource manager modelinde bu görevi tamamlamak olduğunu göz önünde bulundurun. Azure resource manager modeli yeni dağıtımlar için öneririz. Bkz: [SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> En yeni dağıtımların Resource Manager modelini kullanmasını öneririz. Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir.

Azure sanal makineleri (VM'ler) yüksek oranda kullanılabilir SQL Server sistem maliyetini düşürmek için veritabanı yöneticilerine yardımcı olabilir. Bu öğretici bir kullanılabilirlik grubu SQL Server Always On uçtan uca bir Azure ortamı içindeki kullanarak uygulama gösterilmektedir. Öğreticinin sonunda Azure, SQL Server Always On çözümünüz aşağıdaki öğelerden oluşur:

* Bir ön uç dahil olmak üzere birden çok alt ağı ve bir arka uç alt ağ içeren bir sanal ağ.
* Bir Active Directory etki alanı ile etki alanı denetleyicisi.
* Arka uç alt ağına dağıtılan ve Active Directory etki alanına katılan iki SQL Server Vm'lerinin.
* Üç düğümlü Windows Yük devretme kümesi düğüm çoğunluğu çekirdek modeli.
* Bir kullanılabilirlik veritabanının bir kullanılabilirlik grubu iki synchronous-commit çoğaltmalarından ile.

Bu senaryo olduğunda, düşük maliyet veya diğer etkenlere bağlı için değil, Azure üzerinde kendi Basitlik için iyi bir seçimdir. Örneğin, iki düğümlü yük devretme kümesinde çekirdek dosya paylaşım tanığı olarak etki alanı denetleyicisini kullanarak Azure işlem saatleri kaydetmek iki çoğaltma kullanılabilirlik grubu için VM sayısını en aza indirebilirsiniz. Bu yöntem, yukarıdaki yapılandırma birinden tarafından VM sayısını azaltır.

Bu öğretici, her bir adımın ayrıntıları elaborating olmadan, yukarıda açıklanan çözüm ayarlamak için gerekli olan adımları göstermek için tasarlanmıştır. Bu nedenle, GUI yapılandırma adımlarını sağlayarak yerine her adımda hızlı bir şekilde almak için komut dosyası PowerShell kullanır. Bu öğretici aşağıdaki varsayılır:

* Sanal makine abone olan bir Azure hesabı zaten var.
* Yüklediğiniz [Azure PowerShell cmdlet'lerini](/powershell/azure/overview).
* Always On kullanılabilirlik grupları şirket içi çözümler için sağlam bir anlayış zaten var. Daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Azure aboneliğinize bağlanmak ve sanal ağ oluşturma
1. Bir PowerShell penceresinde, yerel bilgisayarınızda Azure modülünü içeri aktarın, yayımlama ayarları dosyası makinenize indirmek ve indirilen yayımlama ayarlarını içeri aktararak, PowerShell oturumunuz Azure aboneliğinize bağlanmak.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    **Get-AzurePublishSettingsFile** komutu otomatik olarak Azure yönetim sertifikasıyla oluşturur ve makinenize indirir. Bir tarayıcı otomatik olarak açılır ve Azure aboneliğiniz için Microsoft hesap kimlik bilgilerini girmeniz istenir. İndirilen **.publishsettings** dosyası Azure aboneliğinizi yönetmek için gereken tüm bilgileri içerir. Bu dosya yerel bir dizine kaydedildikten sonra bunu kullanarak içeri **Import-AzurePublishSettingsFile** komutu.

   > [!NOTE]
   > .Publishsettings dosyasını Azure Abonelikleriniz ve hizmetleri yönetmek için kullanılan (kodlanmamış), kimlik bilgilerini içerir. Geçici olarak kaynak dizinlerinizi (örneğin, Libraries\Documents klasöründe) dışında depolama ve alma işlemi tamamlandıktan sonra silmek için bu dosya için en iyi güvenlik uygulaması değil. .Publishsettings dosyasını erişim kazanır kötü niyetli bir kullanıcı düzenleme, oluşturma ve Azure hizmetlerinizi silin.

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

    Komutlarınızı daha sonra başarılı olmak için aşağıdakilere dikkat edin:

   * Değişkenleri **$storageAccountName** ve **$dcServiceName** bulut depolama hesabı ve bulut sunucunuzu, Internet'te sırasıyla tanımlamak için kullanılırlar çünkü benzersiz olması gerekir.
   * Değişkenleri belirtir adları **$affinityGroupName** ve **$virtualNetworkName** daha sonra kullanacağınız sanal ağ yapılandırması belgede yapılandırılır.
   * **$sqlImageName** , SQL Server 2012 Service Pack 1 Enterprise Edition içeren VM görüntüsü güncelleştirilmiş adını belirtir.
   * Basitleştirmek için **Contoso! 000** tüm Öğreticisi kullanılan aynı parola.

3. Bir benzeşim grubu oluşturun.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Bir yapılandırma dosyasını içeri aktararak bir sanal ağ oluşturun.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Yapılandırma dosyasında aşağıdaki XML belgesi içeriyor. Kısaca, adlı bir sanal ağ belirtir **ContosoNET** adlı benzeşim grubundaki **ContosoAG**. Adres alanı sahip **10.10.0.0/16** ve iki alt ağa sahip **10.10.1.0/24** ve **10.10.2.0/24**, olduğu alt ağ ön ve arka alt ağ, sırasıyla. Burada, Microsoft SharePoint gibi istemci uygulamalarını yerleştirebilirsiniz ön alt ağıdır. SQL Server Vm'lerinin nereye geri alt ağıdır. Değiştirirseniz **$affinityGroupName** ve **$virtualNetworkName** değişkenleri daha önce de aşağıdaki karşılık gelen adları değiştirmeniz gerekir.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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

5. Oluşturulan ve aboneliğinizde geçerli depolama hesabı olarak ayarlayın benzeşim grubuyla ilişkili bir depolama hesabı oluşturun.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Etki alanı denetleyicisi sunucusuna yeni bulut hizmeti ve kullanılabilirlik kümesinde oluşturun.

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

    Bu piped komutları şunları yapın:

   * **AzureVMConfig yeni** bir VM yapılandırması oluşturur.
   * **Ekleme AzureProvisioningConfig** bir tek başına Windows server'ın yapılandırma parametrelerini sağlar.
   * **Ekleme AzureDataDisk** None olarak ayarlanmış önbelleğe alma seçeneği ile Active Directory verilerini depolamak için kullanacağınız veri diski ekler.
   * **Yeni-AzureVM** yeni bir bulut hizmeti ve yeni bir Azure VM yeni bulut hizmeti oluşturur.

7. Yeni VM tamamen sağlanması için bekleyin ve çalışma dizininizi Uzak Masaüstü dosyası indirilemedi. Yeni Azure VM sağlamak için uzun bir süre aldığından `while` kullanıma hazır olana kadar yeni VM Yoklama döngüsü devam eder.

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

Etki alanı denetleyici sunucusu artık başarıyla kaynak sağlandı. Ardından, bu etki alanı denetleyicisi sunucu üzerinde Active Directory etki alanı yapılandıracaksınız. Yerel bilgisayarınızda PowerShell penceresini açık bırakın. İki SQL Server VM oluşturmak için daha sonra yeniden kullanacaksınız.

## <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisini Yapılandır
1. Uzak Masaüstü dosyası başlatarak etki alanı denetleyicisi sunucusuna bağlanın. Makine yöneticisinin AzureAdmin kullanıcı adı ve parolası kullanmak **Contoso! 000**, yeni VM oluştururken belirttiğiniz.
2. Yönetici modunda bir PowerShell penceresi açın.
3. Aşağıdaki komutu çalıştırarak **DCPROMO. EXE** ayarlamak için komut **corp.contoso.com** M sürücüsü veri dizinleri ile etki alanı.

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

    Komut bittikten sonra VM otomatik olarak yeniden başlatılır.

4. Etki alanı denetleyicisi sunucusuna Uzak Masaüstü dosyası başlatarak yeniden bağlanın. Bu süre olarak oturum açın **CORP\Administrator**.
5. Yönetici modunda bir PowerShell penceresi açın ve aşağıdaki komutu kullanarak Active Directory PowerShell modülünü içeri aktarın:

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

    **CORP\Install** SQL Sunucu hizmeti örneği, yük devretme kümesinin ve kullanılabilirlik grubu ile ilgili her şeyi yapılandırmak için kullanılır. **CORP\SQLSvc1** ve **CORP\SQLSvc2** iki SQL Server VM'ler için SQL Server hizmet hesabı olarak kullanılır.
7. Ardından, vermek için aşağıdaki komutları çalıştırın **CORP\Install** etki alanında bilgisayar nesneleri oluşturma izni.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Yukarıda belirtilen GUID, bilgisayar nesnesi türü için GUID'dir. **CORP\Install** hesap gereksinimlerini **tüm özellikleri oku** ve **bilgisayar nesneleri oluşturma** yük devretme kümesi için etkin doğrudan nesneleri oluşturma izni. **Tüm özellikleri oku** izni zaten verilen CORP\Install için varsayılan olarak, açıkça vermeniz gerekmez. Yük devretme kümesi oluşturmak için gereken izinler hakkında daha fazla bilgi için bkz: [yük devretme kümesi adım adım Kılavuzu: Active Directory hesaplarını yapılandırma](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Active Directory ve kullanıcı nesneleri yapılandırma bitirdikten sonra iki SQL Server Vm'lerinin oluşturacak ve bu etki alanına ekleyin.

## <a name="create-the-sql-server-vms"></a>SQL Server Vm'lerinin oluşturma
1. Yerel bilgisayarınızda açık PowerShell penceresi kullanmaya devam edin. Aşağıdaki ek değişkenleri tanımlayın:

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

    IP adresi **10.10.0.4** oluşturduğunuz ilk VM genellikle atandığı **10.10.0.0/16** Azure sanal ağınızın alt ağ. Bu etki alanı denetleyicisi sunucunuzun adresini çalıştırarak olduğunu doğrulamalısınız **IPCONFIG**.
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

    Yukarıdaki komut ilgili olarak aşağıdakileri unutmayın:

   * **AzureVMConfig yeni** istenen kullanılabilirlik kümesi adı ile bir VM yapılandırması oluşturur. Sonraki sanal makineleri aynı kullanılabilirlik kümesi adı ile aynı kullanılabilirlik kümesine birleşik oluşturulan.
   * **Ekleme AzureProvisioningConfig** VM oluşturduğunuz Active Directory etki alanına katılır.
   * **Set-AzureSubnet** geri alt ağda VM yerleştirir.
   * **Yeni-AzureVM** yeni bir bulut hizmeti ve yeni bir Azure VM yeni bulut hizmeti oluşturur. **DnsSettings** parametresi belirtir yeni bulut hizmeti sunucuları için DNS sunucusu IP adresi olduğunu **10.10.0.4**. Etki alanı denetleyicisi sunucunun IP adresidir. Bu parametre, başarılı bir şekilde Active Directory etki alanına katılmak bulut hizmetinde yeni VM'ler etkinleştirmek için gereklidir. Bu parametre olmadan, VM sağlandıktan sonra etki alanı denetleyici sunucusu birincil DNS sunucusu olarak kullanmak için VM IPv4 ayarları el ile ayarlayın ve ardından VM Active Directory etki alanına gerekir.
3. Aşağıdaki yöneltilen adlı SQL Server VM'ler oluşturmak için komutları çalıştırma **ContosoSQL1** ve **ContosoSQL2**.

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

    Yukarıdaki komutlarda ilgili olarak aşağıdakileri unutmayın:

   * **AzureVMConfig yeni** aynı kullanılabilirlik kümesi adı etki alanı denetleyici sunucusu ve SQL Server 2012 Service Pack 1 Enterprise Edition görüntüsü sanal makineye Galerisi kullanır. Okuma yalnızca önbelleğe alma için aynı zamanda işletim sistemi diski ayarlar (hiçbir yazma önbelleğini). Veritabanı dosyalarını VM'e ekleyin ayrı veri diski geçirmek ve hiçbir okuma veya yazma önbelleği ile yapılandırma öneririz. Ancak, sonraki en iyi şey işletim sistemi disk üzerinde okuma önbelleği kaldırılamıyor çünkü işletim sistemi diskte yazma önbelleğini kaldırmaktır.
   * **Ekleme AzureProvisioningConfig** VM oluşturduğunuz Active Directory etki alanına katılır.
   * **Set-AzureSubnet** geri alt ağda VM yerleştirir.
   * **Ekleme AzureEndpoint** istemci uygulamalarının bu SQL Server Hizmetleri örnekler Internet'te erişebilmesi için erişim uç noktaları ekler. Farklı bağlantı noktaları ContosoSQL1 ve ContosoSQL2 verilir.
   * **Yeni-AzureVM** ContosoQuorum aynı bulut hizmetindeki yeni SQL Server VM oluşturur. Bunları aynı kullanılabilirlik kümesinde olmasını istiyorsanız, sanal makineleri aynı bulut hizmetinde yerleştirmeniz gerekir.
4. Her VM çalışma dizininizi, Uzak Masaüstü dosyası indirilemedi ve tamamen sağlanması her bir VM için bekleyin. `for` Döngü döngüleri üç yeni VM'ler ve bunların her biri için en üst düzey süslü ayraçlar içindeki komutları yürütür.

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

    SQL Server Vm'lerinin şimdi sağlanır ve çalışıyor, ancak bunlar varsayılan seçeneklerle SQL Server ile birlikte yüklenen.

## <a name="initialize-the-failover-cluster-vms"></a>Yük devretme kümesi sanal makineleri Başlat
Bu bölümde, yük devretme kümesi ve SQL Server yüklemesi kullanacağınız üç sunucu değiştirmeniz gerekir. Bu avantajlar şunlardır:

* Tüm sunucuları: yüklemenize gerek **Yük Devretme Kümelemesi** özelliği.
* Tüm sunucuları: eklemenize gerek **CORP\Install** makine olarak **yönetici**.
* ContosoSQL1 ve yalnızca ContosoSQL2: eklemenize gerek **CORP\Install** olarak bir **sysadmin** varsayılan veritabanı rolü.
* ContosoSQL1 ve yalnızca ContosoSQL2: eklemenize gerek **NT AUTHORITY\SYSTEM** bir oturum açma ve aşağıdaki izinlerle olarak:

  * Herhangi bir kullanılabilirlik grubu Değiştir
  * SQL bağlantı
  * VIEW server state
* ContosoSQL1 ve yalnızca ContosoSQL2: **TCP** Protokolü SQL Server VM üzerinde zaten etkin. Ancak, yine SQL Server'ın uzaktan erişim için Güvenlik Duvarı'nı açmak gerekir.

Artık başlamaya hazırsınız. İle başlayarak **ContosoQuorum**, aşağıdaki adımları izleyin:

1. Bağlanmak **ContosoQuorum** Uzak Masaüstü dosyaları başlatmasını tarafından. Makine yöneticisinin kullanıcı adı kullanma **AzureAdmin** ve parola **Contoso! 000**, sanal makineleri oluştururken belirttiğiniz.
2. Bilgisayarlar başarıyla için birleştirilmiş olduğunu doğrulayın **corp.contoso.com**.
3. SQL Server yüklemesi otomatik başlatma görevlerini devam etmeden önce tamamlanmasını bekleyin.
4. Yönetici modunda bir PowerShell penceresi açın.
5. Windows Yük Devretme Kümelemesi özelliğini yükleyin.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Ekleme **CORP\Install** yerel yönetici olarak.

        net localgroup administrators "CORP\Install" /Add
7. ContosoQuorum dışında oturum açın. Bu sunucu ile artık bitirdiniz.

        logoff.exe

Ardından, başlatma **ContosoSQL1** ve **ContosoSQL2**. Aşağıdaki iki SQL Server VM'ler için aynı adımları izleyin.

1. Uzak Masaüstü dosyaları başlatarak iki SQL Server VM bağlayın. Makine yöneticisinin kullanıcı adı kullanma **AzureAdmin** ve parola **Contoso! 000**, sanal makineleri oluştururken belirttiğiniz.
2. Bilgisayarlar başarıyla için birleştirilmiş olduğunu doğrulayın **corp.contoso.com**.
3. SQL Server yüklemesi otomatik başlatma görevlerini devam etmeden önce tamamlanmasını bekleyin.
4. Yönetici modunda bir PowerShell penceresi açın.
5. Windows Yük Devretme Kümelemesi özelliğini yükleyin.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Ekleme **CORP\Install** yerel yönetici olarak.

        net localgroup administrators "CORP\Install" /Add
7. SQL Server PowerShell sağlayıcısı içeri aktarın.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Ekleme **CORP\Install** varsayılan SQL Server örneğinde sysadmin rolüne olarak.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. Ekleme **NT AUTHORITY\SYSTEM** bir oturum açma yukarıda açıklanan üç izinlerle olarak.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. SQL Server'ın uzaktan erişim için Güvenlik Duvarı'nı açın.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Her iki VM dışında oturum açın.

         logoff.exe

Son olarak, kullanılabilirlik grubu yapılandırmak hazırsınız. SQL Server PowerShell sağlayıcısı çalışmanın tümünü gerçekleştirmek için kullanacağınız **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>Kullanılabilirlik grubu yapılandırma
1. Bağlanmak **ContosoSQL1** Uzak Masaüstü dosyaları başlatmasını tarafından yeniden. Makine hesabı kullanarak oturum açmak yerine, kullanarak oturum açın **CORP\Install**.
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
4. SQL Server PowerShell sağlayıcısı içeri aktarın.

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
7. Karşıdan **CreateAzureFailoverCluster.ps1** gelen [Always On kullanılabilirlik grupları Azure VM'de yük devretme kümesi oluşturma](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) yerel çalışma dizinine. Bir işlev yük devretme kümesi oluşturmanıza yardımcı olması için bu betiği kullanırsınız. Windows Yük Devretme Kümelemesi Azure ağ ile nasıl etkileşim önemli bilgiler için bkz: [Azure Virtual Machines'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Çalışma dizininizi değiştirebilir ve yük devretme kümesi ile indirilen komut dosyası oluşturabilirsiniz.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. Always On kullanılabilirlik grupları varsayılan SQL Server örnekleri için etkinleştirmek **ContosoSQL1** ve **ContosoSQL2**.

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
10. Bir yedekleme dizini oluşturmak ve SQL Server hizmeti hesapları için izinler verebilirsiniz. Bu dizin, ikincil çoğaltma kullanılabilirlik veritabanını hazırlamak için kullanacağız.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Bir veritabanı oluşturma **ContosoSQL1** adlı **MyDB1**, hem tam yedekleme hem de bir günlük yedeklemesi alın ve bunları geri **ContosoSQL2** ile **WITH NORECOVERY** seçeneği.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. Kullanılabilirlik grubu uç SQL Server sanal makinelerin oluşturun ve uygun izinlere uç noktalarda ayarlayın.

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
13. Kullanılabilirlik çoğaltmalarının oluşturun.

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
14. Son olarak, kullanılabilirlik grubu oluşturun ve ikincil çoğaltma kullanılabilirlik grubuna katılın.

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
Artık başarıyla SQL Server Always On Azure'da bir kullanılabilirlik grubu oluşturarak uyguladık. Bu kullanılabilirlik grubu için bir dinleyici yapılandırmak için bkz: [Azure'da Always On kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın](../classic/ps-sql-int-listener.md).

Azure'da SQL Server kullanma hakkında diğer bilgi için bkz: [Azure virtual machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
