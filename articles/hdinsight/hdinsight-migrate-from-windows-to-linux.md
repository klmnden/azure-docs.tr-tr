---
title: Windows tabanlı HDInsight Linux tabanlı HDInsight için - Azure geçişi
description: Bir Linux tabanlı HDInsight kümesine bir Windows tabanlı HDInsight kümesinden geçirmeyi öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: hrasheed
ms.openlocfilehash: 49f55416cb9224736acd7b8ac5eac5b6c5ba5979
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707589"
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Windows tabanlı HDInsight kümesinden bir Linux tabanlı bir kümeye geçirme

Bu belgede, HDInsight üzerinde Windows ve Linux arasındaki farklar hakkında ayrıntılı bilgi sağlar. Ayrıca var olan iş yükleri için Linux tabanlı küme geçiş yapmaya yönelik rehberlik sağlar.

Windows tabanlı HDInsight bulutta Apache Hadoop kullanmak için kolay bir yol sağlarken, Linux tabanlı bir kümeye geçirmek gerekebilir. Örneğin, çözümünüz için gerekli olan Linux tabanlı araçları ve teknolojileri yararlanmak için. Pek çok Hadoop ekosistemindeki Linux tabanlı sistemler üzerinde geliştirilen ve Windows tabanlı HDInsight ile kullanmak için kullanılamıyor olabilir. Bir çok kitap, videolar ve diğer eğitim malzemeleri Hadoop ile çalışırken, bir Linux sistem kullandığınız varsayılır.

