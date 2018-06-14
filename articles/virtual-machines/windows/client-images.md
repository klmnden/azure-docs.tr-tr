---
title: Azure'da Windows istemci görüntüleri kullanmak | Microsoft Docs
description: Windows 7, Windows 8 veya Windows 10 azure'da geliştirme ve test senaryoları için dağıtmak için Visual Studio abonelik avantajlarını kullanma
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: aaab69f452db9d4f11af2b5cfd2cd9ff6ac79954
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
ms.locfileid: "28103685"
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Windows İstemcisi Azure üzerinde geliştirme ve test senaryoları için kullanın.
Uygun bir Visual Studio (önceki adıyla MSDN) aboneliğiniz varsa Azure geliştirme ve test senaryoları için Windows 10 Enterprise (x64) sağlanan veya Windows 7, Windows 8 kullanabilirsiniz. Bu makalede, Windows 7, Windows 8.1, Windows 10 Enterprise Azure ve aşağıdaki Azure galeri görüntüleri kullanımını çalıştırmak için uygunluk gereksinimleri özetlenmektedir.

![Azure portalından görüntü ayrıntıları](./media/client-images/windows-client-msdn-images.png) 

> [!NOTE]
> Azure Galerisi'ndeki Windows 10 Pro ve Windows 10 Pro N görüntü için lütfen [çok kullanıcılı barındırma hakları ile azure'da Windows 10 dağıtma](windows-desktop-multitenant-hosting-deployment.md)
>![Azure portalından Pro görüntü ayrıntıları](./media/client-images/windows-client-pro-images.png) 
>

## <a name="subscription-eligibility"></a>Abonelik uygunluk
Etkin Visual Studio abonelerinden (bir Visual Studio abonelik lisans kişiler) Windows İstemcisi geliştirme ve sınama amacıyla kullanabilirsiniz. Windows İstemcisi, kendi donanım ve herhangi bir türde Azure aboneliği çalışan Azure sanal makineler üzerinde kullanılabilir. Windows İstemcisi dağıtılan veya Azure üzerinde normal üretim kullanımı için kullanılan veya etkin Visual Studio abonelerinden olmayan kişiler tarafından kullanılan.

Size kolaylık olması için belirli Windows 10 görüntüleri içinde Azure galerisinden kullanılabilir [uygun geliştirme ve test sunar](#eligible-offers). Visual Studio abonelerinden teklif herhangi bir türde içinde için de [yeterli hazırlayın ve oluşturun](prepare-for-upload-vhd-image.md) bir 64-bit Windows 7, Windows 8 veya Windows 10 görüntüsü ve ardından [karşıya yüklemek için Azure](upload-generalized-managed.md). Kullanım geliştirme ve test için etkin Visual Studio aboneler tarafından sınırlı kalır.

## <a name="eligible-offers"></a>Uygun teklifleri
Aşağıdaki tabloda, Windows 10 Azure Galerisine dağıtmak uygun olan kimlikleri teklif ayrıntıları verilmektedir. Windows 10 görüntüler, yalnızca aşağıdaki teklifleri için görünür durumdadır. Windows İstemcisi farklı Teklif türü çalıştırmak için gereken visual Studio aboneleri gerektirir [yeterli hazırlayın ve oluşturun](prepare-for-upload-vhd-image.md) bir 64-bit Windows 7, Windows 8 veya Windows 10 görüntüsü ve [Azure'a yükleyin](upload-generalized-managed.md).

| Teklif Adı | Teklif Numarası | Kullanılabilir istemci görüntüleri |
|:--- |:---:|:---:|
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test uzmanı aboneleri](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium with MSDN (avantajı)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise aboneleri](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise Dev/Test](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Azure aboneliğiniz denetleyin
Teklif kimliği bilmiyorsanız, bu iki yoldan biriyle Azure Portalı aracılığıyla elde edebilirsiniz:  

- Üzerinde *abonelikleri* penceresi:

  ![Teklif kimliği ayrıntıları Azure portalından](./media/client-images/offer-id-azure-portal.png) 

- Veya tıklatın **faturalama** ve abonelik kimliğinizi tıklatın Teklif kimliği görünür *faturalama* penceresi.

Teklif kimliği de görüntüleyebilirsiniz ['Abonelik' sekmesini](http://account.windowsazure.com/Subscriptions) Azure hesap portalının:

![Azure hesap portalından Teklif kimliği ayrıntıları](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Sonraki adımlar
Kullanarak, Vm'lerde artık dağıtabilirsiniz [PowerShell](quick-create-powershell.md), [Resource Manager şablonları](ps-template.md), veya [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

