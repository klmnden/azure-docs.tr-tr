---
title: Azure PowerShell betik örneği - blob kapsayıcısı toplam fatura boyutunu hesaplamak | Microsoft Docs
description: Azure Blob depolamadaki bir kapsayıcıda toplam boyutu, faturalandırma için hesaplayın.
services: storage
documentationcenter: na
author: WenJason
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
origin.date: 11/07/2017
ms.date: 04/22/2019
ms.author: v-jay
ms.openlocfilehash: 02b4cfcc6d88430701f653665269532a4eb7092f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60460648"
---
# <a name="calculate-the-total-billing-size-of-a-blob-container"></a>Bir blob kapsayıcısı toplam fatura boyutunu Hesapla

Bu betik, faturalandırma maliyetlerini tahmin etmek amacıyla Azure Blob depolamadaki bir kapsayıcıda boyutunu hesaplar. Betik, bir kapsayıcıdaki blobları boyutunu toplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Bu PowerShell Betiği, faturalandırma için bir kapsayıcı boyutu hesaplar. Diğer amaçlar için kapsayıcı boyutu hesaplanıyor olup [toplam Blob Depolama kapsayıcısının boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-size-powershell.md) bir tahmin sağlar daha basit bir komut dosyası.

## <a name="determine-the-size-of-the-blob-container"></a>Blob kapsayıcısı boyutunu belirler

Blob kapsayıcısı toplam boyutu kapsayıcının boyutu ve kapsayıcıdaki tüm blobları boyutunu içerir.

Aşağıdaki bölümlerde blob kapsayıcılar ve bloblar için depolama kapasitesini nasıl hesaplandığını açıklar. Aşağıdaki bölümde, dizedeki karakter sayısını Len(X) anlamına gelir.

### <a name="blob-containers"></a>BLOB kapsayıcıları

Hesaplama aşağıdaki blob kapsayıcı tüketilen depolama miktarını tahmin edin açıklar:

```
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
```

Dökümü aşağıda verilmiştir:
* her kapsayıcı için yük 48 bayt son değiştirilme zamanı, izinler, genel ayarları ve bazı sistem meta verileri içerir.

* Kapsayıcı adı Unicode olarak depolanır, bu nedenle karakter sayısını almak ve iki ile çarpın.

* Her depolanan blob kapsayıcı meta veri bloğu için dize değeri uzunluğu (ASCII) adının uzunluğu depolarız.

* Oturum tanımlayıcısı başına 512 bayt imzalı tanımlayıcı adı, başlangıç zamanı, süre sonu ve izinleri içerir.

### <a name="blobs"></a>Bloblar

Aşağıdaki hesaplamaları blob başına kullanılan depolama miktarı tahmin etmek nasıl gösterir.

* Blok blobu (temel blob veya anlık görüntü):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
   SizeInBytes(data in unique committed data blocks stored) +
   SizeInBytes(data in uncommitted data blocks)
   ```

* Sayfa blobu (temel blob veya anlık görüntü):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   number of nonconsecutive page ranges with data * 12 bytes +
   SizeInBytes(data in unique pages stored)
   ```

Dökümü aşağıda verilmiştir:

* 124 bayt yük içeren blob için:
    - Son Değiştirme Tarihi
    - Boyut
    - Önbellek denetimi
    - Content-Type
    - İçerik dili
    - İçerik kodlama
    - İçerik MD5
    - İzinler
    - Anlık görüntü bilgileri
    - Kiralama
    - Bazı sistem meta verileri

* Blob adı Unicode olarak depolanır, bu nedenle karakter sayısını almak ve iki ile çarpın.

* Her depolanmış meta veri bloğu için dize değeri uzunluğu (ASCII saklanır), adının uzunluğu ekleyin.

* Blok blobları için:
  * engelleme listesi için 8 bayt.
  * Blok kimliği boyutu bayt cinsinden zaman blokların sayısı.
  * Kaydedilen ve kaydedilmeyen bloğu tüm verilerin boyutu.

    >[!NOTE]
    >Bu boyut, anlık görüntüler kullanıldığında, bu temel ya da anlık görüntü blob yalnızca benzersiz verilerini içerir. İşlenmemiş blokları bir hafta sonra kullanılmıyorsa, atık olarak toplanmış değildirler. Bundan sonra bunların doğru faturalama sayılmaz.

* Sayfa blobları için:
  * Veri ardışık olmayan sayfa aralıklarını 12 bayt sayısı çarpı. Bu benzersiz sayfa aralıklarını çağırırken gördüğünüz sayısıdır **GetPageRanges** API.

  * Verilerin tüm saklı sayfalarının bayt cinsinden boyutu.

    >[!NOTE]
    >Anlık görüntüler kullanıldığında, bu boyutun yalnızca temel blob veya sayılmakta anlık görüntü blob için benzersiz sayfalar içerir.

## <a name="sample-script"></a>Örnek betik

