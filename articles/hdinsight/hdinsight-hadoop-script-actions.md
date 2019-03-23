---
title: HDInsight - Azure ile betik eylemi geliştirme
description: Betik eylemi Hadoop kümeleriyle özelleştirmeyi öğrenin. Betik eylemi, bir Hadoop kümesinde çalışan ek yazılımlar yüklemek veya bir kümeye yüklü uygulamaların yapılandırmasını değiştirmek için kullanılabilir.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: d8f7808401b2e11a38b239a353e3b7af2ffcffb3
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361311"
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Betik eylemi betikleri HDInsight Windows tabanlı kümeler için geliştirme
HDInsight için betik eylemi betikleri yazmayı öğrenin. Betik eylemi betikleri kullanma hakkında daha fazla bilgi için bkz. [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md). Linux tabanlı HDInsight kümeleri için yazılmış aynı makalesi için bkz [HDInsight için betik eylemi geliştirme betikleri](hdinsight-hadoop-script-actions-linux.md).


> [!IMPORTANT]  
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı HDInsight kümeleri için çalışır. HDInsight yalnızca Windows üzerinde HDInsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Betik eylemleri ile Linux tabanlı kümeler hakkında daha fazla bilgi için bkz. [(Linux) HDInsight ile betik eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).


Betik eylemi, bir Apache Hadoop kümesinde çalışan ek yazılımlar yüklemek veya bir kümeye yüklü uygulamaların yapılandırmasını değiştirmek için kullanılabilir. Betik eylemleri HDInsight kümeleri dağıtırken, küme düğümleri üzerinde çalışan betikleridir ve kümedeki düğümlerin HDInsight yapılandırmasını tamamladıktan sonra yürütülür. Betik eylemi sistem Yönetici hesap ayrıcalığı altında yürütülür ve küme düğümleri için tam erişim hakları sağlar. Her küme içinde belirtildikleri sırada yürütülecek betik eylemleri listesiyle sağlanabilir.

