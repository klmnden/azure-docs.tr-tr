---
title: Betik eylemi geliştirme Hdınsight - Azure ile | Microsoft Docs
description: Betik eylemi olan Hadoop kümeleri özelleştirmeyi öğrenin. Betik eylemi, Hadoop küme üzerinde çalışan ek yazılım yüklemesi veya bir kümeye yüklü uygulamalar yapılandırmasını değiştirmek için kullanılabilir.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: ac2a087bb0a9d8cac15dfea2448a9c42cee4a1f4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Hdınsight Windows tabanlı kümeler için betik eylemi betikleri geliştirme
Hdınsight için betik eylemi betikler yazma hakkında bilgi edinin. Betik eylemi komut dosyalarını kullanma hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md). Linux tabanlı Hdınsight kümeleri için yazılmış aynı makale için bkz: [Hdınsight betik eylemi geliştirme betikleri](hdinsight-hadoop-script-actions-linux.md).



> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri için geçerlidir. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Betik eylemleri Linux tabanlı kümelerde ile kullanma hakkında daha fazla bilgi için bkz: [(Linux) Hdınsight ile betik eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).
>
>



Betik eylemi, Hadoop küme üzerinde çalışan ek yazılım yüklemesi veya bir kümeye yüklü uygulamalar yapılandırmasını değiştirmek için kullanılabilir. Hdınsight kümeleri dağıtıldığında küme düğümleri üzerinde çalışan bir komut dosyası komut dosyası eylemlerdir ve kümedeki düğümlerin Hdınsight yapılandırmasını tamamladıktan sonra yürütülür. Betik eylemi sistem yönetici hesabı ayrıcalıklarıyla yürütülür ve küme düğümleri için tam erişim hakları sağlar. Her küme, belirtilen sırada yürütülecek komut dosyası eylemlerin bir listesi ile sağlanabilir.

> [!NOTE]
> Aşağıdaki hata iletisini karşılaşırsanız:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Adının yazımını denetleyin veya bir yol dahilse, yolun doğru olduğundan emin olun ve yeniden deneyin.
> Yardımcı yöntemler eklemediniz olmasıdır.  Bkz: [özel komut dosyaları için yardımcı yöntemleri](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).
>
>

## <a name="sample-scripts"></a>Örnek komut dosyaları
Betik eylemi, Windows işletim sisteminde Hdınsight kümeleri oluşturmak için Azure PowerShell komut dosyasıdır. Aşağıdaki komut dosyasını site yapılandırma dosyalarını yapılandırmaya ilişkin bir örnek gösterilmektedir:

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

Komut dosyası, dört parametre, yapılandırma dosyasının adını, ayarlamak istediğiniz değeri değiştirmek istediğiniz özellik ve açıklamasını alır. Örneğin:

    hive-site.xml hive.metastore.client.socket.timeout 90

Bu parametreler hive.metastore.client.socket.timeout değeri hive-site.xml dosyasında 90'a ayarlayın.  Varsayılan değer 60 saniyedir.

