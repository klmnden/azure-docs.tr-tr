---
title: Azure Data Lake Analytics için CI/CD işlem hattı ayarlama | Microsoft Docs
description: Sürekli tümleştirme ve sürekli dağıtım için Azure Data Lake Analytics ayarlama konusunda bilgi edinin.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a55c8fd95409de4fb1ea725d234de907f79d3c7b
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889697"
---
# <a name="how-to-set-up-cicd-pipeline-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için CI/CD işlem hattı ayarlama

Bu belgede, U-SQL işleri ve U-SQL veritabanları için CI/CD işlem hattı ayarlama konusunda bilgi edinin.

## <a name="cicd-for-u-sql-job"></a>U-SQL işi için CI/CD

Visual Studio için Azure Data Lake araçları orgnize U-SQL betikleri yardımcı olan U-SQL projesi türü sağlar. U-SQL kodunuzu yönetmek için U-SQL projesi kullanarak kolayca daha fazla CI/CD senaryoları sağlar.

## <a name="build-u-sql-project"></a>U-SQL projesi oluşturmak

U-SQL projesi karşılık gelen parametre geçirerek MSBuild ile oluşturulabilir. U-SQL projeleri için derleme işlemi ayarlamak için aşağıdaki adımları izleyin.

### <a name="project-migration"></a>Proje geçişi

U-SQL projesi için derleme görevi kurmadan önce U-SQL projesi en son sürümünü kullandığınızdan emin olun. U-SQL projesi dosyası düzenleyicide açıp içeri aktarma öğelerini olup olmadığını denetleyin:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Aksi durumda, projeyi geçirmek için iki seçeneğiniz vardır:

- 1. seçenek: eski yukarıdaki öğesi alınacak değiştirin.
- 2. seçenek: eski proje 2.3.3000.0 sürümü Visual Studio için Azure Data Lake Araçları ' açın. Eski proje şablonu, en son sürüme otomatik olarak yükseltilecektir. Sürüm 2.3.3000.0 sonra yeni oluşturulan projeye doğrudan yeni şablonu kullanır.

### <a name="get-nuget-package"></a>NuGet paketini alma

MSBuild, U-SQL projesi türü için yerleşik destek sağlamaz. Bu özelliği eklemek için çözümünüze bir başvuru eklemeniz gerekir [Microsoft.Azure.DataLake.USQL.SDK Nuget paketini](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) , gerekli dil hizmeti ekler.

NuGet paket başvurusu eklemek için Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **NuGet paketlerini Yönet**. Adlı bir dosya ekleyebilirsiniz `packages.config` çözüm klasöründe ve içine içerikleri ekleyin.

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>U-SQL veritabanı başvuruları yönetme

Sorgu ifadeleri U-SQL veritabanı nesneleri için U-SQL projesi U-SQL betiklerini varsa, örneğin, bir U-SQL tablo sorgulama veya bir derleme başvurusu, önce bu nesneleri tanımını içeren karşılık gelen U-SQL veritabanı projeye başvurması gerekir Bu U-SQL projesi oluşturma.

[U-SQL veritabanı projesi hakkında daha fazla bilgi edinin](data-lake-analytics-data-lake-tools-develop-usql-database.md)

### <a name="build-u-sql-project-with-msbuild-command-line"></a>MSBuild komut satırı ile U-SQL projesi oluşturmak

