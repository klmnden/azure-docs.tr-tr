---
title: Betik eylemleri, Azure'ı kullanarak HDInsight kümelerini özelleştirin
description: Özel bileşenler betik eylemlerini kullanarak Linux tabanlı HDInsight kümelerine ekleyin. Betik eylemleri, küme yapılandırmasını özelleştirebilir veya ek hizmetler ve yardımcı programları Hue, Solr ve r gibi eklemek için kullanılan Bash betikleridir
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 80c2d25fa24acff92a462f0289259792f217fbfd
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361702"
---
# <a name="customize-linux-based-hdinsight-clusters-by-using-script-actions"></a>Betik eylemlerini kullanarak Linux tabanlı HDInsight kümeleri özelleştirme

Azure HDInsight sağlar adlı bir yapılandırma yöntemi **betik eylemlerini** küme özelleştirmek için özel komut dosyaları çağırır. Bu betikler, ek bileşenler yükleme ve yapılandırma ayarlarını değiştirmek için kullanılır. Betik eylemleri, küme oluşturma sırasında veya sonrasında kullanılabilir.

> [!IMPORTANT]  
> Zaten çalışan bir kümede betik eylemleri kullanma olanağı, yalnızca Linux tabanlı HDInsight kümeleri için kullanılabilir.
>
> Linux üzerinde HDInsight sürüm 3.4 veya üzeri kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight Windows emeklilik](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Betik eylemleri, bir HDInsight uygulaması olarak Azure Marketi'nde ayrıca yayımlanabilir. HDInsight uygulamaları hakkında daha fazla bilgi için bkz. [HDInsight uygulama Azure Marketi'nde yayımlama](hdinsight-apps-publish-applications.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>İzinler

Bir etki alanına katılmış HDInsight kümesi için betik eylemleri kümeyle kullandığınızda gerekli olan iki Apache Ambari izni vardır:

* **AMBARI. ÇALIŞTIRMA\_ÖZEL\_KOMUT**. Ambari yöneticisi rolü varsayılan olarak bu izne sahiptir.
* **KÜME. ÇALIŞTIRMA\_ÖZEL\_KOMUT**. HDInsight küme yöneticisinin ve Ambari Yöneticisi varsayılan olarak bu izne sahip.

İzinleri olan etki alanına katılmış HDInsight ile çalışma hakkında daha fazla bilgi için bkz. [yönetme HDInsight kümeleri ile Kurumsal güvenlik paketi](./domain-joined/apache-domain-joined-manage.md).

## <a name="access-control"></a>Erişim denetimi

Hesabınızı Yöneticisi veya Azure aboneliğinizin sahibi değilseniz, en az olmalıdır, HDInsight kümesi içeren kaynak grubuna katkıda bulunan erişimi.

En az bir HDInsight kümesi, biriyle oluşturursanız, Azure aboneliğinde katkıda bulunan erişimi daha önce kaydetmiş olmalıdır HDInsight için sağlayıcı. Sağlayıcı kaydı, aboneliğe Katkıda Bulunan erişimi olan bir kullanıcı abonelikte ilk kez kaynak oluşturduğunda gerçekleşir. Bir kaynak oluşturmadan da gerçekleştirilebilir, [REST kullanarak bir sağlayıcısını kaydetme](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Erişim yönetimiyle çalışma hakkında daha fazla bilgi alın:

* [Azure portalında erişim yönetimi ile çalışmaya başlama](../role-based-access-control/overview.md)
* [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../role-based-access-control/role-assignments-portal.md)

## <a name="understand-script-actions"></a>Betik eylemlerini anlama

Betik eylemi çalıştıran bir HDInsight kümesindeki düğümler üzerinde Bash komut dosyasıdır. Özellikleri ve betik eylemleri özelliklerini aşağıdaki gibidir:

* HDInsight kümesinden erişilebilir bir URI üzerinde depolanmış olması gerekir. Olası depolama konumlarını şunlardır:

    * HDInsight kümesi tarafından erişilebilir olan bir Azure Data Lake Storage hesabı. Azure Data Lake Store ile HDInsight kullanma hakkında daha fazla bilgi için bkz: [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

        Data Lake depolama Gen1 içinde depolanan bir komut dosyası için URI biçimi `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]  
        > HDInsight, Data Lake depolamaya erişmek için kullandığı hizmet sorumlusu, betik okuma erişiminiz olması gerekir.

    * Ya da bir Azure depolama hesabındaki bir blob HDInsight küme için birincil ya da ek depolama hesabı. HDInsight, küme oluşturma sırasında hem de bu türlerde depolama hesapları için erişim izni verilir.

    * Genel Dosya Paylaşımı hizmet. Azure Blob, GitHub, OneDrive ve Dropbox verilebilir.

        URI, örneğin bkz [örnek betik eylemi betikleri](#example-script-action-scripts).

        > [!WARNING]  
        > HDInsight yalnızca Azure depolama hesapları ile standart performans katmanı Blob destekler. 

* Yalnızca belirli düğüm türleri üzerinde çalıştırılacak kısıtlanabilir. Baş düğümlerinden veya alt düğümlerinden verilebilir.

* Kalıcı veya geçici olabilir.

    Kalıcı duruma getirilmiş betiklerin küme işlemleri ölçeklendirme aracılığıyla eklenen yeni çalışan düğümlerindeki özelleştirmek için kullanılır. Ölçeklendirme işlemlerini gerçekleştiğinde kalıcı bir betik değişiklikleri de başka bir düğüm türü için uygulanabilir. Bir baş düğüm buna bir örnektir.

  > [!IMPORTANT]  
  > Kalıcı betik eylemleri, benzersiz bir adı olmalıdır.

    Geçici betikleri kalıcı değildir. Bunlar, çalışan düğümlerine betiği çalıştırdıktan sonra kümeye eklenen uygulanmaz. Ardından kalıcı bir betik için geçici bir betik yükseltebilir veya geçici bir betik için kalıcı bir betik indirgeyin.

  > [!IMPORTANT]  
  > Otomatik olarak kalıcı betik eylemleri, küme oluşturma sırasında kullanılır.
  >
  > Başarısız betikleri kalıcı olmayan bile özellikle olması belirtin.

* Yürütme sırasında komut dosyası tarafından kullanılan parametreleri kabul edebilir.

* Küme düğümleri üzerinde kök düzeyinde ayrıcalıkları ile çalıştırın.

* Azure portalı, Azure PowerShell, Azure Klasik CLI veya HDInsight .NET SDK'sı kullanılabilir.

Küme, çalıştırılmış tüm betikler bir geçmişini tutar. Yükseltme veya indirgeme işlemleri için bir komut Kimliğini bulmak gerektiğinde geçmişini yardımcı olur.

> [!IMPORTANT]  
> Betik eylemi tarafından yapılan değişiklikleri geri almak için otomatik bir yolu yoktur. El ile değişikliklerinizi geri veya bunları tersine bir betik sağlayın.

### <a name="script-action-in-the-cluster-creation-process"></a>Küme oluşturma işlemi betik eylemi

Betik eylemleri, küme oluşturma sırasında kullanılan, var olan bir kümede çalışan betik eylemleri biraz farklıdır:

* Komut dosyası otomatik olarak kalıcı hale getirilir.

* Koddaki bir hata, küme oluşturma işlemi başarısız olmasına neden olabilir.

Betik eylemi oluşturma işlemi sırasında çalıştığında, aşağıdaki diyagramda gösterilmektedir:

![HDInsight kümesi özelleştirme ve aşamaları sırasında küme oluşturma][img-hdi-cluster-states]

HDInsight yapılandırılırken betiği çalıştırır. Betik, belirtilen kümedeki tüm düğümler üzerinde paralel olarak çalıştırır. Bu işlem kök ayrıcalıklarıyla düğümlerinde çalışır.

> [!NOTE]  
> Durdurma ve Apache Hadoop ile ilgili hizmetler gibi hizmetler başlatma gibi işlemler gerçekleştirebilirsiniz. Hizmetlerini durdurmak, betik tamamlanmadan önce Ambari hizmet ve diğer Hadoop ile ilgili hizmetlerin çalışır durumda olduğundan emin olun. Bu hizmetler, oluşturulurken kümesinin durumunu ve sistem durumu başarılı bir şekilde belirlemek için gereklidir.


Küme oluşturma sırasında tek seferde çok sayıda betik eylemleri kullanabilirsiniz. Bu betikler, belirtilmiş olması sırayla çağrılır.

> [!IMPORTANT]  
> Betik eylemleri, zaman aşımı 60 dakika veya bunlar içinde tamamlanmalıdır. Betik, küme hazırlama sırasında eşzamanlı diğer Kurulum ve yapılandırma işlemleri çalıştırır. CPU süresi veya ağ bant genişliği gibi kaynak rekabetini betiği geliştirme ortamınızda gösterilenden tamamlanması uzun sürmesine neden olabilir.
>
> Betiği çalıştırmak için gereken süreyi en aza indirmek için indirme ve uygulamalarını kaynağından alınan derleme gibi görevleri kaçının. Uygulamalar önceden derlemek ve ikili Azure Depolama'da saklayın.


### <a name="script-action-on-a-running-cluster"></a>Betik eylemi çalıştıran bir kümede

Zaten çalışan bir küme üzerinde çalışacak bir betik içinde bir hata otomatik olarak başarısız durumuna değiştirmek kümeyi neden olmaz. Bir betik bittikten sonra küme çalışır bir duruma döndürmeniz gerekir.

> [!IMPORTANT]  
> Küme çalışır durumda olsa bile, başarısız olan kodu şeyler bozuk. Örneğin, bir komut dosyası küme için gerekli dosyaları silebilir.
>
> Betik eylemleri, kök ayrıcalıklarıyla çalıştırın. Bir betik yaptığı anladığınızdan emin olun, kümenize uygulamadan önce.

Bir betiği bir kümeye uygulama küme durumunun döndürdüğünüzde **çalıştıran** için **kabul edilen**. Sonra da değişir **HDInsight yapılandırma** ve son olarak, yeniden **çalıştıran** başarılı bir komut dosyası için. Betik durumunu betik eylemi geçmişinde günlüğe kaydedilir. Bu bilgileri, komut başarılı veya başarısız olduğunu bildirir. Örneğin, `Get-AzHDInsightScriptActionHistory` PowerShell cmdlet'i bir betik durumunu gösterir. Bilgileri aşağıdaki metne benzer döndürür:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]  
> Bu kümede çalıştırmak betik eylemleri, Küme oluşturulduktan sonra küme kullanıcı, yönetici parolasını değiştirirseniz, başarısız olabilir. Kalıcı betik eylemleri, hedef alt düğümleri varsa, bu betikleri kümeyi ölçeklediğinizde başarısız olabilir.

## <a name="example-script-action-scripts"></a>Örnek betik eylemi betikleri

Betik eylemi betikler aşağıdaki yardımcı programlar kullanılabilir:

* Azure portal
* Azure PowerShell
* Klasik Azure CLI
* Bir HDInsight .NET SDK'sı

HDInsight, HDInsight kümelerinde aşağıdaki bileşenleri yüklemek için komut dosyaları sağlar:

| Ad | Betik |
| --- | --- |
| Azure Depolama hesabı ekleme |`https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`. Bkz: [HDInsight için ek depolama hesapları ekleme](hdinsight-hadoop-add-storage.md). |
| Hue yükleme |`https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh`. Bkz: [yükleme ve kullanma, HDInsight, Hadoop üzerinde Hue kümeleri](hdinsight-hadoop-hue-linux.md). |
| Presto yükleme |`https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`. Bkz: [yüklemeden ve kullanmadan Presto üzerinde Hadoop tabanlı HDInsight kümeleri](hdinsight-hadoop-install-presto.md). |
| Giraph Yükleme |`https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh`. Bkz: [HDInsight Hadoop üzerinde Apache Giraph'ı yükleme kümeleri](hdinsight-hadoop-giraph-install-linux.md). |
| Hive kitaplıklarını önceden yükleme |`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`. Bkz: [HDInsight kümenizi oluştururken özel Apache Hive kitaplıkları ekleme](hdinsight-hadoop-add-hive-libraries.md). |
| Mono’yu yükleme veya güncelleştirme | `https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash`. Bkz: [yükleme veya güncelleştirme HDInsight üzerinde Mono](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Küme oluşturma sırasında bir betik eylemi kullanın

Bu bölümde, bir HDInsight kümesi oluştururken betik eylemleri kullanabileceğiniz farklı yolları açıklanmaktadır.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Azure portalından küme oluşturma sırasında bir betik eylemi kullanın

1. Bölümünde anlatıldığı gibi bir küme oluşturmak başlangıç [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight kümelerinde ayarlama](hdinsight-hadoop-provision-linux-clusters.md). Küme oluşturma sırasında ulaşırsınız bir __küme özeti__ sayfası. Gelen __küme özeti__ sayfasında __Düzenle__ için bağlantı __Gelişmiş ayarlar__.

    ![Gelişmiş ayarlar bağlantısı](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Gelen __Gelişmiş ayarlar__ bölümünden __betik eylemlerini__. Gelen __betik eylemlerini__ bölümünden __+ gönderme yeni__.

    ![Yeni betik eylemini gönderin](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Kullanım __bir betik seçin__ giriş önceden hazırlanmış bir komut dosyası seçin. Özel bir betik kullanmayı tercih __özel__. Ardından sağlayın __adı__ ve __Bash betiği URI'si__ betiği için.

    ![Select komut dosyası biçiminde bir komut dosyası Ekle](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Aşağıdaki tabloda, formda öğeleri açıklanmıştır:

    | Özellik | Değer |
    | --- | --- |
    | Bir betik seçin | Kendi betiğinizi kullanmayı tercih __özel__. Aksi takdirde, sağlanan betikleri birini seçin. |
    | Ad |Betik eylemi için bir ad belirtin. |
    | Bash betiği URI'si |Betik URI'si belirtin. |
    | HEAD/çalışan/Zookeeper |Betik üzerinde çalıştığı düğümleri belirtin: **HEAD**, **çalışan**, veya **ZooKeeper**. |
    | Parametreler |Komut dosyası tarafından gerekli parametreleri belirtin. |

    Kullanım __bu betik eylemi kalıcı__ betik ölçeklendirme işlemleri sırasında uygulanır emin olmak için giriş.

5. Seçin __Oluştur__ betiği kaydedilemiyor. Kullanabileceğiniz sonra __+ gönderme yeni__ başka bir komut dosyası eklemek için.

    ![Birden çok betik eylemleri](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Komut dosyaları eklemeyi bitirdiğinizde seçin __seçin__ düğmesine ve ardından __sonraki__ dönmek için düğme __küme özeti__ bölümü.

3. Kümeyi oluşturmak için Seç __Oluştur__ gelen __küme özeti__ seçimi.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarından betik eylemi kullanın

Betik eylemleri, Azure Resource Manager şablonları ile kullanılabilir. Bir örnek için bkz. [HDInsight Linux kümesi oluşturma ve çalıştırma betik eylemi](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/).

Bu örnekte, aşağıdaki kodu kullanarak betik eylemi eklenir:

```json
"scriptActions": [
    {
        "name": "setenvironmentvariable",
        "uri": "[parameters('scriptActionUri')]",
        "parameters": "headnode"
    }
]
```

Şablon dağıtma hakkında daha fazla bilgi alın:

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Azure PowerShell üzerinden küme oluşturma sırasında bir betik eylemi kullanın

Bu bölümde, kullandığınız [Ekle AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightscriptaction) cmdlet'ini küme özelleştirme betikleri çağırma. Başlamadan önce yükleme ve Azure PowerShell yapılandırma emin olun. HDInsight PowerShell cmdlet'lerini çalıştırmak için bir iş istasyonu yapılandırma hakkında daha fazla bilgi için bkz: [genel bakış, Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0#run-or-install).

Aşağıdaki betik, PowerShell kullanarak bir küme oluştururken bir betik eylemi uygulanacak gösterilmektedir:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Uygulamanın, küme oluşturulmadan önce birkaç dakika sürebilir.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>HDInsight .NET SDK küme oluşturma sırasında bir betik eylemi kullanın

HDInsight .NET SDK'sı bir .NET uygulamasından HDInsight ile çalışmak kolaylaştıran istemci kitaplıkları sağlar. Bir kod örneği için bkz. [.NET SDK kullanarak HDInsight kümelerini oluşturmak Linux tabanlı](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-to-a-running-cluster"></a>Betik eylemi çalıştıran bir kümeye uygulama

Bu bölümde, betik eylemleri çalıştıran bir kümeye uygulama açıklanmaktadır.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Betik eylemi çalıştıran bir kümeye Azure portalından uygulayın.

Git [Azure portalında](https://portal.azure.com):

1. Sol menüden **tüm hizmetleri**.

1. Altında **ANALYTICS**seçin **HDInsight kümeleri**.

1. Kümenizi varsayılan görünüm açılır listeden seçin.

1. Varsayılan görünümünden altında **ayarları**seçin **betik eylemlerini**.

1. Üst kısmından **betik eylemlerini** sayfasında **+ gönderme yeni**.

    ![Bir betik çalıştıran bir kümeye ekleme](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Kullanım __bir betik seçin__ giriş önceden hazırlanmış bir komut dosyası seçin. Özel bir betik kullanmayı tercih __özel__. Ardından sağlayın __adı__ ve __Bash betiği URI'si__ betiği için.

    ![Select komut dosyası biçiminde bir komut dosyası Ekle](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Aşağıdaki tabloda, formda öğeleri açıklanmıştır:

    | Özellik | Değer |
    | --- | --- |
    | Bir betik seçin | Kendi betiğinizi kullanmayı tercih __özel__. Aksi takdirde, sağlanan bir betik seçin. |
    | Ad |Betik eylemi için bir ad belirtin. |
    | Bash betiği URI'si |Betik URI'si belirtin. |
    | HEAD/çalışan/Zookeeper |Betik üzerinde çalıştığı düğümleri belirtin: **HEAD**, **çalışan**, veya **ZooKeeper**. |
    | Parametreler |Komut dosyası tarafından gerekli parametreleri belirtin. |

    Kullanım __bu betik eylemi kalıcı__ betik ölçeklendirme işlemleri sırasında uygulanan emin olmak için giriş.

5. Son olarak, seçin **Oluştur** betiği bir kümeye uygulama düğmesi.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Betik eylemi çalıştıran bir kümeye Azure Powershell'den uygulayın.

Başlamadan önce yükleme ve Azure PowerShell yapılandırma emin olun. HDInsight PowerShell cmdlet'lerini çalıştırmak için bir iş istasyonu yapılandırma hakkında daha fazla bilgi için bkz: [genel bakış, Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0#run-or-install).

Aşağıdaki örnek bir betik eylemi çalıştıran bir kümeye uygulama işlemini gösterir:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

İşlem tamamlandıktan sonra aşağıdaki metne benzer bilgiler alırsınız:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Betik eylemi çalıştıran bir kümeye Azure CLI'dan uygulayın.

Başlamadan önce Azure CLI'yı yüklediğinizde ve emin olun. Daha fazla bilgi için [Klasik Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-classic-cli?view=azure-cli-latest).

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. Azure Resource Manager moduna geçin:

    ```bash
    azure config mode arm
    ```

2. Azure aboneliğiniz için kimlik doğrulaması:

    ```bash
    azure login
    ```

3. Betik eylemi çalıştıran bir kümeye geçerlidir:

    ```bash
    azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>
    ```

    Bu komutun parametreleri atlarsanız, bunlar için istenir. Betik ile belirtirseniz `-u` parametrelerini kabul eden kullanarak belirtebilirsiniz `-p` parametresi.

    Geçerli düğüm türleri `headnode`, `workernode`, ve `zookeeper`. Betik, birkaç düğüm türleri için uygulanması gereken, noktalı virgülle ayrılmış türlerini belirtmek `;`. Örneğin, `-n headnode;workernode`.

    Betik kalıcı hale getirmek için ekleme `--persistOnSuccess`. Aynı zamanda betik daha sonra kullanarak kalıcı yapılabilir `azure hdinsight script-action persisted set`.

    İş tamamlandıktan sonra aşağıdaki metni gibi bir çıktı alın:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-by-using-rest-api"></a>REST API kullanarak bir betik eylemi çalıştıran bir kümeye uygulama

Bkz: [REST API'SİNDE Azure HDInsight küme](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>Betik eylemi çalıştıran bir kümeye HDInsight .NET SDK'sından uygulayın.

Bir kümeye betikleri uygulamak için .NET SDK'sını kullanarak bir örnek için bkz. [çalışan Linux tabanlı HDInsight kümesine göre bir betik eylemi uygulamak](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-and-promote-and-demote-script-actions"></a>Geçmişini görüntüleme ve yükseltme ve betik eylemleri düzeyini düşür

### <a name="the-azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüden **tüm hizmetleri**.

1. Altında **ANALYTICS**seçin **HDInsight kümeleri**.

1. Kümenizi varsayılan görünüm açılır listeden seçin.

1. Varsayılan görünümünden altında **ayarları**seçin **betik eylemlerini**.

4. Betik eylemleri bölümünde bu küme için komut geçmişini görüntüler. Bu bilgiler, kalıcı duruma getirilmiş betiklerin listesini içerir. Aşağıdaki ekran görüntüsünde bu kümede Solr betik çalıştırıldıktan gösterir. Ekran görüntüsünde herhangi bir kalıcı duruma getirilmiş betiklerin göstermez.

    ![Betik eylemleri](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Geçmişi görüntülemek için bir betik seçin **özellikleri** bölümü için bu betiği. Ekranın üst kısmından betiği yeniden çalıştırın ya da bunu yükseltin.

    ![Betik eylemleri, özellikleri](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Üç nokta simgesine belirleyebilirsiniz **...** , eylemleri gerçekleştirmek için betik eylemleri bölümünde girişlerde sağındaki.

    ![Betik eylemleri, üç nokta](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet'i | İşlev |
| --- | --- |
| `Get-AzHDInsightPersistedScriptAction` |Kalıcı betik eylemleri hakkında bilgi alın. |
| `Get-AzHDInsightScriptActionHistory` |Betik eylemleri küme veya belirli bir komut dosyası ayrıntılarını uygulanan geçmişini alır. |
| `Set-AzHDInsightPersistedScriptAction` |Kalıcı betik eylemi için bir geçici betik eylemi tanıtın. |
| `Remove-AzHDInsightPersistedScriptAction` |Kalıcı betik eylemi geçici bir eyleme düşürür. |

> [!IMPORTANT]  
> `Remove-AzHDInsightPersistedScriptAction` bir komut dosyası tarafından gerçekleştirilen eylemler geri değil. Bu cmdlet yalnızca, kalıcı bayrağını kaldırır.

Aşağıdaki örnek betik, ve ardından bir komut dosyası düzeyini düşür cmdlet'leri kullanmayı gösterir.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="the-azure-classic-cli"></a>Klasik Azure CLI

| Cmdlet'i | İşlev |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Kalıcı betik eylemleri bir listesini alın. |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Belirli kalıcı betik eylemi hakkında bilgi alın. |
| `azure hdinsight script-action history list <clustername>` |Betik eylemleri, küme uygulanan geçmişini alır. |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Belirli bir betik eylemi hakkında bilgi alın. |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Kalıcı betik eylemi için bir geçici betik eylemi tanıtın. |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Kalıcı betik eylemi geçici bir eyleme düşürür. |

> [!IMPORTANT]  
> `azure hdinsight script-action persisted delete` bir komut dosyası tarafından gerçekleştirilen eylemler geri değil. Bu cmdlet yalnızca, kalıcı bayrağını kaldırır.

### <a name="the-hdinsight-net-sdk"></a>HDInsight .NET SDK'sı

Komut geçmişi bir kümeden almak için .NET SDK'sını kullanarak bir örnek için yükseltmek veya betikleri indirgemek için bkz [ çalışan Linux tabanlı HDInsight kümesine göre bir betik eylemi uygulamak](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]  
> Bu örnek ayrıca .NET SDK'sını kullanarak bir HDInsight uygulaması yüklemek nasıl gösterir.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynaklı yazılım desteği

Microsoft Azure HDInsight hizmeti, açık kaynak teknolojilerini Apache Hadoop geçici olarak biçimlendirilmiş bir kaynak ekosisteminiz kullanır. Microsoft Azure için açık kaynak teknolojilerini genel düzeyde destek sağlar. Daha fazla bilgi için **destek kapsamı** bölümünü [Azure desteği SSS](https://azure.microsoft.com/support/faq/). HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

İki tür açık kaynaklı bileşenlerin HDInsight hizmetinde kullanılabilir:

* **Yerleşik bileşenlerini**. Bu bileşenler, HDInsight kümelerinde önceden ve kümeyi temel işlevlerini sağlar. Aşağıdaki bileşenler, bu kategoriye aittir:

  * [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) ResourceManager.
  * Hive sorgu dili [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).
  * [Apache Mahout](https://mahout.apache.org/). 
    
    Tam küme bileşenleri listesini kullanılabilir [Apache Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilen nelerdir?](hdinsight-component-versioning.md)

* **Özel bileşenler**. Kümeye bir kullanıcı olarak yükleyebilir veya iş yükünüzü Topluluğu'nda kullanılabilir veya sizin tarafınızdan oluşturulan herhangi bir bileşeni kullanın.

> [!WARNING]  
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir. Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun gidermenize yardımcı olması için ticari açıdan makul destek alırsınız. Microsoft Support sorunu çözmek mümkün olabilir. Veya, bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak amacıyla isteyebilirler. Birçok topluluk siteleri kullanılabilir. Örnekler [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) ve [Stack Overflow](https://stackoverflow.com). 
>
> Apache projeleri de proje siteleri [Apache Web sitesi](https://apache.org). Bir örnek [Hadoop](https://hadoop.apache.org/).

HDInsight hizmeti, özel bileşenler kullanmak için birkaç yol sağlar. Aynı düzeyde desteği nasıl bir bileşen kullanılan veya kümeye yüklü olsun geçerlidir. Aşağıdaki listede açıklanmıştır en yaygın yolu özel bileşenler HDInsight kümelerinde kullanılır:

1. **İş gönderme**. Hadoop ya da diğer türleri yürütün veya özel bileşenler kullanan işlerinin kümeye gönderilebilir.

2. **Küme özelleştirmesi**. Küme oluşturma sırasında ek ayarlar ve küme düğümlerinde yüklü özel bileşenler belirtebilirsiniz.

3. **Örnekleri**. Popüler özel bileşenler için Microsoft ve diğerleri HDInsight kümelerinde bu bileşenlerin nasıl kullanılabileceğini örneklerin sağlayabilir. Bu örnekler desteği olmadan sağlanır.

## <a name="troubleshooting"></a>Sorun giderme

Betik eylemleri tarafından günlüğe kaydedilen bilgi görüntülemek için Ambari web kullanıcı Arabirimi kullanabilirsiniz. Betik, küme oluşturma sırasında başarısız olursa, günlükleri kümeyle ilişkilendirilmiş varsayılan depolama hesabını mevcuttur. Bu bölümde, bu seçeneklerin kullanarak günlükleri alma konusunda bilgi sağlar.

### <a name="the-apache-ambari-web-ui"></a>The Apache Ambari web UI

1. Tarayıcınızda, Git https://CLUSTERNAME.azurehdinsight.net. **CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.

    İstendiğinde yönetici hesap adı girin **yönetici**ve küme için parola. Bir web formunda yönetici kimlik bilgilerini yeniden girmeniz gerekebilir.

2. Sayfanın üstündeki çubuğundan seçin **ops** girişi. Ambari aracılığıyla küme işlemi geçerli ve önceki işlemlerinin bir listesini görüntüler.

    ![Ambari web kullanıcı Arabirimi seçili ops çubuğuyla](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Girişlerine sahip bulma **çalıştırma\_customscriptaction** içinde **işlemleri** sütun. Betik eylemleri çalıştırdığınızda, bu girişleri oluşturulur.

    ![İşlemlerinin ekran görüntüsü](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    Görüntülemek için **STDOUT** ve **STDERR** çıktı, select **run\customscriptaction** girişi ve detaya bağlantılar. Bu çıkış, komut dosyası çalıştığında oluşturulur ve yararlı bilgiler olabilir.

### <a name="access-logs-from-the-default-storage-account"></a>Varsayılan depolama hesabından günlüklerine erişme

Küme oluşturma bir betik hatası nedeniyle başarısız olursa, günlükler küme depolama hesabında saklanır.

* Depolama günlüklerini kullanılabilir `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![İşlemlerinin ekran görüntüsü](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    Bu dizin altında günlükleri ayrı olarak için düzenlenir **baş**, **çalışan düğümü**, ve **zookeeper düğümü**. Aşağıdaki örneklere bakın:

    * **Baş düğüm**: `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Çalışan düğümü**: `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper düğümü**: `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Tüm **stdout** ve **stderr** karşılık gelen ana bilgisayarının depolama hesabına yüklenir. Bir **çıkış -\*.txt** ve **hataları -\*.txt** her betik eylemi. **Çıkış *.txt** URI bilgileri ana bilgisayarda çalıştırılan komut dosyası içerir. Bu bilgilerin bir örnek aşağıda gösterilmiştir:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Bir betik eylemi küme tekrar tekrar aynı ada sahip oluşturmak mümkündür. Bu durumda, ilgili günlükler göre ayırt edebilir **tarih** klasör adı. Örneğin, bir küme için klasör yapısı **mycluster**, oluşturulmuş farklı tarihler için aşağıdaki günlük girişlerini benzer görünür:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Aynı gün aynı ada sahip bir komut dosyası eylemi kümesi oluşturursanız, ilgili günlük dosyalarını tanımlamak için benzersiz önekini kullanabilirsiniz.

* 12:00 AM, gece yarısından itibaren yakın bir küme oluşturursanız, bu günlük dosyaları iki güne yayılan mümkündür. Bu durumda, iki farklı tarih klasörü aynı kümenin bakın.

* Varsayılan kapsayıcı için günlük dosyalarını karşıya yükleme, özellikle büyük kümeler için en fazla beş dakika sürebilir. Günlüklere erişmek istiyorsanız, betik eylemi başarısız olursa bu nedenle hemen kümeyi silmeniz gerekir.

### <a name="ambari-watchdog"></a>Ambari izleme

> [!WARNING]  
> Ambari izleme, Linux tabanlı HDInsight kümenizdeki hdinsightwatchdog parolasını değiştirmeyin. Bu hesabın parolasını değiştirme, yeni betik eylemleri HDInsight kümesinde çalıştırma olanağı keser.

### <a name="cant-import-name-blobservice"></a>BlobService adı alınamıyor

__Belirtiler__. Betik eylemi başarısız olur. Ambari, işlemi görüntülediğinizde şu hataya benzer bir metin görüntüler:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Neden__. HDInsight kümesi ile eklenmiştir. Python Azure depolama istemci yükseltirseniz, bu hata oluşur. HDInsight, Azure depolama istemci 0.20.0 bekliyor.

__Çözüm__. Bu hatayı gidermek için el ile her küme düğümüne kullanarak bağlanmak `ssh`. Doğru depolama istemci sürümünü yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
sudo pip install azure-storage==0.20.0
```

Kümeye SSH ile bağlanma hakkında daha fazla bilgi için bkz. [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>Küme oluşturma sırasında kullanılan komut geçmişi göstermiyor

Kümenize 15 Mart 2016'dan önce oluşturulduysa bir betik eylemi geçmişi girişi göremeyebilirsiniz. Küme yeniden boyutlandırma, betik eylemi geçmişinde görünmesini betikleri neden olur.

İki istisna mevcuttur:

* Kümenizi, 1 Eylül 2015'den önce oluşturuldu. Bu tarih, betik eylemleri tanıtılan andır. Bu tarihten önce oluşturulan herhangi bir küme betik eylemleri küme oluşturmak için kullanılan uygulanamadı.

* Birden çok betik eylemleri küme oluşturma sırasında kullandığınız. Ya da birden fazla komut dosyası için birden fazla komut dosyası veya aynı ad, aynı URI ancak farklı parametreler için aynı adı kullanılır. Bu durumlarda, aşağıdaki hatayı alıyorsunuz:

    Yeni betik eylemi mevcut komut çakışan betik adları nedeniyle bu kümede yeniden çalıştırabilirsiniz. Küme oluşturma sırasında sağlanan betik adları tüm benzersiz olmalıdır. Mevcut kodlar üzerinde yeniden boyutlandırma çalıştırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions-linux.md)
* [Yükleme ve HDInsight kümeleri üzerinde Apache giraph'ı kullanma](hdinsight-hadoop-giraph-install-linux.md)
* [Bir HDInsight kümesine ek depolama alanı ekleme](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Küme oluşturma sırasında aşamaları"
