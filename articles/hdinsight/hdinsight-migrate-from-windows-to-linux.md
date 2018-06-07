---
title: Windows tabanlı Hdınsight'ta Linux tabanlı Hdınsight'a - Azure geçirme | Microsoft Docs
description: Linux tabanlı Hdınsight kümesi için bir Windows tabanlı Hdınsight kümeden geçiş öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: ff35be59-bae3-42fd-9edc-77f0041bab93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: larryfr
ms.openlocfilehash: 964fa9853dc8bb4daae73905e05409deb775fd26
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626760"
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Bir Windows tabanlı Hdınsight kümeden Linux tabanlı bir kümeye geçirme

Bu belge, Windows'da Hdınsight Linux arasındaki farklar hakkında ayrıntılar sağlar. Ayrıca var olan iş yükleri Linux tabanlı bir kümeye geçirme hakkında yönergeler sağlar.

Windows tabanlı Hdınsight Hadoop bulutta kullanmak için kolay bir yol sağlarken, Linux tabanlı bir kümeye geçirmek gerekebilir. Örneğin, Linux tabanlı araçlar ve çözümünüz için gerekli olan teknolojileri yararlanmak için. Pek çok Hadoop ekosistemindeki Linux tabanlı sistemlerde geliştirilmiş ve Windows tabanlı Hdınsight ile kullanılmak üzere müsait olmayabilir. Birçok defterleri, videolar ve diğer eğitim malzemesi Hadoop ile çalışırken, Linux sistemi kullandığınız varsayılır.

> [!NOTE]
> Hdınsight kümeleri kümedeki düğümlerin işletim sistemi olarak Ubuntu uzun süreli destek (LTS) kullanın. Hdınsight ile Ubuntu sürüm diğer bileşen sürümü oluşturma bilgilerle birlikte hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Geçiş görevleri

Geçiş için genel iş akışı aşağıdaki gibidir.

![Geçiş iş akışı diyagramı](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Geçiş sırasında gerekli olabilecek değişiklikleri anlamak için bu belgenin her bölümünü okuyun.

2. Linux tabanlı bir küme bir test/kalite güvence ortamı oluşturun. Linux tabanlı bir küme oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-provision-linux-clusters.md).

3. Var olan işleri, veri kaynakları ve havuzlarını yeni ortamına kopyalayın.

4. İşleriniz yeni kümede beklendiği gibi çalıştığından emin olmak için doğrulama testleri gerçekleştirme.

Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra geçiş kapalı kalma süresini zamanlayın. Bu kesinti sırasında aşağıdaki eylemleri gerçekleştirin:

1. Küme düğümlerinde yerel olarak depolanan tüm geçici verileri yedekleyin. Örneğin, doğrudan bir baş düğüm üzerinde depolanan verileri varsa.

2. Windows tabanlı küme silin.

3. Windows tabanlı küme tarafından kullanılan varsayılan veri deposu kullanarak Linux tabanlı bir küme oluşturun. Linux tabanlı küme var olan üretim verilerinizi karşı çalışmaya devam edebilirsiniz.

4. Yedeklediğiniz herhangi bir geçici veri içeri aktarın.

5. Başlangıç işleri/yeni küme kullanarak işlemeye devam et.

### <a name="copy-data-to-the-test-environment"></a>Verileri test ortamına kopyalayın.

İşlerini ve verileri kopyalamak için birçok yöntem vardır, ancak bu bölümde açıklanan iki basit için doğrudan yöntemlerdir dosyaları bir test kümeye taşıyın.

#### <a name="hdfs-copy"></a>HDFS kopyalama

Verileri test kümeye üretim kümeden kopyalamak için aşağıdaki adımları kullanın. Bu adımları kullanın `hdfs dfs` Hdınsight ile birlikte yardımcı programı.

1. Varolan kümeniz için depolama hesabı ve varsayılan kapsayıcı bilgilerini bulun. Aşağıdaki örnek, bu bilgileri almak için PowerShell kullanır:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. Bir test ortamı oluşturmak için Hdınsight belge oluşturma Linux tabanlı kümelerde'ndaki adımları izleyin. Küme oluşturmadan önce durdurun ve bunun yerine seçin **isteğe bağlı yapılandırma**.

3. İsteğe bağlı yapılandırma bölümünden seçin **bağlantılı depolama hesapları**.

4. Seçin **depolama anahtarı eklemek**ve istendiğinde, adım 1'de PowerShell komut dosyası tarafından döndürülen depolama hesabını seçin. Tıklatın **seçin** her bölümünde. Son olarak, küme oluşturun.

