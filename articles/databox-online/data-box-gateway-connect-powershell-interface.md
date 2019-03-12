---
title: Bağlanmak ve Microsoft Azure veri kutusu ağ geçidi aygıtı Windows PowerShell arabirimi üzerinden yönetmek | Microsoft Docs
description: Bağlanmak ve ardından veri kutusu ağ geçidi Windows PowerShell arabirimi üzerinden yönetmek açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 0ca570235ac2a87b62c5d0fcebbd24dc5186e37d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555221"
---
# <a name="manage-an-azure-data-box-gateway-device-via-windows-powershell"></a>Windows PowerShell aracılığıyla bir Azure veri kutusu ağ geçidi cihazı yönetme

Azure veri kutusu ağ geçidi çözüm, verileri ağ üzerinden Azure'a gönderme sağlar. Bu makalede, bazı veri kutusu ağ geçidi cihazınız için yapılandırma ve yönetim görevleri açıklanır. Azure portalı, yerel web kullanıcı Arabirimi veya Windows PowerShell arabiriminden Cihazınızı yönetmek için kullanabilirsiniz.

Bu makalede PowerShell arabirimini kullanarak bunu görevlere odaklanır.

Bu makalede, aşağıdaki yordamları içerir:

- PowerShell arabirimine bağlanma
- Bir destek oturumu başlatın
- Destek paketi oluşturma
- Sertifikayı karşıya yükleme
- DHCP olmayan ortamda önyükleme
- Cihaz bilgilerini görüntüle

> [!IMPORTANT]
> Azure veri kutusu ağ geçidi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanma

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="start-a-support-session"></a>Bir destek oturumu başlatın

[!INCLUDE [Connect to support runspace](../../includes/data-box-edge-gateway-connect-support.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="boot-up-in-non-dhcp-environment"></a>DHCP olmayan ortamda önyükleme

[!INCLUDE [Boot up in non-DHCP environment](../../includes/data-box-edge-gateway-boot-non-dhcp.md)]

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüle

[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.
