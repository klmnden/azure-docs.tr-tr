---
title: "Hdınsight için ek Azure depolama hesapları ekleme | Microsoft Docs"
description: "Ek Azure depolama hesapları olan bir Hdınsight kümesine eklemeyi öğrenin."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/22/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 72045d363516a2f16d45e3f8ee157ddd9d9242bd
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>Hdınsight için ek depolama hesapları ekleme

Hdınsight için ek Azure depolama hesapları eklemek için betik eylemleri kullanmayı öğrenin. Bu belgede yer alan adımlar bir depolama hesabı olan bir Linux tabanlı Hdınsight kümesine ekleyin.

> [!IMPORTANT]
> Bu belgedeki oluşturulduktan sonra ek depolama alanı bir kümeye ekleme hakkındaki bilgilerdir. Küme oluşturma sırasında depolama hesapları ekleme hakkında daha fazla bilgi için bkz: [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Nasıl çalışır?

Bu betiği aşağıdaki parametreleri alır:

* __Azure depolama hesabı adı__: Hdınsight kümesine eklemek için depolama hesabının adı. Hdınsight, komut dosyasını çalıştırdıktan sonra okuma ve bu depolama hesabında depolanan verileri yazma.

* __Azure depolama hesabı anahtarı__: depolama hesabına erişim izni veren bir anahtar.

* __-p__ (isteğe bağlı): belirtilmişse, anahtarı şifrelenmemiş ve düz metin olarak core-site.xml dosyasında depolanır.

İşleme sırasında komut aşağıdaki eylemleri gerçekleştirir:

* Depolama hesabı küme için core-site.xml yapılandırmasının zaten varsa, komut dosyası çıkar ve başka hiçbir eylem gerçekleştirilir.

* Depolama hesabı var ve anahtarı kullanılarak erişilebilir doğrular.

* Küme kimlik bilgilerini kullanarak anahtarı şifreleyen.

* Depolama hesabı core-site.xml dosyasına ekler.

* Durdurur ve Oozie, YARN, MapReduce2 ve HDFS hizmetleri yeniden başlatır. Durdurma ve bu hizmetleri başlatma yeni depolama hesabı kullanmak üzere sağlar.

> [!WARNING]
> Hdınsight kümesi farklı bir konumda bir depolama hesabıyla desteklenmiyor.

## <a name="the-script"></a>Komut dosyası

__Komut dosyası konumu__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Gereksinimleri__:

* Komut dosyası üzerinde uygulanması gereken __baş düğümler__.

## <a name="to-use-the-script"></a>Betik kullanmak için

Bu komut, Azure portalı, Azure PowerShell veya Azure CLI 1.0 kullanılabilir. Daha fazla bilgi için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) belge.

> [!IMPORTANT]
> Özelleştirme belgede sağlanan adımları kullanarak, bu komut dosyasını uygulamak için aşağıdaki bilgileri kullanın:
>
> * Tüm örnek betik eylemi URI bu komut dosyası (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh) için URI ile değiştirin.
> * Kümeye eklenecek depolama hesabı anahtarı ve Azure depolama hesabı adı ile örnek parametreleri değiştirin. Azure portalını kullanıyorsanız, bu parametreler boşlukla ayrılmış olması gerekir.
> * Bu komut dosyası olarak işaretlemek gerekmez __kalıcı__, doğrudan bir küme için Ambari yapılandırmasını güncelleştirir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Azure portalından veya Araçlar görüntülenmeyen depolama hesapları

Hdınsight kümesi Azure portalında görüntülerken seçme __depolama hesapları__ altında girdisi __özellikleri__ bu betik eylemi eklenen depolama hesaplarına görüntülemez. Azure PowerShell ve Azure CLI ek depolama alanı hesabı ya da görüntülenmez.

Komut dosyası küme için core-site.xml yapılandırmasının yalnızca değiştirdiğinden depolama bilgiler görüntülenmiyor. Bu bilgiler Azure management API'leri kullanarak küme bilgi alınırken kullanılmaz.

