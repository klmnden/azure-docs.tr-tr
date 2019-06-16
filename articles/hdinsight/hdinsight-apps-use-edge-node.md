---
title: HDInsight - Azure, Apache Hadoop kümelerinde boş kenar düğümlerini kullanma
description: Boş bir kenar düğümünü istemci olarak kullanılabilir ve ardından test/konak HDInsight uygulamalarınızı bir HDInsight kümesine ekleme.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: df35ee9791dc1090385e2d2aed5966a1292ddc64
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64708174"
---
# <a name="use-empty-edge-nodes-on-apache-hadoop-clusters-in-hdinsight"></a>HDInsight, Apache Hadoop kümelerinde boş kenar düğümlerini kullanma

Bir HDInsight kümesi için boş bir kenar düğümünü eklemeyi öğrenin. Bir Linux sanal makinesi ile aynı istemci araçları yüklü ve yapılandırılmış olduğu gibi baş düğümler, ancak olmadan bir boş kenar düğümüdür [Apache Hadoop](https://hadoop.apache.org/) çalışan hizmetleri. Kümeye erişen istemci uygulamalarınızı test etme ve istemci uygulamalarınızı barındırmak için kenar düğümünü kullanabilirsiniz. 

Kümeyi oluşturduğunuzda yeni bir küme için var olan bir HDInsight kümesine, boş bir kenar düğümünü ekleyebilirsiniz. Boş bir kenar düğümünü ekleme yapılır Azure Resource Manager şablonu kullanarak.  Aşağıdaki örnek nasıl yapıldığını gösterir kullanarak bir şablonu:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Aşağıdaki örnekte gösterildiği gibi isteğe bağlı olarak çağırabilirsiniz bir [betik eylemi](hdinsight-hadoop-customize-cluster-linux.md) yükleme gibi ek yapılandırma, gerçekleştirilecek [Apache Hue](hdinsight-hadoop-hue-linux.md) kenar düğümünde. Betik eylemi betiği web üzerinde genel olarak erişilebilir olması gerekir.  Örneğin, komut dosyası Azure depolama alanında depolanır, genel kapsayıcılar veya genel BLOB'lar kullanın.

Edge düğüm sanal makine boyutu, HDInsight kümesi çalışan düğümü vm boyutu gereksinimlerini karşılaması gerekir. Önerilen çalışan düğümü vm boyutları için bkz: [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Kenar düğümüne oluşturduktan sonra kenar düğümüne SSH kullanarak bağlanma ve HDInsight Hadoop kümesinde erişmek için İstemci Araçları'nı çalıştırın.

> [!WARNING]   
> Kenar düğümünde yüklü özel bileşenler Microsoft ticari açıdan makul destek alırsınız. Bu, karşılaştığınız sorunları çözmede neden olabilir. Ya da daha fazla yardım almak için topluluk kaynaklarına başvurulabilir. En etkin topluluktan Yardım almak için siteler bazıları şunlardır:
>
> * [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [https://stackoverflow.com](https://stackoverflow.com).
>
> Bir Apache teknolojisi kullanıyorsanız, proje siteleri Apache aracılığıyla Yardım bulmak mümkün olabilir [ https://apache.org ](https://apache.org), gibi [Apache Hadoop](https://hadoop.apache.org/) site.

> [!IMPORTANT]
> Ubuntu görüntülerinde yayımlanmasını, 3 ay içinde yeni HDInsight kümesi oluşturmak için kullanılabilir hale gelir. Ocak 2019'den itibaren (uç düğümleri dahil) çalışan kümeleridir **değil** otomatik düzeltme eki uygulandı. Müşteriler, çalışan bir küme düzeltme eki için betik eylemleri veya başka mekanizmalar kullanmalısınız.  Daha fazla bilgi için [HDInsight için işletim sistemi düzeltme eki uygulama](./hdinsight-os-patching.md).

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Mevcut bir kümeye bir kenar düğümü Ekle
Bu bölümde, mevcut bir HDInsight kümesine bir kenar düğümü eklemek için bir Resource Manager şablonu kullanın.  Resource Manager şablonu bulunabilir [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-add-edge-node/). Resource Manager şablonu konumunda bulunan bir betik eylemi çağırır https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. Betik eylemleri gerçekleştirmez.  Bunu çağıran bir betik eylemi bir Resource Manager şablonundan göstermektir.

**Boş bir kenar düğümünü mevcut bir kümeye eklemek için**

1. Azure'da oturum açın ve Azure Resource Manager şablonu Azure portalında açmak için aşağıdaki görüntüye tıklayın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Aşağıdaki özellikleri yapılandırın:
   
   * **Abonelik**: Kümeyi oluşturmak için kullanılan bir Azure aboneliği seçin.
   * **Kaynak grubu**: Mevcut bir HDInsight kümesi için kullanılan kaynak grubunu seçin.
   * **Konum**: Var olan HDInsight kümesinin konumu seçin.
   * **Küme adı**: Mevcut bir HDInsight kümesinin adını girin.
   * **Kenar düğüm boyutu**: Sanal makine boyutlarından birini seçin. Vm boyutu, çalışan düğümüne vm boyutu gereksinimlerini karşılaması gerekir. Önerilen çalışan düğümü vm boyutları için bkz: [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Kenar düğümü önek**: Varsayılan değer **yeni**.  Varsayılan değer, edge düğüm adı kullanmaktır **edgenode yeni**.  Önek portalından özelleştirebilirsiniz. Ayrıca, şablondan tam adını da özelleştirebilirsiniz.

4. Denetleme **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**ve ardından **satın alma** kenar düğümüne oluşturmak için.

>[!IMPORTANT]  
> Var olan HDInsight kümesi için Azure kaynak grubu seçtiğinizden emin olun.  Aksi takdirde, "iç içe kaynak üzerinde istenen işlem gerçekleştirilemiyor. hata iletisi alıyorum Üst kaynak '&lt;ClusterName >' bulunamadı. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Bir küme oluştururken bir kenar düğümü Ekle
Bu bölümde, bir kenar düğümü ile HDInsight kümesi oluşturmak için Resource Manager şablonu kullanın.  Resource Manager şablonu bulunabilir [Azure hızlı başlangıç şablonları Galerisi'ne](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). Resource Manager şablonu konumunda bulunan bir betik eylemi çağırır https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. Betik eylemleri gerçekleştirmez.  Bunu çağıran bir betik eylemi bir Resource Manager şablonundan göstermektir.

**Bir kenar düğümü ile bir HDInsight kümesi oluşturma**

1. Henüz yoksa, bir HDInsight kümesi oluşturun.  Bkz: [HDInsight Hadoop kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).
2. Azure'da oturum açın ve Azure Resource Manager şablonu Azure portalında açmak için aşağıdaki görüntüye tıklayın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Aşağıdaki özellikleri yapılandırın:
   
   * **Abonelik**: Kümeyi oluşturmak için kullanılan bir Azure aboneliği seçin.
   * **Kaynak grubu**: Küme için kullanılan yeni bir kaynak grubu oluşturun.
   * **Konum**: Kaynak grubu için bir konum seçin.
   * **Küme adı**: Oluşturulacak yeni küme için bir ad girin.
   * **Küme oturum açma kullanıcı adı**: Hadoop HTTP kullanıcı adını girin.  Varsayılan ad, **admin** şeklindedir.
   * **Küme oturum açma parolası**: Hadoop HTTP kullanıcı parolasını girin.
   * **SSH kullanıcı adı**: SSH kullanıcı adı girin. Varsayılan ad **sshuser**.
   * **SSH parolası**: SSH kullanıcı parolasını girin.
   * **Yükleme komut dosyası eylemi**: Bu öğreticide gitmek için varsayılan değer tutun.
     
     Bazı özellikler şablondaki olmuştur: Küme türü, küme çalışan düğümü sayısı, Edge düğüm boyutu ve kenar düğümünün adı.
4. Denetleme **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**ve ardından **satın alma** içeren edge düğümü kümeyi oluşturun.

## <a name="add-multiple-edge-nodes"></a>Birden çok uç düğümleri Ekle

Birden çok uç düğümleri bir HDInsight kümesine ekleyebilirsiniz.  Birden çok uç düğüm yapılandırması yalnızca yapılabilir Azure Resource Manager şablonlarını kullanarak.  Bu makalenin başında şablon örneğine bakın.  Güncelleştirmeye gerek duyduğunuz **targetInstanceCount** oluşturmak istediğiniz uç düğüm sayısını yansıtacak şekilde.

## <a name="access-an-edge-node"></a>Kenar düğümüne erişin
Kenar düğümünün ssh uç noktası olan &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.  Örneğin, yeni-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Kenar düğümüne Azure portalında bir uygulama olarak görünür.  Portal, SSH kullanarak kenar düğümüne erişmek için bilgiler sağlar.

**Kenar düğümünün SSH uç noktası doğrulamak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. HDInsight kümesi ile bir kenar düğümünü açın.
3. **Uygulamalar**'a tıklayın. Kenar düğümüne göreceksiniz.  Varsayılan ad **edgenode yeni**.
4. Kenar düğümüne tıklayın. SSH uç noktası göreceksiniz.

**Kenar düğümüne Hive'ı kullanmak için**

1. Kenar düğümüne bağlanmak için SSH kullanın. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Kenar düğümüne SSH kullanarak bağlandıktan sonra Hive konsolunu açmak için aşağıdaki komutu kullanın:
   
        hive
3. Kümedeki Hive tablolarını göstermek için aşağıdaki komutu çalıştırın:
   
        show tables;

## <a name="delete-an-edge-node"></a>Bir kenar düğümü Sil
Azure portalından bir kenar düğümü silebilirsiniz.

**Kenar düğümüne erişin**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. HDInsight kümesi ile bir kenar düğümünü açın.
3. **Uygulamalar**'a tıklayın. Kenar düğümleri listesini göreceksiniz.  
4. Silin ve ardından istediğiniz kenar düğümüne sağ **Sil**.
5. Onaylamak için **Evet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir kenar düğümü ekleme ve kenar düğümüne erişmek nasıl öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): Bir HDInsight uygulamalarını kümelerinize yüklemeyi öğrenin.
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamaları yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi'nde yayımlama konusunda bilgi edinin.
* [MSDN: Bir HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Linux tabanlı Apache Hadoop kümelerini Resource Manager şablonlarını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.

