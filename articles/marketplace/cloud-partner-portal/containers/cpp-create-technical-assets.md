---
title: Azure Containers görüntüsünü teknik varlıklarınızı oluşturun | Azure Market
description: Azure kapsayıcısı için teknik varlıkları oluşturun.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: pabutler
ms.openlocfilehash: c639389fdd0d4624152fcdfa4432be09a18a97bc
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794336"
---
# <a name="prepare-your-container-technical-assets"></a>Teknik varlıkları kapsayıcınızı hazırlama

Bu makalede, adımları ve kapsayıcı teklifi Azure Marketi yapılandırma gereksinimlerini açıklar.

## <a name="before-you-begin"></a>Başlamadan önce

Gözden geçirme [Azure Container Instances](https://docs.microsoft.com/azure/container-instances) belgeleri, hızlı başlangıç kılavuzlarımız, Öğreticilerimiz ve Örneklerimizle sağlar.

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgiler

Tasarım, geliştirme ve test bu varlıkları zaman ayırın ve Azure platformu ve teklif oluşturmak için kullanılan teknolojileri teknik bilgi gerektirir.
 
Çözüm etki alanınız ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgilere sahip olmanız gerekir:

-   Temel düzeyde bilinmesini [Azure Hizmetleri](https://azure.microsoft.com/services/) 
-   Nasıl yapılır [tasarlayabilir ve Azure uygulamaları tasarlama](https://azure.microsoft.com/solutions/architecture/)
-   Bilgisine [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage) ve [Azure ağ iletişimi](https://azure.microsoft.com/services/?filter=networking)
-   Bilgisine [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
-   Bilgisine [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Önerilen Araçlar

Birini veya ikisini de kapsayıcı görüntünüzü yönetmenize yardımcı olması için aşağıdaki komut dosyası ortamları seçin:

-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [Azure CLI](https://docs.microsoft.com/cli/azure)

Ayrıca, aşağıdaki araçları, geliştirme ortamınızı eklemenizi öneririz:

-   [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Dahili Hat: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Dahili Hat: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Dahili Hat: [JSON prettify](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Ayrıca araçlar da gözden geçirme öneririz [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfası ve Visual Studio kullanıyorsanız [Visual Studio Market](https://marketplace.visualstudio.com/).

## <a name="create-the-container-image"></a>Kapsayıcı görüntüsü oluşturma

Daha fazla bilgi için aşağıdakilere bakın:

* [Öğretici: Azure Container Instances'a dağıtılacak için kapsayıcı görüntüsü oluşturma](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
* [Öğretici: Oluşturun ve kapsayıcı görüntülerini Azure Container kayıt defteri görevler ile bulutta dağıtın](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task)

## <a name="next-steps"></a>Sonraki adımlar

[Kapsayıcı teklifinizi oluşturun](./cpp-create-offer.md)