Bu komut dosyasını kullanarak kümeye eklenen depolama hesabı bilgilerini görüntülemek için Ambari REST API kullanın. Kümeniz için bu bilgileri almak için aşağıdaki komutları kullanın:

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> Ayarlama `$clusterName` Hdınsight kümesinin adı. Ayarlama `$storageAccountName` depolama hesabının adı. İstendiğinde, küme oturum açma (Yönetici) ve parola girin.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> Ayarlama `$PASSWORD` küme oturum açma (Yönetici) hesap parolası için. Ayarlama `$CLUSTERNAME` Hdınsight kümesinin adı. Ayarlama `$STORAGEACCOUNTNAME` depolama hesabının adı.
>
> Bu örnekte [curl (http://curl.haxx.se/)](http://curl.haxx.se/) ve [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) almak ve JSON verilerini ayrıştırılamadı.

Bu komut, kullanırken değiştirin __CLUSTERNAME__ Hdınsight kümesi adı. Değiştir __parola__ küme için HTTP oturum açma parolası ile. Değiştir __STORAGEACCOUNT__ betik eylemi kullanarak eklenen depolama hesabı adı. Bu komuttan döndürülen bilgi için aşağıdaki metni benzer görünür:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Bu metin, depolama hesabına erişmek için kullanılan şifreli bir anahtar örneğidir.

### <a name="unable-to-access-storage-after-changing-key"></a>Depolama anahtarı değiştirdikten sonra erişilemiyor

Bir depolama hesabı anahtarı değiştirirseniz, Hdınsight depolama hesabı artık erişemez. Hdınsight, küme için core-site.xml anahtar önbelleğe alınmış bir kopyasını kullanır. Önbelleğe alınan bu kopyayı yeni anahtarı eşleşecek şekilde güncelleştirmeniz gerekir.

Betik eylemi yeniden çalıştıran mu __değil__ anahtar depolama hesabı için bir giriş zaten var olup olmadığını görmek için komut dosyasını denetler gibi güncelleştirin. Bir giriş zaten varsa, herhangi bir değişiklik yapmaz.

Bu sorunu gidermek için depolama hesabı için varolan bir girişi kaldırmanız gerekir. Varolan bir girişi kaldırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında Hdınsight kümeniz için Ambari Web kullanıcı arabirimini açın. URI https://CLUSTERNAME.azurehdinsight.net ' dir. __CLUSTERNAME__ değerini kümenizin adıyla değiştirin.

    İstendiğinde, kümeniz için HTTP oturum açma kullanıcı adı ve parola girin.

2. Sayfanın sol Hizmetleri listesinden seçin __HDFS__. Ardından __yapılandırmalar__ sayfasının ortasında sekmesi.

3. İçinde __filtre...__  değeri alanına, __fs.azure.account__. Bu kümeye eklenen herhangi bir ek depolama alanı hesabı girişlerini döndürür. İki tür giriş vardır; __keyprovider__ ve __anahtar__. Her ikisi de anahtar adının bir parçası olarak depolama hesabı adını içerir.

    Aşağıdaki örnek girişlerdir adlı bir depolama hesabı için __mystorage__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Gereksinim kaldırmak için depolama hesabı için anahtarlar tanımladıktan sonra kırmızı kullanmak '-' silmek için giriş sağındaki simgesi. Ardından __kaydetmek__ yaptığınız değişiklikleri kaydetmek için düğmesi.

5. Değişiklikler kaydedildikten sonra depolama hesabı ve yeni anahtar değeri kümeye eklemek için betik eylemi kullanın.

### <a name="poor-performance"></a>Düşük performans

Hdınsight kümesi farklı bir bölgede depolama hesabı ise, düşük performans karşılaşabilirsiniz. Farklı bir bölgeye verilere ağ trafiğini bölgesel Azure veri merkezinin dışındaki ve gecikme getirebilir ortak Internet üzerinden gönderir.

> [!WARNING]
> Hdınsight kümesi farklı bir bölgede bir depolama hesabı kullanılması desteklenmez.

### <a name="additional-charges"></a>Ek ücretlere

Hdınsight kümesi farklı bir bölgede depolama hesabı ise, Azure faturalama hakkında ek çıkış ücretlerini fark edebilirsiniz. Verileri bir bölgesel veri merkezi ayrıldığında bir çıkış ücret uygulanır. Farklı bir bölgede başka bir Azure veri merkezi için trafiği hedefleyen olsa bile bu ücretsiz olarak uygulanır.

> [!WARNING]
> Hdınsight kümesi farklı bir bölgede bir depolama hesabı kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Ek depolama hesapları olan bir Hdınsight kümesine ekleme öğrendiniz. Betik eylemleri hakkında daha fazla bilgi için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md)
