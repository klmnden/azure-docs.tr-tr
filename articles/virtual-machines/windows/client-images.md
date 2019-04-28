---
title: Azure'da Windows istemci görüntülerini kullanma | Microsoft Docs
description: Windows 7, Windows 8 veya Windows 10'da Azure dev/test senaryoları için dağıtmak için Visual Studio abonelik avantajlarını kullanma
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: f791b17f2729af3efd2dff5d7884a168f8377154
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61403122"
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Windows istemci Azure'da geliştirme/test senaryoları için kullanın.
Azure'da geliştirme/test senaryoları için Windows 10 Enterprise (x64), uygun bir Visual Studio (eski adıyla MSDN) aboneliğiniz sağlanan ya da Windows 7, Windows 8, kullanabilirsiniz. Bu makalede, Windows 7, Windows 8.1, Windows 10 Enterprise, Azure ve Azure Galerisi şu görüntüleri kullanımını çalıştırmak için uygunluk gereksinimleri özetlenmektedir.

![Azure portalından görüntü ayrıntıları](./media/client-images/windows-client-msdn-images.png) 

> [!NOTE]
> Azure Galerisi'ndeki Windows 10 Pro ve Windows 10 Pro N görüntü için başvurun [çok Kiracılı barındırma hakları ile azure'da Windows 10 dağıtma](windows-desktop-multitenant-hosting-deployment.md)
>![Azure portalından Pro görüntü ayrıntıları](./media/client-images/windows-client-pro-images.png) 
>

## <a name="subscription-eligibility"></a>Abonelik uygunluk
Etkin Visual Studio aboneleri (Visual Studio Abonelik lisansı almış kişiler), Windows istemci geliştirme ve test amacıyla kullanabilirsiniz. Windows istemci donanım ve herhangi bir türde Azure aboneliğinde çalışan Azure sanal makineler kullanılabilir. Windows istemci dağıtılan veya normal üretim kullanımı için Azure üzerinde kullanılan veya etkin Visual Studio aboneleri olmayan kişiler tarafından kullanılır.

Kolaylık olması için belirli Windows 10 görüntüleri içinde Azure galerisinden kullanılabilir [uygun geliştirme ve test teklifleri](#eligible-offers). Visual Studio aboneleri teklif herhangi bir tür içinde aynı zamanda [yeterince hazırlayın ve oluşturun](prepare-for-upload-vhd-image.md) bir 64 bit Windows 7, Windows 8 veya Windows 10 görüntüsü ve ardından [karşıya Azure'a](upload-generalized-managed.md). Kullanımı, etkin Visual Studio aboneleri tarafından geliştirme ve test için sınırlı kalır.

## <a name="eligible-offers"></a>Uygun teklifler
Aşağıdaki tabloda, Azure Galerisi aracılığıyla Windows 10 dağıtmak uygun olan kimlikleri Teklif Ayrıntıları. Windows 10 görüntüleri, yalnızca aşağıdaki teklifler için görülebilir. Windows istemci farklı bir teklif tür çalıştırmak isteyen visual Studio aboneleri için gerekli [yeterince hazırlayın ve oluşturun](prepare-for-upload-vhd-image.md) bir 64 bit Windows 7, Windows 8 veya Windows 10 görüntüsü ve [sonra azure'a](upload-generalized-managed.md).

| Teklif Adı | Teklif Numarası | Kullanılabilir istemci görüntüleri |
|:--- |:---:|:---:|
| [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium with MSDN (avantaj)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise aboneleri](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Kurumsal geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Azure aboneliğinizi denetleyin
Teklif Kimliğinizi bilmiyorsanız, şu iki yoldan biriyle Azure portalından alabilirsiniz:  

- Üzerinde *abonelikleri* penceresi:

  ![Azure portalından Teklif kimliği ayrıntıları](./media/client-images/offer-id-azure-portal.png) 

- Veya tıklayın **faturalama** ve abonelik kimliğinizi'ye tıklayın Teklif kimliği görünür *faturalama* penceresi.

Teklif kimliği de görüntüleyebilirsiniz ['Subscriptions' sekmesinde](https://account.windowsazure.com/Subscriptions) Azure hesap Portalı'nın:

![Azure hesap Portalı'ndan Teklif kimliği ayrıntıları](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Sonraki adımlar
Şimdi kullanarak sanal makinelerinizi dağıtmak [PowerShell](quick-create-powershell.md), [Resource Manager şablonları](ps-template.md), veya [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

