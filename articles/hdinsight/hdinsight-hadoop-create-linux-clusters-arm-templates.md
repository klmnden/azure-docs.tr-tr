---
title: Şablonlar - Azure HDInsight'ı kullanarak Apache Hadoop kümeleri oluşturma
description: Resource Manager şablonları kullanarak için HDInsight kümeleri oluşturmayı öğrenin
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 73402421a87d2cf14719ff34201890ea96c90519
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715244"
---
# <a name="create-apache-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Resource Manager şablonlarını kullanarak HDInsight Apache Hadoop kümelerini oluşturun
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu makalede, Azure Resource Manager şablonları kullanarak Azure HDInsight kümeleri oluşturmak için birkaç yol öğreneceksiniz. Daha fazla bilgi için [Azure Resource Manager şablonu ile uygulama dağıtma](../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma araçları ve özellikleri hakkında bilgi edinmek için bu sayfanın üst kısmındaki sekme seçicisini tıklayın veya bakın [küme oluşturma yöntemleri](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu makalede bulunan yönergeleri takip etmek için ihtiyacınız vardır:

* Bir [Azure aboneliği](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell ve/veya Azure Klasik CLI.

### <a name="resource-manager-templates"></a>Resource Manager şablonları
Resource Manager şablonu tek ve eşgüdümlü bir işlemle uygulamanız için aşağıdaki kaynakları oluşturmak üzere kolaylaştırır:
* HDInsight kümeleri ve bağımlı kaynaklarını (örneğin, varsayılan depolama hesabı).
* Diğer kaynakları (kullanmak için Azure SQL veritabanı gibi [Apache Sqoop](https://sqoop.apache.org/)).

Şablonda, uygulama için gereken kaynakları tanımlayın. Ayrıca farklı ortamlar için değer girmek için dağıtım parametreleri belirtirsiniz. Şablonda, JSON ve dağıtımınız için değerleri oluşturmak için kullandığınız ifadeler bulunur.

HDInsight şablon örneği bulabilirsiniz [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?term=hdinsight). Platformlar arası kullanmak [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) ile [Resource Manager uzantısını](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) veya iş istasyonunuzda bir dosyaya şablonu kaydetmek için bir metin düzenleyicisi. 

Resource Manager şablonları hakkında daha fazla bilgi için aşağıdaki makaleleri ve örnekler bakın:

* [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonları ile uygulama dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Microsoft.HDInsight/clusters](/azure/templates/microsoft.hdinsight/allversions) şablon başvurusu
* [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Hdinsight&pageNumber=1&sort=Popular)

## <a name="generate-templates"></a>Şablon oluşturma

Resource Manager, farklı araçlar kullanarak aboneliğinizdeki mevcut kaynaklardan bir Resource Manager şablonunu dışarı aktarmak sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz. Daha fazla bilgi için [şablonları dışarı aktarma](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).

## <a name="deploy-using-the-portal"></a>Portalı kullanarak dağıtma

Azure portalını kullanarak Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için [özel şablondan kaynakları dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="deploy-using-powershell"></a>PowerShell kullanarak dağıtma

Azure PowerShell kullanarak Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md) ve [SAS belirteci ve Azure PowerShell ile özel Resource Manager şablonu dağıtma](../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtma

Klasik CLI kullanarak Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md) ve [SAS belirteci ve Azure CLI ile özel Resource Manager şablonu dağıtma](../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="deploy-using-the-rest-api"></a>REST API kullanarak dağıtma
REST API kullanarak bir Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Resource Manager REST API'si ile dağıtma](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Visual Studio ile dağıtma
 Bir kaynak grubu projesi oluşturma ve kullanıcı arabirimi aracılığıyla Azure'a dağıtmak için Visual Studio'yu kullanın. Projenize eklemek için kaynak türünü seçin. Bu kaynaklar için Resource Manager şablonu otomatik olarak eklenir. Proje şablonu dağıtmak için bir PowerShell betiğini de sağlar.

Visual Studio ile kaynak gruplarını kullanarak bir giriş için bkz. [Visual Studio aracılığıyla Azure kaynak gruplarını oluşturma ve dağıtma](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir HDInsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* Daha fazla HDInsight için ilgili şablonları görmek [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?term=hdinsight).
* .NET istemci kitaplığı aracılığıyla kaynak dağıtmaya ilişkin bir örnek için bkz [.NET kitaplıkları ve şablon kullanarak kaynakları dağıtma](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bir uygulama dağıtımının ayrıntılı örneği için bkz: [sağlayın ve tahmin edilebilir bir biçimde azure'da mikro hizmetlerin dağıtımı](../app-service/deploy-complex-application-predictably.md).
* Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](../solution-dev-test-environments.md).
* Azure Resource Manager şablonunun bölümleri hakkında bilgi edinmek için [şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Bir Azure Resource Manager şablonunda kullanabileceğiniz işlevleri bir listesi için bkz. [şablon işlevleri](../azure-resource-manager/resource-group-template-functions.md).
