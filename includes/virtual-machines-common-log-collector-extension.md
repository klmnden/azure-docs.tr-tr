
Microsoft Azure bulut hizmeti ile ilgili sorunları tanılama sorunları ortaya çıktığında sanal makinelerde hizmetin günlük dosyalarını toplama gerektirir. Ağda tek seferlik günlükleri koleksiyona AzureLogCollector uzantısı isteğe bağlı bir veya daha fazla bulut hizmeti Vm'lerden (web rolleri ve çalışan rolleri) kullanın ve tüm açmadan uzaktan herhangi birini Azure depolama hesabı için – toplanan dosya aktarımı VM'ler.

> [!NOTE]
> Günlüğe kaydedilen bilgileri çoğunu açıklamalarını http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp bulunabilir.
> 
> 

Toplanacak dosyaları türlerinde bağımlı koleksiyon iki moddan vardır.

* Azure Konuk Aracısı günlükleri yalnızca (GA). Bu koleksiyon modu Azure Konuk aracıları ve diğer Azure bileşenleri ile ilgili tüm günlükleri içerir.
* Tüm günlükleri (tam). Bu koleksiyon modu artı GA modunda tüm dosyaları toplar:
  
  * Sistem ve uygulama olay günlükleri
  * HTTP Hata günlüklerini
  * IIS günlükleri
  * Kurulum günlükleri
  * diğer sistem günlükleri

Her iki koleksiyon modlarında aşağıdaki yapısını koleksiyonunu kullanarak ek veri koleksiyon klasörleri belirtilebilir:

* **Ad**: ZIP dosyasının içinde alt klasör adı olarak toplanacak kullanılacak koleksiyonun adını.
* **Konum**: sanal makinede burada dosya toplanacak klasörün yolu.
* **SearchPattern**: toplanacak dosyaların adlarını düzeni. Varsayılan değer "*"
* **Özyinelemeli**: dosyaları klasörünün altında toplanan yinelemeli olacaksa.

## <a name="prerequisites"></a>Ön koşullar
* Oluşturulan ZIP dosyaları kaydetmek uzantı için bir depolama hesabı olması gerekir.
* Azure PowerShell cmdlet'leri V0.8.0 kullandığınızdan emin olmanız gerekir veya üstü. Daha fazla bilgi için bkz: [Azure indirmeleri](https://azure.microsoft.com/downloads/).

## <a name="add-the-extension"></a>Uzantısı Ekle
Kullanabileceğiniz [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlet'leri veya [Hizmet Yönetimi REST API'lerine](https://msdn.microsoft.com/library/ee460799.aspx) AzureLogCollector uzantısı eklemek için.

Bulut Hizmetleri, var olan Azure Powershell cmdlet'i için **kümesi AzureServiceExtension**, bulut hizmet rolü örneklerinin uzantısını etkinleştirmek için kullanılabilir. Bu uzantı Bu cmdlet'i etkinleştirilmiş her zaman, günlük toplama seçili roller seçili rol örneklerini tetiklenir.

Sanal makineler, mevcut Azure Powershell cmdlet'i için **kümesi AzureVMExtension**, sanal makinelerde uzantısını etkinleştirmek için kullanılabilir. Bu uzantı cmdlet'leri aracılığıyla etkinleştirilmiş her zaman, günlük toplama her örneğinde tetiklenir.

Dahili olarak, bu uzantı JSON tabanlı PublicConfiguration ve PrivateConfiguration kullanır. Ortak ve özel yapılandırması için örnek JSON düzenini verilmiştir.

### <a name="publicconfiguration"></a>PublicConfiguration
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

### <a name="privateconfiguration"></a>PrivateConfiguration
    {

    }

> [!NOTE]
> Bu uzantının gerekmez **privateConfiguration**. İçin boş bir yapı sağlar **– PrivateConfiguration** bağımsız değişkeni.
> 
> 

Bir veya daha fazla örneğini bir bulut hizmeti veya koleksiyonları çalıştırmak ve toplanan dosyaları belirtilen Azure hesabına göndermek için her bir VM üzerinde tetikler sanal makine seçilen rollerin AzureLogCollector eklemek için iki aşağıdaki adımlardan birini izleyebilirsiniz.

## <a name="adding-as-a-service-extension"></a>Bir hizmeti uzantı olarak ekleme
1. Azure PowerShell aboneliğinize bağlanmak için yönergeleri izleyin.
2. Ekleme ve AzureLogCollector uzantısını etkinleştirmek istediğiniz hizmet adı, yuva, rolleri ve rol örnekleri belirtin.
   
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
3. Dosyaları toplanacak ek veri klasörü belirtin (Bu adım isteğe bağlıdır).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > Belirteci kullanabilirsiniz `%roleroot%` bir sabit sürücü kullanmayan rol kök sürücüsünde belirtmek için.
   > 
   > 
4. Azure depolama hesabı adı ve toplanan dosyaları karşıya yüklenecek anahtarı sağlayın.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. (Makalenin sonunda yer alan) SetAzureServiceLogCollector.ps1 AzureLogCollector uzantısı için bir bulut hizmeti etkinleştirmek için şu şekilde çağırın. Yürütme tamamlandığında, yüklenen dosya altında bulabilirsiniz`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

Aşağıdaki komut dosyasına iletilen parametreler tanımıdır. (Bu aşağıda da kopyalanır.)

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

* *ServiceName*: Bulut hizmeti adı.
* *Rolleri*: "WebRole1" veya "WorkerRole1" gibi rollerinin bir listesi.
* *Örnekleri*:--virgülle ayırarak rol örneklerinin adlarının bir listesini bir joker karakter dizesi kullanın ("*") tüm rol örnekleri için.
* *Yuva*: Yuva adı. "Üretim" veya "Hazırlama".
* *Mod*: Koleksiyon modu. "Tam" veya "GA".
* *StorageAccountName*: toplanan verileri depolamak için ad, Azure depolama hesabı.
* *StorageAccountKey*: ad, Azure depolama hesabı anahtarı.
* *AdditionalDataLocationList*: aşağıdaki yapısını listesi:
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a>Bir VM uzantısı olarak ekleme
Azure PowerShell aboneliğinize bağlanmak için yönergeleri izleyin.

1. Hizmet adı, VM ve koleksiyon modu belirtin.
   
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
2. Azure depolama hesabı adı ve toplanan dosyaları karşıya yüklenecek anahtarı sağlayın.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. (Makalenin sonunda yer alan) SetAzureVMLogCollector.ps1 AzureLogCollector uzantısı için bir bulut hizmeti etkinleştirmek için şu şekilde çağırın. Yürütme tamamlandığında, yüklenen dosya https://YouareStorageAccountName.blob.core.windows.net/vmlogs altında bulabilirsiniz

Aşağıdaki komut dosyasına iletilen parametreler tanımıdır. (Bu aşağıda da kopyalanır.)

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

* ServiceName: Bulut hizmeti adınız.
* VMName VM adı.
* : Koleksiyon modu. "Tam" veya "GA".
* StorageAccountName: toplanan verileri depolamak için Azure depolama hesabının adı.
* StorageAccountKey: Azure depolama hesabı anahtarı adı.
* AdditionalDataLocationList: Aşağıdaki yapısını bir listesi:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Uzantı PowerShell komut dosyaları
SetAzureServiceLogCollector.ps1

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

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
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
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
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

    #we just provide a empty privateConfig object
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
    #This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"


SetAzureVMLogCollector.ps1

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
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
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

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
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
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

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

## <a name="next-steps"></a>Sonraki Adımlar
Şimdi inceleyin veya çok basit bir konumdan günlüklerinizi kopyalayın.