> [!NOTE]  
> Aşağıdaki hata iletisini karşılaşırsanız:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage: ' % S'terim 'HDIFile Kaydet' cmdlet'i, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol varsa, yolun doğru olduğundan emin olun ve yeniden deneyin.
> 
> Yardımcı yöntemler eklemediğiniz olmasıdır.  Bkz: [özel komut dosyaları için yardımcı yöntemler](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sample-scripts"></a>Örnek komut dosyaları
Betik eylemi, Windows işletim sisteminde HDInsight kümeleri oluşturmak için Azure PowerShell Betiği verilmiştir. Aşağıdaki komut dosyasını site yapılandırma dosyalarını yapılandırmaya ilişkin bir örnek verilmiştir:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Komut dosyası, dört parametre, yapılandırma dosyasının adını, ayarlamak istediğiniz değeri değiştirmek istediğiniz özelliği ve açıklamasını alır. Örneğin:

    hive-site.xml hive.metastore.client.socket.timeout 90

Bu parametreleri hive.metastore.client.socket.timeout değeri hive-site.xml dosyasında 90'a ayarlayın.  Varsayılan değer 60 saniyedir.

Bu örnek betik, aynı zamanda bulunabilir [ https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1 ](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight, HDInsight kümelerinde ek bileşenleri yüklemek için birkaç komut dosyaları sağlar:

| Ad | Betik |
| --- | --- |
| **Spark'ı yükleme** | `https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1`. Bkz: [yükleme ve kullanma, HDInsight üzerinde Apache Spark kümeleri][hdinsight-install-spark]. |
| **R yükleme** | `https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1`. Bkz: [yükleme ve HDInsight kümelerinde R kullanma](r-server/r-server-hdinsight-manage.md#install-additional-r-packages-on-the-cluster). |
| **Giraph yükleme** | `https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1`. Bkz: [HDInsight üzerinde Apache giraph'ı yükleme ve kullanma kümeleri](hdinsight-hadoop-giraph-install.md). |
| **Hive kitaplıklarını önceden yükleme** | `https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1`. Bkz: [HDInsight kümelerinde kitaplıkları Apache Hive Ekle](hdinsight-hadoop-add-hive-libraries.md) |


Betik eylemi, Azure portalı, Azure PowerShell veya HDInsight .NET SDK'sı kullanılarak dağıtılabilir.  Daha fazla bilgi için bkz. [betik eylemi kullanarak özelleştirme HDInsight kümeleri] [hdınsight-kümesi-özelleştirme].

> [!NOTE]  
> Örnek betikler yalnızca HDInsight kümesi sürüm 3.1 veya üstünde çalışır. HDInsight küme sürümleri hakkında daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Özel komut dosyaları için yardımcı yöntemleri
Betik eylem Yardımcısı yöntemleri özel betikler yazılırken kullanabileceğiniz yardımcı programları ' dir. Bu yöntemler, şurada tanımlanan [ https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1 ](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)ve aşağıdaki örneği kullanarak betiğinizde eklenebilir:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Bu komut dosyası tarafından sağlanan yardımcı yöntemler şunlardır:

| Yardımcı yöntemi | Açıklama |
| --- | --- |
| **HDIFile Kaydet** |Bir dosya belirtilen Tekdüzen Kaynak Tanımlayıcısı (URI) öğesinden kümeye atanan Azure VM düğüm ile ilişkili olan yerel diskteki bir konuma indirin. |
| **HDIZippedFile genişletin** |Sıkıştırılmış bir dosyanın sıkıştırmasını açın. |
| **Çağırma HDICmdScript** |Bir betik cmd.exe çalıştırın. |
| **Yazma HDILog** |Betik eylemi için kullanılan özel komut dosyasından çıkış yazın. |
| **Get-Services** |Burada komut dosyasını çalıştırır makine üzerinde çalışan hizmetlerin bir listesini alın. |
| **Get-Service** |Giriş olarak belirli hizmet adıyla belirli bir hizmet için ayrıntılı bilgi almak (hizmet adı, işlem kimliği, durum, vb.) makinedeki burada betiğini yürütür. |
| **Get-HDIServices** |HDInsight Hizmetleri burada betiğini yürütür bilgisayar üzerinde çalışan bir listesini alın. |
| **Get-HDIService** |Özel HDInsight hizmeti adını girdi olarak, belirli bir hizmet için ayrıntılı bilgiler alın (hizmet adı, işlem kimliği, durum, vb.) makinedeki burada betiğini yürütür. |
| **Get-ServicesRunning** |Burada bir betik yürütür bilgisayar üzerinde çalışan hizmetler listesini alın. |
| **Get-ServiceRunning** |Burada komut dosyası yürütülür (adına göre) belirli bir hizmet bilgisayarda çalışıp çalışmadığını denetleyin. |
| **Get-HDIServicesRunning** |HDInsight Hizmetleri burada betiğini yürütür bilgisayar üzerinde çalışan bir listesini alın. |
| **Get-HDIServiceRunning** |Burada komut dosyası yürütülür (adına göre) belirli bir HDInsight hizmeti bilgisayarda çalışıp çalışmadığını denetleyin. |
| **Get-HDIHadoopVersion** |Hadoop burada betiğini yürütür bilgisayarda yüklü sürümünü alır. |
| **Test-IsHDIHeadNode** |Burada bir betik yürütür bilgisayar bir baş düğüm olup olmadığını denetleyin. |
| **Test-IsActiveHDIHeadNode** |Burada bir betik yürütür bilgisayar etkin bir baş düğüm olup olmadığını denetleyin. |
| **Test-IsHDIDataNode** |Burada bir betik yürütür bilgisayar veri düğümü olup olmadığını denetleyin. |
| **HDIConfigFile Düzenle** |Yapılandırma dosyaları hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml veya yarn-site.xml düzenleyin. |

## <a name="best-practices-for-script-development"></a>Betik geliştirme için en iyi uygulamalar
Bir HDInsight kümesi için özel bir betik geliştirirken akılda tutulması gereken birkaç en iyi uygulama vardır:

* Hadoop sürüm kontrol edin.

    Yalnızca HDInsight (Hadoop 2.4) 3.1 sürümünü ve yukarıda bir kümede özel bileşenleri yüklemek için betik eylemi kullanarak destek. Özel betiğinizde kullanmalısınız **Get-HDIHadoopVersion** betikte diğer görevleri gerçekleştirme ile devam etmeden önce Hadoop sürümü denetlemek için yardımcı yöntemi.
* Komut dosyası kaynakları kararlı bağlantıları verilmektedir.

    Kullanıcılar tüm betikleri ve bir küme özelleştirmesinde kullanılan diğer yapılar kümenin kullanım ömrü boyunca kullanılabilir kalmasını ve bu dosyaların sürümleri süresince değiştirmeyin emin olmanız gerekir. Bu kaynaklar, kümedeki düğümlerin yeniden görüntü gerekliyse gereklidir. İndirmek ve her şeyi kullanıcı denetimleri bir depolama hesabında arşivlemek için en iyi yöntem olacaktır. Bu hesap, varsayılan depolama hesabı veya özelleştirilmiş bir küme için dağıtım zamanında belirtilen ek depolama hesabı olabilir.
    Belgelerde, örneğin, sağlanmış bu depolama hesabındaki kaynaklara yerel bir kopyasını Spark ve R küme örnekleri özelleştirilmiş: https://hdiconfigactions.blob.core.windows.net/.
* Küme özelleştirmesi betiğini kez etkili olduğundan emin olun.

    Bir HDInsight kümesi düğümleri kümenin kullanım süresi boyunca başlatıldığında, beklediğiniz gerekir. Bir küme başlatıldığı zaman küme özelleştirme betik çalıştırılır. Bu betik, yalnızca küme başlangıçta oluşturulduğu ilk kez betiği çalıştırdıktan sonra bunu durumu yeniden görüntü üzerinde komut kümesi aynı döndürülür emin olması anlamında ıdempotent özelleştirilmiş olacak şekilde tasarlanmalıdır. Özel bir komut dosyası ilk çalıştırılmasında D:\AppLocation uygulama yüklü değilse, örneğin, ardından görüntüsü yeniden oluşturuluyor, bağlı her sonraki alıştırmada, betik adımları diğer devam etmeden önce uygulama D:\AppLocation konumda var olup olmadığını denetlemelisiniz komut dosyası.
* Özel bileşenler en uygun konuma yükler.

    Küme düğümleri başlatıldığı zaman kaynak sürücü C:\ ve D:\ sistem sürücüsünün, veri kaybına ve bu sürücüleri yüklü uygulamaların výsledek yeniden biçimlendirildi. Kümenin parçası olan bir Azure sanal makinesi (VM) düğüm arıza ve yeni bir düğüm tarafından değiştirilirse bu kayıp da meydana gelmiş olabilir. D:\ sürücüsüne veya C:\apps konumu kümedeki bileşenleri yükleyebilirsiniz. Diğer tüm konumlara C:\ sürücüsüne ayrılmıştır. Burada uygulamaları veya kitaplıkları küme özelleştirme betikte yüklenecek konumu belirtin.
* Yüksek kullanılabilirlik kümesi mimarisi emin olun.

    HDInsight, yüksek kullanılabilirlik, bir baş düğüm (HDInsight Hizmetleri çalıştırdığınız) etkin moda ve bir baş düğüm (içinde hangi HDInsight Hizmetleri çalışmıyor) bekleme modunda olduğu için bir Aktif-Pasif mimari vardır. HDInsight Hizmetleri kesilirse düğümleri etkin ve Pasif modlar arasında geçiş yapma. Yüksek kullanılabilirlik için iki baş düğümüne hizmetlerini yüklemek için betik eylemi kullandıysanız, HDInsight yük devretmeyi mekanizması otomatik olarak bu kullanıcı tarafından yüklenen Hizmetleri'nde başarısız mümkün olmadığını unutmayın. Bu nedenle kullanıcı tarafından yüklenen hizmetleri üzerinde yüksek oranda kullanılabilir olması beklenen bir HDInsight baş düğümü Aktif-Pasif modda, kendi yük devretme mekanizmasına sahip veya etkin-etkin modda olmalıdır.

    Baş düğüm rolü bir değer olarak belirtildiğinde her iki baş düğüm üzerinde bir HDInsight betik eylemi komutu çalıştırır *ClusterRoleCollection* parametresi. Özel bir betik tasarlarken, bu nedenle betiğinizi bu kurulumu farkında olduğundan emin olun. Burada aynı hizmetleri yüklenir ve her iki baş düğümü üzerinde çalışmaya ve birbiriyle rekabet son sorunlar çalıştırmamalısınız. Betik eylemi yüklenen yazılım böyle olaylara dayanıklı olacak şekilde bu Ayrıca, yeniden görüntüleme sırasında veri kaybı olduğunu unutmayın. Uygulamalar birçok düğümüne dağıtılmış yüksek oranda kullanılabilir verilerle çalışmak için tasarlanmış olmalıdır. Adede kadar 1/5 kümedeki düğümlerin aynı anda görüntüsü yeniden oluşturulabildiği.
* Azure Blob depolamayı kullanmak için özel bileşenler yapılandırın.

    Hadoop dağıtılmış dosya sistemi (HDFS) depolama kullanmak için varsayılan yapılandırma, küme düğümleri üzerinde yüklediğiniz özel bileşenler olabilir. Bunun yerine Azure Blob depolamayı kullanmak için yapılandırmayı değiştirmeniz gerekir. Bir küme reimage HDFS dosya sistemi ile biçimlendirilmiş ve burada depolanan tüm verileri kaybedersiniz. Azure Blob Depolama kullanan, bunun yerine verileriniz korunur sağlar.

## <a name="common-usage-patterns"></a>Ortak kullanım desenleri
Bu bölümde, kendi özel betik yazarken karşılaşabileceğiniz genel kullanım düzenlerine bazıları uygulama yönergeler sağlanmaktadır.

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma
Genellikle betik eylemi geliştirme ortam değişkenlerini ayarlamak için gereken gönderebilirsiniz. Bir ikili dış sitesinden kümede yüklemek ve 'PATH' ortam değişkeninize yüklü olduğu konumu Ekle örneği için en olası senaryodur. Aşağıdaki kod parçacığı, özel betik ortam değişkenlerini ayarlamak nasıl gösterir.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Bu bildirimi ortam değişkenini ayarlar **MDS_RUNNER_CUSTOM_CLUSTER** makine genelindeki olacak şekilde bu değişkenin kapsamını değerine hem de 'true' olarak ayarlar. Ortam değişkenleri uygun kapsamda – makine ya da kullanıcı ayarlandığını önemlidir. Başvuru [burada] [ 1] ortam değişkenlerini ayarlama hakkında daha fazla bilgi.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Özel komut dosyaları depolandığı konumlara erişim
Bir küme özelleştirmek için kullanılan betikleri ya da kümenin varsayılan depolama hesabı veya genel bir salt okunur kapsayıcıda herhangi bir depolama hesabında olması. Komut dosyanız başka bir yerde bulunan kaynaklara erişen kaynakları ortak olarak okunabilir olması gerekir. Örneğin, bir dosyaya erişmek ve SaveFile HDI komutu kullanarak kaydetmek isteyebilirsiniz.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

Bu örnekte, kapsayıcı emin olmanız gerekir `somecontainer` depolama hesabındaki `somestorageaccount` genel olarak erişilebilir. Aksi takdirde, betik 'Bulunamadı' özel durum oluşturur ve başarısız.

### <a name="pass-parameters-to-the-add-azhdinsightscriptaction-cmdlet"></a>Add-AzHDInsightScriptAction cmdlet parametreleri
Birden çok parametre Ekle-AzHDInsightScriptAction cmdlet'e geçirmek için komut dosyası için tüm parametreleri içeren dize değerini biçimlendirmek gerekir. Örneğin:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

or

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Başarısız bir küme dağıtımı için özel durum
Doğru bildirim istiyorsanız küme özelleştirmesi olgu başarılı olmadı beklendiği gibi bir özel durum ve küme oluşturma başarısız önemlidir. Örneğin, bir dosya varsa işlemek ve burada dosya yok hata durumu işlemek isteyebilirsiniz. Bu, kodun düzgün bir şekilde çıkar ve küme durumunu doğru şekilde bilinen olun. Aşağıdaki kod parçacığı, bunu başarmak nasıl bir örnek sağlar:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

Dosya olmasaydı, bu kod parçacığında, burada betik gerçekten düzgün bir şekilde hata iletisi yazdırdıktan sonra çıkar ve küme "başarılı" Küme özelleştirme işlemi tamamlandı varsayılarak çalışır duruma ulaştığında bir duruma yol açar. Doğru bir şekilde gerçeği temelde küme özelleştirmesi bildirim almak istiyorsanız, bir eksik dosya nedeniyle beklendiği gibi başarısız oldu, bir özel durum ve küme özelleştirme adımı başarısız daha uygundur. Bunu başarmak için bunun yerine aşağıdaki örnek kod parçacığı kullanmalısınız.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Betik eylemi dağıtma denetim listesi
Bu komut dosyalarını dağıtmak hazırlanırken attığımız adımları şunlardır:

1. Özel komut dosyaları, dağıtım sırasında küme düğümleri tarafından erişilebilir bir yerde içeren dosyaları yerleştirin. Bu, varsayılan veya Küme dağıtımı veya başka bir ortak olarak erişilebilen bir depolama kapsayıcısı sırada belirtilen ek depolama hesapları herhangi biri olabilir.
2. Böylece birden çok kez aynı düğümde komut yürütülüp idempotently, yürütmesine emin olmak için betikler içine ekleyin.
3. Kullanım `Write-Output` STDERR yanı sıra STDOUT yazdırmak için Azure PowerShell cmdlet'i. Kullanmayın `Write-Host`.
4. Gibi bir geçici dosya klasörü kullanın `$env:TEMP`, betikler tarafından kullanılan indirilen dosyayı korumak ve betikleri çalıştırdıktan sonra sonra bunları temizleyebilirsiniz.
5. Özel yazılım D:\ veya C:\apps, yalnızca yükleyin. Ayrılmış olduğundan, C: sürücüsündeki diğer konumlarda kullanılmamalıdır. C:\apps klasörü dışında C: sürücüsündeki dosyaları yükleme görüntüsünü yeniden oluşturur düğümün sırasında Kurulum hatalarına neden olabilir.
6. İşletim sistemi düzeyindeki ayarları veya Hadoop hizmeti yapılandırma dosyaları değiştirildi, olay, ortam değişkenleri komut kümesini gibi herhangi bir işletim sistemi düzeyinde ayarlar seçebilir böylece HDInsight hizmetleri yeniden başlatmak isteyebilirsiniz.

## <a name="debug-custom-scripts"></a>Özel komut dosyaları hata ayıklama
Betik Hata günlüklerini, oluşturma sırasında küme için belirttiğiniz varsayılan depolama hesabındaki başka bir çıktı birlikte depolanır. Günlükler, adı olan bir tabloda depolanır *u < \cluster-name-fragment >< \time-stamp > setuplog*. Bu komut kümede çalıştığı düğümleri (baş düğüm ve çalışan düğümleri) tüm kayıtları toplu günlüklerdir.

Günlükleri denetlemek için kolay bir yol, Visual Studio için HDInsight araçları kullanmaktır. Araçları yüklemek için bkz: [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlama](hadoop/apache-hadoop-visual-studio-tools-get-started.md#install-or-update-data-lake-tools-for-visual-studio)

**Visual Studio kullanarak günlüğünü denetlemek için**

1. Visual Studio'yu açın.
2. Tıklayın **görünümü**ve ardından **Sunucu Gezgini**.
3. "Azure" sağ tıklayın, Bağlan **Microsoft Azure abonelikleri**ve ardından kimlik bilgilerinizi girin.
4. Genişletin **depolama**, varsayılan dosya sistemi olarak kullanılan Azure depolama hesabını genişletin, **tabloları**, sonra da tablo adını çift tıklayın.

Ayrıca uzak küme düğümlerine hem STDOUT ve STDERR için özel komut dosyaları görmek için kullanabilirsiniz. Günlükler her düğümde, yalnızca bu düğüme özeldir ve oturum açtığınız **C:\HDInsightLogs\DeploymentAgent.log**. Bu günlük dosyaları, özel betik tüm çıktılarını kaydeder. Spark betik eylemi için bir örnek günlük kod parçacığı şöyle görünür:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


Bu oturum, HEADNODE0 adlı VM üzerinde Spark betik eylemi yürütüldü ve hiçbir özel durum yürütme sırasında oluşturulan temizleyin.

Bir yürütme hatası oluştuğunda, olay, onu tanımlayan çıkış Ayrıca bu günlük dosyasında yer alır. Bu günlükler sağlanan bilgileri kaynaklanabilecek sorunları betik hata ayıklamaya yardımcı olması gerekir.

## <a name="see-also"></a>Ayrıca bkz.
* [Betik eylemi kullanarak HDInsight kümelerini özelleştirin] [hdınsight-kümesi-özelleştirme]
* [Yükleme ve Apache Spark HDInsight kümeleri kullanma][hdinsight-install-spark]
* [Yükleme ve HDInsight kümeleri üzerinde Apache giraph'ı kullanma](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