5. Küme oluşturulduktan sonra onu kullanarak bağlanmak **SSH.** Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

6. SSH oturumundan dosyaları bağlantılı depolama hesabından yeni varsayılan depolama hesabına kopyalamak için aşağıdaki komutu kullanın. KAPSAYICI, PowerShell tarafından döndürülen kapsayıcı bilgileri ile değiştirin. Değiştir __hesap__ hesap adına sahip. Veri yoluna bir veri dosyası yolu değiştirin.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]
    > Verileri içeren dizin yapısı sınama ortamında mevcut değilse, aşağıdaki komutu kullanarak oluşturabilirsiniz:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    `-p` Anahtar yoldaki tüm dizinler oluşturulmasını etkinleştirir.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Azure Storage blobları arasında doğrudan kopyalama

Alternatif olarak, kullanmak isteyebilirsiniz `Start-AzureStorageBlobCopy` Hdınsight dışında depolama hesapları arasında BLOB'ları kopyalamak için Azure PowerShell cmdlet'i. Daha fazla bilgi için bkz. Azure PowerShell kullanarak Azure Storage ile Azure BLOB'ları bölümünü yönetmek için.

## <a name="client-side-technologies"></a>İstemci-tarafı teknolojileri

İstemci-tarafı teknolojileri gibi [Azure PowerShell cmdlet'lerini](/powershell/azureps-cmdlets-docs), [Azure CLI](../cli-install-nodejs.md), veya [Hadoop için .NET SDK'sı](https://hadoopsdk.codeplex.com/) Linux tabanlı kümelerde çalışmaya devam eder. Bu teknolojiler her iki küme işletim sistemi türleri arasında aynıdır REST API'lerini kullanır.

## <a name="server-side-technologies"></a>Sunucu tarafı teknolojileri

Aşağıdaki tabloda, Windows'a özgü olan geçirme sunucu tarafı bileşenlerini rehberlik sağlar.

| Bu teknoloji kullanıyorsanız... | Bu eyleme... |
| --- | --- |
| **PowerShell** (sunucu tarafı kodlar betik küme oluşturma sırasında kullanılan eylemleri de dahil olmak üzere,) |Bash betiklerini yeniden yazın. Betik eylemleri için bkz: [özelleştirme Linux tabanlı Hdınsight ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md) ve [betik eylemi geliştirme Linux tabanlı Hdınsight için](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (sunucu tarafı komut dosyaları) |Azure CLI Linux'ta kullanılabilir olsa da, bu Hdınsight kümesi baş düğümler önceden yüklenmiş gelmez. Azure CLI yükleme hakkında daha fazla bilgi için bkz: [Azure CLI 2.0 ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET bileşenleri** |.NET Linux tabanlı Hdınsight desteklenir [Mono](https://mono-project.com). Daha fazla bilgi için bkz: [Linux tabanlı Hdınsight geçirmek .NET çözümleri](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **Win32 bileşenlerini veya diğer yalnızca Windows teknolojisi** |Kılavuzu, bileşen veya teknoloji bağlıdır. Linux ile uyumlu bir sürümü bulamıyor olabilir. Değilse, alternatif bir çözüm bulmak veya bu bileşeni yeniden yazın. |

> [!IMPORTANT]
> Hdınsight Yönetim SDK Mono ile tamamen uyumlu değil. Hdınsight kümesine dağıtılan çözümleri bir parçası olarak kullanmayın.

## <a name="cluster-creation"></a>Küme oluşturma

Bu bölümde, küme oluşturma farklılıkları hakkında bilgiler sağlar.

### <a name="ssh-user"></a>SSH kullanıcı

Linux tabanlı Hdınsight kümeleri kullanım **güvenli Kabuk (SSH)** küme düğümleri uzaktan erişim sağlamak için protokol. Uzak Masaüstü için Windows tabanlı kümeler farklı olarak, çoğu SSH istemcisi bir grafik kullanıcı deneyimi sağlamaz. Bunun yerine, SSH istemcilerini kümede komutlarını çalıştırmanıza olanak sağlayan bir komut satırı belirtin. Bazı istemciler (gibi [MobaXterm](http://mobaxterm.mobatek.net/)) bir grafik dosya sistemi tarayıcı uzak bir komut satırı yanı sıra sağlayın.

Küme oluşturma sırasında bir SSH kullanıcısı ve ya da sağlamanız gereken bir **parola** veya **ortak anahtar sertifikası** kimlik doğrulaması için.

Parola kullanmaktan daha güvenli olduğu gibi ortak anahtar sertifikası kullanmanızı öneririz. Sertifika kimlik doğrulaması, imzalanmış bir genel/özel anahtar çifti oluşturma, sonra ortak anahtar kümesi oluştururken sağlayarak çalışır. SSH kullanarak sunucuya bağlanırken, özel anahtarı istemcide bağlantı için kimlik doğrulaması sağlar.

Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="cluster-customization"></a>Küme özelleştirmesi

**Betik eylemleri** ile kullanılan Linux tabanlı kümelerde Bash komut dosyasında yazılması gerekir. Linux tabanlı kümelerde betik eylemleri sırasında veya Küme oluşturulduktan sonra kullanabilirsiniz. Daha fazla bilgi için bkz: [özelleştirme Linux tabanlı Hdınsight ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md) ve [betik eylemi geliştirme Linux tabanlı Hdınsight için](hdinsight-hadoop-script-actions-linux.md).

Başka bir özelleştirme özelliği **önyükleme**. Windows kümeler için bu özellik ile Hive kullanmak için ek kitaplıklara konumunu belirtmenizi sağlar. Küme oluşturulduktan sonra Bu kitaplıklar otomatik olarak Hive sorguları kullanmak zorunda kalmadan kullanılabilir `ADD JAR`.

Linux tabanlı kümeler için önyükleme özelliği bu işlevleri sağlamaz. Bunun yerine, kısmında belgelenen betik eylemi kullanın [eklemek Hive kitaplıkları küme oluşturma sırasında](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Sanal Ağlar

Linux tabanlı Hdınsight kümeleri Resource Manager sanal ağlar gerektirirken Klasik sanal ağlar, yalnızca çalışmak Windows tabanlı Hdınsight kümeleri. Bir Klasik sanal ağındaki Linux hdınsight'a bağlanması gereken kaynaklarınız varsa, bkz: [Klasik sanal ağ kaynak yöneticisi sanal ağa bağlanma](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Yapılandırma gereksinimleri hakkında daha fazla bilgi için bkz: [kullanarak bir sanal ağ genişletme Hdınsight yetenekleri](hdinsight-extend-hadoop-virtual-network.md) belge.

## <a name="management-and-monitoring"></a>Yönetim ve izleme

Birçok iş geçmişi veya Yarn kullanıcı Arabiriminde, gibi Windows tabanlı Hdınsight ile kullanmış olabilirsiniz Uı'lar web Ambari kullanılabilir. Ayrıca, Ambari Hive görünümü, web tarayıcısı kullanarak Hive sorguları çalıştırmak için bir yol sağlar. Ambari Web kullanıcı Arabirimi Linux tabanlı kümelerde kullanılabilir https://CLUSTERNAME.azurehdinsight.net.

Ambari ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari uyarıları

Ambari küme ile ilgili olası sorunları size bildiren bir uyarı sistem sahiptir. Görüntüleri de REST API aracılığıyla alabilir ancak uyarılar sarı veya kırmızı girişleri Ambari Web kullanıcı Arabirimi olarak görünür.

> [!IMPORTANT]
> Ambari uyarılar belirtir, var. *olabilir* bir sorun, burada *olan* bir sorun. Örneğin, normal olarak erişebilir olsa da HiveServer2 erişilemeyeceğini bir uyarı alabilirsiniz.
>
> Birçok uyarılar aralık-temelli sorguları bir hizmet olarak uygulanır ve belirli bir zaman çerçevesi içinde bir yanıt bekler. Uyarı hizmetinin kapalı olduğunu mutlaka anlamına gelmez şekilde yalnızca o BT beklenen zaman çerçevesinde sonuç döndürmedi.

## <a name="file-system-locations"></a>Dosya sistemi konumları

Linux küme dosya sistemi Windows tabanlı Hdınsight kümeleri farklı yerleştirilir. Sık kullanılan Bul dosyaları için aşağıdaki tabloyu kullanın.

| Bul gerek... | Bulunuyor... |
| --- | --- |
| Yapılandırma |`/etc`. Örneğin, `/etc/hadoop/conf/core-site.xml` |
| Günlük dosyaları |`/var/logs` |
| Hortonworks veri Platformu (HDP) |`/usr/hdp`. İki dizini bulunan Burada, geçerli HDP sürümü olan bir vardır ve `current`. `current` Dizin dosyaları ve dizinleri sürüm numarası dizininde sembolik bağlantılar içerir. `current` Dizindir sağlanan sürüm HDP dosyaları bu yana sürüm numarası değişikliklerini HDP erişmek için kolay bir yol olarak güncelleştirilir. |
| hadoop streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Genel olarak, dosyanın adını biliyorsanız, dosya yolunu bulmak için bir SSH oturumunda aşağıdaki komutu kullanabilirsiniz:

    find / -name FILENAME 2>/dev/null

Bir dosya adı ile joker karakterler de kullanabilirsiniz. Örneğin, `find / -name *streaming*.jar 2>/dev/null` 'dosya adı bir parçası olarak akış' sözcüğünü içeren herhangi bir jar dosya yolunu döndürür.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig ve MapReduce

Pig ve MapReduce iş yükleri, Linux tabanlı kümelerde benzerdir. Ancak, Linux tabanlı Hdınsight kümeleri Hadoop, Hive veya Pig daha yeni sürümleri kullanılarak oluşturulabilir. Bu sürüm farklılıklar nasıl değişiklikleri çıkarabilir, varolan çözümleri işlevi. Hdınsight ile dahil bileşenlerinin sürümleri hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](hdinsight-component-versioning.md).

Linux tabanlı Hdınsight Uzak Masaüstü işlevselliği sağlamaz. Bunun yerine, küme baş düğümler uzaktan bağlanmak için SSH kullanabilirsiniz. Daha fazla bilgi için aşağıdaki belgelere bakın:

* [SSH ile Hive kullanma](hdinsight-hadoop-use-hive-ssh.md)
* [SSH ile pig kullanma](hadoop/apache-hadoop-use-pig-ssh.md)
* [SSH ile MapReduce kullanma](hadoop/apache-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]
> Bir dış Hive meta depo kullanırsanız, Linux tabanlı Hdınsight ile kullanmadan önce meta depo yedeklemeniz gerekir. Linux tabanlı Hdınsight uyumsuzlukları olabilir Hive daha yeni sürümleriyle kullanılabilir önceki sürümleri tarafından oluşturulan meta deponuz ile.

Aşağıdaki grafikte, Hive iş yüklerinizi geçirme hakkında yönergeler sağlanmaktadır.

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| **Hive düzenleyicisinin** |[Ambari Hive görünümünü](hadoop/apache-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;` Tez etkinleştirmek için |Tez Linux tabanlı kümeler için varsayılan yürütme altyapısı olduğundan set deyimi artık gerekli değildir. |
| C# kullanıcı tanımlı işlevler | C# Linux tabanlı Hdınsight bileşenleriyle doğrulama hakkında daha fazla bilgi için bkz: [Linux tabanlı Hdınsight geçirmek .NET çözümleri](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD dosyaları veya bir Hive işi bir parçası olarak çağrılan sunucuda komut dosyaları |Bash betiklerini kullanın |
| `hive` komut Uzak Masaüstü |Kullanım [Beeline](hadoop/apache-hadoop-use-hive-beeline.md) veya [Hive bir SSH oturumunda](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| C# kullanıcı tanımlı işlevler | C# Linux tabanlı Hdınsight bileşenleriyle doğrulama hakkında daha fazla bilgi için bkz: [Linux tabanlı Hdınsight geçirmek .NET çözümleri](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD dosyaları veya Pig işi bir parçası olarak çağrılan sunucuda komut dosyaları |Bash betiklerini kullanın |

### <a name="mapreduce"></a>MapReduce

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| C# Eşleyici ve reducer bileşenleri | C# Linux tabanlı Hdınsight bileşenleriyle doğrulama hakkında daha fazla bilgi için bkz: [Linux tabanlı Hdınsight geçirmek .NET çözümleri](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD dosyaları veya bir Hive işi bir parçası olarak çağrılan sunucuda komut dosyaları |Bash betiklerini kullanın |

## <a name="oozie"></a>Oozie

> [!IMPORTANT]
> Bir dış Oozie meta depo kullanırsanız, Linux tabanlı Hdınsight ile kullanmadan önce meta depo yedeklemeniz gerekir. Linux tabanlı Hdınsight uyumsuzlukları olabilir Oozie daha yeni sürümleriyle kullanılabilir önceki sürümleri tarafından oluşturulan meta deponuz ile.

Oozie iş akışları Kabuk eylemlerin sağlar. Kabuk Eylemler işletim sistemi için varsayılan kabuk komut satırı komutlarını çalıştırmak için kullanın. Windows Kabuğu kullanan Oozie iş akışları varsa, Linux Kabuğu ortamda (Bash) yararlanmayı iş akışlarını yeniden gerekir. Kabuk Eylemler Oozie ile kullanma hakkında daha fazla bilgi için bkz: [Oozie Kabuk eylem uzantısı](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

C# uygulama kullanan bir iş akışı varsa, bu uygulamalar Linux ortamında doğrulayın. Daha fazla bilgi için bkz: [Linux tabanlı Hdınsight geçirmek .NET çözümleri](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| Üzerinde Windows tabanlı kullanmam... | Linux tabanlı... |
| --- | --- |
| Storm Panosu |Storm panosunu kullanılabilir değil. Bkz: [dağıtma ve yönetme Storm topolojileri Linux tabanlı Hdınsight üzerinde](storm/apache-storm-deploy-monitor-topology-linux.md) topolojileri göndermek yöntemleri |
| Storm kullanıcı Arabirimi |Storm kullanıcı Arabirimi şu adresten edinilebilir https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio oluşturmak, dağıtmak ve C# veya karma topolojiler yönetmek için |Visual Studio oluşturmak, dağıtmak ve C# (SCP.NET) ya da karma topolojiler hdınsight'ta Linux tabanlı Storm üzerinde yönetmek için kullanılabilir. Yalnızca, 28/10/2016 sonrasında oluşturulan kümeleri ile de kullanılabilir. |

## <a name="hbase"></a>HBase

HBase znode üst Linux tabanlı kümelerde olduğu `/hbase-unsecure`. Bu değeri yerel HBase Java API kullanan uygulamaları herhangi bir Java istemcisini yapılandırmasında ayarlayın.

Bkz: [bir Java tabanlı HBase uygulaması derleme](hdinsight-hbase-build-java-maven.md) bu değeri ayarlayan bir örnek istemcisi için.

## <a name="spark"></a>Spark

Spark kümeleri Önizleme sırasında Windows kümelerinde kullanılabilir. Spark GA yalnızca Linux tabanlı kümelerde ile kullanılabilir. Yayın Linux tabanlı Spark kümesi için bir Windows tabanlı Spark Önizleme kümeden geçiş yol yoktur.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory özel .NET etkinlikler

Azure Data Factory özel .NET etkinlikler Linux tabanlı Hdınsight kümelerinde şu anda desteklenmemektedir. Bunun yerine, özel etkinlikler ADF hattınızı bir parçası olarak uygulanması için aşağıdaki yöntemlerden birini kullanmalıdır.

* .NET etkinliklerini Azure Batch havuzunda yürütün. Kullanım Azure Batch bağlantılı hizmet bölümüne bakın [bir Azure Data Factory ardışık düzeninde özel etkinlikleri kullanmak](../data-factory/transform-data-using-dotnet-custom-activity.md)
* Etkinlik MapReduce etkinliği olarak uygular. Daha fazla bilgi için bkz: [MapReduce programlardan çağırma Data Factory](../data-factory/transform-data-using-hadoop-map-reduce.md).

### <a name="line-endings"></a>Satır sonları

Genel olarak, Linux tabanlı sistemler LF kullanırken CRLF, satır sonları Windows tabanlı sistemlerde kullanın. Mevcut veri üreticileri ve tüketicileri LF ile çalışması için değiştirmeniz gerekebilir.

Örneğin, sorgu için Azure PowerShell kullanarak Windows tabanlı bir küme Hdınsight'ta CRLF verilerle döndürür. Linux tabanlı bir küme ile aynı sorgu LF döndürür. Linux tabanlı bir kümeye geçirmeden önce satır bitiş çözümünüzü ile ilgili bir sorun neden olursa görmek için test edin.

Her zaman küme düğümleri üzerinde çalışan bir komut dosyası için bitiş satır olarak LF kullanın. CRLF kullanırsanız, komut dosyaları Linux tabanlı bir kümede çalışırken hatalar görebilirsiniz.

Komut dosyaları katıştırılmış CR karakterler dizelerle içermiyorsa, aşağıdaki yöntemlerden birini kullanarak satır sonları değişiklik toplu düzenleyebilirsiniz:

* **Kümeye karşıya yüklemeden önce**: satır sonları kümeye komut dosyasını karşıya yüklemeden önce CRLF LF için değiştirmek için aşağıdaki PowerShell ifadeleri kullanın.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Kümeye karşıya sonra**: komut dosyasını değiştirmek için Linux tabanlı küme için bir SSH oturumunda aşağıdaki komutu kullanın.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Sonraki Adımlar

* [Linux tabanlı Hdınsight kümeleri oluşturma hakkında bilgi edinin](hdinsight-hadoop-provision-linux-clusters.md)
* [Hdınsight'a bağlanmak için SSH kullanın](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Ambari kullanarak Linux tabanlı bir kümeyi yönetmek](hdinsight-hadoop-manage-ambari.md)
