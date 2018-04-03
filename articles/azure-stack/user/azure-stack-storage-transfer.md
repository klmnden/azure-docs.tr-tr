---
title: Azure yığın depolama için Araçlar
description: Aktarım araçları Azure yığın depolama birimi verileri hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/30/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: e26a38b8fd7d008a46eba2c41075c5af09a6616a
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="tools-for-azure-stack-storage"></a>Azure yığın depolama için Araçlar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Microsoft Azure yığın diskler, BLOB'lar, tablolar, kuyruklar ve hesap yönetim işlevleri için depolama hizmetleri kümesi sağlar. Yönetmek veya için veya Azure yığın depolama biriminden verileri taşımak istiyorsanız, bir dizi Azure Storage araçları kullanabilirsiniz. Bu makalede kullanılabilen araçlar hızlı bir bakış sağlar.

Gereksinimlerinize en uygun aracı bağlıdır:
* [AzCopy](#azcopy)

    Verileri bir nesneden başka depolama hesabınızda veya depolama hesapları arasında kopyalamak için karşıdan yükleyebileceğiniz bir depolama özgü komut satırı yardımcı programı.

* [Azure PowerShell](#azure-powershell)

    Bir görev tabanlı komut satırı kabuğu ve betik dilidir özellikle sistem yönetimi için tasarlanmıştır.

* [Azure CLI](#azure-cli)

    Azure ve Azure yığın platformları ile çalışmak için bir komut kümesi sağlayan bir açık kaynak, platformlar arası aracı.

* [Microsoft Depolama Gezgini (Önizleme)](#microsoft-azure-storage-explorer)

    Kullanımı kolay bir tek başına uygulama kullanıcı arabirimi ile.

Depolama Hizmetleri nedeniyle arasındaki farklar Azure ve Azure yığını, aşağıdaki bölümlerde açıklanan her aracı için bazı belirli gereksinimleri olabilir. Azure yığın depolama ve Azure depolama birimi arasında bir karşılaştırma için bkz: [Azure yığın depolama: farklar ve konuları](azure-stack-acs-differences.md).


## <a name="azcopy"></a>AzCopy
AzCopy ve Microsoft Azure Blob ve en uygun performans ile basit komutları kullanarak Table storage veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır. Verileri bir nesneden diğerine depolama hesabınızda veya depolama hesapları arasında kopyalayabilirsiniz. AzCopy iki sürümü vardır: Windows ve Linux üzerinde AzCopy AzCopy. Azure yığını yalnızca Windows sürümünü destekler. 
 
### <a name="download-and-install-azcopy"></a>AzCopy yükleyip 

[Karşıdan](https://aka.ms/azcopyforazurestack) Azure yığın AzCopy desteklenen bir Windows sürümü. Yükleyin ve AzCopy Azure yığında Azure aynı şekilde kullanın. Daha fazla bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](../../storage/common/storage-use-azcopy.md). 

 - 1802 için güncelleştirme veya daha yeni sürümlerini [AzCopy 7.1.0 karşıdan](https://aka.ms/azcopyforazurestack20170417).
 - Önceki sürümü için [AzCopy 5.0.0 karşıdan](https://aka.ms/azcopyforazurestack20150405).

### <a name="azcopy-command-examples-for-data-transfer"></a>Veri aktarımı için AzCopy komut örnekleri
Aşağıdaki örnekler ve Azure yığın blob'lara ait veriler kopyalama için birkaç tipik senaryolar gösterilmektedir. Daha fazla bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](../../storage/storage-use-azcopy.md). 
#### <a name="download-all-blobs-to-local-disk"></a>Tüm BLOB'lar yerel diske yükle
```azcopy
AzCopy.exe /source:https://myaccount.blob.local.azurestack.external/mycontainer /dest:C:\myfolder /sourcekey:<key> /S
```
#### <a name="upload-single-file-to-virtual-directory"></a>Sanal dizin için tek dosya karşıya yükleme 
```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.local.azurestack.external/mycontainer/vd /DestKey:key /Pattern:abc.txt
```
#### <a name="move-data-between-azure-and-azure-stack-storage"></a>Verileri Azure yığın depolama ve Azure arasında taşıma 
Azure Storage ve Azure yığın arasında zaman uyumsuz veri aktarımı desteklenmiyor. aktarıma belirtmek zorunda **/SyncCopy** seçeneği. 
```azcopy 
Azcopy /Source:https://myaccount.blob.local.azurestack.external/mycontainer /Dest:https://myaccount2.blob.core.windows.net/mycontainer2 /SourceKey:AzSKey /DestKey:Azurekey /S /SyncCopy
```

### <a name="azcopy-known-issues"></a>Azcopy bilinen sorunlar
* Dosya depolama henüz Azure yığın içinde kullanılabilir olmadığından dosya depolama üzerinde herhangi bir AzCopy işlemi kullanılabilir değil.
* Azure Storage ve Azure yığın arasında zaman uyumsuz veri aktarımı desteklenmiyor. Aktarıma belirtebilirsiniz **/SyncCopy** verileri kopyalamak için seçeneği.
* Azcopy Linux sürümü için Azure yığın depolama desteklenmiyor. 

## <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell cmdlet'leri hem Azure hem de Azure yığın hizmetleri yönetmek için sağlayan bir modüldür. Sistem yönetimi için özel olarak tasarlanan görev tabanlı bir komut satırı kabuğu ve betik dili olarak tanımlanır.

### <a name="install-and-configure-powershell-for-azure-stack"></a>PowerShell'i yükleme ve Azure yığını için yapılandırma
Azure yığın uyumlu Azure PowerShell modülleri Azure yığın ile çalışmak için gereklidir. Daha fazla bilgi için bkz: [Azure yığını için PowerShell yükleme](azure-stack-powershell-install.md) ve [Azure yığın kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md) daha fazla bilgi için.

### <a name="powershell-sample-script-for-azure-stack"></a>Azure yığınının PowerShell örnek betiği 
Bu örnek, başarılı bir şekilde sahip olduğunuzu varsayar [Azure yığını için PowerShell yükleme](azure-stack-powershell-install.md). Bu komut, yapılandırmayı tamamlamak ve yerel PowerShell ortamına hesabınızı eklemek için kimlik bilgileri, Azure yığın Kiracı isteyin yardımcı olur. Sonra komut dosyası varsayılan Azure aboneliği ayarlamak, yeni bir depolama hesabı oluşturma, yeni bir kapsayıcı bu yeni depolama hesabı oluşturur ve varolan bir görüntü dosyası (blob) kapsayıcıya karşıya yükleme. Bu kapsayıcıdaki tüm blob'lara komut dosyasını listeler sonra yerel bilgisayarınızda yeni bir hedef dizin oluşturun ve görüntü dosyasını indirin.

1. Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  
2. Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).  
3. Açık **Windows PowerShell ISE** ve **yönetici olarak çalıştır**, tıklatın **dosya** > **yeni** yeni bir komut dosyası oluşturmak için.
4. Aşağıdaki betiği kopyalayıp yeni betik dosyasına yapıştırın.
5. Yapılandırma ayarlarınızı temel alan komut dosyası değişkenleri güncelleştirin. 
6. Not: Bu komut dosyası kök altında çalıştırılması gereken indirilen **AzureStack_Tools**. 

```PowerShell 
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environemnt name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudiance = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase!
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\downlaod" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudiance

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Login-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

# Set a default Azure subscription.
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a new Resource Group 
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a new storage account.
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS

# Set a default storage account.
Set-AzureRmCurrentStorageAccount -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName 

# Create a new container.
New-AzureStorageContainer -Name $ContainerName -Permission Off

# Upload a blob into a container.
Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

# List all blobs in a container.
Get-AzureStorageBlob -Container $ContainerName

# Download blobs from the container:
# Get a reference to a list of all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName

# Create the destination directory.
New-Item -Path $DestinationFolder -ItemType Directory -Force  

# Download blobs into the local destination directory.
$blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder

# end
```

### <a name="powershell-known-issues"></a>PowerShell bilinen sorunlar 
Geçerli uyumlu Azure PowerShell modülü Azure yığınının 1.2.10 sürümüdür. En son Azure PowerShell sürümünden farklıdır. Bu fark depolama hizmetleri işlemi etkiler:

* Dönüş değeri biçimi `Get-AzureRmStorageAccountKey` sürümünde 1.2.10 iki özelliği vardır: `Key1` ve `Key2`, geçerli Azure sürümü tüm hesap anahtarları içeren bir dizi döndürür.
   ```
   # This command gets a specific key for a Storage account, 
   # and works for Azure PowerShell version 1.4, and later versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Value[0]

   # This command gets a specific key for a Storage account, 
   # and works for Azure PowerShell version 1.3.2, and previous versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Key1

   ```
   Daha fazla bilgi için bkz: [Get-AzureRmStorageAccountKey](https://docs.microsoft.com/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey?view=azurermps-4.1.0).

## <a name="azure-cli"></a>Azure CLI
Azure CLI Azure kaynaklarını yönetmek için Azure komut satırı deneyimidir. MacOS, Linux ve Windows yükleme ve komut satırından çalıştırın. 

Azure CLI, yönetmek ve komut satırından Azure kaynaklarını yönetme ve Azure Resource Manager karşı iş otomasyon komut dosyaları oluşturmak için optimize edilmiştir. Birçok zengin veri erişimi de dahil olmak üzere Azure yığın portalda bulunan aynı işlevleri sağlar.

Azure yığını, Azure CLI Sürüm 2.0 gerektirir. Yükleme ve Azure CLI Azure yığın ile yapılandırma hakkında daha fazla bilgi için bkz: [yükleyin ve Azure yığın CLI yapılandırma](azure-stack-version-profiles-azurecli2.md). Azure CLI 2.0 ile Azure yığın depolama hesabınızdaki kaynaklara çalışma çeşitli görevleri gerçekleştirmek için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Azure CLI2.0 Azure Storage ile kullanma](../../storage/storage-azure-cli.md)

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure CLI örnek komut dosyasında Azure yığını 
CLI yükleme ve yapılandırmasını tamamladıktan sonra Azure yığın depolama kaynakları ile etkileşim kurmak için bir küçük Kabuk örnek betiği çalışmak için aşağıdaki adımları deneyebilirsiniz. Betik önce depolama hesabınıza yeni bir kapsayıcı oluşturur sonra varolan bir dosyayı (blob) bu kapsayıcıya yükler, kapsayıcıdaki tüm blob'lara listeler ve son olarak, yerel bilgisayarınızda belirttiğiniz bir hedefe dosyasını indirir. Bu komut dosyasını çalıştırmadan önce başarılı bir şekilde bağlandığınızdan emin olun ve hedef Azure yığın oturum açın. 
1. Sık kullandığınız metin düzenleyicisinde açın, sonra kopyalayın ve önceki komut düzenleyiciye yapıştırın.
2. Komut dosyanızın değişken yapılandırma ayarlarınızı yansıtacak şekilde güncelleştirin. 
3. Gerekli değişkenleri güncelleştirdikten sonra komut dosyasını kaydedin ve düzenleyicinizi çıkın. Sonraki adımlar komut dosyası my_storage_sample.sh adlı varsayılmıştır.
4. Komut dosyası yürütülebilir, olarak gerekiyorsa işaretleyin: `chmod +x my_storage_sample.sh`
5. Betiğini yürütün. Örneğin, Bash içinde: `./my_storage_sample.sh`

```bash
#!/bin/bash
# A simple Azure Stack Storage example script

export AZURESTACK_RESOURCE_GROUP=<resource_group_name>
export AZURESTACK_RG_LOCATION="local"
export AZURESTACK_STORAGE_ACCOUNT_NAME=<storage_account_name>
export AZURESTACK_STORAGE_CONTAINER_NAME=<container_name>
export AZURESTACK_STORAGE_BLOB_NAME=<blob_name>
export FILE_TO_UPLOAD=<file_to_upload>
export DESTINATION_FILE=<destination_file>

echo "Creating the resource group..."
az group create --name $AZURESTACK_RESOURCE_GROUP --location $AZURESTACK_RG_LOCATION

echo "Creating the storage account..."
az storage account create --name $AZURESTACK_STORAGE_ACCOUNT_NAME --resource-group $AZURESTACK_RESOURCE_GROUP --account-type Standard_LRS

echo "Creating the blob container..."
az storage container create --name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Uploading the file..."
az storage blob upload --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --file $FILE_TO_UPLOAD --name $AZURESTACK_STORAGE_BLOB_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Listing the blobs..."
az storage blob list --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --output table

echo "Downloading the file..."
az storage blob download --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --name $AZURESTACK_STORAGE_BLOB_NAME --file $DESTINATION_FILE --output table

echo "Done"
```

## <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini

Microsoft Azure Storage Gezgini, Microsoft'tan bir tek başına uygulamadır. Windows, macOS ve Linux Azure Storage ve Azure yığın depolama verileri ile kolayca çalışmanızı sağlar. Azure yığın depolama verilerinizi yönetmek için kolay bir yol istiyorsanız, ardından Microsoft Azure Storage Gezgini kullanmayı düşünün.

 - Azure Storage Gezgini Azure yığın ile çalışmak için yapılandırma hakkında daha fazla bilgi edinmek için [Depolama Gezgini Azure yığın abonelik](azure-stack-storage-connect-se.md).
 - Microsoft Azure Storage Gezgini hakkında daha fazla bilgi için bkz: [Depolama Gezgini (Önizleme) ile çalışmaya başlama](../../vs-azure-tools-storage-manage-with-storage-explorer.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Gezgini bir Azure yığın aboneliğine bağlanma](azure-stack-storage-connect-se.md)
* [Depolama Gezgini (Önizleme) ile çalışmaya başlama](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure tutarlı Depolama: farklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md)
* [Microsoft Azure Depolama'ya Giriş](../../storage/common/storage-introduction.md)