> [!NOTE]  
> HDInsight kümeleri kümedeki düğümler için işletim sistemi olarak Ubuntu uzun süreli destek (LTS) kullanın. Sürümü ile HDInsight, Ubuntu, yanı sıra diğer bileşen sürümü oluşturma bilgiler hakkında daha fazla bilgi için bkz: [HDInsight bileşen sürümü](hdinsight-component-versioning.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="migration-tasks"></a>Geçiş görevleri

Geçiş için genel iş akışı aşağıdaki gibidir.

![Geçiş iş akışı diyagramı](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Geçiş sırasında gerekli olabilecek değişiklikleri anlamak için bu belgenin her bölümünü okuyun.

2. Bir test/kalite güvencesi ortamı olarak Linux tabanlı bir küme oluşturun. Linux tabanlı küme oluşturma ile ilgili daha fazla bilgi için bkz: [oluşturma Linux tabanlı HDInsight kümelerinde](hdinsight-hadoop-provision-linux-clusters.md).

3. Mevcut işleri, veri kaynağı ve havuz yeni ortama kopyalayın.

4. İşlerinizi yeni kümede beklendiği gibi çalıştığından emin olmak için doğrulama sınaması gerçekleştirin.

Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra geçiş kapalı kalma süresi zamanlayın. Bu kesinti sırasında aşağıdaki eylemleri gerçekleştirin:

1. Küme düğümleri üzerinde yerel olarak depolanan tüm geçici verileri yedekleyin. Örneğin, doğrudan bir baş düğüm üzerinde depolanan verileri varsa.

2. Windows tabanlı küme silin.

3. Windows tabanlı kümeyle aynı varsayılan veri deposunu kullanarak Linux tabanlı bir küme oluşturun. Linux tabanlı küme var olan üretim verileriniz çalışmaya devam edebilirsiniz.

4. Yedeklediğiniz herhangi bir geçici veri içeri aktarın.

5. Başlangıç işleri/yeni küme kullanarak işleme devam edin.

### <a name="copy-data-to-the-test-environment"></a>Test ortamı için veri kopyalama

İşler ve veri kopyalamak için birçok yöntem vardır, ancak bu bölümde açıklanan iki basit için doğrudan yöntemlerdir test kümesi için hazırlayın.

#### <a name="hdfs-copy"></a>HDFS kopyalama

Verileri üretim kümeden test kümeye kopyalamak için aşağıdaki adımları kullanın. Bu adımları kullanın `hdfs dfs` HDInsight ile birlikte sağlanan yardımcı programı.

1. Depolama hesabı ve varsayılan kapsayıcı bilgileri mevcut kümenizin bulun. Aşağıdaki örnek, bu bilgileri almak için PowerShell kullanır:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. Bir test ortamı oluşturmak için HDInsight belge oluşturma Linux tabanlı kümelerde'ndaki adımları izleyin. Kümeyi oluşturmadan önce durdurun ve bunun yerine seçin **isteğe bağlı yapılandırma**.

3. İsteğe bağlı yapılandırma bölümünden seçin **bağlantılı depolama hesapları**.

4. Seçin **depolama anahtarı Ekle**ve istendiğinde, adım 1'de PowerShell komut dosyası tarafından döndürülen depolama hesabı seçin. Tıklayın **seçin** her bölümdeki. Son olarak, kümeyi oluşturun.

5. Küme oluşturulduktan sonra onu kullanarak bağlanma **SSH.** Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

6. SSH oturumundan, yeni varsayılan depolama hesabına bağlı depolama hesabındaki dosyaları kopyalamak için aşağıdaki komutu kullanın. KAPSAYICI, PowerShell tarafından döndürülen kapsayıcı bilgileri ile değiştirin. Değiştirin __hesabı__ hesap adına sahip. Veri yolu, bir veri dosyası yoluyla değiştirin.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]  
    > Verileri içeren dizin yapısı test ortamı mevcut değilse aşağıdaki komutu kullanarak oluşturabilirsiniz:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    `-p` Anahtar yolundaki tüm dizin oluşturulmasına olanak tanır.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Azure Depolama'daki blobları arasında doğrudan kopyalama

Alternatif olarak kullanmak isteyebilirsiniz `Start-AzStorageBlobCopy` HDInsight dışında depolama hesapları arasında BLOB'ları kopyalamak için Azure PowerShell cmdlet'i. Nasıl daha fazla bilgi için bkz. Azure depolama ile Azure PowerShell kullanarak Azure BLOB'ları bölümünü yönetmek için.

## <a name="client-side-technologies"></a>İstemci tarafı teknolojileri

Gibi istemci tarafı teknolojilerin [Azure PowerShell cmdlet'lerini](/powershell/azureps-cmdlets-docs), [Klasik Azure CLI'yı](../cli-install-nodejs.md), veya [Apache Hadoop için .NET SDK'sı](https://hadoopsdk.codeplex.com/) Linux tabanlı kümeler çalışmaya devam eder. Bu teknolojiler arasında her iki küme işletim sistemi türleri aynı olan REST API'lerini kullanır.

## <a name="server-side-technologies"></a>Sunucu tarafı teknolojileri

Aşağıdaki tabloda, Windows özgü olan geçirme sunucu tarafı bileşenleri rehberlik sağlar.

| Bu teknoloji kullanıyorsanız... | Bu eyleme... |
| --- | --- |
| **PowerShell** (sunucu tarafı betikleri, betik eylemleri, küme oluşturma sırasında kullanılan dahil) |Bash betiklerini yeniden yazın. Betik eylemleri için bkz: [özelleştirme Linux tabanlı HDInsight ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md) ve [betik eylemi geliştirme için Linux tabanlı HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure Klasik CLI** (sunucu tarafı komut dosyaları) |Azure Klasik CLI'yı Linux üzerinde mevcut olsa da, bunu HDInsight küme baş düğümleri üzerinde önceden yüklü gelmez. Klasik Azure CLI yükleme hakkında daha fazla bilgi için bkz. [Klasik Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET bileşenleri** |.NET üzerinde Linux tabanlı HDInsight ile desteklenen [Mono](https://mono-project.com). Daha fazla bilgi için [geçirme .NET çözümlerini Linux tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **Win32 bileşenlerini veya diğer yalnızca Windows teknolojileri** |Bileşen veya teknoloji Kılavuzu bağlıdır. Linux ile uyumlu bir sürümünü bulamıyor olabilir. Değilse, alternatif bir çözüm bulmak veya bu bileşeni yeniden yazın. |

> [!IMPORTANT]  
> HDInsight Yönetimi SDK'sı Mono ile tam olarak uyumlu değildir. HDInsight kümesi için dağıtılan çözümlerin bir parçası olarak kullanmayın.

## <a name="cluster-creation"></a>Küme oluşturma

Bu bölümde, küme oluşturma farklılıkları hakkında bilgiler sağlar.

### <a name="ssh-user"></a>SSH kullanıcı

Linux tabanlı HDInsight kümeleri kullanım **güvenli Kabuk (SSH)** küme düğümlerine uzaktan erişim sağlamak için protokol. Uzak Masaüstü için Windows tabanlı kümeler, bir grafik kullanıcı deneyimi çoğu SSH istemcisi sağlamaz. Bunun yerine, küme üzerinde komut çalıştırabilirsiniz olanak tanıyan bir komut satırı SSH istemcisi sağlar. Bazı istemciler (gibi [MobaXterm](https://mobaxterm.mobatek.net/)) bir grafik dosya sistemi tarayıcı uzak komut satırı ek belirtin.

Küme oluşturma sırasında bir SSH kullanıcısı ve ya da sağlamalısınız bir **parola** veya **ortak anahtar sertifikasını** kimlik doğrulaması için.

Parola kullanmaktan daha güvenli olsa da ortak anahtar sertifikasını kullanarak öneririz. Sertifika kimlik doğrulaması, imzalı bir ortak/özel anahtar çifti oluşturma, ardından kümeyi oluştururken ortak anahtarı sağlayarak çalışır. SSH kullanarak sunucuya bağlanırken, özel anahtarı bir istemcide bağlantı için kimlik doğrulaması sağlar.

Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="cluster-customization"></a>Küme özelleştirmesi

**Betik eylemleri** ile kullanılan Linux tabanlı kümeler Bash komut dosyası yazılabilir olmalıdır. Linux tabanlı kümeler betik eylemleri, küme oluşturma sırasında veya sonrasında kullanabilirsiniz. Daha fazla bilgi için [özelleştirme Linux tabanlı HDInsight ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md) ve [betik eylemi geliştirme için Linux tabanlı HDInsight](hdinsight-hadoop-script-actions-linux.md).

Başka bir özelleştirme özelliği **önyükleme**. Bu özellik Windows kümeleri için Hive'ı kullanmak için ek kitaplıklar konumunu belirtmenize olanak sağlar. Küme oluşturulduktan sonra bu kitaplıkları otomatik olarak kullanmak zorunda kalmadan Hive sorguları ile kullanılabilir `ADD JAR`.

Linux tabanlı kümeler için önyükleme özelliği bu işlevsellik sağlamaz. Bunun yerine, belirtilmiştir betik eylemi kullanın [ekleyin, Apache Hive kitaplıkları küme oluşturma sırasında](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Sanal Ağlar

Linux tabanlı HDInsight kümeleri Resource Manager sanal ağları gerektirirken Klasik sanal ağları, yalnızca iş Windows tabanlı HDInsight kümeleri. Bir Klasik sanal ağdaki HDInsight Linux küme bağlanması gerekir kaynaklarınız varsa, bkz. [klasik bir sanal ağ Resource Manager sanal ağa bağlanma](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Yapılandırma gereksinimleri hakkında daha fazla bilgi için bkz. [kullanarak bir sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md) belge.

## <a name="management-and-monitoring"></a>Yönetim ve izleme

Birçok web kullanıcı arabirimleri, iş geçmişi ya da Yarn UI gibi Windows tabanlı HDInsight ile kullanmış olabilirsiniz, Apache Ambari kullanılabilir. Ayrıca, Ambari Hive görünümünü web tarayıcınızı kullanarak Hive sorguları çalıştırmak için bir yol sağlar. Ambari Web kullanıcı arabirimini Linux tabanlı kümelerde kullanılabilir https://CLUSTERNAME.azurehdinsight.net.

Ambari ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Apache Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari Uyarıları

Ambari, küme olası sorunları size söyleyebilir bir uyarı sistemine sahiptir. Ayrıca bunları REST API aracılığıyla alabilir ancak uyarılar Ambari Web kullanıcı arabiriminde, kırmızı veya sarı girişleri olarak görünür.

> [!IMPORTANT]  
> Ambari uyarıları belirtin, burada *olabilir* bir sorun, burada *olduğu* sorun. Örneğin, normalde erişimi olsa da HiveServer2 erişilemeyeceğini bir uyarı alabilirsiniz.
>
> Çok sayıda uyarı aralığı tabanlı sorgular karşı bir hizmet olarak uygulanır ve belirli bir zaman çerçevesi içinde bir yanıt bekler. Uyarı hizmetinin kapalı olduğunu mutlaka gelmez şekilde, BT beklenen zaman çerçevesi içindeki sonuç döndürmedi.

## <a name="file-system-locations"></a>Dosya sistemi konumları

Linux küme dosya sistemi Windows tabanlı HDInsight kümeleri farklı yerleştirilir. Yaygın olarak kullanılan Bul dosyaları için aşağıdaki tabloyu kullanın.

| Bulmanız gerekir... | Bulunduğu... |
| --- | --- |
| Yapılandırma |`/etc`. Örneğin, `/etc/hadoop/conf/core-site.xml` |
| Günlük dosyaları |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. İki dizin bulunan Burada, geçerli HDP sürümü vardır ve `current`. `current` Dizini dosyalara ve dizinlere sürüm numarası dizininde simgesel bağlantılar içerir. `current` Dizindir sağlanan sürüm HDP dosyaları sonraki sürüm numarası değişiklikler HDP erişmenin kolay bir yol olarak güncelleştirilir. |
| hadoop streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Genel olarak, dosya adını biliyorsanız, dosya yolunu bulmak için bir SSH oturumunda aşağıdaki komutu kullanabilirsiniz:

    find / -name FILENAME 2>/dev/null

Ayrıca, dosya adı ile joker karakterler kullanabilirsiniz. Örneğin, `find / -name *streaming*.jar 2>/dev/null` 'dosya adının bir parçası olarak akış' sözcüğünü içeren tüm jar dosyalarının yolunu döndürür.

## <a name="apache-hive-apache-pig-and-mapreduce"></a>Apache Hive, Apache Pig ve MapReduce

Pig ve MapReduce iş yüklerini, Linux tabanlı kümelerde benzerdir. Ancak, Linux tabanlı HDInsight kümeleri, Hadoop, Hive ve Pig daha yeni sürümleri kullanılarak oluşturulabilir. Bu sürümü farkları nasıl değişikliklere neden olabilir, mevcut çözümleri işlevi. HDInsight ile eklenen bileşenlerin sürümlerini hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md).

Linux tabanlı HDInsight, uzak masaüstü işlevselliğini sağlamaz. Bunun yerine, küme baş düğümlerine uzaktan bağlanmak için SSH kullanabilirsiniz. Daha fazla bilgi için aşağıdaki belgelere bakın:

* [Apache Hive ile SSH kullanma](hdinsight-hadoop-use-hive-ssh.md)
* [Apache Pig ile SSH kullanma](hadoop/apache-hadoop-use-pig-ssh.md)
* [SSH ile MapReduce kullanma](hadoop/apache-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]  
> Bir dış Hive meta veri deposu kullanıyorsanız, Linux tabanlı HDInsight ile kullanmadan önce meta veri deposu yedeklemelisiniz. Linux tabanlı HDInsight uyumsuzluklar sahip olabilecek Hive daha yeni sürümleriyle kullanılabilir ile önceki sürümleriyle oluşturulan meta depolar.

Aşağıdaki grafikte, Hive iş yüklerinizi geçirme hakkında yönergeler sağlar.

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| **Hive Düzenleyicisi** |[Apache Ambari görünümünde Hive](hadoop/apache-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;` Tez etkinleştirme |Apache Tez Linux tabanlı kümeler için varsayılan yürütme altyapısı olduğundan set deyimi artık gerekli değildir. |
| C# kullanıcı tanımlı işlevler | Linux tabanlı HDInsight ile C# bileşenleri doğrulama hakkında daha fazla bilgi için bkz: [geçirme .NET çözümlerini Linux tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD dosyaları veya betikleri bir Hive işi bir parçası olarak çağrılan sunucuda |Bash betiklerini kullanma |
| `hive` Uzak Masaüstü komutu |Kullanım [Apache Hive Beeline](hadoop/apache-hadoop-use-hive-beeline.md) veya [Apache Hive bir SSH oturumundan](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| C# kullanıcı tanımlı işlevler | Linux tabanlı HDInsight ile C# bileşenleri doğrulama hakkında daha fazla bilgi için bkz: [geçirme .NET çözümlerini Linux tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD dosyaları veya betikleri bir Pig işi bir parçası olarak çağrılan sunucuda |Bash betiklerini kullanma |

### <a name="mapreduce"></a>MapReduce

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| C# bileşenleri Eşleyici ve Azaltıcı | Linux tabanlı HDInsight ile C# bileşenleri doğrulama hakkında daha fazla bilgi için bkz: [geçirme .NET çözümlerini Linux tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD dosyaları veya betikleri bir Hive işi bir parçası olarak çağrılan sunucuda |Bash betiklerini kullanma |

## <a name="apache-oozie"></a>Apache Oozie

> [!IMPORTANT]  
> Dış Oozie meta depo kullanırsanız, Linux tabanlı HDInsight ile kullanmadan önce meta veri deposu yedeklemelisiniz. Linux tabanlı HDInsight uyumsuzluklar sahip olabilecek Oozie daha yeni sürümleriyle kullanılabilir ile önceki sürümleriyle oluşturulan meta depolar.

Oozie ile iş akışlarını Kabuğu Eylemler izin verir. Kabuk Eylemler, komut satırı komutlarını çalıştırmak için işletim sistemi için varsayılan kabuğunu kullanın. Windows Kabuğu kullanan Oozie iş akışları varsa, Linux Kabuk ortamını (Bash) yararlanmayı iş akışlarını yeniden yazmanız gerekir. Kabuk Eylemler ile Oozie kullanma hakkında daha fazla bilgi için bkz. [Oozie Kabuk eylem uzantısı](https://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Bir C# uygulaması kullanan bir iş akışı varsa, bu uygulamaların bir Linux ortamı doğrulayın. Daha fazla bilgi için [geçirme .NET çözümlerini Linux tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| Storm Panosu |Storm panosunu kullanılamıyor. Bkz: [Linux tabanlı HDInsight üzerinde dağıtma ve yönetme Apache Storm topolojilerini](storm/apache-storm-deploy-monitor-topology-linux.md) topolojileri göndermek yöntemleri |
| Storm kullanıcı Arabirimi |Storm kullanıcı arabirimini kullanılabilir https://CLUSTERNAME.azurehdinsight.net/stormui |
| Oluşturmanıza, dağıtmanıza ve C# veya karma topolojileri izleyip yönetmek için visual Studio |Visual Studio oluşturmanıza, dağıtmanıza ve C# (SCP.NET) ya da Linux tabanlı HDInsight üzerinde Storm üzerinde karma topolojiler yönetmek için kullanılabilir. Yalnızca, 10/28/2016'dan sonra oluşturulan kümeleri ile de kullanılabilir. |

## <a name="apache-hbase"></a>Apache HBase

HBase için znode üst Linux tabanlı kümelerde `/hbase-unsecure`. Bu değer yerel HBase Java API kullanan uygulamalardan herhangi bir Java istemci yapılandırmasını ayarlayın.

Bkz: [bir Java tabanlı Apache HBase uygulaması](hbase/apache-hbase-build-java-maven-linux.md) bu değeri ayarlayan bir örnek istemci için.

## <a name="spark"></a>Spark

Spark kümeleri, Önizleme sırasında Windows kümelerinde kullanılabilir. Spark GA yalnızca Linux tabanlı kümeler ile kullanılabilir. Bir yayın Linux tabanlı Spark kümesine bir Windows tabanlı Spark Önizleme kümeden geçiş yolu yoktur.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory özel .NET etkinlikleri

Azure Data Factory özel .NET etkinlikleri, Linux tabanlı HDInsight kümelerinde şu anda desteklenmemektedir. Bunun yerine, özel etkinlikler ADF işlem hattınızı bir parçası olarak uygulamak için aşağıdaki yöntemlerden birini kullanmalısınız.

* .NET etkinliklerinin Azure Batch havuzunda yürütün. Kullanım Azure Batch hizmeti bölümünü bağlı bakın [bir Azure Data Factory işlem hattında özel etkinlikler kullanma](../data-factory/transform-data-using-dotnet-custom-activity.md)
* Etkinlik MapReduce etkinliği olarak uygulayın. Daha fazla bilgi için [Data factory'den MapReduce programlarını çağırma](../data-factory/transform-data-using-hadoop-map-reduce.md).

### <a name="line-endings"></a>Satır sonları

Genel olarak, Linux tabanlı sistemlerde LF kullanırken Windows tabanlı sistemlerde satır sonlarını CRLF, kullanın. Mevcut veri üreticileri ve tüketicileri LF ile çalışacak şekilde değiştirmeniz gerekebilir.

Örneğin, sorgu için Azure PowerShell kullanarak Windows tabanlı bir kümede HDInsight CRLF verilerle döndürür. Linux tabanlı küme ile aynı sorgu LF döndürür. Linux tabanlı bir kümeye geçirmeden önce satır sonu çözümünüz ile ilgili bir sorun neden olmadığını görmek için test edin.

Küme düğümleri üzerinde çalışan bir komut dosyası için bitiş satırı olarak her zaman LF kullanın. CRLF kullanırsanız, betikler, Linux tabanlı bir kümede çalışırken hataları görebilirsiniz.

Betikleri katıştırılmış CR karakterler içeren dizeleri içermiyorsa, aşağıdaki yöntemlerden birini kullanarak satır sonlarını değişiklik topluca ekleyebilirsiniz:

* **Kümeye karşıya yüklemeden önce**: Satır sonlarını, kümeye betiği karşıya yüklemeden önce LF için CRLF değiştirmek için aşağıdaki PowerShell ifadeleri kullanın.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Kümeye karşıya yükledikten sonra**: Komut dosyasını değiştirmek için Linux tabanlı küme için bir SSH oturumunda aşağıdaki komutu kullanın.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Sonraki Adımlar

* [Linux tabanlı HDInsight kümeleri oluşturmayı öğrenin](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight için bağlanmak için SSH kullanın](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Apache Ambari kullanarak Linux tabanlı küme yönetme](hdinsight-hadoop-manage-ambari.md)