Proje geçiş ve NuGet paketi alma sonra U-SQL projesi oluşturmak için ek bağımsız değişkenler standart MSBuild komut satırında çağırabilirsiniz:

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder
``` 

Bağımsız değişken tanımı ve değerleri şunlardır:

* USQLSDKPath < U-SQL Nuget paketini > \build\runtime =: Bu parametre için U-SQL dil hizmeti NuGet paketinin yükleme yolu belirtir.
* USQLTargetType birleştirme ya da SyntaxCheck =:
    * Birleştirme: Birleştirme modu .cs .py ve .r dosya ve satır içleri elde edilen kullanıcı tanımlı kod kitaplığı gibi arka plan kod dosyaları derler. (bir dll ikili, Python veya R gibi kodu) içine U-SQL betiği.
    * SyntaxCheck: SyntaxCheck modu ilk U-SQL betiği ile arka plan kod dosyalarında birleştirir ve ardından kodunuzu doğrulamak için U-SQL betiği derler.
* DataRoot =<DataRoot path>: DataRoot SyntaxCheck modu için yalnızca gerekli. Betik SyntaxCheck moduyla oluşturulurken MSBuild betiğindeki veritabanı nesnelere başvurular denetler. Derlemeden önce derleme makinenin DataRoot klasörü U-SQL veritabanından başvurulan nesneleri içeren bir eşleşen yerel ortam ayarladığınızdan emin olun. Bu veritabanı bağımlılıklar da yönetebilirsiniz [bir U-SQL veritabanı projesine başvurma](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project). MSBuild yalnızca veritabanı nesneleri başvurusu, dosyaları değil dikkat edin.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Visual Studio Team Service ile sürekli tümleştirme

Komut satırı yanı sıra Müşteriler ayrıca Visual Studio derleme veya MSBuild görevi Visual Studio Team Service U-SQL projeleri oluşturmak için kullanabilirsiniz. Derleme görevi ayarlamanız için emin olun:

1.  Çözüm için NuGet geri yükleme görevi başvurulan NuGet paketi dahil olmak üzere ekleme `Azure.DataLake.USQL.SDK`, böylece MSBuild U-SQL dil hedefleri bulabilirsiniz. 

    ![U-SQL projesi için Data Lake ayarlamak CI CD MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

2.  VSTS derleme tanımında bu bağımsız değişkenleri tanımlayabilir veya set MSBuild bağımsız değişkenleri ve bağımsız değişkenler aşağıdaki gibi Visual Studio derleme veya MSBuild görevinde ayarlayabilirsiniz.

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/<your project name>/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.1019-preview/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory)
    ```

    ![Data Lake ayarlamak CI CD MSBuild değişkenlerinin U-SQL projesi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

### <a name="u-sql-project-build-output"></a>U-SQL projesi yapı çıkış

Derlemeyi çalıştırdıktan sonra tüm betiklerde U-SQL projesi oluşturulur ve adında bir zip dosyasına yüzdelik `USQLProjectName.usqlpack`. Klasör yapısı, projenizdeki sıkıştırılmış oluşturma çıktısında tutulacak.

>[!NOTE]
>
>Arka plan kod dosyasında her bir U-SQL betiği için satır içi betik yapı çıkışını ifadesine olarak birleştirilir.
>

## <a name="test-u-sql-script"></a>Test U-SQL betiği

Azure Data Lake U-SQL betiği ve C# UDO'su/UDAG/UDF için test projeleri sağlar:
* [U-SQL betiği ve genişletilmiş C# kodu için test çalışmaları eklemeyi öğrenin](data-lake-analytics-cicd-test.md#test-u-sql-scripts)
* [Visual Studio Team Service bu test çalışmalarını çalıştırma hakkında bilgi edinin](data-lake-analytics-cicd-test.md#run-test-cases-in-visual-studio-team-service)

## <a name="u-sql-job-deployment"></a>U-SQL işi dağıtımı

Kod derleme ve test ile doğruladıktan sonra işlem, U-SQL işlerini doğrudan Visual Studio Team Service gönderebildiği **Azure PowerShell görev**. Azure Data Lake Store/Azure Blob depolama alanına da betiği dağıtabilirsiniz ve [Azure Data Factory zamanlanmış işlerinizi](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-visual-studio-team-service"></a>Visual Studio Team Service aracılığıyla U-SQL işlerini gönderme

U-SQL projesi bir zip dosyası adlı derleme çıkışı **USQLProjectName.usqlpack** projedeki tüm bir U-SQL betikleri içerir. Kullanabileceğiniz [Visual Studio Team Service Azure PowserShell görevde](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) ile U-SQL göndermek için örnek PowserShell betiği aşağıda işleri doğrudan Visual Studio Team Service derleme veya yayın işlem hattı.

```powershell
param(
    [Parameter(Mandatory=$true)][string]$AnalyticsAccountName, #ADLA account name
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, #Root folder (e.g. artifacts root folder)
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
        # [System.IO.Compression.ZipFile]::ExtractToDirectory($USQLPackfile, $UnzipOutput, 0)
        # $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
        # Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue | Where-Object {$_.DirectoryName -match $subFolder}

    return $USQLFiles
}

Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."
    # submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "($usqlFiles.IndexOf())Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzureRmDataLakeAnalyticsJob -Account $AnalyticsAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        LogJobInformation $jobToSubmit
        
        Write-Output "waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzureRmDataLakeAnalyticsJob -Account $AnalyticsAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
        
        # ProcessResult $jobResult
    }
}

Function ProcessResult($jobResult)
{
    if ($jobResult.Result -eq "Failed")
    {
        Write-Error "Job Failed. Job Id: $($jobResult.JobId), Job Name: $($jobResult.Name), Log: $($jobResult.LogFolder)"
    }
    else
    {
        Write-Output "Job Succeeded. Job Id: $($jobResult.JobId), Job Name: $($jobResult.Name)"
    }
}

Function LogJobInformation($jobInfo)
{
    Write-Output "************************************************************************"
    Write-Output ([string]::Format("Job Id: {0}", $(DefaultIfNull $jobInfo.JobId)))
    Write-Output ([string]::Format("Job Name: {0}", $(DefaultIfNull $jobInfo.Name)))
    Write-Output ([string]::Format("Job State: {0}", $(DefaultIfNull $jobInfo.State)))
    Write-Output ([string]::Format("Job Started at: {0}", $(DefaultIfNull  $jobInfo.StartTime)))
    Write-Output ([string]::Format("Job Ended at: {0}", $(DefaultIfNull  $jobInfo.EndTime)))
    Write-Output ([string]::Format("Job Result: {0}", $(DefaultIfNull $jobInfo.Result)))
    Write-Output "************************************************************************"
}

Function DefaultIfNull($item)
{
    if ($item -ne $null)
    {
        return $item
    }
    return ""
}

Function Main()
{

    Write-Output "Starting USQL script deployment..."

    # Submit ADLA jobs with usql scripts in given sub-folder.
    # Order is important here. Scripts with least dependency goes first followed 
    # by scripts which more dependencies.
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>U-SQL işlerini Azure Data Factory aracılığıyla dağıtma

Bunun yanında, doğrudan Visual Studio Team Service U-SQL işlerini gönderme ayrıca yerleşik betikleri Azure Data Lake Store/Azure Blob depolama alanına yükleyebilirsiniz ve [Azure Data Factory zamanlanmış işlerinizi](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Kullanım [Visual Studio Team Service Azure PowerShell görevde](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) ile U-SQL karşıya yüklemek için aşağıdaki örnek PowerShell Betiği, Azure Data Lake Store hesabına komutlar.

```powershell
param(
    [Parameter(Mandatory=$true)][string]$ADLSName, #ADLA account name
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot #Root folder (e.g. artifacts root folder)
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading DLL files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles
    Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $usqlScripts.FullName -Destination "/ScriptResource2/$usqlScripts" -Force -Recurse
    # $files = @(get-childitem $usqlScripts -recurse)
    # foreach($file in $files)
    # {
    #    Write-Host "Uploading file: $($file.Name)"
    #    Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/ScriptResource/$file" -Force -Recurse
    # }
}

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    return $UnzipOutput
}

UploadResources
```

## <a name="cicd-for-u-sql-database"></a>U-SQL veritabanı için CI/CD

U-SQL veritabanı projesi şablonu geliştirmek, yönetmek ve U-SQL veritabanları, hızlı ve kolay bir şekilde dağıtmak için geliştiricilerin yardımcı olan Visual Studio için Azure Data Lake araçları sağlar. [U-SQL veritabanı projesi hakkında daha fazla bilgi](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>U-SQL veritabanı projesi derleme

### <a name="get-nuget-package"></a>NuGet paketini alma

MSBuild, U-SQL veritabanı proje türü için yerleşik destek sağlamaz. Bu özelliği eklemek için çözümünüze bir başvuru eklemeniz gerekir [Microsoft.Azure.DataLake.USQL.SDK Nuget paketini](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) , gerekli dil hizmeti ekler.

NuGet paket başvurusu ekleme için sizin Çözüm Gezgini'nde çözüme sağ tıklayın ve seçin **NuGet paketlerini Yönet** çözümü için ardından aramak ve NuGet paketini yükleyin. Çözüm klasöründe "packages.config" adlı bir dosya ekleyin ve içine içerikleri ekleyin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-database-project-with-msbuild-command-line"></a>MSBuild komut satırı ile U-SQL veritabanı projesi derleme

Standart MSBuild komut satırını ve U-SQL veritabanı projenizi yapılandırmak için ek bağımsız değişken olarak U-SQL SDK'sı NuGet paketi başvurusu gibi geçişi çağırabilirsiniz:

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Bağımsız değişkenler `USQLSDKPath=<U-SQL Nuget package>\build\runtime` U-SQL dil hizmeti için NuGet paketinin yükleme yolu gösterir.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Visual Studio Team Service ile sürekli tümleştirme

Komut satırı yanı sıra müşteriler de kullanabilirsiniz **Visual Studio derleme** veya **MSBuild görevi** U-SQL veritabanı projeleri Visual Studio Team Service içinde oluşturulacak. Derleme görevi ayarlamanız için emin olun:

1.  Çözüm için NuGet geri yükleme görevi başvurulan NuGet paketi dahil olmak üzere ekleme `Azure.DataLake.USQL.SDK`, böylece MSBuild U-SQL dil hedefleri bulabilirsiniz. 

    ![U-SQL veritabanı projesi için Data Lake kümesi CI CD MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

2.  VSTS derleme tanımında bu bağımsız değişkenleri tanımlayabilir veya set MSBuild bağımsız değişkenleri ve bağımsız değişkenler aşağıdaki gibi Visual Studio derleme veya MSBuild görevinde ayarlayabilirsiniz.

```
/p:USQLSDKPath=$(Build.SourcesDirectory)/<your project name>/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.1019-preview/build/runtime
```

![Data Lake U-SQL veritabanı projesi için CI CD MSBuild değişkenleri ayarlama](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

### <a name="u-sql-database-project-build-output"></a>U-SQL veritabanı projesi derleme çıkışı

U-SQL veritabanı projesi sonekiyle adlı bir U-SQL veritabanı dağıtım paketi için çıkış derleme `.usqldbpack`. `.usqldbpack` Pakettir bir zip dosyası içeren tüm deyimler DDL klasör ve tüm .dll ve ek dosyaları tek bir U-SQL betiği Temp klasörünün içindeki derlemeler için.

## <a name="test-table-valued-function-and-stored-procedure"></a>Tablo değerli işlev testi ve saklı yordam

Tablo değerli işlevler ve saklı yordamlar için test çalışmalarını doğrudan ekleme şu anda desteklenmiyor. Geçici bir çözüm olarak, bu işlevler çağırma U-SQL betikleri sahip bir U-SQL projesi oluşturun ve test çalışmaları için yazma. Tablo değerli işlevler ve U-SQL veritabanı projede tanımlanan saklı yordamlar için test çalışmaları ayarlamak için aşağıdaki adımları izleyin:

1.  Test amaçlı U-SQL projesi oluşturun ve saklı yordamlar ve tablo değerli işlevler çağırma U-SQL betikleri yazın.
2.  Bu U-SQL projesi için veritabanı başvurusu ekleyin. Tablo değerli işlev ve saklı yordam tanımında alabilmek için DDL deyimi içeren veritabanı projesine başvurmanız gerekir. [Veritabanı başvurusu hakkında daha fazla bilgi](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Tablo değerli işlevler ve saklı yordamları çağıran bir U-SQL betikleri için test çalışmaları ekleyin. [U-SQL betiği için test çalışmaları eklemeyi öğrenin](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-visual-studio-team-service"></a>Visual Studio Team Service aracılığıyla U-SQL veritabanı dağıtma

`PackageDeploymentTool.exe` programlama ve U-SQL veritabanı dağıtım package(.usqldbpack) dağıtmaya yardımcı komut satırı arabirimi sağlar. SDK'sı dahil [U-SQL SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), build/runtime/PackageDeploymentTool.exe adresindeki bulmayla. Kullanarak `PackageDeploymentTool.exe`, Azure Data Lake Analytics ve yerel hesap için U-SQL veritabanlarını dağıtabilirsiniz.

>[!NOTE]
>
>PowerShell komut satırı desteğini ve Visual Studio Team Service U-SQL veritabanı dağıtım görevi desteği yayın üzerinde bir yoludur.
>

Visual Studio Team Service veritabanı dağıtım görevi ayarlamak için aşağıdaki adımları izleyin:

1. Derlemede bir PowerShell Betiği görev ekleyin veya yayın işlem ve PowerShell betiğini yürütün. Bu görev için Azure SDK'sı bağımlılıkları almak için yardımcı `PackageDeploymentTool.exe`. Bu bağımlılıklar bazı belirli bir klasöre yüklemek için - outputfolder parametresini belirleyebilirsiniz. Bu klasör yolunu geçirin `PackageDeploymentTool.exe` 2. adımda. 

    ```powershell
    param (
        [string]$outputfolder = "RequiredDll",
        [string]$workingfolder = ""
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.2.3-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.3.3-preview -outf Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.3.3-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force

    echo "Copy required DLLs to output folder..."

    mkdir $outputfolder -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview\lib\net452\*.dll $outputfolder
    copy Microsoft.Azure.Management.DataLake.Store.2.3.3-preview\lib\net452\*.dll $outputfolder
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $outputfolder
    ```

2. Ekleme bir **komut satırı görevi** içinde derleme veya yayın işlem hattı ve betik çağırma doldurun `PackageDeploymentTool.exe`. Örnek komut aşağıdaki gibidir: 

    * U-SQL veritabanını yerel olarak dağıtma

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Etkileşimli kimlik doğrulaması modu, Azure Data Lake Analytics hesabı için U-SQL veritabanı dağıtmak için kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Azure Data Lake Analytics hesabı için U-SQL veritabanı dağıtmak için kimlik doğrulaması secrete kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete>
        ```

    * CertFile kimlik doğrulaması, Azure Data Lake Analytics hesabı için U-SQL veritabanı dağıtmak için kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

**PackageDeploymentTool.exe parametre açıklaması:**

**Ortak parametreleri:**

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|Paket|Dağıtılacak U-SQL veritabanı dağıtım paketi yolu|Null|true|
|Database|Veritabanı adı dağıtılacak şekilde / veya oluşturulan|ana|false|
|Günlük dosyası|Oturum, varsayılan olarak standart çıkış (konsol) dosyasının yolu|Null|false|
|LogLevel|Günlük düzeyi: ayrıntılı, Normal, uyarı, hata|LogLevel.Normal|false|

**Parametre yerel dağıtımı için:**

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|DataRoot|Yerel veri kök klasörünün yolu|Null|true|

**Azure Data Lake Analytics dağıtım parametresi:**

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|Hesap|Hangi Azure Data Lake Analytics hesabı tarafından adına dağıtma belirtir|Null|true|
|ResourceGroup|Azure Data Lake Analytics hesabı için Azure kaynak grubu adı|Null|true|
|SubscriptionId|Azure Data Lake Analytics hesabı için Azure abonelik kimliği|Null|true|
|Kiracı|Kiracı adı (AAD dizin etki alanı adı, bulabilirsiniz, Abonelik Yönetimi sayfasında Azure Portalı'nda)|Null|true|
|AzureSDKPath|Azure SDK'sı bağımlı derlemelerin aranacağı yol|Null|true|
|Etkileşimli|Etkileşimli mod veya kimlik doğrulaması için kullanma|false|false|
|ClientID|Hiçbiri AAD uygulama kimliği hiçbiri için gerekli etkileşimli kimlik doğrulaması, etkileşimli kimlik doğrulaması|Null|hiçbiri için gerekli etkileşimli kimlik doğrulaması|
|Secrete|Hiçbiri için secrete/parola etkileşimli kimlik doğrulaması, yalnızca kullanması gereken güvenilen ve güvenli bir ortamda|Null|hiçbiri için gerekli etkileşimli kimlik doğrulaması veya SecreteFile kullanın|
|SecreteFile|Dosya secrete/parola hiçbiri için etkileşimli kimlik doğrulaması kaydeder, geçerli kullanıcı tarafından salt okunabilir sakladığınızdan emin olun|Null|hiçbiri için gerekli etkileşimli kimlik doğrulaması veya Secrete kullanın|
|CertFile|Varsayılan dosya kaydeder hiçbiri için X.509 Sertifika etkileşimli kimlik doğrulaması, istemci kullanılacak kimlik doğrulaması secrete|Null|false|
|JobPrefix|U-SQL DDL işlemi veritabanı dağıtımı için önek|Deploy_ + DateTime.Now|false|

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md)
- [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
- [U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın](data-lake-analytics-data-lake-tools-develop-usql-database.md)