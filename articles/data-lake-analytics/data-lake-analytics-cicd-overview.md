---
title: Azure Data Lake Analytics için bir CI/CD işlem hattı ayarlama
description: Sürekli tümleştirme ve sürekli dağıtım için Azure Data Lake Analytics ayarlama konusunda bilgi edinin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 09/14/2018
ms.openlocfilehash: b035be727df2dfecb613da79681affd740c69bec
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60333885"
---
# <a name="how-to-set-up-a-cicd-pipeline-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için bir CI/CD işlem hattı ayarlama  

Bu makalede, bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı U-SQL işleri ve U-SQL veritabanları için ayarlama konusunda bilgi edinin.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-cicd-for-u-sql-jobs"></a>U-SQL işleri için CI/CD kullanma

Visual Studio için Azure Data Lake araçları, U-SQL betikleri düzenlemenize yardımcı olur U-SQL projesi türü sağlar. U-SQL kodunuzu yönetmek için U-SQL projesi kullanarak daha fazla CI/CD senaryoları kolaylaştırır.

## <a name="build-a-u-sql-project"></a>U-SQL projesi oluşturmak

U-SQL projesi kılan Microsoft Build Engine (MSBuild) ile ilgili parametreler geçirerek oluşturulabilir. U-SQL projesi için bir yapı işlemi ayarlamak için bu makaledeki adımları izleyin.

### <a name="project-migration"></a>Proje geçişi

U-SQL projesi için bir derleme görevi ayarlama önce U-SQL projesi en son sürümüne sahip olduğunuzdan emin olun. U-SQL projesi dosya Düzenleyicisi'nde açın ve bunlar sahip olduğunuzu doğrulayın öğeleri al:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Aksi durumda, projeyi geçirmek için iki seçeneğiniz vardır:

- 1\. seçenek: Eski içeri aktarma öğesi önceki adlarıyla değiştirin.
- 2\. seçenek: Eski proje, Visual Studio için Azure Data Lake Araçları ' açın. 2\.3.3000.0 yeni bir sürümü kullanın. Eski proje şablonu, en son sürüme otomatik olarak yükseltilecektir. 2\.3.3000.0 yeni sürümleri ile oluşturulan yeni projeler yeni şablonu kullanın.

### <a name="get-nuget"></a>Get NuGet

MSBuild, U-SQL projeleri için yerleşik destek sağlamaz. Bu destek almak için çözümünüze için bir başvuru eklemeniz gerekir [Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) gerekli dil hizmeti ekleyen bir NuGet paketi.

NuGet paket başvurusu eklemek için Visual Studio Çözüm Gezgini'nde çözüme sağ tıklayıp seçin **NuGet paketlerini Yönet**. Adlı bir dosya ekleyebilirsiniz `packages.config` Çözüm klasörü ve aşağıdaki içeriği içine put:

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180620" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>U-SQL veritabanı başvuruları yönetme

U-SQL betiklerini bir U-SQL projesi U-SQL veritabanı nesneleri için sorgu ifadeleri olabilir. Bu durumda, U-SQL projesi oluşturmadan önce nesnelerin tanımı içeren karşılık gelen U-SQL veritabanı projeye başvurması gerekir. Örnek bir U-SQL tablo sorgulamak veya bir derleme başvurusu olur. 

Daha fazla bilgi edinin [U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md).

>[!NOTE]
>BIRAKMA deyimi yanlışlıkla silme sorunu neden olabilir. BIRAKMA deyimi etkinleştirmek için MSBuild bağımsız değişkenleri açıkça belirtmeniz gerekir. **AllowDropStatement** derleme ve bırakma gibi veri olmayan ilgili bırak işlemi etkinleştirecek tablo değerli işlev. **AllowDataDropStatement** etkinleştirecek bırakma işlemi, bırakma table ve schema bırakma gibi ilgili verileri. AllowDropStatement AllowDataDropStatement kullanmadan önce etkinleştirmeniz gerekir.
>

### <a name="build-a-u-sql-project-with-the-msbuild-command-line"></a>MSBuild komut satırı ile bir U-SQL projesi oluşturmak

