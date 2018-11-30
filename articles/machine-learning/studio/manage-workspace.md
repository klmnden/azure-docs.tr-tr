---
title: Machine Learning çalışma alanı - Azure Machine Learning Studio yönetme | Microsoft Docs
description: Azure Machine Learning çalışma alanları, erişimi yönetmek ve dağıtmak ve ML API web hizmetlerini yönetme
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: c470d10b933b31d4cfe151fb541c2182562a2805
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314113"
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanını yönetme

> [!NOTE]
> Machine Learning Web Hizmetleri portalında Web hizmetlerini yönetme hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md).
> 
> 

Azure Portal'da Machine Learning çalışma alanlarını yönetebilirsiniz.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalında bir çalışma alanı yönetmek için:

1. Oturum [Azure portalında](https://portal.azure.com/) bir Azure aboneliğinde yönetici hesabını kullanarak.
2. Sayfanın üst kısmındaki arama kutusuna "machine learning çalışma alanları" girin ve ardından **Machine Learning çalışma alanları**.
3. Yönetmek istediğiniz çalışma alanına tıklayın.

Standart kaynak yönetim bilgilerine ve seçeneklerin yanı sıra şunları yapabilirsiniz:

- Görünüm **özellikleri** - bu sayfa çalışma alanını ve kaynak bilgilerini görüntüler ve bu çalışma alanı ile bağlı abonelik ve kaynak grubunda değiştirebilirsiniz.
- **Depolama anahtarlarını yeniden eşitleme** -çalışma alanı depolama hesabı anahtarları tutar. Depolama hesabı anahtarı değiştirir sonra tıklayabilirsiniz **anahtarları yeniden eşitleme** anahtarları çalışma alanı ile eşitlenecek.

Bu çalışma alanıyla ilişkili web hizmetleri yönetmek için Machine Learning Web Hizmetleri portalını kullanın. Bkz: [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md) eksiksiz bilgi.

> [!NOTE]
> Yeni web hizmetleri ve dağıtımı, web hizmeti dağıtıldığı abonelik üzerinde katkıda bulunan veya yönetici rol atanması gerekir. Machine learning çalışma alanı için başka bir kullanıcı davet, dağıtmak veya web hizmetlerini yönetme önce bir abonelik üzerinde katkıda bulunan veya yönetici rolü atamanız gerekir. 
> 
>Erişim izinleri ayarlama hakkında daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Azure Resource Manager şablonları ile Machine Learning'i dağıtma](deploy-with-resource-manager-template.md). 