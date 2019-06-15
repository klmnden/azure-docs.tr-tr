---
title: Azure portalında bir Linux VM için FQDN oluşturma | Microsoft Docs
description: FQDN, Azure portalında sanal makine için bir kaynak yöneticisi tabanlı veya tam etki alanı adı oluşturma öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 54d1f77823b982ee8f49122c46b0a01cb27390f2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62129426"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Bir tam etki alanı adını Azure portalında bir Linux VM'si için oluşturun.

İçinde bir sanal makine (VM) oluştururken [Azure portalında](https://portal.azure.com), sanal makine için bir genel IP kaynağı otomatik olarak oluşturulur. Bu IP adresi, sanal Makineye uzaktan erişmek için kullanın. Portal oluşturma değil ancak bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), veya FQDN, ekleyebileceğiniz bir VM oluşturulduktan sonra. Bu makalede, bir DNS adı veya FQDN oluşturma adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>Bir FQDN oluşturma
Bu makalede, bir VM zaten oluşturmuş olduğunuzu varsayar. Gerekirse, [portalda VM oluşturma](quick-create-portal.md) veya [Azure CLI ile](quick-create-cli.md). Sanal makinenizin çalışır duruma geldikten sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık uzaktan ile olduğu gibi bu DNS adını kullanarak VM'ye bağlanabilirsiniz `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Sonraki adımlar
VM'NİZİN genel bir IP ve DNS adı olan, ortak uygulama çerçeveleri veya ngınx, MongoDB, Docker gibi hizmetleri dağıtabilirsiniz vs.

Ayrıca daha fazla ilgili bilgi edinebilirsiniz [Kaynak Yöneticisi'ni kullanarak](../../azure-resource-manager/resource-group-overview.md) Azure dağıtımlarınızı oluşturma ipuçları için.

