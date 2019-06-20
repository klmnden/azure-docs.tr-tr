---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 072864d565e2edbddd4b7df851ad0e30daf7e5fa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188395"
---
Microsoft Azure bulut hizmeti sorunları tanılama, sorunlar ortaya çıktığında sanal makineler hizmetin günlük dosyalarını toplama gerektirir. AzureLogCollector uzantısı isteğe bağlı bir veya daha fazla bulut hizmeti Vm'lerden (web rolleri ve çalışan rolleri için) tek seferlik günlüklerin toplanmasını gerçekleştirin ve tüm hizmetlerde oturum uzaktan olmadan bir Azure depolama hesabına – toplanan dosyaları aktarmak için kullanabileceğiniz Sanal makinelerin.

> [!NOTE]
> Günlüğe kaydedilen bilgileri çoğu için açıklamalar bulunabilir http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Toplanacak dosyaları türlerine bağımlı koleksiyonun iki mod vardır.

* **Azure Konuk Aracısı günlükleri yalnızca (GA)** . Bu koleksiyon modu Azure Konuk aracısı ve diğer Azure bileşenlerini ilgili tüm günlükleri içerir.
* **Tüm günlükler (tam)** . Bu koleksiyon modu GA modunda, plus tüm dosyaları toplar:
  
  * Sistem ve uygulama olay günlükleri
  * HTTP hata günlükleri
  * IIS Günlükleri
  * Kurulum günlükleri
  * diğer sistem günlükleri

Her iki koleksiyon modda aşağıdaki yapıya koleksiyonunu kullanarak ek veri koleksiyon klasörleri belirtilebilir:

* **Ad**: Toplanan dosyaları zip dosyası içinde bir alt klasör adı olarak kullanılacak koleksiyonun adı.
* **Konum**: Sanal makinede toplanacak dosyalarının bulunduğu klasörü yolu.
* **SearchPattern**: Toplanacak dosya adlarını deseni. Varsayılan değer "\*"
* **Özyinelemeli**: Belirtilen konum altında bulunan yinelemeli olarak toplanacak dosyaları olması durumunda.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](./updated-for-az.md)]

* Oluşturulan zip dosyaları kaydetmek uzantı için bir depolama hesabına sahip.
* Azure PowerShell. Bkz: [Azure PowerShell yükleme](/powershell/azure/install-az-ps)] yükleme yönergeleri için.

