---
title: "Azure CLI - Azure Hdınsight kullanarak Hadoop kümelerini yönetme | Microsoft Docs"
description: "Azure hdınsight'ta Hadoop kümelerini yönetmek için Azure komut satırı arabirimi kullanmayı öğrenin. Azure CLI, Windows, Mac ve Linux üzerinde çalışır."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: jgao
ms.openlocfilehash: 093042da6f7d51cec3111f073da0ce3a66f2cddc
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Azure CLI kullanarak hdınsight'ta Hadoop kümelerini yönetme
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Nasıl kullanacağınızı öğrenin [Azure komut satırı arabirimi](../cli-install-nodejs.md) Azure hdınsight'ta Hadoop kümelerini yönetmek için. Azure CLI, Node.js içinde uygulanmıştır. Windows, Mac ve Linux da dahil olmak üzere, Node.js'yi destekleyen herhangi bir platformda kullanılabilir. Hdınsight şu anda desteklememektedir [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).

Bu makalede, yalnızca Hdınsight ile Azure CLI kullanarak yer almaktadır. Azure CLI kullanma hakkında genel bir kılavuz için bkz: [yükleyin ve Azure CLI yapılandırma][azure-command-line-tools].

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure CLI** - Yükleme ve yapılandırma bilgileri için bkz. [Azure CLI'yı yükleme ve yapılandırma](../cli-install-nodejs.md).
* **Azure'a bağlanmak**, aşağıdaki komutu kullanarak:

    ```cli
    azure login
    ```
  
    Bir iş veya okul hesabı kullanarak kimlik doğrulama gerçekleştirme konusunda daha fazla bilgi için bkz. [Azure CLI'dan Azure aboneliğine bağlanma](/cli/azure/authenticate-azure-cli).
* Şu komutu kullanarak **Azure Resource Manager moduna geçin**:
  
    ```cli
    azure config mode arm
    ```

Yardım almak için kullanmak **-h** geçin.  Örneğin:

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>CLI ile kümeleri oluşturma
Bkz: [Azure CLI kullanarak Hdınsight'ta kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Liste ve küme ayrıntıları göster
Liste ve küme ayrıntıları görüntülemek için aşağıdaki komutları kullanın:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Komut satırı görünümünü küme listesi][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Küme silme
Bir küme silmek için aşağıdaki komutu kullanın:

```cli
azure hdinsight cluster delete <Cluster Name>
```

Ayrıca, küme içeren kaynak grubunu silerek bir küme silebilirsiniz. Lütfen unutmayın, bu varsayılan depolama hesabı dahil olmak üzere gruptaki tüm kaynakları siler.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Hadoop küme boyutunu değiştirmek için:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>Bir küme için HTTP erişimi etkinleştir/devre dışı bırak

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Bir küme için RDP erişimini etkinleştir/devre dışı bırak

```cli
azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
azure hdinsight cluster disable-rdp-access [options] <Cluster Name>
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, farklı Hdınsight küme yönetim görevlerini gerçekleştirmek öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight Azure Portalı'nı kullanarak yönetme][hdinsight-admin-portal]
* [Hdınsight Azure PowerShell kullanarak yönetme][hdinsight-admin-powershell]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Azure CLI kullanma][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Liste ve kümeleri Göster"
