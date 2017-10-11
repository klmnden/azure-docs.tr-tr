---
title: "Linux tabanlı Hdınsight - Azure ile betik eylemi geliştirme | Microsoft Docs"
description: "Linux tabanlı Hdınsight kümelerini özelleştirme için Bash betiklerini kullanmayı öğrenin. Hdınsight betik eylemi özelliğidir sırasında veya Küme oluşturulduktan sonra komut dosyaları çalıştırmanıza olanak sağlar. Komut dosyaları, küme yapılandırma ayarlarını değiştirmek veya ek yazılım yüklemek için kullanılabilir."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 7f1a0bd8c7e60770d376f10eaea136a55c632c5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="script-action-development-with-hdinsight"></a>Hdınsight ile betik eylemi geliştirme

Bash betiklerini kullanarak Hdınsight kümenize özelleştirmeyi öğrenin. Betik eylemleri, sırasında veya Küme oluşturulduktan sonra Hdınsight özelleştirmek için bir yoldur.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-are-script-actions"></a>Betik eylemleri nelerdir

Betik eylemleri Azure yapılandırma değişiklikler yapabilir veya yazılım yükleme küme düğümlerinde çalışır Bash betikleridir. Betik eylemi kök olarak yürütülen ve küme düğümleri için tam erişim hakları sağlar.

Betik eylemleri aşağıdaki yöntemleri kullanarak uygulanabilir:

| Bir komut dosyası uygulamak için bu yöntemi kullanın... | Sırasında oluşturma küme... | Çalıştıran bir kümede... |
| --- |:---:|:---:|
| Azure portalına |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure CLI |&nbsp; |✓ |
| HDInsight .NET SDK'sı |✓ |✓ |
| Azure Resource Manager şablonu |✓ |&nbsp; |

Betik eylemleri uygulamak için bu yöntemleri kullanma hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemleri kullanılarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Komut dosyası geliştirme için en iyi yöntemler

Hdınsight kümesi için özel bir komut dosyası geliştirirken dikkate alınması gereken birkaç en iyi yöntemler vardır:

