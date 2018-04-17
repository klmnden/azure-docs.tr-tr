---
title: Hdınsight'ta - Azure Hadoop kümeleri üzerinde boş kenar düğümünü kullanmak | Microsoft Docs
description: Nasıl bir istemci olarak kullanılabilir ve ardından test/ana Hdınsight uygulamalarınızı Hdınsight kümesi için bir boş kenar düğümüne eklenir.
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: ''
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 01/11/2018
ms.author: jgao
ms.openlocfilehash: 0e5e05a1a5c084854cd911188777dedf40817227
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a>Hdınsight'ta Hadoop kümeleri üzerinde boş kenar düğümleri kullanın

Bir Hdınsight kümesine bir boş kenar düğümüne eklemeyi öğrenin. Bir boş kenar düğümüne yüklenir ve yapılandırılır. headnodes olduğu gibi aynı istemci araçları ile ancak çalışan hiçbir Hadoop Hizmetleri Linux sanal makine var. Kenar düğümüne küme erişmek, istemci uygulamalarınızı test etme ve istemci uygulamalarını barındırmak için kullanabilirsiniz. 

Var olan Hdınsight kümesine, Küme oluşturduğunuzda, yeni bir kümeye boş kenar düğümüne ekleyin. Bir boş kenar düğümüne ekleyerek yapılır Azure Resource Manager şablonu kullanarak.  Aşağıdaki örnek nasıl yapıldığını gösteren bir şablon kullanarak:

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

Aşağıdaki örnekte gösterildiği gibi isteğe bağlı olarak çağırabilirsiniz bir [betik eylemi](hdinsight-hadoop-customize-cluster-linux.md) yükleme gibi ek yapılandırma gerçekleştirmeniz [Apache ton](hdinsight-hadoop-hue-linux.md) kenar düğümüne içinde. Betik eylemi betik Web'de genel olarak erişilebilir olmalıdır.  Örneğin, komut dosyası Azure depolama alanında depolanıyorsa, genel kapsayıcılar veya genel BLOB'lar kullanın.

Edge düğüm sanal makine boyutu Hdınsight küme çalışan düğümü vm boyutu gereksinimlerini karşılaması gerekir. Önerilen çalışan düğümü vm boyutları için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Bir edge düğümünü oluşturduktan sonra SSH kullanarak kenar düğümüne bağlanmak ve Hdınsight Hadoop kümesinde erişmek için istemci araçlarını çalıştırın.

