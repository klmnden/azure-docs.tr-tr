---
title: Uzantıları - Azure'ı kullanarak dağıtım sonrası yapılandırmaları sağlamak | Microsoft Docs
description: Dağıtım sonrası yapılandırmaları sağlamak için Azure Resource Manager Şablonu Uzantıları'nı kullanmayı öğrenin.
services: azure-resource-manager
documentationcenter: na
author: mumian
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: eb46966c3a28b3fa4c2b23668109b7c5d23a609b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60390877"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Dağıtım sonrası yapılandırmaları uzantılarını kullanarak sağlayın.

Şablon, Azure kaynaklarında dağıtım sonrası yapılandırma ve otomasyon görevleri sunan küçük uygulamalar uzantılarıdır. En popüler sanal makine uzantıları olur. Bkz: [sanal makine uzantıları ve özellikleri Windows için](../virtual-machines/extensions/features-windows.md), ve [Linux yönelik sanal makine uzantılarına ve özelliklerine](../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Uzantılar

Mevcut uzantılar şunlardır:

- [Microsoft.Compute/virtualMachines/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets ve uzantıları](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight kümeleri ve uzantıları](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters/extensions)
- [Microsoft.Sql sunucuları ve veritabanları/uzantıları](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Kullanılabilir uzantıları bulmak için Gözat [şablon başvurusu](https://docs.microsoft.com/azure/templates/). İçinde **başlığa göre filtreleme**, girin **uzantısı**.

Bu uzantıları kullanmayı öğrenmek için bkz:

- [Öğretici: Sanal makine uzantıları Azure Resource Manager şablonları ile dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md).
- [Öğretici: Azure Resource Manager şablonları ile SQL BACPAC dosyalarını içeri aktarın](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Sanal makine uzantıları Azure Resource Manager şablonları ile dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md)
