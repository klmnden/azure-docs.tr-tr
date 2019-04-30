---
title: Azure HDInsight kümeleri özelleştirmek için betik eylemleri geliştirme
description: HDInsight kümeleri özelleştirmek için Bash betiklerini kullanmayı öğrenin. Betik eylemleri, küme yapılandırma ayarlarını değiştirmek veya ek yazılımlar yüklemek için küme oluşturma sırasında veya sonrasında betikleri çalıştırmanıza olanak tanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: 66132a2a6a7b5b89bca0767efe7c194ca3dec051
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60590790"
---
# <a name="script-action-development-with-hdinsight"></a>HDInsight ile betik eylemi geliştirme

Bash betiklerini kullanarak HDInsight kümenize özelleştirmeyi öğrenin. Betik eylemleri, HDInsight küme oluşturma sırasında veya sonrasında özelleştirmek için bir yoludur.

## <a name="what-are-script-actions"></a>Betik eylemleri nelerdir

Betik eylemleri yapma yapılandırma değişiklikleri veya yazılım yükleme için Azure küme düğümleri üzerinde çalışan Bash betikleridir. Betik eylemi kök olarak yürütülür ve küme düğümleri için tam erişim hakları sağlar.

Betik eylemleri aşağıdaki yöntemleri uygulanabilir:

| Bir betik uygulamak için bu yöntemi kullanın... | Sırasında oluşturma küme... | Çalışan bir kümede... |
| --- |:---:|:---:|
| Azure portal |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Klasik Azure CLI |&nbsp; |✓ |
| HDInsight .NET SDK'sı |✓ |✓ |
| Azure Resource Manager Şablonu |✓ |&nbsp; |