## <a name="add-the-extension"></a>Uzantıyı ekleme
Kullanabileceğiniz [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlet'leri veya [Hizmet Yönetimi REST API'lerini](https://msdn.microsoft.com/library/ee460799.aspx) AzureLogCollector uzantısı eklemek için.

Bulut Hizmetleri, mevcut Azure Powershell cmdlet'i için **kümesi AzureServiceExtension**, bulut Hizmeti rol örneklerinde uzantıyı etkinleştirmek için kullanılabilir. Bu uzantı Bu cmdlet ile etkin her seferinde, günlük toplama seçilen rollerin seçili rol örneklerinde tetiklenir.

Sanal makineler, mevcut Azure Powershell cmdlet'i için **kümesi AzureVMExtension**, sanal makinelerde uzantıyı etkinleştirmek için kullanılabilir. Bu uzantı cmdlet'leri aracılığıyla etkin her seferinde, günlük toplama her örneğinde tetiklenir.

Dahili olarak, bu uzantı, JSON tabanlı PublicConfiguration ve PrivateConfiguration kullanır. Genel ve özel yapılandırması için örnek JSON düzenini verilmiştir.

### <a name="publicconfiguration"></a>PublicConfiguration

```json
{
    "Instances":  "*",
    "Mode":  "Full",
    "SasUri":  "SasUri to your storage account with sp=wl",
    "AdditionalData":
    [
      {
              "Name":  "StorageData",
              "Location":  "%roleroot%storage",
              "SearchPattern":  "*.*",
              "Recursive":  "true"
      },
      {
            "Name":  "CustomDataFolder2",
            "Location":  "c:\customFolder",
            "SearchPattern":  "*.log",
            "Recursive":  "false"
      },
    ]
}
```

### <a name="privateconfiguration"></a>PrivateConfiguration

```json
{

}
```

> [!NOTE]
> Bu uzantı gerekmiyor **privateConfiguration**. Yalnızca boş bir yapısına sağlayabilir **– PrivateConfiguration** bağımsız değişken.
> 
> 

Bir veya daha fazla örneğini bir bulut hizmeti veya sanal makine seçilen rollerin koleksiyonları çalıştırmak ve belirtilen Azure hesabı için toplanan dosyaları göndermek için her VM'de tetikler AzureLogCollector eklemek için aşağıdaki iki adımlardan birini izleyebilirsiniz.

## <a name="adding-as-a-service-extension"></a>Bir hizmet uzantısı olarak ekleme
1. Azure PowerShell aboneliğinize bağlanmak için yönergeleri izleyin.
2. Ekleme ve AzureLogCollector uzantıyı etkinleştirmek istediğiniz hizmet adı, yuva, rolleri ve rol örnekleri belirtin.

   ```powershell
   #Specify your cloud service name
   $ServiceName = 'extensiontest2'

   #Specify the slot. 'Production' or 'Staging'
   $slot = 'Production'

   #Specified the roles on which the extension will be installed and enabled
   $roles = @("WorkerRole1","WebRole1")

   #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
   $instances = @("*")

   #Specify the collection mode, "Full" or "GA"
   $mode = "GA"
   ```

3. Dosyaları toplanacak ek veri klasörü belirtin (Bu adım isteğe bağlıdır).

   ```powershell
   #add one location
   $a1 = New-Object PSObject

   $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
   $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
   $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
   $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

   $AdditionalDataList+= $a1
   #more locations can be added....
   ```

   > [!NOTE]
   > Belirteci kullanabilirsiniz `%roleroot%` bir sabit sürücü kullanmaz olduğundan rol kök sürücüsünde belirtmek için.
   > 
   > 
4. Azure depolama hesabı adı ve toplanan dosyalar karşıya yüklenecek bir anahtar sağlar.

   ```powershell
   $StorageAccountName = 'YourStorageAccountName'
   $StorageAccountKey  = 'YourStorageAccountKey'
   ```

5. (Bir makalenin sonunda bulunur) SetAzureServiceLogCollector.ps1 yönelik bir bulut hizmeti AzureLogCollector uzantıyı etkinleştirmek için şu şekilde çağırın. Yürütme tamamlandıktan sonra karşıya yüklenen dosya altında bulabilirsiniz. `https://YourStorageAccountName.blob.core.windows.net/vmlogs`

   ```powershell
   .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList
   ```

Aşağıdaki betiğe geçirilen parametreler tanımıdır. (Bu aşağıda da kopyalanır.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances,

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: Bulut hizmeti adı.
* **rolleri**: "WebRole1" veya "WorkerRole1" gibi rollerinin listesi.
* **Örnekleri**: Rol örneği--virgülle ayırarak adları listesini bir joker karakter dizesi kullanın ("*") tüm rol örnekleri için.
* **Yuva**: Yuva adı. "Üretim" veya "Hazırlama".
* **Modu**: Koleksiyon modu. "Tam" veya "GA".
* **StorageAccountName**: Toplanan verileri depolamak için Azure depolama hesabı adı.
* **StorageAccountKey**: Azure depolama hesabı anahtarı adı.
* **AdditionalDataLocationList**: Aşağıdaki yapı listesi:

  ```powershell
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="adding-as-a-vm-extension"></a>VM uzantısı ekleme
Azure PowerShell aboneliğinize bağlanmak için yönergeleri izleyin.

1. Hizmet adı, VM ve koleksiyon modu belirtin.

   ```powershell
   #Specify your cloud service name
   $ServiceName = 'YourCloudServiceName'

   #Specify the VM name
   $VMName = "'YourVMName'"

   #Specify the collection mode, "Full" or "GA"
   $mode = "GA"

   Specify the additional data folder for which files will be collected (this step is optional).

   #add one location
   $a1 = New-Object PSObject

   $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
   $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
   $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
   $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

   $AdditionalDataList+= $a1
        #more locations can be added....
   ```
  
2. Azure depolama hesabı adı ve toplanan dosyalar karşıya yüklenecek bir anahtar sağlar.

   ```powershell
   $StorageAccountName = 'YourStorageAccountName'
   $StorageAccountKey  = 'YourStorageAccountKey'
   ```

3. (Bir makalenin sonunda bulunur) SetAzureVMLogCollector.ps1 yönelik bir bulut hizmeti AzureLogCollector uzantıyı etkinleştirmek için şu şekilde çağırın. Yürütme tamamlandıktan sonra karşıya yüklenen dosya altında bulabilirsiniz. `https://YourStorageAccountName.blob.core.windows.net/vmlogs`

Aşağıdaki betiğe geçirilen parametreler tanımıdır. (Bu aşağıda da kopyalanır.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string] $VMName ,

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: Bulut hizmeti adı.
* **VMName**: VM adı.
* **Modu**: Koleksiyon modu. "Tam" veya "GA".
* **StorageAccountName**: Toplanan verileri depolamak için Azure depolama hesabı adı.
* **StorageAccountKey**: Azure depolama hesabı anahtarı adı.
* **AdditionalDataLocationList**: Aşağıdaki yapı listesi:

  ```
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="extention-powershell-script-files"></a>Uzantı PowerShell komut dosyaları
### <a name="setazureservicelogcollectorps1"></a>SetAzureServiceLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances = '*',

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)

$publicConfig = New-Object PSObject

if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be separated by ,
{
  $instanceText = $Instances[0]
  for ($i = 1;$i -lt $Instances.Count;$i++)
  {
    $instanceText = $instanceText+ "," + $Instances[$i]
  }
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
}
else  #For all instances if not specified.  The value should be a space or *
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
}

if ($Mode -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
  "Container ($ContainerName) doesn't exist. Creating it now.."
  New-AzStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json
"publicConfig is:  $publicConfigJSON"

#we just provide an empty privateConfig object
$privateconfig = "{
}"

if ($Roles -ne $null)
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}
else
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}

#
#This is an optional step: generate a sasUri to the container so it can be shared with other people if needed.
#
$SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
$SasUri = $SasUri + "&restype=container&comp=list"
Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"
```

### <a name="setazurevmlogcollectorps1"></a>SetAzureVMLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
              [Parameter(Mandatory=$true)]
              [string]   $ServiceName,

              [Parameter(Mandatory=$false)]
              [string] $VMName ,

              [Parameter(Mandatory=$false)]
              [string]   $Mode = 'Full',

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountName,

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountKey,

              [Parameter(Mandatory=$false)]
              [PSObject[]] $AdditionDataLocationList = $null
        )

$publicConfig = New-Object PSObject
$publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

if ($Mode -ne $null )
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
    "Container ($ContainerName) doesn't exist. Creating it now.."
    New-AzStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
$SasUri = New-AzStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json

Write-Output "PublicConfiguration is: \r\n$publicConfigJSON"

#
#we just provide an empty privateConfig object
#
$privateconfig = "{
}"

if ($VMName -ne $null )
{
      $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
      $VM.VM.OSVirtualHardDisk.OS

      if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
      {
            Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

            #
            #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
            #the presence of SubstatusList field.
            #
            $Completed = $false
            while ($Completed -ne $true)
            {
                    $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                    $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                    if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                    {
                        Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                          $Completed = $true
                    }
                    elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                    {
                          $Completed = $false
                          Write-Output "Waiting for operation to complete..."
                    }
                    else
                    {
                          $Completed = $true
                          Write-Output "Operation completed."

                    $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                          $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                  #
                        # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                          #
                          $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                          $ReadSasUri = New-AzStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                        Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                          #
                          #This is an optional step:  Remove the extension after we are done
                          #
                          Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                    }
                    Start-Sleep -s 5
            }
      }
      else
      {
          Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
      }

}
else
{
  Write-Output "VM name is not specified, the extension cannot be enabled"
}
```

## <a name="next-steps"></a>Sonraki Adımlar
Şimdi inceleyin veya günlüklerinizi basit bir konumdan kopyalayın.

