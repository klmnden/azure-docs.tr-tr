---
title: Betik eylemleri - Azure kullanarak HDInsight kümelerini özelleştirin
description: Özel bileşenler betik eylemlerini kullanarak Linux tabanlı HDInsight kümelerine ekleyin. Betik eylemleri, küme yapılandırmasını özelleştirebilir veya ek hizmetler ve yardımcı programları Hue, Solr ve r gibi eklemek için kullanılan Bash betikleridir
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: e655624a30332630c28cbd555dac26098adeb68b
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976928"
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-actions"></a>Betik eylemlerini kullanarak Linux tabanlı HDInsight kümeleri özelleştirme

HDInsight sağlar adlı bir yapılandırma yöntemi **betik eylemlerini** küme özelleştirmek için özel komut dosyaları çağırır. Bu betikler, ek bileşenler yükleme ve yapılandırma ayarlarını değiştirmek için kullanılır. Betik eylemleri, küme oluşturma sırasında veya sonrasında kullanılabilir.

> [!IMPORTANT]  
> Zaten çalışan bir kümede betik eylemleri kullanma olanağı, yalnızca Linux tabanlı HDInsight kümeleri için kullanılabilir.
>
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Betik eylemleri, bir HDInsight uygulaması olarak Azure Marketi'nde ayrıca yayımlanabilir. HDInsight uygulamaları hakkında daha fazla bilgi için bkz. [yayımlama HDInsight uygulamalarını Azure Marketi'nde](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>İzinler

Bir etki alanına katılmış HDInsight kümesi kullanıyorsanız, betik eylemleri ile bir küme kullanılırken gerekli olan iki Ambari izni vardır:

* **AMBARI. ÇALIŞTIRMA\_ÖZEL\_KOMUT**: Ambari yöneticisi rolü varsayılan olarak bu izne sahiptir.
* **KÜME. ÇALIŞTIRMA\_ÖZEL\_KOMUT**: HDInsight küme yöneticisinin ve Ambari Yöneticisi varsayılan olarak bu izne sahip.

İzinleri olan etki alanına katılmış HDInsight ile çalışma hakkında daha fazla bilgi için bkz. [etki alanına katılmış HDInsight kümelerini yönetme](./domain-joined/apache-domain-joined-manage.md).

## <a name="access-control"></a>Erişim denetimi

Azure aboneliğinizin Yöneticisi/sahibi değilseniz, hesabınızın en az olmalı **katkıda bulunan** HDInsight kümesi içeren kaynak grubuna erişim.

Ayrıca, bir HDInsight kümesi, biri ile en az oluşturuyorsanız **katkıda bulunan** Azure aboneliğine erişiminiz daha önce kaydetmiş olmalıdır HDInsight için sağlayıcı. Sağlayıcı kaydı, aboneliğe Katkıda Bulunan erişimi olan bir kullanıcı abonelikte ilk kez kaynak oluşturduğunda gerçekleşir. Bu işlem ayrıca [REST ile bir sağlayıcı kaydedilerek](https://msdn.microsoft.com/library/azure/dn790548.aspx) kaynak oluşturulmadan gerçekleştirilebilir.

Erişim yönetimiyle çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure portalında erişim yönetimi ile çalışmaya başlama](../role-based-access-control/overview.md)
* [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../role-based-access-control/role-assignments-portal.md)

## <a name="understanding-script-actions"></a>Betik eylemlerini anlama

Betik eylemi çalıştıran bir HDInsight kümesindeki düğümler üzerinde Bash komut dosyasıdır. Özellikleri ve betik eylemleri özellikleri aşağıda verilmiştir.

* HDInsight kümesinden erişilebilir bir URI üzerinde depolanmış olması gerekir. Olası depolama konumlarını şunlardır:

    * Bir **Azure Data Lake Storage** HDInsight küme tarafından erişilebilir olan hesap. Azure Data Lake Store ile HDInsight kullanma hakkında daha fazla bilgi için bkz: [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

        Data Lake Store içinde depolanan bir betik kullanıldığında, URI biçimi `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]  
        > HDInsight, Data Lake depolamaya erişmek için kullandığı hizmet sorumlusu, betik okuma erişiminiz olması gerekir.

    * Bir blobu bir **Azure depolama hesabı** diğer bir deyişle ya da birincil ya da ek depolama hesabı için HDInsight kümesi. HDInsight, küme oluşturma sırasında hem de bu türlerde depolama hesapları için erişim izni verilir.

    * Paylaşım Hizmeti Azure Blob, GitHub, OneDrive, Dropbox, vb. gibi ortak bir dosya.

        URI, örneğin bkz [örnek betik eylemi betikleri](#example-script-action-scripts) bölümü.

        > [!WARNING]  
        > HDInsight yalnızca Azure depolama hesapları ile standart performans katmanı Blob destekler. 

* Kısıtlanmış olabilir **yalnızca belirli düğüm türleri üzerinde çalıştırma**örnek baş düğümleri veya çalışan düğümleri için.

* Olabilir **kalıcı** veya **geçici**.

    **Kalıcı** komut dosyaları, küme işlemleri ölçeklendirme aracılığıyla eklenen yeni çalışan düğümlerindeki özelleştirmek için kullanılır. Ölçeklendirme işlemlerini gerçekleştiğinde kalıcı bir betik gibi bir baş düğüm, başka bir düğüm türü için değişiklikleri de uygulanabilir.

  > [!IMPORTANT]  
  > Kalıcı betik eylemleri, benzersiz bir adı olmalıdır.

    **Geçici** betikleri kalıcı değildir. Çalışan düğümlerine sahip betiği çalıştırdıktan sonra kümeye eklenen uygulanmaz. Daha sonra kalıcı bir betik için geçici bir betik yükseltebilir veya geçici bir betik için kalıcı bir betik indirgeyin.

  > [!IMPORTANT]  
  > Otomatik olarak kalıcı betik eylemleri, küme oluşturma sırasında kullanılır.
  >
  > Başarısız değildir komut özellikle olması belirtmek bile kalıcı.

* Kabul edebilir **parametreleri** yürütme sırasında komut dosyası tarafından kullanılan.

* Çalıştırma **kök düzeyinde ayrıcalıklara** küme düğümlerinde.

* Aracılığıyla kullanılabilir **Azure portalında**, **Azure PowerShell**, **Klasik Azure CLI'yı**, veya **HDInsight .NET SDK'sı**

Kümedeki çalışan sahip tüm betikler bir geçmişini tutar. Geçmiş, yükseltme veya indirgeme işlemleri için bir komut Kimliğini bulmak gerektiğinde faydalıdır.

> [!IMPORTANT]  
> Betik eylemi tarafından yapılan değişiklikleri geri almak için otomatik bir yolu yoktur. El ile değişikliklerinizi geri veya bunları tersine bir betik sağlayın.

### <a name="script-action-in-the-cluster-creation-process"></a>Küme oluşturma işlemi betik eylemi

Betik eylemleri, küme oluşturma sırasında kullanılan eylemleri var olan bir kümede çalışan komut dosyasından biraz farklıdır:

* Komut dosyası **otomatik olarak kalıcı**.

* A **hatası** betikte küme oluşturma işlemi başarısız olmasına neden olabilir.

Betik eylemi oluşturma işlemi sırasında yürütüldüğünde, aşağıdaki diyagramda gösterilmektedir:

![HDInsight kümesi özelleştirme ve aşamaları sırasında küme oluşturma][img-hdi-cluster-states]

HDInsight yapılandırılırken betiği çalıştırır. Betik, belirtilen kümedeki tüm düğümler üzerinde paralel olarak çalışır ve düğümler üzerinde kök ayrıcalıklarıyla çalıştırır.

> [!NOTE]  
> Durdurma ve Apache Hadoop ile ilgili hizmetler gibi hizmetler başlatma gibi işlemler gerçekleştirebilirsiniz. Hizmetleri durdurun, Ambari hizmet ve önce betik çalıştıran diğer Hadoop ile ilgili hizmetlerin tamamlandığından emin olmanız gerekir. Bu hizmetler, oluşturulurken kümesinin durumunu ve sistem durumu başarılı bir şekilde belirlemek için gereklidir.


Küme oluşturma sırasında tek seferde birden çok betik eylemleri kullanabilirsiniz. Bu betikler, belirtilmiş olması sırayla çağrılır.

> [!IMPORTANT]  
> Betik eylemleri, 60 dakika veya zaman aşımı içinde tamamlamanız gerekir. Betik, küme hazırlama sırasında eşzamanlı diğer Kurulum ve yapılandırma işlemleri çalıştırır. CPU süresi veya ağ bant genişliği gibi kaynak rekabetini betiği geliştirme ortamınızda gösterilenden tamamlanması uzun sürmesine neden olabilir.
>
> Betiği çalıştırmak için gereken süreyi en aza indirmek için yükleme ve kaynak uygulamalardan derleme gibi görevleri kaçının. Uygulamalar önceden derlemek ve ikili Azure Depolama'da saklayın.


### <a name="script-action-on-a-running-cluster"></a>Betik eylemi çalıştıran bir kümede

Bir zaten çalışan bir betikte hata çalıştıran küme otomatik olarak neden başarısız duruma değiştirmek küme. Bir komut dosyası tamamlandıktan sonra küme bir "çalışır" duruma döndürmeniz gerekir.

> [!IMPORTANT]  
> Küme bir 'çalışıyor' durumunda olsa bile, başarısız olan kodu şeyler bozuk. Örneğin, bir komut dosyası küme için gerekli dosyaları silebilirsiniz.
>
> Betik eylemleri, kök ayrıcalıklarıyla çalıştırın. Kümenize uygulamadan önce bir betiği yaptığı anladığınızdan emin olun.

Bir betiği bir kümeye uygulama küme durumunun döndürdüğünüzde **çalıştıran** için **kabul edilen**, ardından **HDInsight yapılandırma**ve son olarak yeniden  **Çalışan** başarılı bir komut dosyası için. Betik durumunu betik eylemi geçmişinde kaydedilir ve betik başarılı veya başarısız olduğunu belirlemek için bu bilgileri kullanın. Örneğin, `Get-AzureRmHDInsightScriptActionHistory` PowerShell cmdlet'i, bir komut durumunu görüntülemek için kullanılabilir. Bilgileri aşağıdaki metne benzer döndürür:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]  
> Küme oluşturulduktan sonra küme kullanıcı (Yönetici) parolası değiştirdiyseniz, betik eylemleri bu kümede çalıştırılan başarısız olabilir. Kalıcı betik eylemleri, hedef alt düğümleri varsa, bu betikleri kümeyi ölçeklediğinizde başarısız olabilir.

## <a name="example-script-action-scripts"></a>Örnek betik eylemi betikleri

Betik eylemi betikler aşağıdaki yardımcı programlar kullanılabilir:

* Azure portal
* Azure PowerShell
* Klasik Azure CLI
* HDInsight .NET SDK'sı

HDInsight, HDInsight kümelerinde aşağıdaki bileşenleri yüklemek için komut dosyaları sağlar:

| Ad | Betik |
| --- | --- |
| **Bir Azure depolama hesabı ekleme** |https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. Bkz: [bir HDInsight kümesine ek depolama alanı eklentisi](hdinsight-hadoop-add-storage.md). |
| **Hue yükleme** |https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Bkz: [yükleme ve kullanma, HDInsight üzerinde Hue kümeleri](hdinsight-hadoop-hue-linux.md). |
| **Presto yükleme** |https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. Bkz: [HDInsight üzerinde Presto yükleme ve kullanma kümeleri](hdinsight-hadoop-install-presto.md). |
| **Solr yükleme** |https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. Bkz: [HDInsight üzerinde Solr yükleme ve kullanma kümeleri](hdinsight-hadoop-solr-install-linux.md). |
| **Giraph yükleme** |https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. Bkz: [HDInsight üzerinde Giraph yükleme ve kullanma kümeleri](hdinsight-hadoop-giraph-install-linux.md). |
| **Hive kitaplıklarını önceden yükleme** |https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. Bkz: [ekleme Hive kitaplıkları HDInsight kümelerinde](hdinsight-hadoop-add-hive-libraries.md). |
| **Mono yükleme veya güncelleştirme** | https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash. Bkz: [yükleme veya güncelleştirme HDInsight üzerinde Mono](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Küme oluşturma sırasında bir betik eylemi kullanın

Bu bölümde, betik eylemleri bir HDInsight kümesi oluştururken kullanabileceğiniz farklı yolları üzerinde örnekler sağlar.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Azure portalından küme oluşturma sırasında bir betik eylemi kullanın

1. Anlatıldığı gibi bir küme oluşturmaya başlayın [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Küme oluşturma sırasında ulaşırsınız bir __küme özeti__ sayfası. Gelen __küme özeti__ sayfasında __Düzenle__ için bağlantı __Gelişmiş ayarlar__.

    ![Gelişmiş ayarlar bağlantısı](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Gelen __Gelişmiş ayarlar__ bölümünden __betik eylemlerini__. Gelen __betik eylemlerini__ bölümünden __+ yeni Gönder__

    ![Yeni betik eylemini gönderin](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Kullanım __bir betik seçin__ girişi önceden yapılmış bir betik seçin. Özel bir betik kullanmayı tercih __özel__ ve ardından sağlayın __adı__ ve __Bash betiği URI'si__ betiği için.

    ![Select komut dosyası biçiminde bir komut dosyası Ekle](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Aşağıdaki tabloda, formda öğeleri açıklanmıştır:

    | Özellik | Değer |
    | --- | --- |
    | Bir betik seçin | Kendi betiğinizi kullanmayı tercih __özel__. Aksi takdirde, sağlanan betikleri birini seçin. |
    | Ad |Betik eylemi için bir ad belirtin. |
    | Bash betiği URI'si |Betik URI'si belirtin. |
    | HEAD/çalışan/Zookeeper |Düğüm belirtin (**baş**, **çalışan**, veya **ZooKeeper**) betiğin çalıştırıldığı üzerinde. |
    | Parametreler |Komut dosyası tarafından gerekli parametreleri belirtin. |

    Kullanım __bu betik eylemi kalıcı__ betik ölçeklendirme işlemleri sırasında uygulandığından emin olmak için giriş.

5. Seçin __Oluştur__ betiği kaydedilemiyor. Ardından __+ gönderme yeni__ başka bir komut dosyası eklemek için.

    ![Birden çok betik eylemleri](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Komut dosyaları eklemeyi tamamladığınızda, kullanın __seçin__ düğmesini ve ardından __sonraki__ dönmek için düğme __küme özeti__ bölümü.

3. Kümeyi oluşturmak için Seç __Oluştur__ gelen __küme özeti__ seçimi.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarından betik eylemi kullanın

Betik eylemleri, Azure Resource Manager şablonları ile kullanılabilir. Bir örnek için bkz. [ https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/ ](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/).

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

Şablon dağıtma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Kaynakları şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Şablonları ve Azure CLI ile kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Azure PowerShell üzerinden küme oluşturma sırasında bir betik eylemi kullanın

Bu bölümde, kullandığınız [Ekle AzureRmHDInsightScriptAction](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightscriptaction) cmdlet'ini küme özelleştirme betikleri çağırma. Devam etmeden önce Azure PowerShell yükleyip yapılandırdığınızdan emin olun. HDInsight PowerShell cmdlet'lerini çalıştırmak için bir iş istasyonu yapılandırma hakkında daha fazla bilgi için bkz: [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview).

Aşağıdaki betik, PowerShell kullanarak küme oluşturma sırasında bir betik eylemi uygulamak gösterilmektedir:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Uygulamanın, küme oluşturulmadan önce birkaç dakika sürebilir.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>HDInsight .NET SDK küme oluşturma sırasında bir betik eylemi kullanın

HDInsight .NET SDK'sı bir .NET uygulamasından HDInsight ile çalışmak kolaylaştıran istemci kitaplıkları sağlar. Bir kod örneği için bkz. [.NET SDK kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-to-a-running-cluster"></a>Betik eylemi çalıştıran bir kümeye uygulama

Bu bölümde, betik eylemleri çalıştıran bir kümeye uygulama konusunda bilgi edinin.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Betik eylemi çalıştıran bir kümeye Azure portalından uygulayın.

Gelen [Azure portalında](https://portal.azure.com):

1. Sol menüden **tüm hizmetleri**.

1. Altında **ANALYTICS**seçin **HDInsight kümeleri**.

1. Kümenizi varsayılan görünüm açılır listeden seçin.

1. Varsayılan görünümünden altında **ayarları**seçin **betik eylemlerini**.

1. Üst kısmından **betik eylemlerini** sayfasında **+ gönderme yeni**.

    ![Bir betik çalıştıran bir kümeye ekleme](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Kullanım __bir betik seçin__ girişi önceden yapılmış bir betik seçin. Özel bir betik kullanmayı tercih __özel__ ve ardından sağlayın __adı__ ve __Bash betiği URI'si__ betiği için.

    ![Select komut dosyası biçiminde bir komut dosyası Ekle](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Aşağıdaki tabloda, formda öğeleri açıklanmıştır:

    | Özellik | Değer |
    | --- | --- |
    | Bir betik seçin | Kendi betiğinizi kullanmayı tercih __özel__. Aksi takdirde, sağlanan bir betik seçin. |
    | Ad |Betik eylemi için bir ad belirtin. |
    | Bash betiği URI'si |Betik URI'si belirtin. |
    | HEAD/çalışan/Zookeeper |Düğüm belirtin (**baş**, **çalışan**, veya **ZooKeeper**) betiğin çalıştırıldığı üzerinde. |
    | Parametreler |Komut dosyası tarafından gerekli parametreleri belirtin. |

    Kullanım __bu betik eylemi kalıcı__ betik ölçeklendirme işlemleri sırasında uygulanan emin olmak için giriş.

5. Son olarak, **Oluştur** betiği bir kümeye uygulama düğmesi.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Betik eylemi çalıştıran bir kümeye Azure Powershell'den uygulayın.

Devam etmeden önce Azure PowerShell yükleyip yapılandırdığınızdan emin olun. HDInsight PowerShell cmdlet'lerini çalıştırmak için bir iş istasyonu yapılandırma hakkında daha fazla bilgi için bkz: [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview).

Aşağıdaki örnek bir betik eylemi çalıştıran bir kümeye uygulama işlemini göstermektedir:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

İşlem tamamlandığında, aşağıdaki metne benzer bilgiler alırsınız:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Betik eylemi çalıştıran bir kümeye Azure CLI'dan uygulayın.

Devam etmeden önce Azure CLI'yı yükleyip yapılandırdığınızdan emin olun. Daha fazla bilgi için [Azure Klasik CLI'yı yükleme](../cli-install-nodejs.md).

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. Azure Resource Manager moduna geçmek için komut satırında aşağıdaki komutu kullanın:

    ```bash
    azure config mode arm
    ```

2. Azure aboneliğiniz için kimlik doğrulaması yapmak için aşağıdakileri kullanın.

    ```bash
    azure login
    ```

3. Betik eylemi çalıştıran bir kümeye uygulamak için aşağıdaki komutu kullanın.

    ```bash
    azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>
    ```

    Bu komutun parametreleri atlarsanız, sizden istenir. Betik ile belirtirseniz `-u` parametrelerini kabul eden belirtebilirsiniz kullanarak `-p` parametresi.

    Geçerli düğüm türleri `headnode`, `workernode`, ve `zookeeper`. Komut dosyası birden çok düğüm türleri için uygulanması gereken, ayrılmış türlerini belirtin. bir ';'. Örneğin, `-n headnode;workernode`.

    Betik kalıcı hale getirmek için ekleme `--persistOnSuccess`. Aynı zamanda betik daha sonra kullanarak kalıcı yapılabilir `azure hdinsight script-action persisted set`.

    İş tamamlandıktan sonra aşağıdaki metne benzer bir çıktı alırsınız:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-using-rest-api"></a>REST API kullanarak çalışan bir küme için bir betik eylemi Uygula

Bkz: [betik eylemleri çalıştıran bir kümede çalıştırmak](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>Betik eylemi çalıştıran bir kümeye HDInsight .NET SDK'sından uygulayın.

Bir kümeye betikleri uygulamak için .NET SDK'sını kullanarak bir örnek için bkz. [ https://github.com/Azure-Samples/hdinsight-dotnet-script-action ](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Betik eylemleri indirgemek geçmişini görüntüleme ve yükseltme

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

1. Oturum [Azure portalında](https://portal.azure.com).

1. Sol menüden **tüm hizmetleri**.

1. Altında **ANALYTICS**seçin **HDInsight kümeleri**.

1. Kümenizi varsayılan görünüm açılır listeden seçin.

1. Varsayılan görünümünden altında **ayarları**seçin **betik eylemlerini**.

4. Bu küme için komut geçmişini betik eylemleri bölümünde görüntülenir. Bu bilgiler, kalıcı duruma getirilmiş betiklerin listesini içerir. Aşağıdaki ekran görüntüsünde, komut dosyası kaldırıldı Solr'ın bu küme üzerinde çalışan görebilirsiniz. Ekran görüntüsünde herhangi bir kalıcı duruma getirilmiş betiklerin göstermez.

    ![Betik eylemleri bölümünde](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Geçmişten bir betik seçerek özellikler bölümü için bu betiği görüntüler. Ekranın üst kısmından betiği yeniden çalıştırın ya da bunu yükseltin.

    ![Betik eylemleri özellikleri](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Ayrıca **...**  eylemleri gerçekleştirmek için betik eylemleri bölümünde girişlerde sağındaki.

    ![Betik eylemleri... kullanımı](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

| Aşağıdaki kullan... | Hedef... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |Kalıcı betik eylemleri hakkında bilgi alma |
| Get-AzureRmHDInsightScriptActionHistory |Betik eylemleri küme veya belirli bir komut dosyası ayrıntılarını uygulanan geçmişini alma |
| Set-AzureRmHDInsightPersistedScriptAction |Kalıcı betik eylemi için bir geçici betik eylemi yükseltir |
| Remove-AzureRmHDInsightPersistedScriptAction |Kalıcı betik eylemi geçici bir eyleme indirger. |

> [!IMPORTANT]  
> Kullanarak `Remove-AzureRmHDInsightPersistedScriptAction` bir betik tarafından gerçekleştirilen eylemler geri almaz. Bu cmdlet yalnızca, kalıcı bayrağını kaldırır.

Aşağıdaki örnek betik, yükseltmek ve ardından bir komut dosyası düzeyini düşür cmdlet'leri kullanmayı gösterir.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-the-azure-classic-cli"></a>Klasik Azure CLI kullanma

| Aşağıdaki kullan... | Hedef... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Kalıcı betik eylemleri bir listesini alın |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Belirli kalıcı betik eylemi hakkında bilgi alma |
| `azure hdinsight script-action history list <clustername>` |Betik eylemleri, küme uygulanan geçmişini alma |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Belirli bir betik eylemi hakkında bilgi alma |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Kalıcı betik eylemi için bir geçici betik eylemi yükseltir |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Kalıcı betik eylemi geçici bir eyleme indirger. |

> [!IMPORTANT]  
> Kullanarak `azure hdinsight script-action persisted delete` bir betik tarafından gerçekleştirilen eylemler geri almaz. Bu cmdlet yalnızca, kalıcı bayrağını kaldırır.

### <a name="using-the-hdinsight-net-sdk"></a>HDInsight .NET SDK'sını kullanma

Komut geçmişi bir kümeden almak için .NET SDK'sını kullanarak bir örnek için yükseltmek veya betikleri indirgemek için bkz [ https://github.com/Azure-Samples/hdinsight-dotnet-script-action ](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]  
> Bu örnek ayrıca .NET SDK'sını kullanarak bir HDInsight uygulamasının nasıl yükleneceğini gösterir.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynaklı yazılım desteği

Microsoft Azure HDInsight hizmeti, açık kaynak teknolojilerini Apache Hadoop geçici olarak biçimlendirilmiş bir kaynak ekosisteminiz kullanır. Microsoft Azure için açık kaynak teknolojilerini genel düzeyde destek sağlar. Daha fazla bilgi için **destek kapsamı** bölümünü [Azure desteği SSS Web sitesine](https://azure.microsoft.com/support/faq/). HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

HDInsight hizmetinde kullanılabilir açık kaynak bileşenleri iki tür vardır:

* **Yerleşik bileşenlerini** -bu bileşenler HDInsight kümelerinde önceden yüklü olan ve kümeyi temel işlevlerini sağlar. Örneğin, [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) ResourceManager, Hive sorgu dili ([HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)) ve [Apache Mahout](https://mahout.apache.org/) kitaplığı, bu kategoriye aittir. Tam küme bileşenleri listesini kullanılabilir [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler](hdinsight-component-versioning.md).
* **Özel bileşenler** -, kümenin bir kullanıcı olarak yükleyebilir veya herhangi bir bileşeni Topluluğu'nda kullanılabilir veya sizin tarafınızdan oluşturulan iş yükünüzü kullanın.

> [!WARNING]  
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir. Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Microsoft desteği sorunu çözmek mümkün olabilir veya bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak için isteyebilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Apache projeleri proje siteleri de [ https://apache.org ](https://apache.org), örneğin: [Hadoop](https://hadoop.apache.org/).

HDInsight hizmeti, özel bileşenler kullanmak için birkaç yol sağlar. Aynı düzeyde desteği nasıl bir bileşen kullanılan veya kümeye yüklü bağımsız olarak geçerlidir. Aşağıdaki listede açıklanmıştır en yaygın yolu özel bileşenler HDInsight kümelerinde kullanılabilir:

1. İş gönderme - Hadoop ya da diğer türleri yürütün veya özel bileşenler kullanan işlerinin kümeye gönderilebilir.

2. Küme özelleştirmesi - küme oluşturma sırasında ek ayarlar ve küme düğümlerinde yüklü özel bileşenler belirtebilirsiniz.

3. Örnekleri - popüler özel bileşenler, Microsoft ve diğerleri için HDInsight kümelerinde bu bileşenlerin nasıl kullanılabileceğini örneklerin sağlayabilir. Bu örnekler desteği olmadan sağlanır.

## <a name="troubleshooting"></a>Sorun giderme

Betik eylemleri tarafından günlüğe kaydedilen bilgi görüntülemek için Ambari web kullanıcı Arabirimi kullanabilirsiniz. Betik, küme oluşturma sırasında başarısız olursa, günlükleri kümeyle ilişkilendirilmiş varsayılan depolama hesabını mevcuttur. Bu bölümde, bu seçeneklerin kullanarak günlükleri alma hakkında bilgi sağlar.

### <a name="using-the-apache-ambari-web-ui"></a>Apache Ambari Web kullanıcı arabirimini kullanarak

1. Tarayıcınızda https://CLUSTERNAME.azurehdinsight.net adresine gidin. CLUSTERNAME HDInsight kümenizin adıyla değiştirin.

    İstendiğinde, küme için Yönetici hesap adı (Yönetici) ve parolayı girin. Bir web formunda yönetici kimlik bilgilerini girmek zorunda kalabilirsiniz.

2. Sayfanın üstündeki çubuğundan seçin **ops** girişi. Ambari aracılığıyla gerçekleştirilen geçerli ve önceki işlemlerin bir listesi görüntülenir.

    ![Ambari web kullanıcı Arabirimi seçili ops çubuğuyla](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Girişlerine sahip bulma **çalıştırma\_customscriptaction** içinde **işlemleri** sütun. Betik eylemleri çalıştırdığınızda, bu girişleri oluşturulur.

    ![İşlemlerinin ekran görüntüsü](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    STDOUT ve STDERR çıktısını görüntülemek için run\customscriptaction girişi seçin ve bağlantılar aracılığıyla detaya gitme. Bu çıkış, komut dosyası çalıştığında, oluşturulur ve faydalı bilgiler içerebilir.

### <a name="access-logs-from-the-default-storage-account"></a>Varsayılan depolama hesabından günlüklerine erişme

Küme oluşturma bir betik hatası nedeniyle başarısız olursa, günlükler küme depolama hesabında saklanır.

* Depolama günlüklerini kullanılabilir `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![İşlemlerinin ekran görüntüsü](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    Bu dizin altında baş düğüm, workernode ve zookeeper düğümleri için günlükleri ayrı olarak düzenlenir. Bazı örnekler şunlardır:

    * **Baş düğüm** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Çalışan düğümü** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper düğümü** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Stdout ve stderr karşılık gelen ana bilgisayarının tüm depolama hesabına yüklenir. Bir **çıkış -\*.txt** ve **hataları -\*.txt** her betik eylemi. Çıkış *.txt dosya URI ana bilgisayarda çalışan komut bilgileri içerir. Bu bilgilerin bir örnek aşağıda gösterilmiştir:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Bir betik eylemi küme tekrar tekrar aynı ada sahip oluşturmak mümkündür. Böyle bir durumda tarih klasör adına göre ilgili günlükler ayırt edebilir. Örneğin, farklı tarihlerinde oluşturulan bir küme (mycluster) klasör yapısı aşağıdaki günlük girdileri benzer görünür:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Aynı gün aynı ada sahip bir komut dosyası eylemi kümesi oluşturursanız, ilgili günlük dosyalarını tanımlamak için benzersiz önekini kullanabilirsiniz.

* 12:00:00 (gece yarısı) ile yakın bir küme oluşturursanız, bu günlük dosyaları iki güne yayılan mümkündür. Böyle durumlarda, iki farklı tarih klasörü aynı kümenin görürsünüz.

* Varsayılan kapsayıcı için günlük dosyalarını karşıya yükleme, özellikle büyük kümeler için 5 dakika kadar sürebilir. Günlüklere erişmek istiyorsanız, betik eylemi başarısız olursa bu nedenle, hemen küme silmemelisiniz.

### <a name="ambari-watchdog"></a>Ambari izleme

> [!WARNING]  
> Linux tabanlı HDInsight kümenizdeki Ambari bekçi (hdinsightwatchdog) için parolayı değiştirmeyin. Bu hesabın parolasını değiştirme, yeni betik eylemleri HDInsight kümesinde çalıştırma olanağı keser.

### <a name="cant-import-name-blobservice"></a>BlobService adı alınamıyor

__Belirtiler__: Betik eylemi başarısız olur. Ambari, işlemi görüntülediğinizde şu hataya benzer bir metin görüntülenir:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Neden__: HDInsight kümesi ile birlikte sağlanan Python Azure depolama istemci yükseltirseniz, bu hata oluşur. HDInsight, Azure depolama istemci 0.20.0 bekliyor.

__Çözüm__: Bu hatayı gidermek için el ile her küme düğümünü kullanarak bağlanmak `ssh` ve doğru depolama istemci sürümünü yeniden yüklemek için aşağıdaki komutu kullanın:

```bash
sudo pip install azure-storage==0.20.0
```

Kümeye SSH ile bağlanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Küme oluşturma sırasında kullanılan komut geçmişi göstermiyor

Kümenize 15 Mart 2016'dan önce oluşturulduysa bir betik eylemi geçmişi girişi göremeyebilirsiniz. Küme yeniden boyutlandırma, betik eylemi geçmişinde görünmesini betikleri neden olur.

İki istisna mevcuttur:

* Kümenizi, 1 Eylül 2015'den önce oluşturulduysa. Bu tarih, betik eylemleri tanıtılan andır. Bu tarihten önce oluşturulan herhangi bir küme betik eylemleri için küme oluşturma birlikte.

* Varsa birden çok betik eylemleri, küme oluşturma sırasında kullanılan ve birden fazla komut dosyası için aynı ad veya aynı adı, aynı URI ancak farklı parametreler birden fazla komut dosyası için kullanılır. Bu durumlarda, aşağıdaki hatayı alırsınız:

    Mevcut komut çakışan betik adları nedeniyle bu kümedeki hiçbir yeni betik eylemleri olabilir çalıştı. Betik adları kümesine sağlanan oluşturma gereken tüm benzersiz olmalıdır. Var olan betikler boyutlandırmada çalıştı.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions-linux.md)
* [Yükleme ve HDInsight kümeleri üzerinde Apache Solr kullanma](hdinsight-hadoop-solr-install-linux.md)
* [Yükleme ve HDInsight kümeleri üzerinde Apache giraph'ı kullanma](hdinsight-hadoop-giraph-install-linux.md)
* [Bir HDInsight kümesine ek depolama alanı ekleme](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Küme oluşturma sırasında aşamaları"
