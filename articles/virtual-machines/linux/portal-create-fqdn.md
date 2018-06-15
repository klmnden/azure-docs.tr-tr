---
title: Azure portalında bir Linux VM için FQDN oluşturma | Microsoft Docs
description: FQDN, Azure portalında sanal makine için Resource Manager tabanlı veya tam etki alanı adının nasıl oluşturulacağını öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81182f5a03821dc42aed2390d9a0145460b1ddd7
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
ms.locfileid: "26707295"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Bir tam etki alanı adını Azure portalında için bir Linux VM oluşturun.

İçinde bir sanal makine (VM) oluşturduğunuzda [Azure portal](https://portal.azure.com), sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. VM uzaktan erişmek için bu IP adresi kullanın. Portal oluşturmaz rağmen bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), veya FQDN, ekleyebileceğiniz bir VM oluşturulduktan sonra. Bu makalede, bir DNS adı veya FQDN oluşturmak için aşağıdaki adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>Bir FQDN oluşturma
Bu makale, bir VM zaten oluşturduğunuzu varsayar. Gerekirse, [portalda bir VM oluşturma](quick-create-portal.md) veya [Azure CLI ile](quick-create-cli.md). VM çalışır durumda sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Şimdi uzaktan ile bu DNS adı gibi kullanarak VM bağlanabileceğiniz `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Sonraki adımlar
VM'yi bir ortak IP ve DNS adı olan, yaygın uygulama çerçeveleri veya nginx, MongoDB, Docker, gibi hizmetleri dağıtabilirsiniz vs.

Ayrıca daha fazla hakkında bilgiyi [Kaynak Yöneticisi'ni kullanarak](../../azure-resource-manager/resource-group-overview.md) Azure dağıtımlarınızı oluşturma ipuçları için.

