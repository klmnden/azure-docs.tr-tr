---
title: HDInsight için ek Azure depolama hesapları ekleme
description: Mevcut bir HDInsight kümesine Azure ek depolama hesapları eklemeyi öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 833198f3b5dd07988bcb5fc85f815ae2c12f1197
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481935"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>HDInsight için ek depolama hesapları ekleme

Betik eylemleri, HDInsight için ek Azure depolama hesapları eklemek için kullanmayı öğrenin. Bu belgede yer alan adımlar, mevcut bir Linux tabanlı HDInsight kümesine bir depolama hesabı ekleyin. Bu makalede açıklanan [Azure depolama](hdinsight-hadoop-use-blob-storage.md) ve yalnızca ek depolama hesapları (varsayılan küme depolama hesabı değil). Bu makalede uygulanmaz [Azure Data Lake depolama Gen1](hdinsight-hadoop-use-data-lake-store.md) ve [Azure Data Lake depolama Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md).

> [!IMPORTANT]  
> Bu belgedeki oluşturulduktan sonra ek depolama alanı bir kümeye ekleme hakkındaki bilgilerdir. Küme oluşturma sırasında depolama hesapları ekleme hakkında daha fazla bilgi için bkz: [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight kümelerinde ayarlama](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Nasıl çalışır?

Bu betik, aşağıdaki parametreleri alır:

* __Azure depolama hesabı adı__: HDInsight kümesine eklemek için depolama hesabı adı. Betiği çalıştırdıktan sonra HDInsight okuyabilir ve bu depolama hesabında depolanan veri yazabilirsiniz.

* __Azure depolama hesabı anahtarı__: Depolama hesabı erişim anahtarı.

* __-p__ (isteğe bağlı): Belirtilmişse anahtarı şifreli ve core-site.xml dosyasının düz metin olarak depolanır.

İşlem sırasında komut aşağıdaki eylemleri gerçekleştirir:

* Depolama hesabı küme için core-site.xml yapılandırmasında varsa, betik çıkar ve başka bir eyleme gerçekleştirilir.

* Depolama hesabı var ve anahtar kullanılarak erişilebilir olduğunu doğrular.

* Küme kimlik bilgilerini kullanarak anahtarı şifreler.

* Depolama hesabı için core-site.xml dosyasının ekler.

* Durdurur ve yeniden başlatır [Apache Oozie](https://oozie.apache.org/), [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), ve [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) Hizmetleri. Durdurma ve başlatma bu hizmetler yeni depolama hesabı kullanmak üzere sağlar.

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda bir depolama hesabının kullanılması desteklenmez.

## <a name="the-script"></a>Komut dosyası

__Betik konumu__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Gereksinimleri__:

* Betik üzerinde uygulanmalıdır __baş düğümlerine__.

## <a name="to-use-the-script"></a>Betiği kullanmak için

Bu betik, Azure portalı, Azure PowerShell ya da Klasik Azure CLI kullanılabilir. Daha fazla bilgi için [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) belge.

> [!IMPORTANT]  
> Özelleştirme belge içinde sağlanan adımları kullanarak, bu komut dosyasını uygulamak için aşağıdaki bilgileri kullanın:
>
> * Tüm örnek betik eylemi URI'si bu betiği için URI ile değiştirin (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).
> * Herhangi bir örnek parametre, Azure depolama hesabı adı ve kümeye eklenmesi için depolama hesabınızın anahtarıyla değiştirin. Azure portalını kullanarak, bu parametreleri bir boşluk ile ayrılması gerekir.
> * Bu komut dosyası olarak işaretle gerekmez __kalıcı__gibi doğrudan kümesi için Ambari yapılandırmasını güncelleştirir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Azure portalı veya araçları görüntülenmeyen depolama hesapları

HDInsight kümesi Azure portalında görüntülerken seçerek __depolama hesapları__ altında girdisi __özellikleri__ bu betik eylemi eklenen depolama hesaplarına görüntülemez. Azure PowerShell ve klasik Azure CLI'yı ek depolama alanı hesabı ya da görüntülemez.

Komut dosyası, yalnızca küme için core-site.xml yapılandırmasını değiştirir çünkü depolama bilgilerini görüntülenmiyor. Bu bilgiler, Azure yönetim API'lerini kullanarak küme bilgileri alınırken kullanılmaz.

Bu betik kullanarak kümeye eklenen depolama hesabı bilgileri görüntülemek için Ambari REST API'yi kullanın. Kümeniz için bu bilgileri almak için aşağıdaki komutları kullanın:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]  
> Ayarlama `$clusterName` HDInsight kümesinin adı. Ayarlama `$storageAccountName` depolama hesabının adı. İstendiğinde küme oturum açma (Yönetici) ve parolayı girin.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]  
> Ayarlama `$PASSWORD` için küme oturum açma (Yönetici) hesabı parolası. Ayarlama `$CLUSTERNAME` HDInsight kümesinin adı. Ayarlama `$STORAGEACCOUNTNAME` depolama hesabının adı.
>
> Bu örnekte [curl (https://curl.haxx.se/) ](https://curl.haxx.se/) ve [jq (https://stedolan.github.io/jq/) ](https://stedolan.github.io/jq/) almak ve JSON verilerini ayrıştırılamadı.

Bu komutu kullanırken değiştirin __CLUSTERNAME__ ile HDInsight kümesinin adı. Değiştirin __parola__ küme için HTTP oturum açma parolası ile. Değiştirin __STORAGEACCOUNT__ ile betik eylemi kullanarak eklenen depolama hesabı adı. Bu komuttan döndürülen bilgileri aşağıdaki metne benzer görünür:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Bu metin, depolama hesabına erişmek için kullanılan şifreli bir anahtar örneğidir.

### <a name="unable-to-access-storage-after-changing-key"></a>Depolama anahtarı değiştirdikten sonra erişilemiyor

Bir depolama hesabı anahtarı değiştirirseniz, HDInsight artık depolama hesabına erişebilir. HDInsight, küme için core-site.xml anahtar önbelleğe alınmış bir kopyasını kullanır. Bu önbelleğe alınmış kopyayı yeni anahtarı eşleşmesi için güncelleştirilmesi gerekir.

Betik eylemi yeniden çalıştırmak mu __değil__ betik depolama hesabı için bir giriş zaten mevcut olup olmadığını denetler anahtarı güncelleştirin. Bir giriş zaten varsa, herhangi bir değişiklik yapmaz.

Bu sorunu çözmek için depolama hesabı için var olan girdiyi kaldırmanız gerekir. Var olan girdiyi kaldırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında HDInsight kümeniz için Ambari Web kullanıcı arabirimini açın. URI https://CLUSTERNAME.azurehdinsight.net. __CLUSTERNAME__ değerini kümenizin adıyla değiştirin.

    İstendiğinde, kümeniz için HTTP oturum açma kullanıcı adı ve parola girin.

2. Sayfanın solundaki hizmetler listesinden seçin __HDFS__. Ardından __yapılandırmaları__ sayfanın ortasındaki sekmesi.

3. İçinde __Filtresi...__  değeri alanına, __fs.azure.account__. Bu kümeye eklenen herhangi bir ek depolama hesapları için girişleri döndürür. İki tür giriş vardır; __keyprovider__ ve __anahtar__. Hem de anahtar adının bir parçası olarak depolama hesabının adını içerir.

    Adlı bir depolama hesabı için örnek girdileri verilmiştir __depolamam__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Kaldırmak için depolama hesabı anahtarlarını uyguladığınızı belirledikten sonra kırmızı kullanın. '-' simgesini silmek için sağ tarafındaki giriş. Ardından __Kaydet__ düğmesini yaptığınız değişiklikleri kaydedin.

5. Değişiklikleri kaydettikten sonra depolama hesabı ve yeni anahtar değeri kümeye eklemek için betik eylemi kullanın.

### <a name="poor-performance"></a>Zayıf performans

HDInsight kümesinden farklı bir bölgede depolama hesabı ise, düşük performansla karşılaşabilirsiniz. Farklı bir bölgede verilerine erişme, bölgesel Azure veri merkezi dışında olan ve gecikme ortaya çıkarabilir genel internet üzerinden ağ trafiği gönderir.

> [!WARNING]  
> HDInsight kümesinden farklı bir bölgede bir depolama hesabının kullanılması desteklenmez.

### <a name="additional-charges"></a>Ek ücret

Depolama hesabı, HDInsight kümesinden farklı bir bölgede ise, Azure faturalandırmanızı ek çıkış ücretlerini fark edebilirsiniz. Veri bölgesel veri merkezi dışına çıktığında, bir çıkış ücreti uygulanır. Trafik farklı bir bölgede başka bir Azure veri merkezi için hedeflenen olsa bile bu ücret uygulanır.

> [!WARNING]  
> HDInsight kümesinden farklı bir bölgede bir depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Mevcut bir HDInsight kümesine ek depolama hesapları ekleme öğrendiniz. Betik eylemleri hakkında daha fazla bilgi için bkz. [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md)