Bu örnek komut dosyası ayrıca bulunabilir [ https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1 ](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

Hdınsight Hdınsight kümelerinde ek bileşenleri yüklemek için çeşitli komut dosyaları sağlar:

| Ad | Betik |
| --- | --- |
| **Spark yükleyin** |https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1. Bkz: [yükleme ve kullanma hdınsight'ta Spark kümeleri][hdinsight-install-spark]. |
| **R yükleme** |https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1. Bkz: [yükleme ve kullanma R Hdınsight kümelerinde][hdinsight-r-scripts]. |
| **Solr yükleyin** |https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1. Bkz: [yükleme ve kullanma Solr hdınsight kümeleri](hdinsight-hadoop-solr-install.md). |
| - **Giraph yükleyin** |https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1. Bkz: [yükleme ve kullanma Giraph hdınsight kümeleri](hdinsight-hadoop-giraph-install.md). |

Betik eylemi, Azure portalı, Azure PowerShell veya Hdınsight .NET SDK kullanarak dağıtılabilir.  Daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak][hdinsight-cluster-customize].

> [!NOTE]
> Örnek betikler yalnızca Hdınsight kümesi sürüm 3.1 veya üzeri çalışır. Hdınsight küme sürümleri hakkında daha fazla bilgi için bkz: [Hdınsight küme sürümleri](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Özel komut dosyaları için yardımcı yöntemleri
Betik eylem Yardımcısı yöntemleri özel komut dosyaları yazılırken kullanabileceğiniz yardımcı programları ' dir. Bu yöntemler tanımlanan [ https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1 ](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), komut dosyalarınızı aşağıdaki örneği kullanarak eklenebilir:

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
| **Save-HDIFile** |Bir dosya belirtilen Tekdüzen Kaynak Tanımlayıcısı (URI) gelen kümeye atanan Azure VM düğümle ilişkilendirilen yerel diskteki bir konuma indirin. |
| **Expand-HDIZippedFile** |Sıkıştırılmış bir dosya sıkıştırmasını açın. |
| **Invoke-HDICmdScript** |Bir komut dosyası cmd.exe çalıştırın. |
| **Write-HDILog** |Bir komut dosyası eylemi için kullanılan özel komut dosyasından çıkış yazma. |
| **Get-Services** |Burada betiği yürüten makinede çalışan hizmetlerin listesini alın. |
| **Get-Service** |Giriş olarak belirli hizmet adı ile belirli bir hizmet için ayrıntılı bilgi almak (hizmet adı, işlem kimliği, durum, vb.) burada betiği yürüten makinede. |
| **Get-HDIServices** |Burada betiği yürüten bilgisayarda çalışan Hdınsight hizmetlerin listesini alın. |
| **Get-HDIService** |Özel Hdınsight hizmet adı ile giriş olarak, belirli bir hizmet için ayrıntılı bilgi almak (hizmet adı, işlem kimliği, durum, vb.) burada betiği yürüten makinede. |
| **Get-ServicesRunning** |Burada betiği yürüten bilgisayarda çalışan hizmetlerin listesini alın. |
| **Get-ServiceRunning** |Burada komut dosyasını çalıştırır (adına göre) belirli bir hizmet bilgisayar üzerinde çalışıp çalışmadığını denetleyin. |
| **Get-HDIServicesRunning** |Burada betiği yürüten bilgisayarda çalışan Hdınsight hizmetlerin listesini alın. |
| **Get-HDIServiceRunning** |Burada komut dosyasını çalıştırır (adına göre) belirli bir Hdınsight hizmet bilgisayarda çalışıp çalışmadığını denetleyin. |
| **Get-HDIHadoopVersion** |Hadoop burada komut dosyasını çalıştırır bilgisayarda yüklü sürümünü alır. |
| **Test-IsHDIHeadNode** |Burada betiği yürüten bilgisayar bir baş düğüm olup olmadığını denetleyin. |
| **Test-IsActiveHDIHeadNode** |Burada betiği yürüten bilgisayar etkin bir baş düğüm olup olmadığını denetleyin. |
| **Test-IsHDIDataNode** |Burada betiği yürüten bilgisayar veri düğümü olup olmadığını denetleyin. |
| **Edit-HDIConfigFile** |Yapılandırma dosyaları hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml veya yarn-site.xml düzenleyin. |

## <a name="best-practices-for-script-development"></a>Komut dosyası geliştirme için en iyi yöntemler
Hdınsight kümesi için özel bir komut dosyası geliştirirken dikkate alınması gereken birkaç en iyi yöntemler vardır:

* Hadoop sürüm denetimi

    Yalnızca Hdınsight sürüm 3.1 (Hadoop 2.4) ve bir kümede özel bileşenleri yüklemek için betik eylemi kullanarak destek üstünde. Özel betiğinizde kullanmalısınız **Get-HDIHadoopVersion** komut dosyasında diğer görevleri gerçekleştirme ile devam etmeden önce Hadoop sürümünü denetlemek için yardımcı yöntemi.
* Komut dosyası kaynaklara kararlı bağlantılar sağlar

    Kullanıcılar tüm betikler ve bir küme özelleştirmesinde kullanılan diğer yapıları kümenin kullanım ömrü kullanılabilir kalmasını ve bu dosyaların sürümleri süresince değiştirmeyin emin olmanız gerekir. Kümedeki düğümler yeniden görüntüleme gerekliyse, bu kaynakları gereklidir. Karşıdan yüklemek ve bir depolama hesabındaki kullanıcı denetimleri her şeyi arşivlemek için en iyi uygulamadır bakın. Bu hesap, varsayılan depolama hesabı veya herhangi bir dağıtım zaman özelleştirilmiş bir küme için belirtilen ek depolama hesapları olabilir.
    Belgelerde, örneğin, sağlanmış bu depolama hesabındaki kaynaklara yerel bir kopyasını küme örnekleri özelleştirilmiş Spark ve R: https://hdiconfigactions.blob.core.windows.net/.
* Küme özelleştirme betik ıdempotent olduğundan emin olun

    Hdınsight kümesi düğümleri küme ömrü boyunca görüntüsü yeniden beklediğiniz gerekir. Bir küme yeniden her küme özelleştirme komut dosyasını çalıştırın. Bu komut dosyasını yeniden görüntüsünü oluşturuyor sonra komut dosyasını aynı küme döndürülür emin olması herkese açık ıdempotent yalnızca küme başlangıçta oluşturulduğu ilk kez betiği çalıştırdıktan sonra durumla durumu özelleştirilmiş olacak şekilde tasarlanmış olması gerekir. Özel bir komut dosyası ilk çalıştırılmasında D:\AppLocation uygulama yüklediyseniz, örneğin, sonra yeniden görüntüsünü oluşturuyor, bağlı her sonraki çalıştırmada betik adımları diğer işlemine devam etmeden önce uygulama D:\AppLocation konumda var olup olmadığını kontrol komut dosyası.
* En iyi konumda özel bileşenlerini yükle

    Küme düğümleri yeniden, D:\ sistem sürücüsü ve C:\ kaynak sürücü, veri kaybına ve bu sürücülerde yüklemiş olduğu uygulamalar sonuçlanır yeniden biçimlendirilen. Kümesinin parçası olan bir Azure sanal makine (VM) düğüm arıza ve yeni bir düğüm tarafından değiştirilirse, bu kaybı da meydana gelmiş olabilir. Bileşenleri D:\ sürücüsüne veya küme C:\apps yerde yükleyebilirsiniz. C:\ sürücüsü üzerindeki diğer tüm konumlara ayrılmıştır. Burada uygulamalar veya kitaplıkları küme özelleştirme betik yüklenecek konumu belirtin.
* Küme mimari yüksek kullanılabilirliğini sağlamak

    Hdınsight bir baş düğüm (Hdınsight Hizmetleri çalıştırdığınız) etkin modda bir baş düğüm olup (hangi Hdınsight'ta Hizmetleri çalışmıyor) bekleme modunda olduğundan, yüksek kullanılabilirlik için bir Aktif-Pasif mimarisi vardır. Hdınsight Hizmetleri kesilirse düğümleri etkin ve Pasif modları. Yüksek kullanılabilirlik için her iki baş düğümünde hizmetlerini yüklemek için bir betik eylemi kullandıysanız, Hdınsight yük devretme mekanizması otomatik olarak bu kullanıcı tarafından yüklenen hizmetleri başarısız mümkün olmadığını unutmayın. Bu nedenle kullanıcı yüklü hizmetler yüksek oranda kullanılabilir olması beklenen Hdınsight baş düğümler üzerinde kendi Aktif-Pasif modu, yük devretme yönteminde sahip veya etkin-etkin modunda olması.

    Baş düğüm rolünü bir değer olarak belirtildiğinde bir Hdınsight betik eylemi komutu her iki baş düğümler üzerinde çalışır *ClusterRoleCollection* parametresi. Bu nedenle özel bir komut dosyası tasarlarken, kodunuzu bu kurulumu farkında olduğundan emin olun. Burada aynı hizmetleri yüklenir ve her iki baş düğümü üzerinde başlatıldı ve birbirleri ile rekabet bitiş sorunlar çalışmamalıdır. Betik eylemi yüklenen yazılım bu tür olayların dayanıklı olması gerekir böylece Ayrıca, yeniden görüntüleme sırasında veri kaybı olmamasına dikkat edin. Uygulamalar birçok düğümüne dağıtılmış yüksek oranda kullanılabilir verilerle çalışmak için tasarlanmış. Aynı anda kadar 1/5 kümedeki düğümlerin yeniden olduğunu unutmayın.
* Azure Blob storage kullanma özel bileşenlerini yapılandırma

    Küme düğümlerine yükleyin özel bileşenler Hadoop dağıtılmış dosya sistemi (HDFS) depolama kullanmak için bir varsayılan yapılandırmaya sahip olabilir. Azure Blob storage kullanmayı yapılandırmasını değiştirmeniz gerekir. Bir küme yeniden görüntü oluşturma, HDFS dosya sistemi ile biçimlendirilmiş ve orada depolanan tüm verileri kaybedersiniz. Azure Blob storage kullanarak, bunun yerine, verilerinizi tutulur sağlar.

## <a name="common-usage-patterns"></a>Genel kullanım desenleri
Bu bölümde, kendi özel bir komut dosyası yazılırken içine çalışabilir ortak kullanım desenlerini bazıları uygulama yönergeler sağlanmaktadır.

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma
Genellikle betik eylemi geliştirme ortam değişkenlerini ayarlama gerek düşündüğünüz. Bir ikili dış sitesinden, kümede yüklemek ve, 'PATH' ortam değişkeni yüklendiği konumun eklediğinizde örneği için en olası senaryodur. Aşağıdaki kod parçacığında özel betik ortam değişkenlerini ayarlama gösterilmiştir.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Bu bildirimi ortam değişkenini ayarlar **MDS_RUNNER_CUSTOM_CLUSTER** makine genelinde olması için bu değişkenin kapsamını 'true' hem de değerine ayarlar. Zaman zaman ortam değişkenleri uygun kapsamda – makine ya da kullanıcı ayarlanır önemlidir. Başvuru [burada] [ 1] ortam değişkenlerini ayarlama hakkında daha fazla bilgi.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Özel komut dosyaları depolandığı konumuna erişim
Bir küme özelleştirmek için kullanılan komut ya da küme için varsayılan depolama hesabı veya başka bir depolama hesabı üzerinde genel bir salt okunur kapsayıcı olması. Kodunuzu başka bir yerde bulunan kaynaklara erişirse bunlar bir ortak olarak erişilebilen olmanız gerekir (en az genel salt okunur). Örneğin bir dosyaya erişmek ve SaveFile HDI komutunu kullanarak kaydetmek isteyebilirsiniz.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

Bu örnekte 'somestorageaccount' depolama hesabındaki ' somecontainer' kapsayıcı genel olarak erişilebilir olduğundan emin olmalısınız. Aksi takdirde, komut dosyası 'Bulunamadı' bir özel durum oluşturur ve başarısız.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Add-AzureRmHDInsightScriptAction cmdlet parametreleri
Birden çok parametre Ekle AzureRmHDInsightScriptAction cmdlet'e için komut dosyası için tüm parametreleri içeren dize değeri biçimlendirmeniz gerekir. Örneğin:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

or

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Başarısız Küme dağıtımı için özel durum
Doğru bir şekilde, bildirim istiyorsanız özelleştirme küme olgu başarılı olmadı beklendiği gibi bir özel durum ve küme oluşturma başarısız önemlidir. Örneğin, bir dosya varsa işlemek ve burada dosya yok hata durumu işlemek isteyebilirsiniz. Bu, komut dosyası düzgün biçimde çıkar ve küme durumunu doğru şekilde bilinen emin olun. Aşağıdaki kod parçacığında, bunu başarmak nasıl bir örnek sağlar:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

Dosya olmasaydı, bu parçacığında, bunu burada komut gerçekte düzgün biçimde hata iletisini yazdırdıktan sonra çıkar ve küme "başarılı" Küme özelleştirme işlemi tamamlandı varsayılarak çalışır duruma ulaştığında bir duruma yol açar. Özelleştirme temelde küme olgu doğru olarak bildirilmesini istiyorsanız, eksik dosya nedeniyle beklendiği gibi başarılı olmadı, bir özel durum ve küme özelleştirme adımı başarısız daha uygundur. Bunu başarmak için aşağıdaki örnek kod parçacığını yerine kullanmanız gerekir.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Betik eylemi dağıtma denetim listesi
Biz bu komut dosyaları dağıtmak hazırlarken sürdü adımlar şunlardır:

1. Dağıtım sırasında küme düğümleri tarafından erişilebilen bir yerde özel komut dosyaları içeren dosyaları yerleştirin. Bu varsayılan veya Küme dağıtımı ya da başka bir genel olarak erişilebilir depolama kapsayıcısı sırada belirtilen ek depolama hesapları olabilir.
2. Komut dosyası birden çok kez aynı düğümde yürütülebilir. böylece idempotently, yürütme emin olmak için betikler içine ekleyin.
3. Kullanım **Write-Output** STDERR yanı sıra STDOUT yazdırmak için Azure PowerShell cmdlet'i. Kullanmayın **Write-Host**.
4. $Env gibi bir geçici dosya klasörünü kullanabilirsiniz: betikler tarafından kullanılan indirilen dosya korumak ve komut dosyaları çalıştırdıktan sonra sonra bunları temizlemek için TEMP.
5. Özel yazılım D:\ veya C:\apps yalnızca yükleyin. Ayrılmış olarak C: sürücüsündeki diğer konumlarda kullanılmamalıdır. C:\apps klasörü dışında C: sürücüsündeki dosyaları yükleme kurulum hataları sırasında düğümün reimages neden olabileceğini unutmayın.
6. İşletim sistemi düzeyinde ayarları veya Hadoop hizmeti yapılandırma dosyaları değiştirilmiş gelmesi durumunda, böylece ortam değişkenleri komut kümesini gibi herhangi bir işletim sistemi düzeyinde ayarlarını seçebilirsiniz Hdınsight hizmetlerini yeniden isteyebilirsiniz.

## <a name="debug-custom-scripts"></a>Özel komut dosyaları hata ayıklama
Komut dosyası hata günlüklerini oluşturulduktan konumundaki küme için belirtilen varsayılan depolama hesabındaki diğer çıktı birlikte depolanır. Günlükler, adı olan bir tabloda depolanır *u < \cluster-name-fragment >< \time-stamp > setuplog*. Tüm betik kümede çalışan düğümleri (baş düğüm ve çalışan düğümleri) kayıtları toplanmış günlükleri şunlardır.

Günlükleri denetlemek için kolay bir yol, Visual Studio için Hdınsight araçları kullanmaktır. Araçları yüklemek için bkz: [Visual Studio Hadoop araçlarını için Hdınsight kullanmaya başlama](hadoop/apache-hadoop-visual-studio-tools-get-started.md#install-or-update-data-lake-tools-for-visual-studio)

**Visual Studio kullanarak günlüğünü denetlemek için**

1. Visual Studio'yu açın.
2. Tıklatın **Görünüm**ve ardından **Sunucu Gezgini**.
3. "Azure" sağ tıklayın, Bağlan **Microsoft Azure abonelikleri**ve ardından kimlik bilgilerinizi girin.
4. Genişletme **depolama**, varsayılan dosya sistemi olarak kullanılan Azure depolama hesabı genişletin, **tabloları**ve tablo adı çift tıklatın.

Ayrıca uzak STDOUT ve STDERR için özel komut dosyaları görmek için küme düğümleri olarak kullanabilirsiniz. Her düğümde günlükleri yalnızca bu düğüme özgüdür ve oturum açtığınız **C:\HDInsightLogs\DeploymentAgent.log**. Bu günlük dosyaları, özel komut dosyasından tüm çıkış kaydedin. Spark betik eylemi için bir örnek günlük parçacığı şöyle görünür:

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


Bu günlük, Spark betik eylemi HEADNODE0 adlı VM üzerinde yürütüldüğü ve yürütme sırasında hiçbir özel durum oluştu, temizleyin.

Bir yürütme hatası meydana gelmesi durumunda, bu açıklayan çıkış bu günlük dosyasında da yer alır. Bu günlükler sağlanan bilgilerin kaynaklanabilecek betik sorunları hata ayıklamaya yardımcı olması gerekir.

## <a name="see-also"></a>Ayrıca bkz.
* [Betik eylemi kullanarak Hdınsight kümelerini özelleştirme][hdinsight-cluster-customize]
* [Yükleme ve Spark Hdınsight kümelerinde kullanma][hdinsight-install-spark]
* [Yükleme ve Hdınsight kümelerinde R kullanma][hdinsight-r-scripts]
* [Yükleme ve Hdınsight kümelerinde Solr kullanma](hdinsight-hadoop-solr-install.md).
* [Yükleme ve Hdınsight kümelerinde Giraph kullanma](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
