---
title: Azure uygulaması teknik varlıklarınızı oluşturun | Microsoft Docs
description: Bir Azure uygulaması teklif için teknik varlıkları oluşturun.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/13/2018
ms.author: pbutlerm
ms.openlocfilehash: a498fb2bc3efcc3a081a0ab854df107135a4e008
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744967"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Azure uygulaması teknik varlıklarınızı hazırlama

Bu makalede, Azure uygulama teklifiniz için teknik varlıkları hazırlığı kaynaklar açıklanır.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki videoyu inceleyin [yapı çözüm şablonları ve yönetilen uygulamalar, Azure Market'te](https://channel9.msdn.com/Events/Build/2018/BRK3603), bir Azure uygulama çözümü ve sonra nasıl tanımlamak için bir Azure Resource Manager şablonu yazmak nasıl bir genel bakış Daha sonra uygulama teklifi Azure Marketinde yayımlama.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


Hızlı Başlangıç kılavuzlarımız, Öğreticilerimiz ve Örneklerimizle sağlayan aşağıdaki Azure uygulama belgelerini inceleyin.

- [Azure Resource Manager şablonları anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- Hızlı Başlangıçlar:

  - [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/)
  - [Github'da Azure hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates)
  - [Uygulama tanımı yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
  - [Hizmet Kataloğu uygulaması dağıtma](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- Öğreticiler:

  - [Tanım dosyaları oluşturun](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Market uygulaması yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

  - Örnekler:

    - [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    - [Yönetilen uygulama çözümleri](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgiler

Tasarım, geliştirme ve test bu varlıkları zaman ayırın ve Azure platformu ve teklif oluşturmak için kullanılan teknolojileri teknik bilgi gerektirir.

Mühendislik ekibiniz, aşağıdaki Microsoft teknolojileri hakkında bilgi edinmeniz gerekir:

- Temel düzeyde bilinmesini [Azure Hizmetleri](https://azure.microsoft.com/services/)
- Nasıl yapılır [tasarlayabilir ve Azure uygulamaları tasarlama](https://azure.microsoft.com/solutions/architecture/)
- Bilgisine [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage), ve [Azure ağ iletişimi](https://azure.microsoft.com/services/?filter=networking)
- Bilgisine [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- Bilgisine [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Önerilen Araçlar

Birini veya ikisini de Azure uygulamanızı yönetmenize yardımcı olması için aşağıdaki komut dosyası ortamları seçin:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure CLI](https://docs.microsoft.com/cli/azure)

Aşağıdaki araçlar geliştirme ortamınızı eklemenizi öneririz:

- [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/) Aşağıdaki uzantılara sahip:

  - Dahili Hat: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Dahili Hat: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Dahili Hat: [JSON prettify](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Ayrıca araçlar da gözden geçirme öneririz [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfası ve Visual Studio kullanıyorsanız [Visual Studio Market](https://marketplace.visualstudio.com/).

## <a name="next-steps"></a>Sonraki adımlar

[Azure uygulama teklifi oluşturma](./cpp-create-offer.md)

