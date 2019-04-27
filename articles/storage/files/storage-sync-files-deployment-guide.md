---
title: Azure dosya eşitleme işlemi dağıtma | Microsoft Docs
description: Baştan sona Azure dosya eşitleme dağıtmayı öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: a5d6f7757b5a4a5a2048c9822c4f52bee6d19bfe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444069"
---
# <a name="deploy-azure-file-sync"></a>Azure Dosya Eşitleme’yi dağıtma
Kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için Azure dosya eşitleme'yi kullanın. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS gibi verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gereken sayıda önbellek olabilir.

Okumanızı öneririz [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) ve [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) önce bu makalede açıklanan adımları tamamlayın.

## <a name="prerequisites"></a>Önkoşullar
* Azure dosya eşitleme dağıtmak istediğiniz bölgede bir Azure depolama hesabı ve bir Azure dosya paylaşın. Daha fazla bilgi için bkz.
    - [Bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability) Azure dosya eşitleme için.
    - [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) bir depolama hesabı oluşturma adım adım bir açıklaması.
    - [Dosya paylaşımı oluşturma](storage-how-to-create-file-share.md) bir dosya paylaşımı oluşturma adım adım bir açıklaması.
* Azure dosya eşitleme ile eşitlenecek Windows Server veya Windows Server kümesinin desteklenen en az bir örnek. Windows Server'ın desteklenen sürümleri hakkında daha fazla bilgi için bkz. [Windows Server ile birlikte çalışabilirlik](storage-sync-files-planning.md#azure-file-sync-system-requirements-and-interoperability).
* PowerShell 5.1, Windows Server'da yüklü emin olun. Windows Server 2012 R2 kullanıyorsanız, en az çalıştırdığınızdan emin olun. PowerShell 5.1. \*. PowerShell 5.1 varsayılan sürümü kullanıma hazır olduğu gibi güvenli bir şekilde Windows Server 2016 üzerinde bu denetimi atlayabilirsiniz. Windows Server 2012 R2 üzerinde PowerShell 5.1 çalıştığını doğrulayabilirsiniz. \* değerinde bakarak **PSVersion** özelliği **$PSVersionTable** nesnesi:

    ```powershell
    $PSVersionTable.PSVersion
    ```

    Küçüktür 5.1 PSVersion değeriniz varsa. \*olarak olacaktır Windows Server 2012 R2'in en yeni yüklemeler çalışmasıyla, indirme ve yükleme kolayca yükseltebilirsiniz [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). İndirmek ve Windows Server 2012 R2 için yüklemek için uygun paket **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

    > [!Note]  
    > Azure dosya eşitleme henüz PowerShell 6 + Windows Server 2012 R2 veya Windows Server 2016'yı desteklemez.