Betik eylemleri uygulamak için bu yöntemleri kullanma hakkında daha fazla bilgi için bkz. [özelleştirme HDInsight kümelerini betik eylemlerini kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Betik geliştirme için en iyi uygulamalar

Bir HDInsight kümesi için özel bir betik geliştirirken akılda tutulması gereken birkaç en iyi uygulama vardır:

* [Apache Hadoop sürümünü hedefleme](#bPS1)
* [Hedef işletim sistemi sürümü](#bps10)
* [Betik kaynakların kararlı bağlantıları sağlayın](#bPS2)
* [Önceden derlenmiş kaynaklarını kullanma](#bPS4)
* [Küme özelleştirmesi betiğini kez etkili olduğundan emin olun](#bPS3)
* [Yüksek kullanılabilirlik kümesi mimarisi](#bPS5)
* [Azure Blob depolamayı kullanmak için özel bileşenlerini yapılandırma](#bPS6)
* [STDOUT ve STDERR bilgilerini yazma](#bPS7)
* [Dosyaları ASCII LF satır sonları ile kaydedin.](#bps8)
* [Geçici hataları kurtarmak için yeniden deneme mantığı kullanın](#bps9)

> [!IMPORTANT]  
> Betik eylemleri, 60 dakika içinde tamamlanması veya işlemi başarısız olur. Düğüm sağlama sırasında diğer Kurulum ve yapılandırma işlemleri aynı anda komut dosyasını çalıştırır. CPU süresi veya ağ bant genişliği gibi kaynak rekabetini betiği geliştirme ortamınızda gösterilenden tamamlanması uzun sürmesine neden olabilir.

### <a name="bPS1"></a>Apache Hadoop sürümünü hedefleme

Farklı sürümlerini HDInsight Hadoop Hizmetleri ve bileşenleri yüklü farklı sürümlerine sahip. Betiğinizi bir hizmet veya bileşenin belirli bir sürümünü görüyorsa, yalnızca betik gerekli bileşenleri içeren bir HDInsight sürümü ile kullanmanız gerekir. HDInsight'ı kullanarak dahil bileşen sürümleri hakkında bilgi bulabilirsiniz [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md) belge.

### <a name="checking-the-operating-system-version"></a>İşletim sistemi sürümü denetleniyor

HDInsight'ın farklı sürümlerini Ubuntu belirli sürümlerinde kullanır. Betiğinizde için denetlemelisiniz işletim sistemi sürümleri arasındaki farklar olabilir. Örneğin, Ubuntu sürümüne bağlı olmayan bir ikili yüklemeniz gerekebilir.

İşletim sistemi sürümü denetlemek için kullanmak `lsb_release`. Örneğin, aşağıdaki betik, bir işletim sistemi sürümüne bağlı olarak belirli tar dosyasını nasıl başvurulacağını gösterir:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

### <a name="bps10"></a> Hedef işletim sistemi sürümü

Linux tabanlı HDInsight, Ubuntu Linux dağıtımı temel alır. HDInsight'ın farklı sürümlerini, kodunuzu nasıl davranacağını değişebilir Ubuntu, farklı sürümlerinde kullanır. Örneğin, HDInsight 3.4 ve önceki Upstart kullanan Ubuntu sürümlerinde bağlıdır. 3.5 ve üzeri sürümleri kullanan Systemd Ubuntu 16.04 üzerinde temel alır. Betiğinizi ikisiyle iş yazılması gerektiğini şekilde Systemd ve Upstart farklı komutları kullanır.

HDInsight 3.4 ve 3.5 arasında başka bir önemli fark `JAVA_HOME` artık Java 8 işaret eder. Aşağıdaki kodu betik Ubuntu 14 ya da 16 üzerinde çalışıp çalışmadığı belirlenemedi gösterilmektedir:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-16-04."
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

Bu kod parçacıklarının en içeren tam komut dosyası bulabilirsiniz https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

HDInsight tarafından kullanılan Ubuntu sürümü için bkz: [HDInsight bileşen sürümü](hdinsight-component-versioning.md) belge.

Systemd Upstart arasındaki farkları anlamak için bkz: [Upstart kullanıcıların Systemd](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Betik kaynakların kararlı bağlantıları sağlayın

İlişkili kaynakları ve betik, kümenin kullanım ömrü boyunca kullanılabilir kalması gerekir. Bu kaynaklar, ölçeklendirme işlemleri sırasında kümeye eklenen yeni düğümlerden kullanıyorsa gereklidir.

İndirmek ve her şeyi aboneliğinizde bir Azure depolama hesabında arşivlemek için en iyi yöntem olacaktır.

> [!IMPORTANT]  
> Kullanılan depolama hesabı kümenin veya genel ve salt okunur bir kapsayıcı için varsayılan depolama hesabı herhangi bir depolama hesabında olmalıdır.

Örneğin, Microsoft tarafından sağlanan örnekleri depolanan [ https://hdiconfigactions.blob.core.windows.net/ ](https://hdiconfigactions.blob.core.windows.net/) depolama hesabı. HDInsight ekibi tarafından korunan bir genel ve salt okunur kapsayıcı konumdur.

### <a name="bPS4"></a>Önceden derlenmiş kaynaklarını kullanma

Betiği çalıştırmak için gereken süreyi azaltmak için kaynak kodu kaynaklardan derleme işlemleri kaçının. Örneğin, kaynakları önceden derlemek ve HDInsight ile aynı veri merkezindeki bir Azure depolama hesabı BLOB depolamaya.

### <a name="bPS3"></a>Küme özelleştirmesi betiğini kez etkili olduğundan emin olun

Betik bir kez etkili olmalıdır. Komut dosyası birden çok kez çalışıyorsa, kümenin her zaman aynı duruma döndürmesi gerekir.

Örneğin, yapılandırma dosyalarını değiştiren bir komut dosyası yinelenen girişler varsa eklememelisiniz birden çok kez çalıştırıldı.

### <a name="bPS5"></a>Yüksek kullanılabilirlik kümesi mimarisi

Linux tabanlı HDInsight kümeleri küme içinde etkin olan iki baş düğümü sağlar ve her iki düğümde de betik eylemleri çalıştırın. Yüklediğiniz bileşenleri tek bir baş düğüm bekliyorsanız, her iki baş düğümlerine bileşenlerini yüklemeyin.

> [!IMPORTANT]  
> HDInsight'ın bir parçası olarak sunulan hizmetler arasında iki baş düğüm gerektiği şekilde yük devretmek için tasarlanmıştır. Bu işlev, betik eylemleri yüklü özel bileşenler genişletilmedi. Özel bileşenler için yüksek kullanılabilirlik gerekiyorsa, kendi yük devretmeyi mekanizması uygulamanız gerekir.

### <a name="bPS6"></a>Azure Blob depolamayı kullanmak için özel bileşenlerini yapılandırma

Kümede yüklemek bileşenleri, Apache Hadoop dağıtılmış dosya sistemi (HDFS) depolama kullanan varsayılan yapılandırması olabilir. HDInsight, varsayılan depolama alanı olarak Azure depolama ya da Data Lake Storage kullanır. Her iki küme silinse bile, veri devam HDFS uyumlu bir dosya sistemi sağlar. WASB veya ADL HDFS yerine kullanılacak Yükleme bileşenleri yapılandırmanız gerekebilir.

İşlemlerinin çoğu için dosya sistemini belirtmek gerekmez. Örneğin, aşağıdaki hadoop common.jar dosyanın yerel dosya sisteminden küme depolama alanına kopyalayan:

```bash
hdfs dfs -put /usr/hdp/current/hadoop-client/hadoop-common.jar /example/jars/
```

Bu örnekte, `hdfs` komutu şeffaf bir şekilde varsayılan küme depolama kullanır. Bazı işlemler için URI belirtmeniz gerekebilir. Örneğin, `adl:///example/jars` için Azure Data Lake depolama Gen1 `abfs:///example/jars` için Data Lake depolama Gen2'ye veya `wasb:///example/jars` Azure depolama için.

### <a name="bPS7"></a>STDOUT ve STDERR bilgilerini yazma

HDInsight, STDOUT ve STDERR yazılan betik çıktısı günlüğe kaydeder. Ambari web kullanıcı arabirimini kullanarak bu bilgileri görüntüleyebilirsiniz.

> [!NOTE]  
> Apache Ambari, yalnızca küme başarıyla oluşturulursa kullanılabilir. Oluşturma başarısız olur ve küme oluşturma sırasında bir betik eylemi kullanmanız, sorun giderme bölümüne bakın. [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) oturum bilgilerine erişmek için diğer yöntemler için.

Ek günlükler eklemek isteyebilirsiniz ancak çoğu yardımcı programları ve yükleme paketleri zaten bilgi STDOUT ve STDERR, yazın. Metin STDOUT göndermek için `echo`. Örneğin:

```bash
echo "Getting ready to install Foo"
```

Varsayılan olarak, `echo` STDOUT dize gönderir. STDERR yönlendirmek için ekleme `>&2` önce `echo`. Örneğin:

```bash
>&2 echo "An error occurred installing Foo"
```

Bu, STDOUT, STDERR (2) için bunun yerine yazılan bilgi yönlendirir. G/ç yeniden yönlendirmesi hakkında daha fazla bilgi için bkz. [ https://www.tldp.org/LDP/abs/html/io-redirection.html ](https://www.tldp.org/LDP/abs/html/io-redirection.html).

Betik eylemleri tarafından günlüğe kaydedilen oturum bilgilerinin görüntüleme ile ilgili daha fazla bilgi için bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a> Dosyaları ASCII LF satır sonları ile kaydedin.

Bash betiklerini ASCII biçiminde LF tarafından sonlandırıldı satırlarla depolanması gerekir. UTF-8 depolanır veya satır sonu olarak CRLF kullanmak dosyaları şu hatayla başarısız olabilir:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a> Geçici hataları kurtarmak için yeniden deneme mantığı kullanın

Eylem, dosyaları apt-get ya da internet üzerinden veri iletmek diğer eylemleri kullanarak paketleri yükleme, indirme işlemi gerçekleştirirken geçici ağ hataları nedeniyle başarısız olabilir. Örneğin, bir yedekleme düğüme yük devretme işleminde ile iletişim kuran uzak kaynak olabilir.

Betiğinizi geçici hatalara karşı dayanıklı hale getirmek için yeniden deneme mantığını uygulayabilir. Aşağıdaki işlev, yeniden deneme mantığı uygulamak gösterilmektedir. Bu, üç kez başarısız olmadan önce işlemi yeniden dener.

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

Aşağıdaki örnekler, bu işlevin nasıl kullanılacağını göstermektedir.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Özel komut dosyaları için yardımcı yöntemleri

Betik eylem Yardımcısı yöntemleri özel betikler yazılırken kullanabileceğiniz yardımcı programları ' dir. Bu yöntemler bulunan [ https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh ](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) betiği. İndirmek ve bunları betiğinizin bir parçası olarak kullanmak için aşağıdakileri kullanın:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Aşağıdaki Yardımcıları betiğinizde kullanmak için kullanılabilir:

| Yardımcısı kullanımı | Açıklama |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Bir dosyayı belirtilen dosya yolu Kaynak URI indirir. Varsayılan olarak, varolan bir dosyanın üzerine yazmaz. |
| `untar_file TARFILE DESTDIR` |Tar dosyasını ayıklar (kullanarak `-xf`) hedef dizine. |
| `test_is_headnode` |Bir küme baş düğümünde, dönüş 1; çalıştı Aksi takdirde 0. |
| `test_is_datanode` |(Çalışan) veri düğümü geçerli düğüm ise 1 döndürür; Aksi takdirde 0. |
| `test_is_first_datanode` |Geçerli düğüm ilk veriler (çalışan) ise düğüm (adlandırılmış workernode0) 1 döndürür; Aksi takdirde 0. |
| `get_headnodes` |Kümenin baş tam etki alanı adını döndürür. Virgülle ayrılmış adlarıdır. Boş bir dize, hata döndürülür. |
| `get_primary_headnode` |Birincil baş tam etki alanı adını alır. Boş bir dize, hata döndürülür. |
| `get_secondary_headnode` |İkincil baş düğümüne tam etki alanı adını alır. Boş bir dize, hata döndürülür. |
| `get_primary_headnode_number` |Birincil baş sayısal sonekini alır. Boş bir dize, hata döndürülür. |
| `get_secondary_headnode_number` |İkincil baş düğümüne sayısal sonekini alır. Boş bir dize, hata döndürülür. |

## <a name="commonusage"></a>Ortak kullanım desenleri

Bu bölümde, kendi özel betik yazarken karşılaşabileceğiniz genel kullanım düzenlerine bazıları uygulama yönergeler sağlanmaktadır.

### <a name="passing-parameters-to-a-script"></a>Bir betik parametreleri geçirme

Bazı durumlarda, kodunuzu parametreleri gerektirebilir. Örneğin, Ambari REST API kullanırken küme için yönetici parolasını gerekebilir.

Komut dosyasına iletilen parametreler olarak bilinir *konumsal parametreler*ve atanan `$1` ilk parametresi için `$2` , ikinci ve bu nedenle açma. `$0` betiğin adı içerir.

Parametreler (') tek tırnak işareti içine alınması gibi değerleri betiğe geçirilen. Geçirilen değerin değişmez değer olarak kabul edilir sağlar.

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

Bir ortam değişkenini ayarlayarak aşağıdaki deyimi tarafından gerçekleştirilir:

    VARIABLENAME=value

Burada VARIABLENAME değişkenin adıdır. Değişken erişmek için `$VARIABLENAME`. Örneğin, konumsal bir parametre tarafından sağlanan parola adlı bir ortam değişkeni bir değer atamak için aşağıdaki deyimi kullanın:

    PASSWORD=$1

Sonraki bilgilere erişimi daha sonra kullanabilir `$PASSWORD`.

Ortam değişkenleri komut dosyası içinde ayarlayın, yalnızca betik kapsamında mevcut. Bazı durumlarda, betik tamamladıktan sonra açık kalır sistem geneli ortam değişkenleri eklemeniz gerekebilir. Sistem geneli ortam değişkenleri eklemek için değişkeni eklemek `/etc/environment`. Örneğin, aşağıdaki deyimi ekler `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Özel komut dosyaları depolandığı konumlara erişim

Bir küme özelleştirmek için kullanılan betikleri aşağıdaki konumlardan birinde depolanması gerekir:

* Bir __Azure depolama hesabı__ kümeyle ilişkili.

* Bir __ek depolama hesabı__ kümeyle ilişkili.

* A __herkese açık şekilde okunabilir URI__. Örneğin, OneDrive, Dropbox veya barındırma hizmeti başka bir dosya üzerinde depolanan verileri URL'si.

* Bir __Azure Data Lake Store hesabına__ HDInsight kümesi ile ilişkili olan. Azure Data Lake Store ile HDInsight kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

    > [!NOTE]  
    > HDInsight, Data Lake depolamaya erişmek için kullandığı hizmet sorumlusu, betik okuma erişiminiz olması gerekir.

Komut dosyası tarafından kullanılan kaynakları da genel kullanıma açık olması gerekir.

Bir Azure depolama hesabına veya Azure Data Lake Storage dosyaların depolanması, Azure ağı içinde her ikisi de olarak hızlı erişim sağlar.

> [!NOTE]  
> Betik başvurmak için kullanılan URI biçimi, kullanılan hizmete bağlı olarak farklılık gösterir. HDInsight kümesi ile ilişkili depolama hesapları için kullanmak `wasb://` veya `wasbs://`. Genel olarak okunabilir URI'ler için kullanmak `http://` veya `https://`. Data Lake depolama için kullanmak `adl://`.

## <a name="deployScript"></a>Betik eylemi dağıtma denetim listesi

Bir betiği dağıtmak hazırlarken adımları sınav zamanı şunlardır:

* Özel komut dosyaları, dağıtım sırasında küme düğümleri tarafından erişilebilir bir yerde içeren dosyaları yerleştirin. Örneğin, kümenin varsayılan depolama alanı. Dosyaları herkese açık şekilde okunabilir barındırma hizmetleri da depolanabilir.
* Betik kez etkili olduğundan emin olun. Bunun yapılması, birden çok kez aynı düğümde yürütülmek üzere betik sağlar.
* Geçici dosya dizini/tmp betikler tarafından kullanılan indirilen dosyaları tutmak ve betikleri çalıştırdıktan sonra sonra temizlenmesi için kullanın.
* İşletim sistemi düzeyindeki ayarları veya Hadoop hizmeti yapılandırma dosyalarını değiştirilirse, HDInsight hizmetleri yeniden başlatmak isteyebilirsiniz.

## <a name="runScriptAction"></a>Betik eylemi çalıştırmayı öğrenin

Betik eylemleri, HDInsight kümeleri aşağıdaki yöntemleri kullanarak özelleştirmek için kullanabilirsiniz:

* Azure portal
* Azure PowerShell
* Azure Resource Manager şablonları
* The HDInsight .NET SDK.

Her yöntemi kullanma hakkında daha fazla bilgi için bkz. [betik eylemi kullanmayı](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Özel betik örnekleri

Microsoft, bir HDInsight kümesinde bileşenleri yüklemek için örnek betikler sağlar. Daha fazla örnek betik eylemleri için aşağıdaki bağlantılara bakın.

* [Yükleme ve HDInsight kümelerinde Hue kullanma](hdinsight-hadoop-hue-linux.md)
* [Yükleme ve HDInsight kümeleri üzerinde Apache giraph'ı kullanma](hdinsight-hadoop-giraph-install-linux.md)

## <a name="troubleshooting"></a>Sorun giderme

Betikler, geliştirdiğiniz kullanırken karşılaşabileceğiniz hatalar şunlardır:

**Hata**: `$'\r': command not found`. Bazen ardından `syntax error: unexpected end of file`.

*Neden*: Bir komut satırları ile CRLF sonlandırdığınızda, bu hataya neden olur. Unix sistemlerinden yalnızca LF olarak satır sonu beklenir.

Komut dosyasını bir Windows ortamda yazıldığı sırada CRLF Windows üzerinde birçok metin düzenleyiciler için bitiş ortak bir çizgi olarak çoğunlukla bu sorun oluşur.

*Çözüm*: Metin düzenleyicinizde bir seçenek ise, UNIX biçimini veya LF sondaysa satır için seçin. Ayrıca bir LF için CRLF değiştirmek için UNIX sisteminde aşağıdaki komutları kullanabilirsiniz:

> [!NOTE]  
> Aşağıdaki komutları için LF CRLF satır sonlarını değiştirmeniz gerekir, kabaca eşdeğerdir. Sisteminizde kullanılabilir yardımcı programları göre seçin.

| Komut | Notlar |
| --- | --- |
| `unix2dos -b INFILE` |Özgün dosya ile yedeklenir bir. BAK uzantısı |
| `tr -d '\r' < INFILE > OUTFILE` |Yalnızca LF sonlarını sürümüyle ÇIKIŞDOSYASI içerir |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Dosyayı doğrudan değiştirir |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |Yalnızca LF sonlarını sürümüyle ÇIKIŞDOSYASI içerir. |

**Hata**: `line 1: #!/usr/bin/env: No such file or directory`.

*Neden*: Komut dosyasının UTF-8 bir bayt sırası işareti (BOM) ile olarak kaydedildiğinde, bu hata oluşur.

*Çözüm*: Dosyayı, ASCII olarak veya bir ürün reçetesi UTF-8 olarak kaydedin. Ayrıca içermeyen ürün reçetesi bir dosya oluşturmak için bir Linux veya UNIX sisteminde aşağıdaki komutu kullanabilirsiniz:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Değiştirin `INFILE` BOM içeren dosya. `OUTFILE` komut dosyası içermeyen ürün reçetesi içeren yeni bir dosya adı olmalıdır.

## <a name="seeAlso"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md)
* Kullanım [HDInsight .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight) HDInsight yöneten .NET uygulamaları oluşturma hakkında daha fazla bilgi edinmek için
* Kullanma [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) HDInsight kümelerinde yönetim eylemleri gerçekleştirmek için REST kullanma hakkında bilgi edinmek için.
