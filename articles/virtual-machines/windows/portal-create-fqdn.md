---
title: "Azure portalında bir Windows VM için FQDN oluşturmak | Microsoft Docs"
description: "FQDN, Azure portalında sanal makine için Resource Manager tabanlı veya tam etki alanı adının nasıl oluşturulacağını öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2d5a555cd873222efcdb29e8eb3aaf128a24414b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Bir tam etki alanı adını Azure portalında için bir Windows VM oluşturma

İçinde bir sanal makine (VM) oluşturduğunuzda [Azure portal](https://portal.azure.com), sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. VM uzaktan erişmek için bu IP adresi kullanın. Portal oluşturmaz rağmen bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), veya FQDN, oluşturabilmeniz VM oluşturulduktan sonra. Bu makalede, bir DNS adı veya FQDN oluşturmak için aşağıdaki adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>Bir FQDN oluşturma
Bu makale, bir VM zaten oluşturduğunuzu varsayar. Gerekirse, [portalda bir VM oluşturma](quick-create-portal.md) veya [Azure PowerShell ile](quick-create-powershell.md). VM çalışır durumda sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Şimdi uzaktan bu DNS adı gibi Uzak Masaüstü Protokolü (RDP) kullanarak VM bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bir ortak IP ve DNS adı, VM sahip, ortak uygulama çerçeveleri veya IIS, SQL ve SharePoint gibi hizmetleri dağıtabilirsiniz.

Ayrıca daha fazla hakkında bilgiyi [Kaynak Yöneticisi'ni kullanarak](../../azure-resource-manager/resource-group-overview.md) Azure dağıtımlarınızı oluşturma ipuçları için.