* Az ve AzureRM PowerShell modülü.
    - Buradaki yönergeleri izleyerek Az modül yüklenebilir: [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-Az-ps). 
    - AzureRM PowerShell modülü, aşağıdaki PowerShell cmdlet'ini çalıştırarak yüklenebilir:
    
        ```powershell
        Install-Module AzureRM
        ```

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Windows Server’ı Azure Dosya Eşitleme ile kullanmaya hazırlama
Yük devretme kümesinde, her sunucu düğümü dahil olmak üzere, Azure dosya eşitleme ile kullanmayı planladığınız her sunucu için devre dışı **Internet Explorer Artırılmış Güvenlik Yapılandırması**. Bu, yalnızca ilk sunucu kaydı için gereklidir. Sunucu kaydedildikten sonra özelliği yeniden etkinleştirebilirsiniz.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Sunucu Yöneticisi'ni açın.
2. Tıklayın **yerel sunucu**:  
    !["Yerel sunucuda" sol tarafındaki Sunucu Yöneticisi kullanıcı Arabirimi](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. **Özellikler** alt bölmesinde **IE Artırılmış Güvenlik Yapılandırması** bağlantısını seçin.  
    ![Sunucu Yöneticisi kullanıcı arabiriminde "IE Artırılmış Güvenlik Yapılandırması" bölmesi](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. İçinde **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda **kapalı** için **Yöneticiler** ve **kullanıcılar**:  
    ![Internet Explorer Artırılmış Güvenlik Yapılandırması pop penceresi "Kapalı" ile işaretli](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Internet Explorer Artırılmış Güvenlik Yapılandırması devre dışı bırakmak için yükseltilmiş bir PowerShell oturumunda aşağıdakileri yürütün:

```powershell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Microsoft Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="deploy-the-storage-sync-service"></a>Depolama Eşitleme Hizmetini Dağıtma 
Yerleştirme ile Azure dosya eşitleme dağıtımı başlatır bir **depolama eşitleme hizmeti** seçili aboneliğinizin bir kaynak grubuna kaynak. Birkaç bu gerektiğinde sağlama öneririz. Sunucularınızın ve bu kaynak arasında bir güven ilişkisi oluşturmanız gerekir ve bir sunucu yalnızca bir depolama eşitleme hizmeti kaydedilebilir. Sonuç olarak, sunucu grupları ayırmak gerektiği kadar depolama Eşitleme Hizmetleri dağıtmak için önerilir. Farklı depolama Eşitleme Hizmetleri sunuculardan birbirleri ile eşitleme yapamıyor aklınızda bulundurun.

> [!Note]
> Depolama eşitleme hizmeti abonelik ve kaynak grubu içinde dağıtıldı erişim izinleri devralınan. Kimlerin erişimi olduğunu dikkatli bir şekilde denetlemenizi öneririz. Yazma erişimine sahip varlıklar, yeni ayarlar dosyası için kayıtlı sunucuların eşitleme başlatabilirsiniz bu depolama eşitleme hizmeti ve veri akışı için erişilebilir olan Azure depolama neden olur.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Depolama eşitleme hizmeti dağıtmak için Git [Azure portalında](https://portal.azure.com/), tıklayın *kaynak Oluştur* ve sonra Azure dosya eşitleme için arama yapın. Arama sonuçlarında seçin **Azure dosya eşitleme**ve ardından **Oluştur** açmak için **dağıtma depolama eşitleme** sekmesi.

Açılan bölmeye aşağıdaki bilgileri girin:

- **Ad**: Depolama Eşitleme Hizmeti için benzersiz bir ad (abonelik başına).
- **Abonelik**: Depolama eşitleme hizmetini oluşturmak istediğiniz aboneliği. Kuruluşunuzun yapılandırma stratejisi bağlı olarak, bir veya daha fazla abonelik erişimi olabilir. Bir Azure aboneliği (örneğin, Azure dosyaları) her bir bulut hizmeti için fatura bilgilerini en temel kapsayıcıdır.
- **Kaynak grubu**: Bir kaynak grubu, bir depolama hesabı veya bir depolama eşitleme hizmeti gibi Azure kaynaklarının mantıksal bir gruptur. Yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu, Azure dosya eşitleme için kullanın. (Kaynak grupları kapsayıcıları olarak ik veya belirli bir proje için kaynaklar gruplandırma gibi mantıksal olarak kuruluşunuz için kaynakları ayırmak için kullanmanızı öneririz.)
- **Konum**: Azure dosya eşitleme'ı dağıtmak istediğiniz bölgeyi. Bu listede yalnızca desteklenen bölgeler kullanılabilir.

İşlemi tamamladığınızda, seçin **Oluştur** depolama eşitleme hizmeti dağıtmak için.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Azure dosya eşitleme yönetim Cmdlet'lerine etkileşim önce DLL içeri ve bir Azure dosya eşitleme yönetim bağlamına oluşturmak gerekir. Azure dosya eşitleme Yönetimi cmdlet'leri henüz Azure PowerShell modüllerinin bir parçası olmadığı için bu gereklidir.

> [!Note]  
> Azure dosya eşitleme Yönetimi cmdlet'leri içeren StorageSync.Management.PowerShell.Cmdlets.dll paket (bilerek) onaylanmamış bir fiil bir cmdlet içerir (`Login`). Adı `Login-AzureStorageSync` eşleşecek şekilde seçilmiştir `Login-AzAccount` cmdlet diğer Azure PowerShell modülü. Azure dosya eşitleme aracısının Azure PowerShell modülü eklendiğinde, bu hata iletisi (ve cmdlet) kaldırılır.

```powershell
$acctInfo = Login-AzAccount

# The location of the Azure File Sync Agent. If you have installed the Azure File Sync 
# agent to a non-standard location, please update this path.
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzResourceGroup -Name $resourceGroup -Location $region
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    -SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region
```

Azure dosya eşitleme bağlamı ile oluşturduktan sonra `Login-AzureRmStorageSync` cmdlet, depolama eşitleme hizmeti oluşturabilirsiniz. Değiştirdiğinizden emin olun `<my-storage-sync-service>` istenen, depolama eşitleme hizmeti adı.

```powershell
$storageSyncName = "<my-storage-sync-service>"
New-AzureRmStorageSyncService -StorageSyncServiceName $storageSyncName
```

---

## <a name="install-the-azure-file-sync-agent"></a>Azure Dosya Eşitleme aracısını yükleme
Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Aracısı'ndan indirebileceğiniz [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Yükleme tamamlandığında, Azure dosya eşitleme aracı yüklemesini başlatmak için MSI paketini çift tıklayın.

> [!Important]  
> Azure dosya eşitleme ile bir yük devretme kümesi kullanmayı planlıyorsanız, kümedeki her düğümde Azure dosya eşitleme aracısının yüklenmesi gerekir. Kümedeki her düğümün Azure dosya eşitleme ile çalışmak için kaydetmiş olmalıdır.

Şunları yapmanız önerilir:
- Sorun giderme ve sunucu bakımı kolaylaştırmak için varsayılan yükleme yolu (C:\Program Files\Azure\StorageSyncAgent) bırakın.
- Azure dosya eşitleme güncel tutmak için Microsoft Update'i etkinleştir. Tüm güncelleştirmeleri, özellik güncelleştirmeleri ve düzeltmeleri dahil olmak üzere, Azure dosya eşitleme aracısı için Microsoft Update sitesinden oluşur. Azure dosya eşitleme için en son güncelleştirmeyi yüklemeniz önerilir. Daha fazla bilgi için [Azure dosya eşitleme güncelleştirme ilkesi](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Azure dosya eşitleme Aracısı yükleme tamamlandığında, sunucu kayıt UI otomatik olarak açılır. Kaydetmeden önce bir depolama eşitleme hizmeti olması gerekir; Depolama eşitleme hizmeti oluşturma konusunda sonraki bölüme bakın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Azure dosya eşitleme aracısının işletim sisteminiz için uygun sürümünü indirin ve sisteminizde yüklemek için şu PowerShell kodunu yürütün.

> [!Important]  
> Azure dosya eşitleme ile bir yük devretme kümesi kullanmayı planlıyorsanız, kümedeki her düğümde Azure dosya eşitleme aracısının yüklenmesi gerekir. Kümedeki her düğümün Azure dosya eşitleme ile çalışmak için kaydetmiş olmalıdır.

```powershell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875004 `
        -OutFile "StorageSyncAgent.exe" 
}
elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875002 `
        -OutFile "StorageSyncAgent.exe" 
}
else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2 and Windows Server 2016")
}

# Extract the MSI from the install package
$tempFolder = New-Item -Path "afstemp" -ItemType Directory
Start-Process -FilePath ".\StorageSyncAgent.exe" -ArgumentList "/C /T:$tempFolder" -Wait

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "$($tempFolder.FullName)\StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.exe", ".\afstemp" -Recurse -Force
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server’ı Depolama Eşitleme Hizmetine kaydetme
Windows Server’ı bir Depolama Eşitleme Hizmeti’ne kaydetmek, sunucunuz (veya kümeniz) ile Depolama Eşitleme Hizmeti arasında bir güven ilişkisi kurar. Bir sunucu yalnızca bir Depolama Eşitleme Hizmeti’ne kaydedilebilir ve aynı Depolama Eşitleme Hizmeti ile ilişkili diğer sunucular ve Azure dosya paylaşımları ile eşitlenebilir.

> [!Note]
> Sunucu kaydı depolama eşitleme hizmeti, Windows server oluşturur ve sunucunun kayıtlı kalır sürece geçerli olan kendi kimliğini kullanır ancak sonradan sunucusu arasında bir güven ilişkisi oluşturmak için Azure kimlik bilgilerinizi kullanır ve Geçerli paylaşılan erişim imzası (depolama SAS) belirteci geçerli değil. Sunucu, bu nedenle sunucunun herhangi bir Eşitleme durduruluyor Azure dosya paylaşımlarınızın erişme olanağını kaldırma, kaydı olduğunda yeni bir SAS belirteci sunucuya yayınlanamıyor.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Sunucu kaydı UI otomatik olarak Azure dosya eşitleme Aracısı yüklendikten sonra açmanız gerekir. Seçili değilse bunu dosya konumundan el ile açabilirsiniz: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Sunucu kaydı UI açıldığında seçin **oturum** başlamak için.

Oturum açtıktan sonra aşağıdaki bilgileri girmeniz istenir:

![Sunucu Kaydı kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure aboneliği**: Depolama eşitleme hizmeti içeren aboneliği (bkz [depolama eşitleme hizmeti dağıtma](#deploy-the-storage-sync-service)). 
- **Kaynak grubu**: Depolama eşitleme hizmeti içeren bir kaynak grubu.
- **Depolama eşitleme hizmeti**: Kaydetmek istediğiniz depolama eşitleme hizmeti adı.

Uygun bilgileri seçtikten sonra seçin **kaydetme** sunucu kaydını tamamlamak için. Kayıt işleminin bir parçası olarak bir kez daha oturum açmanız istenir.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
$registeredServer = Register-AzureRmStorageSyncServer -StorageSyncServiceName $storageSyncName
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Bir eşitleme grubu ve bir bulut uç noktası oluşturma
Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Bir eşitleme grubu içindeki uç noktalar, birbiriyle eşitlenmiş durumda tutulur. Eşitleme grubu, bir Azure dosya paylaşımı ve en az bir sunucu uç noktasını temsil eden bir bulut uç noktası içermelidir. Sunucu uç noktası kayıtlı sunucudaki bir yolu temsil eder. Sunucu uç noktaları, bir sunucu birden çok eşitleme gruplarında olabilir. İstenen eşitleme topolojinizi uygun şekilde tanımlamak gerek duyduğunuz kadar eşitleme grupları oluşturabilirsiniz.

Bulut uç noktası, bir Azure dosya paylaşımı için bir işaretçidir. Tüm sunucu uç noktalarını, bulut uç noktası hub yaparak bulut uç noktasıyla eşitlenir. Depolama hesabı için Azure dosya paylaşımı depolama eşitleme hizmeti ile aynı bölgede bulunması gerekir. Bir özel durum ile Azure dosya paylaşımının tamamen eşitlenir: Bir özel klasörü gizli bir NTFS birimi "Sistem Birim bilgisi" klasörüne karşılaştırılabilir sağlanır. Bu dizin denir ". SystemShareInformation". Bu, diğer uç noktalar ile eşitlenmez önemli eşitleme meta verileri içerir. Kullanmayın veya silmeden!

> [!Important]  
> Herhangi bir bulut uç noktası veya eşitleme grubunda sunucu uç noktası değişiklik yapabilirsiniz ve dosyalarınızı eşitleme grubundaki diğer Uç noktalara eşitlenmiş. Bulut uç noktasına (Azure dosya paylaşımı) doğrudan bir değişiklik yaparsanız değişiklikleri öncelikle bir Azure dosya eşitleme değişiklik algılama iş tarafından bulunması gerekir. Bir değişiklik algılama işi bulut uç noktası için 24 saatte bir kere başlatılır. Daha fazla bilgi için [Azure sık sorulan sorular dosyaları](storage-files-faq.md#afs-change-detection).

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Bir eşitleme grubu oluşturmak için [Azure portalında](https://portal.azure.com/)depolama eşitleme hizmetinize gidin ve ardından **+ eşitleme grubu**:

![Azure portalda yeni bir eşitleme grubu oluşturma](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Açılan bölmede, bulut uç noktası olan bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

- **Eşitleme grubu adı**: Oluşturulacak eşitleme grubu adı. Bu ad Depolama Eşitleme Hizmetinde benzersiz olmalıdır, ancak size mantıklı gelen herhangi bir ad olabilir.
- **Abonelik**: Depolama eşitleme hizmetinde dağıtıldığı abonelik [depolama eşitleme hizmeti dağıtma](#deploy-the-storage-sync-service).
- **Depolama hesabı**: Seçerseniz **depolama hesabını seçin**, eşitlemek istediğiniz Azure dosya paylaşımını içeren depolama hesabı seçin, başka bir bölmesi görünür.
- **Azure dosya paylaşımı**: Eşitlemek istediğiniz Azure dosya paylaşımının adı.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Eşitleme grubu oluşturmak için aşağıdaki PowerShell yürütün. Değiştirmeyi unutmayın `<my-sync-group>` istenen eşitleme grubu adı.

```powershell
$syncGroupName = "<my-sync-group>"
New-AzureRmStorageSyncGroup -SyncGroupName $syncGroupName -StorageSyncService $storageSyncName
```

Eşitleme grubu başarıyla oluşturulduktan sonra bulut uç noktası oluşturabilirsiniz. Değiştirdiğinizden emin olun `<my-storage-account>` ve `<my-file-share>` beklenen değerleri.

```powershell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzureRmStorageSyncCloudEndpoint `
    -StorageSyncServiceName $storageSyncName `
    -SyncGroupName $syncGroupName ` 
    -StorageAccountResourceId $storageAccount.Id `
    -StorageAccountShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Sunucu uç noktası oluşturma
Sunucu uç noktası, bir sunucu birimi üzerindeki klasör gibi kayıtlı bir sunucu üzerindeki belirli bir noktayı temsil eder. Sunucu uç noktası bir kayıtlı sunucu (bir bağlı paylaşım yerine) bir yol olmalıdır ve bulut katmanlaması kullanmak için yolun sistem dışı bir birimde olması gerekir. Ağa bağlı depolama (NAS) desteklenmiyor.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Sunucu uç noktası eklemek için yeni oluşturulan bir eşitleme grubuna gidin ve ardından **sunucusu uç noktası ekleme**.

![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

**Sunucu uç noktası ekle** bölmesinde bir sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

- **Kayıtlı sunucu**: Sunucu veya sunucu uç noktasını oluşturmak için istediğiniz kümenin adıdır.
- **Yol**: Eşitleme grubunun bir parçası olarak eşitlenmesi için Windows sunucu yolu.
- **Bulut Katmanlaması**: Bir anahtar etkinleştirme veya devre dışı bulut katmanlaması. Katmanlama, sık kullanılan veya dosyaları erişilebilir bulut ile Azure dosyaları'na katmanlı.
- **Birim boş alanı**: Sunucu uç noktasını bulunduğu birimde ayırmak için boş alan miktarı. Birim boş alanı, tek bir sunucu uç noktası olan bir birimde % 50'si ayarlanırsa, örneğin, kabaca veri miktarının yarısına Azure dosyaları'na katmanlı. Bağımsız olarak, ister bulut katmanlamayı etkin olduğunda, Azure dosya paylaşımınızı, verilerin tam bir kopyasını her zaman eşitleme grubunda sahip.

Sunucu uç noktası eklemek için seçin **Oluştur**. Dosyalarınız artık Windows Server ve Azure dosya paylaşımı arasında eşitlenmiş durumda tutulur. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Sunucu uç noktası oluşturma ve değiştirdiğinizden emin olun aşağıdaki PowerShell komutları `<your-server-endpoint-path>` ve `<your-volume-free-space>` istenen değerleri.

```powershell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering $true `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
}
else {
    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Azure dosya eşitleme ile ekleme
Önerilen adımları eklemek için Azure dosya eşitleme ile ilk tam dosya kalitesini koruyarak kapalı kalma süresini sıfır ve erişim denetimi listesi (ACL) aşağıdaki gibidir:
 
1. Depolama eşitleme hizmeti dağıtın.
2. Eşitleme grubu oluşturun.
3. Sunucu tam veri kümesi ile Azure dosya eşitleme aracısını yükleyin.
4. Bu sunucuyu kaydetmek ve sunucu uç noktası oluşturun. 
5. Azure dosya paylaşımı (bulut uç noktası) tam yükleme yapmanız eşitleme sağlar.  
6. İlk karşıya yükleme tamamlandıktan sonra Azure dosya eşitleme aracısının kalan sunucuların her birinde yükleyin.
7. Yeni dosya paylaşımları kalan sunucuların her birinde oluşturun.
8. İsterseniz yeni bir dosya paylaşımlarında bulut katmanlama İlkesi ile birlikte sunucu uç noktaları oluşturun. (Bu adım, ilk kurulumu için kullanılabilir olması için ek depolama alanı gerektirir.)
9. Gerçek veri aktarımı olmadan tam ad alanının hızlı geri yükleme yapmak için Azure dosya eşitleme aracısının olanak tanır. Tam ad alanı eşitlemeden sonra eşitleme altyapısı, sunucu uç noktası için bulut katmanlama İlkesi göre yerel disk alanı doldurur. 
10. Eşitleme tamamlandıktan ve istediğiniz gibi topolojinizi test emin olun. 
11. Kullanıcıların ve uygulamaların bu yeni paylaşıma yönlendirin.
12. İsteğe bağlı olarak, sunucular üzerinde yinelenen tüm paylaşımları silebilirsiniz.
 
İlk ekleme için ek depolama alanı yok ve var olan paylaşımlar için eklemek istiyorsanız, Azure dosya paylaşımlarında veri önceden çekirdeğini oluşturabileceğiniz. Kapalı kalma süresi kabul edebilir ve kesinlikle ilk onboarding işlemi sırasında veri değişiklikleri sunucu paylaşımlarındaki garanti ve yalnızca, bu yaklaşım önerilir. 
 
1. Onboarding işlemi sırasında herhangi bir sunucu verileri değiştiremediğinden emin olmak.
2. Ön üretim Azure dosya Robocopy, doğrudan bir SMB kopyasına SMB üzerinden herhangi bir veri aktarımı aracını kullanarak, örneğin sunucu verileriyle paylaşır. Bu nedenle önceden dengeli dağıtım için kullanılamaz AzCopy veri SMB üzerinden yüklenmez bu yana.
3. Azure dosya eşitleme topolojisi için mevcut paylaşımlarına işaret eden istenen sunucu uç noktaları oluşturun.
4. Eşitleme tamamlamak uzlaştırma işleminde tüm uç noktalarda olanak tanır. 
5. Uzlaştırma tamamlandıktan sonra değişiklikleri paylaşımları açabilirsiniz.
 
Şu anda, yaklaşım önceden dengeli dağıtım ilişkin birkaç sınırlama olduğunu lütfen unutmayın- 
- Dosyalarda tam uygunlukta korunmaz. Örneğin, ACL'ler ve zaman damgaları dosyaları kaybedersiniz.
- Veri değişikliklerini çalışıyor tam eşitleme topolojisi önce sunucusunda hem de çalışan sunucu Uç noktalara çakışmalara neden olabilir.  
- Bulut uç noktası oluşturulduktan sonra Azure dosya eşitleme ilk eşitleme başlatmadan önce dosyaları bulutta algılamak için bir işlem çalıştırır. Bu işlemi tamamlamak için harcanan süre, ağ hızı, kullanılabilir bant genişliğini ve dosya ve klasör sayısı gibi çeşitli faktörlere bağlı olarak değişir. Önizleme sürümünde kabaca tahmin için yaklaşık 10 dosyaları/sn üzerinden Algılama işlemini çalıştırır.  Bulutta ön hazırlığı yapılmış veriler olduğunda bu nedenle, çalışmaları hızlı önceden dengeli dağıtım olsa bile, tam olarak çalışan bir sistemi almak için toplam süreyi önemli ölçüde uzun olabilir.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>DFS Çoğaltma (DFS-R) dağıtımı için Azure dosya eşitleme geçirme
DFS-R dağıtımı için Azure dosya eşitleme geçirmek için:

1. Değiştirdiğiniz DFS-R topolojisini temsil eden bir eşitleme grubu oluşturun.
2. Veri kümesini geçirmek için DFS-R topolojide olduğu sunucusunda başlatın. Azure dosya eşitleme, bu sunucuya yükleyin.
3. Bu sunucuyu kaydetmek ve ilk sunucunun geçirilmesi için sunucu uç noktası oluşturun. Bulut etkinleştirmeyin katmanlama.
4. Tüm Azure dosya paylaşımınızı (bulut uç noktası) için veri eşitleme'nin olanak tanır.
5. Yükleyin ve Azure dosya eşitleme aracısının kalan DFS-R sunucuların her birinde kaydedin.
6. Disable DFS-R. 
7. Sunucu uç noktası DFS-R sunucuların her birinde oluşturun. Bulut etkinleştirmeyin katmanlama.
8. Eşitleme tamamlandıktan ve istediğiniz gibi topolojinizi test emin olun.
9. DFS-r devre dışı bırakma
10. Bulut katmanlaması Mayıs şimdi etkinleştirilmesini istediğiniz gibi herhangi bir sunucu uç noktasında.

Daha fazla bilgi için [dağıtılmış dosya sistemi (DFS) ile Azure dosya eşitleme Interop](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme sunucusu uç noktası Ekle Kaldır](storage-sync-files-server-endpoint.md)
- [Kaydolun veya Azure dosya eşitleme ile bir sunucunun kaydını Kaldır](storage-sync-files-server-registration.md)
- [Azure dosya eşitleme İzleyicisi](storage-sync-files-monitoring.md)