* [Hedef Hadoop sürümü](#bPS1)
* [Hedef işletim sistemi sürümü](#bps10)
* [Komut dosyası kaynaklara kararlı bağlantılar sağlar](#bPS2)
* [Önceden derlenmiş kaynakları kullanın](#bPS4)
* [Küme özelleştirme betik ıdempotent olduğundan emin olun](#bPS3)
* [Küme mimari yüksek kullanılabilirliğini sağlamak](#bPS5)
* [Azure Blob storage kullanma özel bileşenlerini yapılandırma](#bPS6)
* [STDOUT ve STDERR bilgi yazma](#bPS7)
* [Dosyaları ASCII olarak LF satır sonları ile kaydedin.](#bps8)
* [Geçici hataları kurtarmak için yeniden deneme mantığı kullanın](#bps9)

> [!IMPORTANT]
> Betik eylemleri 60 dakika içinde tamamlamanız gerekir veya işlemi başarısız olur. Düğüm sağlama işlemi sırasında diğer Kurulum ve yapılandırma işlemleri eşzamanlı olarak komut dosyasını çalıştırır. CPU süresi veya ağ bant genişliği gibi kaynakları için rekabet geliştirme ortamınızı makinelerinden tamamlanması daha uzun sürmesine betik neden olabilir.

### <a name="bPS1"></a>Hedef Hadoop sürümü

Farklı sürümlerini Hdınsight Hadoop Hizmetleri ve bileşenleri yüklü farklı sürümlerine sahip. Komut bir hizmet veya bileşenin belirli bir sürümünü görüyorsa yalnızca komut dosyasını gerekli bileşenleri içerir Hdınsight sürümü ile kullanmanız gerekir. Hdınsight kullanma ile dahil bileşen sürümleri hakkında bilgi bulabilirsiniz [Hdınsight bileşen sürümü oluşturma](hdinsight-component-versioning.md) belge.

### <a name="bps10"></a>Hedef işletim sistemi sürümü

Linux tabanlı Hdınsight Ubuntu Linux dağıtım temel alır. Hdınsight farklı sürümlerini farklı sürümlerine ilişkin kodunuzu nasıl davranacağını değişebilir Ubuntu kullanır. Örneğin, Hdınsight 3.4 ve önceki Upstart kullanmak Ubuntu sürümlerinde dayalı. Sürüm 3.5 Systemd kullanan Ubuntu 16.04 üzerinde temel alır. İkisi ile çalışmak için komut dosyanızı yazılması gereken şekilde Systemd ve Upstart farklı komutlarını kullanır.

Başka bir önemli fark Hdınsight 3.4 3.5 arasındaki `JAVA_HOME` şimdi Java 8 işaret eder.

Kullanarak işletim sistemi sürümü denetleyebilirsiniz `lsb_release`. Aşağıdaki kod, komut dosyası Ubuntu 14 veya 16 üzerinde çalışıp çalışmadığı belirlenemedi gösterilmiştir:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Bu parçacıkları https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh adresindeki içeren tam komut dosyası bulabilirsiniz.

Hdınsight tarafından kullanılan Ubuntu sürümü için bkz: [Hdınsight bileşen sürümü](hdinsight-component-versioning.md) belge.

Systemd ve Upstart arasındaki farkları anlamak için bkz: [Systemd Upstart kullanıcılar için](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Komut dosyası kaynaklara kararlı bağlantılar sağlar

Komut dosyası ve ilişkili kaynakları kümenin kullanım ömrü kullanılabilir kalmalıdır. Yeni düğümler için küme ölçeklendirme işlemleri sırasında eklenirse, bu kaynakları gereklidir.

Karşıdan yükle ve her şeyi aboneliğinizi Azure depolama hesabında arşivlemek için en iyi uygulamadır bakın.

> [!IMPORTANT]
> Kullanılan depolama hesabı başka bir depolama hesabı üzerinde Küme ya da bir ortak, salt okunur kapsayıcı için varsayılan depolama hesabı olması gerekir.

Örneğin, Microsoft tarafından sağlanan örnekleri depolanmış [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) depolama hesabı. Bu Hdınsight ekibi tarafından korunan bir ortak, salt okunur kapsayıcıdır.

### <a name="bPS4"></a>Önceden derlenmiş kaynakları kullanın

Komut dosyasını çalıştırmak için gereken süreyi azaltmak için kaynak kodu kaynaklardan derleme işlemleri kaçının. Örneğin, önceden derleme kaynakları ve Hdınsight aynı veri merkezindeki bir Azure depolama hesabı blob depolayın.

### <a name="bPS3"></a>Küme özelleştirme betik ıdempotent olduğundan emin olun

Komut dosyaları ıdempotent olması gerekir. Komut dosyası birden çok kez çalıştırıyorsa, bu her zaman aynı duruma küme döndürmelidir.

Örneğin, yapılandırma dosyalarını değiştiren bir komut dosyası yinelenen giriş varsa eklememelisiniz birden çok kez çalıştı.

### <a name="bPS5"></a>Küme mimari yüksek kullanılabilirliğini sağlamak

Linux tabanlı Hdınsight kümeleri küme içinde etkin olan iki baş düğümler sağlar ve her iki düğümde betik eylemleri çalıştırın. Yüklediğiniz bileşenlerin tek bir baş düğüm bekliyorsanız, bileşenleri her iki baş düğümler üzerinde yüklemeyin.

> [!IMPORTANT]
> Hdınsight bir parçası olarak sağlanan hizmetlerin iki baş düğümler arasında gerektiğinde yük devredecek biçimde tasarlanmıştır. Bu işlevsellik, betik eylemleri yüklü özel bileşenlerine genişletilmedi. Özel bileşenler için yüksek kullanılabilirlik gerekiyorsa, kendi yük devretme mekanizması uygulamalıdır.

### <a name="bPS6"></a>Azure Blob storage kullanma özel bileşenlerini yapılandırma

Kümede yüklemeniz bileşenleri Hadoop dağıtılmış dosya sistemi (HDFS) depolama kullanan bir varsayılan yapılandırmaya sahip olabilir. Hdınsight, varsayılan depolama alanı olarak Azure Storage veya Data Lake Store kullanır. Her iki küme silinse bile veri devam ederse bir HDFS uyumlu bir dosya sistemi sağlar. WASB veya ADL HDFS yerine kullanmak için yüklediğiniz bileşenler yapılandırmanız gerekebilir.

İşlemlerinin çoğu için dosya sistemi belirtmeniz gerekmez. Örneğin, aşağıdaki giraph examples.jar dosyanın yerel dosya sisteminden küme depolama birimine kopyalar:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

Bu örnekte, `hdfs` komut varsayılan küme depolama saydam olarak kullanır. Bazı işlemler için URI belirtmeniz gerekebilir. Örneğin, `adl:///example/jars` Data Lake Store için veya `wasb:///example/jars` Azure depolama.

### <a name="bPS7"></a>STDOUT ve STDERR bilgi yazma

Hdınsight STDOUT ve STDERR yazılan komut dosyası çıkışını günlüğe kaydeder. Ambari web kullanıcı arabirimini kullanarak bu bilgileri görüntüleyebilirsiniz.

> [!NOTE]
> Ambari, yalnızca küme başarıyla oluşturulduysa kullanılabilir. Küme oluşturma ve oluşturma başarısız sırasında bir betik eylemi kullanın, sorun giderme bölümüne bakın. [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) günlüğe kaydedilen bilgileri erişme diğer yolları için.

Ek günlük eklemek isteyebilirsiniz ancak çoğu yardımcı programları ve yükleme paketleri zaten bilgi STDOUT ve STDERR, yazma. Metin STDOUT göndermek için kullanmak `echo`. Örneğin:

```bash
echo "Getting ready to install Foo"
```

Varsayılan olarak, `echo` STDOUT dizesi gönderir. STDERR yönlendirmek için ekleme `>&2` önce `echo`. Örneğin:

```bash
>&2 echo "An error occurred installing Foo"
```

STDOUT STDERR (2) için bunun yerine yazılan bilgilerin yönlendirir. G/ç yeniden yönlendirme hakkında daha fazla bilgi için bkz: [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Betik eylemleri tarafından günlüğe kaydedilen bilgi görüntüleme hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a>Dosyaları ASCII olarak LF satır sonları ile kaydedin.

Bash betiklerini ASCII biçiminde LF tarafından sonlandırıldı çizgili depolanması gerekir. UTF-8 olarak depolanır veya satır sonu olarak CRLF kullanan dosyalar, şu hata ile başarısız:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a>Geçici hataları kurtarmak için yeniden deneme mantığı kullanın

Get apt veya Internet üzerinden veri aktaran diğer eylemler kullanarak paketleri yükleme dosyaları indirilirken eylemi geçici ağ hataları nedeniyle başarısız olabilir. Örneğin, bir yedekleme düğüme yapabilmesini sürecinde ile iletişim kuran uzak kaynak olabilir.

Kodunuzu geçici hataları esnek hale getirmek için yeniden deneme mantığı uygulayabilirsiniz. Aşağıdaki işlevi, yeniden deneme mantığını uygulaması gösterilmiştir. Bu, üç kez başarısız olmadan önce işlemi yeniden dener.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

Aşağıdaki örnekler, bu işlevi kullanmak nasıl ekleyebileceğiniz gösterilmektedir.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Özel komut dosyaları için yardımcı yöntemleri

Betik eylem Yardımcısı yöntemleri özel komut dosyaları yazılırken kullanabileceğiniz yardımcı programları ' dir. Bu yöntemler bulunan[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) komut dosyası. Karşıdan yüklemek ve komut dosyanızı bir parçası olarak kullanmak için aşağıdakileri kullanın:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Aşağıdaki Yardımcıları komut dosyanız için kullanılabilir:

| Yardımcı kullanımı | Açıklama |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Bir dosya için belirtilen dosya yolu Kaynak URI'sı yükler. Varsayılan olarak, varolan bir dosyanın üzerine yazmaz. |
| `untar_file TARFILE DESTDIR` |Tar dosyasını ayıklar (kullanarak `-xf`) hedef dizin. |
| `test_is_headnode` |Bir küme baş düğümünde, dönüş 1; çalışan Aksi takdirde, 0. |
| `test_is_datanode` |Geçerli düğüm (çalışan) veri düğümü ise 1 döner; Aksi takdirde, 0. |
| `test_is_first_datanode` |Geçerli düğüm (çalışan) ilk veri ise düğümü (adlandırılmış workernode0) 1 döner; Aksi takdirde, 0. |
| `get_headnodes` |Kümede headnodes tam etki alanı adını döndürür. Virgülle ayrılmış adlardır. Boş bir dize hatası döndürülür. |
| `get_primary_headnode` |Birincil headnode tam etki alanı adını alır. Boş bir dize hatası döndürülür. |
| `get_secondary_headnode` |İkincil headnode tam etki alanı adını alır. Boş bir dize hatası döndürülür. |
| `get_primary_headnode_number` |Birincil headnode sayısal sonekini alır. Boş bir dize hatası döndürülür. |
| `get_secondary_headnode_number` |İkincil headnode sayısal sonekini alır. Boş bir dize hatası döndürülür. |

## <a name="commonusage"></a>Genel kullanım desenleri

Bu bölümde, kendi özel bir komut dosyası yazılırken içine çalışabilir ortak kullanım desenlerini bazıları uygulama yönergeler sağlanmaktadır.

### <a name="passing-parameters-to-a-script"></a>Bir komut dosyası parametreleri geçirme

Bazı durumlarda, komut parametreleri gerektirebilir. Örneğin, Ambari REST API kullanırken küme için yönetici parolasını gerekebilir.

Komut dosyasına iletilen parametreler olarak bilinen *konumsal parametreler*ve atanan `$1` ilk parametresi için `$2` ikinci için ve böylece açma. `$0`komut dosyasının kendisini adını içerir.

Parametreler (') tek tırnak içine alınması gibi değerler komut dosyasına iletilen. Geçirilen değeri sabit değer olarak davranılır sağlar.

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

Bir ortam değişkeni ayarı aşağıdaki deyimi tarafından gerçekleştirilir:

    VARIABLENAME=value

Burada VARIABLENAME değişkenin adıdır. Değişkeni erişmek için `$VARIABLENAME`. Örneğin, parola adlı bir ortam değişkeni bir konumsal parametre tarafından sağlanan bir değer atamak için şu deyimi kullanın:

    PASSWORD=$1

Sonraki bilgilere erişimi daha sonra kullanabilir `$PASSWORD`.

Komut dosyası içinde ayarlamak ortam değişkenleri yalnızca betik kapsamında mevcut. Bazı durumlarda, komut dosyası tamamlandıktan sonra korunur sistem geneli ortam değişkenleri eklemeniz gerekebilir. Sistem geneli ortam değişkenleri eklemek için değişkeni eklemek `/etc/environment`. Örneğin, aşağıdaki ifadeyi ekler `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Özel komut dosyaları depolandığı konumuna erişim

Bir küme özelleştirmek için kullanılan komut aşağıdaki konumlardan birinde depolanması gerekir:

* Bir __Azure depolama hesabı__ kümeyle ilişkili.

* Bir __ek depolama alanı hesabı__ kümesi ile ilişkili.

* A __herkese açık şekilde okunabilir URI__. Örneğin, OneDrive, Dropbox veya barındırma hizmeti başka bir dosyaya depolanan verileri URL'sine.

* Bir __Azure Data Lake Store hesabı__ Hdınsight kümesi ile ilişkili. Hdınsight ile Azure Data Lake Store kullanma hakkında daha fazla bilgi için bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > Hdınsight Data Lake Store erişmek için kullandığı hizmet sorumlusu komut dosyasını okuma erişimi olması gerekir.

Komut dosyası tarafından kullanılan kaynakları da genel kullanıma açık olması gerekir.

Bir Azure depolama hesabı ya da Azure Data Lake Store dosyaların depolanması Azure ağından her ikisi olarak hızlı erişim sağlar.

> [!NOTE]
> Komut dosyası başvurmak için kullanılan URI biçimi kullanılan hizmete bağlı olarak farklılık gösterir. Hdınsight kümesi ile ilişkili depolama hesapları için `wasb://` veya `wasbs://`. Genel olarak okunabilir URI'ler için kullanmak `http://` veya `https://`. Data Lake Store için kullanma `adl://`.

### <a name="checking-the-operating-system-version"></a>İşletim sistemi sürümü denetimi

Hdınsight farklı sürümlerini Ubuntu belirli sürümlerini kullanır. Komut dosyanız için denetlemelisiniz işletim sistemi sürümleri arasındaki farklar olabilir. Örneğin, Ubuntu sürümüne bağlı bir ikili yüklemeniz gerekebilir.

İşletim sistemi sürümü denetlemek için kullanmak `lsb_release`. Örneğin, aşağıdaki komut dosyası işletim sistemi sürümüne bağlı olarak belirli tar dosyasına başvurmak gösterilmiştir:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Betik eylemi dağıtma denetim listesi

Biz bu komut dosyaları dağıtmak hazırlarken sürdü adımlar şunlardır:

* Dağıtım sırasında küme düğümleri tarafından erişilebilen bir yerde özel komut dosyaları içeren dosyaları yerleştirin. Örneğin, kümenin varsayılan depolama. Dosyaları herkese açık şekilde okunabilir barındırma hizmetleri de depolanabilir.
* Komut dosyası impotent olduğunu doğrulayın. Bunun yapılması, birden çok kez aynı düğümde yürütülmek üzere betik sağlar.
* Komut dosyaları tarafından kullanılan indirilen dosyaları korumak ve komut dosyaları çalıştırdıktan sonra sonra bunları temizlemek için bir geçici dosya dizin tmp kullanın.
* İşletim sistemi düzeyinde ayarlarını veya Hadoop hizmeti yapılandırma dosyalarını değiştirdiyseniz, Hdınsight hizmetlerini yeniden başlatmak istediğinizi düşünelim.

## <a name="runScriptAction"></a>Betik eylemi gerçekleştirme

Aşağıdaki yöntemleri kullanarak Hdınsight kümelerini özelleştirme için betik eylemleri kullanın:

* Azure portalına
* Azure PowerShell
* Azure Resource Manager şablonları
* Hdınsight .NET SDK'sı.

Her yöntemi kullanma hakkında daha fazla bilgi için bkz: [betik eyleminin nasıl kullanılacağını](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Özel kod örnekleri

Microsoft, bir Hdınsight kümesine bileşenleri yüklemek için örnek komut dosyaları sağlar. Daha fazla örnek betik eylemleri için aşağıdaki bağlantılara bakın.

* [Yükleme ve Hdınsight kümelerinde ton kullanın](hdinsight-hadoop-hue-linux.md)
* [Yükleme ve Hdınsight kümelerinde Solr kullanma](hdinsight-hadoop-solr-install-linux.md)
* [Yükleme ve Hdınsight kümelerinde Giraph kullanma](hdinsight-hadoop-giraph-install-linux.md)
* [Yükleme veya Hdınsight kümelerinde Mono yükseltme](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a>Sorun giderme

Geliştirilmiş komut dosyaları kullanırken karşılaşabileceğiniz hatalar şunlardır:

**Hata**: `$'\r': command not found`. Bazen arkasından `syntax error: unexpected end of file`.

*Neden*: bir komut satırları ile CRLF sonlandırdığınızda, bu hataya neden olur. UNIX sistemleri yalnızca LF satır bitiş olarak bekler.

Komut dosyası bir Windows ortamı yazılan CRLF birçok metin düzenleyicileri Windows'da için bitiş ortak bir çizgi olarak çoğunlukla oluşur.

*Çözümleme*: metin düzenleyicinizde bir seçenek ise, UNIX biçimini veya LF için satır sonu seçin. Bir LF CRLF değiştirmek için bir Unix sistem üzerinde aşağıdaki komutları de kullanabilirsiniz:

> [!NOTE]
> Aşağıdaki komutlar için LF CRLF satır sonları değiştirmeniz gerekir, kabaca eşdeğerdir. Sisteminizde yardımcı programları göre seçin.

| Komut | Notlar |
| --- | --- |
| `unix2dos -b INFILE` |Özgün dosya ile yedeklenir bir. BAK uzantısı |
| `tr -d '\r' < INFILE > OUTFILE` |Yalnızca LF sonları sürümüyle ÇIKIŞDOSYASI içerir |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Dosyayı doğrudan değiştirir |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |ÇIKIŞDOSYASI yalnızca LF sonları ile bir sürüm içeriyor. |

**Hata**: `line 1: #!/usr/bin/env: No such file or directory`.

*Neden*: UTF-8 bir bayt sırası işareti (BOM) ile olarak komut dosyası kaydedildiğinde bu hata oluşur.

*Çözümleme*: dosyayı olarak ASCII veya UTF-8 bir ürün reçetesi olmadan olarak kaydedin. Bir dosya AĞACI olmadan oluşturmak için bir Linux veya UNIX sistemde aşağıdaki komutu de kullanabilirsiniz:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Değiştir `INFILE` AĞACI içeren dosya ile. `OUTFILE`Ürün reçetesi olmadan komut dosyasını içeren yeni bir dosya adı olmalıdır.

## <a name="seeAlso"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md)
* Kullanım [Hdınsight .NET SDK'sı başvurusu](https://msdn.microsoft.com/library/mt271028.aspx) Hdınsight yönetmek .NET uygulamaları oluşturma hakkında daha fazla bilgi edinmek için
* Kullanmak [Hdınsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) REST Hdınsight kümelerinde yönetim eylemleri gerçekleştirmek için nasıl kullanılacağını öğrenin.