> [!WARNING] 
> Kenar düğümüne yüklenir özel bileşenler Microsoft ticari koşulların elverdiği oranda makul desteği alabilirsiniz. Bu, karşılaştığınız sorunları çözme konusunda neden olabilir. Veya daha fazla yardım almak için topluluk kaynaklarına adlandırılabilir. En etkin topluluktan Yardım almak için sitelere bazıları şunlardır:
>
> * [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [http://stackoverflow.com](http://stackoverflow.com).
>
> Bir Apache teknolojisi kullanıyorsanız, Apache yardımla üzerinde proje siteleri bulamıyor olabilir [ http://apache.org ](http://apache.org), gibi [Hadoop](http://hadoop.apache.org/) site.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Varolan bir kümeye bir kenar düğümüne ekleyin
Bu bölümde, bir kenar düğümü olan bir Hdınsight kümesine eklemek için bir Resource Manager şablonunu kullanın.  Resource Manager şablonu bulunabilir [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/). Resource Manager şablonu konumunda bulunan bir betik eylemi çağırır https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. Betik herhangi bir eylem gerçekleştirmez.  Bu arama betik eylemi bir Resource Manager şablonunda göstermektir.

**Bir boş kenar düğümüne varolan bir kümeye eklemek için**

1. Henüz yoksa, Hdınsight kümesi oluşturun.  Bkz: [Hadoop Öğreticisi: Hdınsight'ta Hadoop kullanmaya başlamanıza](hadoop/apache-hadoop-linux-tutorial-get-started.md).
2. Azure'da oturum açın ve Azure portalında Azure Resource Manager şablonunu açmak için aşağıdaki görüntüye tıklayın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Aşağıdaki özellikleri yapılandırın:
   
   * **Abonelik**: kümeyi oluşturmak için kullanılan Azure aboneliğini seçin.
   * **Kaynak grubu**: Varolan Hdınsight kümesi için kullanılan kaynak grubunu seçin.
   * **Konum**: olan bir Hdınsight kümesine konumunu seçin.
   * **Küme adı**: var olan bir Hdınsight kümesi adını girin.
   * **Kenar düğüm boyutu**: VM boyutları birini seçin. Vm boyutu çalışan düğümü vm boyutu gereksinimlerini karşılaması gerekir. Önerilen çalışan düğümü vm boyutları için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Kenar düğümü önek**: varsayılan değer **yeni**.  Varsayılan değer, edge düğüm adı kullanmaktır **edgenode yeni**.  Portal önekten özelleştirebilirsiniz. Şablondan tam adını da özelleştirebilirsiniz.

4. Denetleme **hüküm ve koşulları yukarıda belirtildiği ediyorum**ve ardından **satın alma** kenar düğümüne oluşturmak için.

>[!IMPORTANT]
> Azure kaynak grubu için var olan Hdınsight kümesi seçtiğinizden emin olun.  Aksi takdirde, "iç içe kaynak üzerinde istenen işlem gerçekleştirilemiyor. hata iletisi alıyorum Üst kaynak '&lt;ClusterName >' bulunamadı. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Bir küme oluştururken bir kenar düğümüne ekleyin
Bu bölümde, bir kenar düğümüne ile Hdınsight kümesi oluşturmak için Resource Manager şablonunu kullanın.  Resource Manager şablonu bulunabilir [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). Resource Manager şablonu konumunda bulunan bir betik eylemi çağırır https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. Betik herhangi bir eylem gerçekleştirmez.  Bu arama betik eylemi bir Resource Manager şablonunda göstermektir.

**Bir boş kenar düğümüne varolan bir kümeye eklemek için**

1. Henüz yoksa, Hdınsight kümesi oluşturun.  Bkz: [Hdınsight'ta Hadoop kullanmaya başlamanıza](hadoop/apache-hadoop-linux-tutorial-get-started.md).
2. Azure'da oturum açın ve Azure portalında Azure Resource Manager şablonunu açmak için aşağıdaki görüntüye tıklayın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Aşağıdaki özellikleri yapılandırın:
   
   * **Abonelik**: kümeyi oluşturmak için kullanılan Azure aboneliğini seçin.
   * **Kaynak grubu**: küme için kullanılan yeni bir kaynak grubu oluşturun.
   * **Konum**: Kaynak grubu için bir konum seçin.
   * **Küme adı**: oluşturmak yeni küme için bir ad girin.
   * **Oturum açma kullanıcı adı küme**: Hadoop HTTP kullanıcı adı girin.  Varsayılan ad **yönetici**.
   * **Oturum açma parolası küme**: Hadoop HTTP kullanıcı parolasını girin.
   * **SSH kullanıcı adı**: SSH kullanıcı adı girin. Varsayılan ad **sshuser**.
   * **SSH parolası**: SSH kullanıcı parolasını girin.
   * **Yükleme komut dosyası eylemi**: Bu öğreticide gitmek için varsayılan değer tutun.
     
     Bazı özellikler sabit kodlanmış şablondaki olmuştur: küme türü, küme çalışan düğüm sayısı, Edge düğüm boyutu ve kenar düğüm adı.
4. Denetleme **hüküm ve koşulları yukarıda belirtildiği ediyorum**ve ardından **satın alma** kenar düğümü ile küme oluşturmak için.

## <a name="access-an-edge-node"></a>Bir kenar düğümüne erişin
Kenar düğümüne ssh uç noktadır &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.  Örneğin, yeni-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Kenar düğümüne Azure Portal'da bir uygulama olarak görünür.  Portal SSH kullanarak kenar düğümüne erişmeye bilgileri verir.

**Edge düğüm SSH uç noktası doğrulamak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Hdınsight kümesi ile bir edge düğümünü açın.
3. Tıklatın **uygulamaları** küme dikey penceresinden. Kenar düğümünü göreceksiniz.  Varsayılan ad **edgenode yeni**.
4. Kenar düğümüne tıklayın. SSH uç noktası göreceksiniz.

**Edge düğüm üzerinde Hive kullanmak için**

1. Kenar düğümüne bağlanmak için SSH kullanın. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. SSH kullanarak kenar düğümüne bağlandıktan sonra Hive konsolunu açmak için aşağıdaki komutu kullanın:
   
        hive
3. Hive tablolarını kümede göstermek için aşağıdaki komutu çalıştırın:
   
        show tables;

## <a name="delete-an-edge-node"></a>Bir kenar düğümüne Sil
Azure portalından bir kenar düğümüne silebilirsiniz.

**Bir kenar düğümüne erişmek için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Hdınsight kümesi ile bir edge düğümünü açın.
3. Tıklatın **uygulamaları** küme dikey penceresinden. Edge düğüm listesi göreceksiniz.  
4. Silin ve ardından istediğiniz kenar düğümüne sağ **silmek**.
5. Onaylamak için **Evet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir kenar düğümüne eklemeyi ve kenar düğümüne erişim öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): HDInsight uygulamalarını kümelerinize yükleme hakkında bilgi alın.
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Resource Manager şablonları kullanarak HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.