```powershell

# this script will show how to get the total size of the blobs in a container
# before running this, you need to create a storage account, create a container,
#    and upload some blobs into the container
# note: this retrieves all of the blobs in the container in one command.
#       connect Azure with Login-AzAccount -EnvironmentName AzureChinaCloud before you run the script.
#       requests sent as part of this tool will incur transactional costs. 
# command line usage: script.ps1 -ResourceGroup {YourResourceGroupName} -StorageAccountName {YourAccountName} -ContainerName {YourContainerName}
#


param(
    [Parameter(Mandatory=$true)]
    [string]$ResourceGroup,

    [Parameter(Mandatory=$true)]
    [string]$StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]$ContainerName
)

#Set-StrictMode will cause Get-AzureStorageBlob returns result in different data types when there is only one blob
#Set-StrictMode -Version 2

$VerbosePreference = "Continue"

if((Get-Module -ListAvailable Az.Storage) -eq $null)
{
    throw "Azure Powershell not found! Please install from https://docs.microsoft.com/en-us/powershell/azure/install-Az-ps"
}

# function Retry-OnRequest
function Retry-OnRequest
{
    param(
        [Parameter(Mandatory=$true)]
        $Action)
    
    # It could encounter various of temporary errors, like network errors, or storage server busy errors.
    # Should retry the request on transient errors

    # Retry on storage server timeout errors
    $clientTimeOut = New-TimeSpan -Minutes 15
    $retryPolicy = New-Object -TypeName Microsoft.WindowsAzure.Storage.RetryPolicies.ExponentialRetry -ArgumentList @($clientTimeOut, 10)        
    $requestOption = @{}
    $requestOption.RetryPolicy = $retryPolicy

    # Retry on temporary network errors
    $shouldRetryOnException = $false
    $maxRetryCountOnException = 3

    do
    {
        try
        {
            return $Action.Invoke($requestOption)
        }
        catch
        {
            if ($_.Exception.InnerException -ne $null -And $_.Exception.InnerException.GetType() -Eq [System.TimeoutException] -And $maxRetryCountOnException -gt 0)
            {
                $shouldRetryOnException = $true
                $maxRetryCountOnException--
            }
            else
            {
                $shouldRetryOnException = $false
                throw
            }
        }
    }
    while ($shouldRetryOnException)

}

# function Get-BlobBytes

function Get-BlobBytes
{
    param(
        [Parameter(Mandatory=$true)]
        $Blob,
        [Parameter(Mandatory=$false)]
        [bool]$IsPremiumAccount = $false)

    # Base + blobname
    $blobSizeInBytes = 124 + $Blob.Name.Length * 2

    # Get size of metadata
    $metadataEnumerator=$Blob.ICloudBlob.Metadata.GetEnumerator()
    while($metadataEnumerator.MoveNext())
    {
        $blobSizeInBytes += 3 + $metadataEnumerator.Current.Key.Length + $metadataEnumerator.Current.Value.Length
    }

    if (!$IsPremiumAccount)
    {
        if($Blob.BlobType -eq [Microsoft.WindowsAzure.Storage.Blob.BlobType]::BlockBlob)
        {
            $blobSizeInBytes += 8
            # Default is Microsoft.WindowsAzure.Storage.Blob.BlockListingFilter.Committed. Need All
            $action = { param($requestOption) return $Blob.ICloudBlob.DownloadBlockList([Microsoft.WindowsAzure.Storage.Blob.BlockListingFilter]::All, $null, $requestOption) }                

            $blocks=Retry-OnRequest $action      

            if ($null -eq $blocks)
            {
                $blobSizeInBytes += $Blob.ICloudBlob.Properties.Length
            }
            else
            {
                $blocks | ForEach-Object { $blobSizeInBytes += $_.Length + $_.Name.Length }
            }  
        }
        elseif($Blob.BlobType -eq [Microsoft.WindowsAzure.Storage.Blob.BlobType]::PageBlob)
        {
            # It could cause server time out issue when trying to get page ranges of highly fragmented page blob 
            # Get page ranges in segment can mitigate chance of meeting such kind of server time out issue
            # See https://blogs.msdn.microsoft.com/windowsazurestorage/2012/03/26/getting-the-page-ranges-of-a-large-page-blob-in-segments/ for details.
            $pageRangesSegSize = 148 * 1024 * 1024L
            $totalSize = $Blob.ICloudBlob.Properties.Length
            $pageRangeSegOffset = 0
        
            $pageRangesTemp = New-Object System.Collections.ArrayList
        
            while ($pageRangeSegOffset -lt $totalSize)
            {
                $action = {param($requestOption) return $Blob.ICloudBlob.GetPageRanges($pageRangeSegOffset, $pageRangesSegSize, $null, $requestOption) }

                Retry-OnRequest $action | ForEach-Object { $pageRangesTemp.Add($_) }  | Out-Null
                $pageRangeSegOffset += $pageRangesSegSize
            }

            $pageRanges = New-Object System.Collections.ArrayList

            foreach ($pageRange in $pageRangesTemp)
            {
                if($lastRange -eq $Null)
                {
                    $lastRange = New-Object PageRange
                    $lastRange.StartOffset = $pageRange.StartOffset
                    $lastRange.EndOffset =  $pageRange.EndOffset
                }
                else
                {
                    if (($lastRange.EndOffset + 1) -eq $pageRange.StartOffset)
                    {
                        $lastRange.EndOffset = $pageRange.EndOffset
                    }
                    else
                    {
                        $pageRanges.Add($lastRange)  | Out-Null
                        $lastRange = New-Object PageRange
                        $lastRange.StartOffset = $pageRange.StartOffset
                        $lastRange.EndOffset =  $pageRange.EndOffset
                    }
                }
            }

            $pageRanges.Add($lastRange) | Out-Null
            $pageRanges |  ForEach-Object { 
                    $blobSizeInBytes += 12 + $_.EndOffset - $_.StartOffset 
                }
        }
        else
        {
            $blobSizeInBytes += $Blob.ICloudBlob.Properties.Length
        }
        return $blobSizeInBytes
    }
    else
    {
        $blobSizeInBytes += $Blob.ICloudBlob.Properties.Length
    }
    return $blobSizeInBytes
}

# function Get-ContainerBytes

function Get-ContainerBytes
{
    param(
        [Parameter(Mandatory=$true)]
        [Microsoft.WindowsAzure.Storage.Blob.CloudBlobContainer]$Container,
        [Parameter(Mandatory=$false)]
        [bool]$IsPremiumAccount = $false)

    # Base + name of container
    $containerSizeInBytes = 48 + $Container.Name.Length*2

    # Get size of metadata
    $metadataEnumerator = $Container.Metadata.GetEnumerator()
    while($metadataEnumerator.MoveNext())
    {
        $containerSizeInBytes += 3 + $metadataEnumerator.Current.Key.Length + $metadataEnumerator.Current.Value.Length
    }

    # Get size for SharedAccessPolicies
    $containerSizeInBytes += $Container.GetPermissions().SharedAccessPolicies.Count * 512

    # Calculate size of all blobs.
    $blobCount = 0
    $Token = $Null
    $MaxReturn = 5000

    do {
        $Blobs = Get-AzStorageBlob -Context $storageContext -Container $Container.Name -MaxCount $MaxReturn -ContinuationToken $Token
        if($Blobs -eq $Null) { break }

        #Set-StrictMode will cause Get-AzureStorageBlob returns result in different data types when there is only one blob
        if($Blobs.GetType().Name -eq "AzureStorageBlob")
        {
            $Token = $Null
        }
        else
        {
            $Token = $Blobs[$Blobs.Count - 1].ContinuationToken;
        }

        $Blobs | ForEach-Object {
                $blobSize = Get-BlobBytes $_ $IsPremiumAccount
                $containerSizeInBytes += $blobSize
                $blobCount++

                if(($blobCount % 1000) -eq 0)
                {
                    Write-Verbose("Counting {0} Sizing {1} " -f $blobCount, $containerSizeInBytes)
                }
            }
    }
    While ($Token -ne $Null)

    return @{ "containerSize" = $containerSizeInBytes; "blobCount" = $blobCount }
}

#Login-AzAccount -EnvironmentName AzureChinaCloud

$storageAccount = Get-AzStorageAccount -ResourceGroupName $ResourceGroup -Name $StorageAccountName -ErrorAction SilentlyContinue
if($storageAccount -eq $null)
{
    throw "The storage account specified does not exist in this subscription."
}

$storageContext = $storageAccount.Context

if (-not ([System.Management.Automation.PSTypeName]'PageRange').Type)
{
    $Source = "
        public class PageRange
        {
            public long StartOffset;
            public long EndOffset;
        }"
    Add-Type -TypeDefinition $Source
}

$containers = New-Object System.Collections.ArrayList
if($ContainerName.Length -ne 0)
{
    $container = Get-AzStorageContainer -Context $storageContext -Name $ContainerName -ErrorAction SilentlyContinue |
        ForEach-Object { $containers.Add($_) } | Out-Null
}
else
{
    Get-AzStorageContainer -Context $storageContext | ForEach-Object { $containers.Add($_) } | Out-Null
}

$sizeInBytes = 0
$IsPremiumAccount = ($storageAccount.Sku.Tier -eq "Premium")

if($containers.Count -gt 0)
{
    $containers | ForEach-Object {
        Write-Output("Calculating container {0} ..." -f $_.CloudBlobContainer.Name)
        $result = Get-ContainerBytes $_.CloudBlobContainer $IsPremiumAccount
        $sizeInBytes += $result.containerSize

        Write-Output("Container '{0}' with {1} blobs has a sizeof {2:F2} MB." -f $_.CloudBlobContainer.Name,$result.blobCount,($result.containerSize/1MB))
    }
}
else
{
    Write-Warning "No containers found to process in storage account '$StorageAccountName'."
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [toplam Blob Depolama kapsayıcısının boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-size-powershell.md) kapsayıcı boyutu tahmini sağlayan basit bir komut dosyası.

- Azure depolama faturalamasını hakkında daha fazla bilgi için bkz: [anlama Windows Azure depolama Faturalaması](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

- Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

- Ek depolama PowerShell Betiği örnekleri bulabilirsiniz [Azure depolama için PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
