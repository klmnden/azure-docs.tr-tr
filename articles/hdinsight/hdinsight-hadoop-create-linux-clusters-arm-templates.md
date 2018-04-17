---
title: Şablonları - Azure Hdınsight kullanarak Hadoop kümeleri oluşturma | Microsoft Docs
description: Resource Manager şablonları kullanarak için Hdınsight kümeleri oluşturma hakkında bilgi edinin
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: 1c50c3fbef5495171693b64e410f858af6176147
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Resource Manager şablonları kullanarak Hdınsight'ta Hadoop kümeleri oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu makalede, Azure Resource Manager şablonları kullanarak Azure Hdınsight kümeleri oluşturmak için çeşitli yollar hakkında bilgi edineceksiniz. Daha fazla bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma araçları ve özellikleri hakkında bilgi edinmek için bu sayfanın üst kısmındaki sekme seçicisini tıklatın veya bkz [küme oluşturma yöntemleri](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu makaledeki yönergeleri izleyin, gerekir:

* Bir [Azure aboneliği](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell ve/veya Azure CLI.

### <a name="resource-manager-templates"></a>Resource Manager şablonları
Resource Manager şablonu içinde tek ve eşgüdümlü bir işlemle uygulamanız için aşağıdaki resoruces oluşturmak kolay hale getirir:
* Hdınsight kümeleri ile bağımlı kaynakları (örneğin, varsayılan depolama hesabı)
* Diğer kaynaklar (örneğin, Apache Sqoop kullanmak için Azure SQL veritabanı)

Şablonda, uygulama için gereken kaynakları tanımlayın. Ayrıca, farklı ortamlar için değer girmesini dağıtım parametrelerini belirtin. JSON ve dağıtımınız için değerleri oluşturmak için kullandığınız deyimleri şablon oluşur.

Hdınsight şablon örnekleri konumunda bulabilirsiniz [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/?term=hdinsight). Platformlar arası kullanmak [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) ile [Resource Manager uzantısını](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) veya bir metin düzenleyicisi şablonu istasyonunuzda bir dosyaya kaydedin. 

Resource Manager şablonları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yazar Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md)
* [Bir uygulamayı Azure Resource Manager şablonları ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Şablonları oluştur

Resource Manager, farklı araçlar kullanılarak, aboneliğinizdeki mevcut kaynaklardan bir Resource Manager şablonu dışarı aktarmak sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

- Azure portal: bkz [mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma](../azure-resource-manager/resource-manager-export-template.md).
- Azure PowerShell: Bkz [PowerShell ile dışarı aktarma Azure Resource Manager şablonları](../azure-resource-manager/resource-manager-export-template-powershell.md).
- Azure CLI: Bkz [verme Azure Resource Manager şablonları Azure CLI ile](../azure-resource-manager/resource-manager-export-template-cli.md).


## <a name="deploy-using-the-portal"></a>Portalı kullanarak dağıtın

Azure Portalı'nı kullanarak bir Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için bkz: [özel şablon kaynaklardan dağıtmak](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="deploy-using-powershell"></a>PowerShell kullanarak dağıtın

Azure PowerShell kullanarak bir Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../azure-resource-manager/resource-group-template-deploy.md) ve [dağıtma özel Resource Manager şablonu SAS belirteci ve Azure PowerShell ile](../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="deploy-using-cli"></a>CLI kullanarak dağıtın

Azure CLI kullanarak bir Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](../azure-resource-manager/resource-group-template-deploy-cli.md) ve [dağıtma özel Resource Manager şablonu SAS belirteci ve Azure CLI ile](../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="deploy-using-the-rest-api"></a>REST API kullanarak dağıtma
REST API kullanarak bir Resource Manager şablonu dağıtabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Resource Manager REST API kaynaklarla dağıtmak](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Visual Studio ile dağıtma
 Bir kaynak grubu projesi oluşturun ve kullanıcı arabirimi aracılığıyla Azure'a dağıtmak için Visual Studio'yu kullanın. Projenize eklemek için kaynak türünü seçin. Bu kaynaklar, Resource Manager şablonu otomatik olarak eklenir. Proje şablonu dağıtmak için bir PowerShell betiğini de sağlar.

Visual Studio kaynak gruplarıyla kullanmaya giriş bilgileri için bkz: [Visual Studio üzerinden Azure kaynak gruplarını oluşturma ve dağıtma](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight kümesi oluşturmanın birkaç yolu öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* Daha fazla Hdınsight için ilgili şablonları için bkz: [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/?term=hdinsight).
* .NET istemci kitaplığını kaynaklarına dağıtma ilişkin bir örnek için bkz: [kaynakları .NET kitaplıkları ve bir şablon kullanarak dağıtmak](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bir uygulama dağıtımının ayrıntılı örneği için bkz: [sağlamak ve mikro beklendiği azure'da dağıtmak](../app-service/app-service-deploy-complex-application-predictably.md).
* Çözümünüzü farklı ortamlarda dağıtmaya yönelik kılavuz için bkz. [Microsoft Azure’da geliştirme ve test ortamları](../solution-dev-test-environments.md).
* Azure Resource Manager şablonu bölümleri hakkında bilgi edinmek için [şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).
* Bir Azure Resource Manager şablonunda kullanabileceğiniz işlevleri bir listesi için bkz: [şablon işlevleri](../azure-resource-manager/resource-group-template-functions.md).