Önce projeyi geçirmek ve NuGet paketini alın. Sonra U-SQL projesi oluşturmak için aşağıdaki ek bağımsız değişkenler standart MSBuild komut satırında çağırın: 

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder;/p:EnableDeployment=true
``` 

Bağımsız değişken tanımı ve değerler aşağıdaki gibidir:

* **USQLSDKPath =\<U-SQL Nuget paketini > \build\runtime**. Bu parametre, U-SQL dil hizmeti için NuGet paketinin yükleme yolu belirtir.
* **USQLTargetType birleştirme ya da SyntaxCheck =** :
    * **Birleştirme**. Birleştirme modu arka plan kod dosyaları derler. Örnekler **.cs**, **.py**, ve **.r** dosyaları. Bu satır içleri U-SQL betiğini elde edilen kullanıcı tanımlı kod kitaplığa. Örnekler bir dll ikili, Python veya R kodu.
    * **SyntaxCheck**. SyntaxCheck modu, arka plan kod dosyaları ilk U-SQL betiği ile birleştirir. Sonra kodunuzu doğrulamak için U-SQL betiği derler.
* **DataRoot =\<DataRoot yolu >** . DataRoot yalnızca SyntaxCheck modu için gereklidir. Betik SyntaxCheck moduyla oluşturduğunda, MSBuild betiğindeki veritabanı nesnelere başvurular denetler. Yapılandırmadan önce başvurulan derleme makinenin DataRoot klasörü U-SQL veritabanında nesneleri içeren bir eşleşen yerel ortamı ayarlayın. Bu veritabanı bağımlılıklar da yönetebilirsiniz [bir U-SQL veritabanı projesine başvurma](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project). MSBuild, veritabanı nesne başvuruları, dosyaları değil yalnızca denetler.
* **EnableDeployment = true** veya **false**. EnableDeployment, derleme işlemi sırasında başvurulan U-SQL veritabanı dağıtmak için izin verip vermediğini belirtir. U-SQL veritabanı projesi başvuru ve veritabanı nesnelerini U-SQL betiğinizde kullanmak, bu parametre kümesine **true**.

### <a name="continuous-integration-through-azure-pipelines"></a>Azure işlem hatları aracılığıyla sürekli tümleştirme

Komut satırında ek olarak, Azure işlem hatları, U-SQL projesi oluşturmak için de Visual Studio derleme veya bir MSBuild görevi kullanabilirsiniz. Bir derleme işlem hattı ayarlayın için derleme işlem hattı, iki görevi eklediğinizden emin olun: NuGet geri yükleme görevi ve bir MSBuild görevi.

![U-SQL projesi için MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

1.  İçeren çözüm başvurulan NuGet paketini almak için NuGet geri yükleme görev eklemek `Azure.DataLake.USQL.SDK`, böylece MSBuild U-SQL dil hedefleri bulabilirsiniz. Ayarlama **Gelişmiş** > **hedef dizin** için `$(Build.SourcesDirectory)/packages` MSBuild bağımsız değişkenleri örnek 2. adımda doğrudan kullanmak istiyorsanız.

    ![U-SQL projesi için NuGet geri yükleme görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  MSBuild bağımsız değişkenleri aşağıdaki örnekte gösterildiği gibi Visual Studio derleme araçları ya da bir MSBuild görevi ayarlayın. Veya Azure işlem hatları derleme işlem hattı, bu bağımsız değişkenleri tanımlayabilirsiniz.

    ![U-SQL projesi için CI/CD MSBuild değişkenleri tanımlayın](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory) /p:EnableDeployment=true
    ```

### <a name="u-sql-project-build-output"></a>U-SQL projesi yapı çıkış

Bir derlemeyi çalıştırdıktan sonra tüm betiklerde U-SQL projesi oluşturulur ve çıktı olarak adlandırılan bir zip dosyasına `USQLProjectName.usqlpack`. Klasör yapısı, projenizdeki sıkıştırılmış oluşturma çıktısında tutulur.

> [!NOTE]
>
> Her bir U-SQL komut dosyası için arka plan kod dosyaları, betik derleme çıkışı için bir satır içi deyimi olarak birleştirilir.
>

## <a name="test-u-sql-scripts"></a>U-SQL betikleri test

Azure Data Lake, U-SQL betikleri ve C# UDO'su/UDAG/UDF için test projeleri sağlar:
* Bilgi edinmek için nasıl [U-SQL betikleri ve genişletilmiş C# kodu için test çalışmalarını Ekle](data-lake-analytics-cicd-test.md#test-u-sql-scripts).
* Bilgi edinmek için nasıl [Azure işlem hatlarında test çalışmaları](data-lake-analytics-cicd-test.md#run-test-cases-in-azure-devops).

## <a name="deploy-a-u-sql-job"></a>U-SQL işi dağıtma

Kod derleme ve test sürecinde doğruladıktan sonra U-SQL işleri bir Azure PowerShell görev aracılığıyla Azure işlem hatlarını doğrudan gönderebilirsiniz. Azure Data Lake Store veya Azure Blob depolama alanına da betiği dağıtabilirsiniz ve [Azure Data Factory zamanlanmış işlerinizi](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-azure-pipelines"></a>Azure işlem hatları ile U-SQL işlerini gönderme

U-SQL projesi bir zip dosyası adlı derleme çıkışı **USQLProjectName.usqlpack**. Zip dosyasını projeye tüm bir U-SQL betikleri içerir. Kullanabileceğiniz [Azure PowerShell görev](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) , işlem hattı aşağıdaki örnek PowerShell Betiği ile U-SQL işlerini Azure işlem hatlarını doğrudan göndermek için.

```powershell
<#
    This script can be used to submit U-SQL Jobs with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and submit all scripts one-by-one.

    Note: the code behind file for each U-SQL script will be merged into the built U-SQL script in build output.
          
    Example :
        USQLJobSubmission.ps1 -ADLAAccountName "myadlaaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\" -DegreeOfParallelism 2
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLAAccountName, # ADLA account name to submit U-SQL jobs
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

## Get U-SQL scripts in U-SQL project build output(.usqlpack file)
Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue

    return $USQLFiles
}

## Submit U-SQL scripts to ADLA account one-by-one
Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."

    # Submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzDataLakeAnalyticsJob -Account $ADLAAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        
        LogJobInformation $jobToSubmit
        
        Write-Output "Waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzDataLakeAnalyticsJob -Account $ADLAAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
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
    Write-Output ([string]::Format("ADLA account: {0}", $ADLAAccountName))
    Write-Output ([string]::Format("Root folde for usqlpack: {0}", $ArtifactsRoot))
    Write-Output ([string]::Format("AU count: {0}", $DegreeOfParallelism))

    Write-Output "Starting USQL script deployment..."
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>U-SQL işlerini Azure Data Factory aracılığıyla dağıtma

U-SQL işlerini Azure işlem hatlarını doğrudan gönderebilirsiniz. Veya Azure Data Lake Store veya Azure Blob Depolama için oluşturulan komut dosyalarını karşıya yükleyebilirsiniz ve [Azure Data Factory zamanlanmış işlerinizi](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Kullanım [Azure PowerShell görev](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) Azure U-SQL betikleri bir Azure Data Lake Store hesabına yüklemek için işlem hatları ile aşağıdaki örnek PowerShell Betiği olarak:

```powershell
<#
    This script can be used to upload U-SQL files to ADLS with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and upload all scripts to ADLS one-by-one.
          
    Example :
        FileUpload.ps1 -ADLSName "myadlsaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\"
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLSName, # ADLS account name to upload U-SQL scripts
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DestinationFolder = "USQLScriptSource" # Destination folder in ADLS
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles

    $files = @(get-childitem $usqlScripts -recurse)
    foreach($file in $files)
    {
        Write-Host "Uploading file: $($file.Name)"
        Import-AzDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/$(Join-Path $DestinationFolder $file)" -Force
    }
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

    return Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue
}

UploadResources
```

## <a name="cicd-for-a-u-sql-database"></a>U-SQL veritabanı için CI/CD

Visual Studio için Azure Data Lake araçları, geliştirme, yönetme ve U-SQL veritabanları dağıtma yardımcı olan U-SQL veritabanı proje şablonları sağlar. Daha fazla bilgi edinin bir [U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>U-SQL veritabanı projesi derleme

### <a name="get-the-nuget-package"></a>NuGet paketini alma

MSBuild, U-SQL veritabanı projeleri için yerleşik destek sağlamaz. Bu özelliği almak için çözümünüz için bir başvuru eklemeniz gerekir [Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) gerekli dil hizmeti ekleyen bir NuGet paketi.

NuGet paket başvurusu eklemek için Visual Studio Çözüm Gezgini'nde çözüme sağ tıklayın. Seçin **NuGet paketlerini Yönet**. Öğesini arayın ve NuGet paketini yükleyin. Adlı bir dosya ekleyebilirsiniz **packages.config** Çözüm klasörü ve aşağıdaki içeriği içine put:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-a-database-project-with-the-msbuild-command-line"></a>U-SQL veritabanı projesi MSBuild komut satırı ile derleme

U-SQL veritabanı projenizi oluşturmak için standart MSBuild komut satırını arayın ve U-SQL SDK'sı NuGet paketi başvurusu ek bağımsız değişken olarak geçirin. Aşağıdaki örneğe bakın: 

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Bağımsız değişken `USQLSDKPath=<U-SQL Nuget package>\build\runtime` U-SQL dil hizmeti için NuGet paketinin yükleme yolu gösterir.

### <a name="continuous-integration-with-azure-pipelines"></a>Azure işlem hatları ile sürekli tümleştirme

Komut satırında ek olarak, Azure işlem hatları, U-SQL veritabanı projeleri derlemek için Visual Studio derleme veya bir MSBuild görevi kullanabilirsiniz. Yapı görev oluşturmak için derleme işlem hattı, iki görevi eklediğinizden emin olun: NuGet geri yükleme görevi ve bir MSBuild görevi.

   ![U-SQL projesi için CI/CD MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 


1. İçeren çözüm başvurulan NuGet paketini almak için NuGet geri yükleme görev eklemek `Azure.DataLake.USQL.SDK`, böylece MSBuild U-SQL dil hedefleri bulabilirsiniz. Ayarlama **Gelişmiş** > **hedef dizin** için `$(Build.SourcesDirectory)/packages` MSBuild bağımsız değişkenleri örnek 2. adımda doğrudan kullanmak istiyorsanız.

   ![U-SQL projesi için CI/CD NuGet görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2. MSBuild bağımsız değişkenleri aşağıdaki örnekte gösterildiği gibi Visual Studio derleme araçları ya da bir MSBuild görevi ayarlayın. Veya Azure işlem hatları derleme işlem hattı, bu bağımsız değişkenleri tanımlayabilirsiniz.

   ![U-SQL veritabanı projesi için CI/CD MSBuild değişkenleri tanımlayın](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

   ```
   /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime
   ```
 
### <a name="u-sql-database-project-build-output"></a>U-SQL veritabanı projesi derleme çıkışı

U-SQL veritabanı projesi sonekiyle adlı bir U-SQL veritabanı dağıtım paketi için çıkış derleme `.usqldbpack`. `.usqldbpack` Tek bir U-SQL betiği bir DDL klasördeki tüm DDL deyimleri içeren bir zip dosyası bir pakettir. Tüm içeren **.dll** ve ek dosyaları geçici bir klasörde derleme.

## <a name="test-table-valued-functions-and-stored-procedures"></a>Test tablo değerli işlevler ve saklı yordamlar

Tablo değerli işlevler ve saklı yordamlar için test çalışmalarını doğrudan ekleme şu anda desteklenmemektedir. Geçici bir çözüm olarak, bu işlevler ve için test durumlarını yazmak ve U-SQL betiklerini içeren bir U-SQL projesi oluşturabilirsiniz. Tablo değerli işlevler ve U-SQL veritabanı projede tanımlanan saklı yordamlar için test çalışmaları ayarlamak için aşağıdaki adımları uygulayın:

1.  Test amacıyla bir U-SQL projesi oluşturun ve saklı yordamlar ve tablo değerli işlevler çağırma U-SQL betikleri yazma.
2.  U-SQL projesi için veritabanı başvurusu ekleyin. Tablo değerli işlev ve saklı yordam tanımında almak için DDL deyimi içeren veritabanı projesine başvurmanız gerekir. Daha fazla bilgi edinin [veritabanı başvuruları](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Tablo değerli işlevler ve saklı yordamları çağıran bir U-SQL betikleri için test çalışmaları ekleyin. Bilgi edinmek için nasıl [U-SQL betikleri için test çalışmalarını Ekle](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-azure-pipelines"></a>Azure işlem hatları ile U-SQL veritabanı dağıtma

`PackageDeploymentTool.exe` komut satırı arabirimi, U-SQL veritabanı dağıtım paketleri, dağıtmak ve programlama sağlar **.usqldbpack**. SDK'sı dahil [U-SQL SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)konumunda bulunan **build/runtime/PackageDeploymentTool.exe**. Kullanarak `PackageDeploymentTool.exe`, U-SQL veritabanları Azure Data Lake Analytics ve yerel hesaplar için dağıtabilirsiniz.

> [!NOTE]
>
> U-SQL veritabanı dağıtımı şu anda beklemede PowerShell komut satırı desteğiyle ve Azure işlem hatları bırakma görevi desteği.
>

Bir veritabanı dağıtım görevi Azure işlem hatları ayarlamak için aşağıdaki adımları uygulayın:

1. Bir yapı içinde bir PowerShell Betiği görev ekleyin veya yayın işlem ve aşağıdaki PowerShell betiğini yürütün. Bu görev için Azure SDK'sı bağımlılıkları almak için yardımcı `PackageDeploymentTool.exe` ve `PackageDeploymentTool.exe`. Ayarlayabileceğiniz **- AzureSDK** ve **- DBDeploymentTool** belirli klasörlere dağıtım aracı ve bağımlılıklarını yüklemek için parametreleri. Geçirmek **- AzureSDK** yolu `PackageDeploymentTool.exe` olarak **- AzureSDKPath** parametre 2. adımda. 

    ```powershell
    <#
        This script is used for getting dependencies and SDKs for U-SQL database deployment.
        PowerShell command line support for deploying U-SQL database package(.usqldbpack file) will come soon.
        
        Example :
            GetUSQLDBDeploymentSDK.ps1 -AzureSDK "AzureSDKFolderPath" -DBDeploymentTool "DBDeploymentToolFolderPath"
    #>

    param (
        [string]$AzureSDK = "AzureSDK", # Folder to cache Azure SDK dependencies
        [string]$DBDeploymentTool = "DBDeploymentTool", # Folder to cache U-SQL database deployment tool
        [string]$workingfolder = "" # Folder to execute these command lines
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.5.1-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.4.1-preview -outf Microsoft.Azure.Management.DataLake.Store.2.4.1-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip
    iwr https://www.nuget.org/api/v2/package/Newtonsoft.Json/6.0.8 -outf Newtonsoft.Json.6.0.8.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.DataLake.USQL.SDK/ -outf USQLSDK.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.4.1-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.4.1-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force
    Expand-Archive Newtonsoft.Json.6.0.8.zip -DestinationPath Newtonsoft.Json.6.0.8 -Force
    Expand-Archive USQLSDK.zip -DestinationPath USQLSDK -Force

    echo "Copy required DLLs to output folder..."

    mkdir $AzureSDK -Force
    mkdir $DBDeploymentTool -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.Azure.Management.DataLake.Store.2.4.1-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $AzureSDK
    copy Newtonsoft.Json.6.0.8\lib\net45\*.dll $AzureSDK
    copy USQLSDK\build\runtime\*.* $DBDeploymentTool
    ```

2. Ekleme bir **komut satırı görevi** bir derleme veya yayın işlem hattı ve dolgu çağırarak betiğinde `PackageDeploymentTool.exe`. `PackageDeploymentTool.exe` bulunduğu altında tanımlanmış **$DBDeploymentTool** klasör. Örnek komut aşağıdaki gibidir: 

    * Yerel olarak bir U-SQL veritabanı dağıtın:

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Etkileşimli kimlik doğrulaması modu için bir Azure Data Lake Analytics hesabı bir U-SQL veritabanı dağıtmak için kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Kullanım **secrete** kimlik doğrulaması için bir Azure Data Lake Analytics hesabı bir U-SQL veritabanı dağıtmak için:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Kullanım **certFile** kimlik doğrulaması için bir Azure Data Lake Analytics hesabı bir U-SQL veritabanı dağıtmak için:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

### <a name="packagedeploymenttoolexe-parameter-descriptions"></a>PackageDeploymentTool.exe parametre açıklamaları

#### <a name="common-parameters"></a>Ortak parametreleri

| Parametre | Açıklama | Varsayılan Değer | Gerekli |
|---------|-----------|-------------|--------|
|Paket|Dağıtılacak U-SQL veritabanı dağıtım paketi yolu.|Null|true|
|Database|Dağıtılan ya da oluşturulan veritabanı adı.|ana|false|
|Günlük dosyası|Günlük dosyasının yolu. Varsayılan olarak standart çıkış (konsol).|Null|false|
|LogLevel|Günlük düzeyi: Ayrıntılı, Normal, uyarı veya hata.|LogLevel.Normal|false|

#### <a name="parameter-for-local-deployment"></a>Parametresi için yerel dağıtımı

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|DataRoot|Yerel veri kök klasörünün yolu.|Null|true|

#### <a name="parameters-for-azure-data-lake-analytics-deployment"></a>Azure Data Lake Analytics dağıtımı için parametreleri

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|Hesap|Hesap adına göre dağıtmak için Azure Data Lake Analytics hesabını belirtir.|Null|true|
|ResourceGroup|Azure Data Lake Analytics hesabı için Azure kaynak grubu adı.|Null|true|
|SubscriptionId|Azure Data Lake Analytics hesabı için Azure abonelik kimliği.|Null|true|
|Kiracı|Kiracı adı, Azure Active Directory (Azure AD) etki alanı adıdır. Azure portalında abonelik yönetimi sayfasındaki bulun.|Null|true|
|AzureSDKPath|Azure SDK'sı bağımlı derlemelerin aranacağı yol.|Null|true|
|Etkileşimli|Gerekmediğini etkileşimli mod kimlik doğrulaması için kullanılacak.|false|false|
|ClientId|Azure AD uygulama kimliği etkileşimli olmayan kimlik doğrulaması için gereklidir.|Null|Etkileşimli olmayan kimlik doğrulaması için gereklidir.|
|Secrete|Secrete veya etkileşimli olmayan kimlik doğrulaması için parola. Yalnızca güvenilen ve güvenli ortamında kullanılmalıdır.|Null|Kullanım SecreteFile yoksa etkileşimli olmayan kimlik doğrulaması için gereklidir.|
|SecreteFile|Dosyayı secrete veya etkileşimli olmayan kimlik doğrulaması için parola kaydeder. Yalnızca geçerli kullanıcı tarafından okunabilen sakladığınızdan emin olun.|Null|Kullanım Secrete yoksa etkileşimli olmayan kimlik doğrulaması için gereklidir.|
|CertFile|Dosya X.509 Sertifika etkileşimli olmayan kimlik doğrulama için kaydeder. Varsayılan istemci kullanmaktır secrete kimlik doğrulaması.|Null|false|
| JobPrefix | U-SQL DDL işin veritabanı dağıtımı için önek. | Deploy_ + DateTime.Now | false |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Analytics kodunuzu test etmek nasıl](data-lake-analytics-cicd-test.md).
- [U-SQL betiğini yerel makinenizde çalıştırma](data-lake-analytics-data-lake-tools-local-run.md).
- [U-SQL veritabanı geliştirme için U-SQL veritabanı proje kullanmak](data-lake-analytics-data-lake-tools-develop-usql-database.md).
