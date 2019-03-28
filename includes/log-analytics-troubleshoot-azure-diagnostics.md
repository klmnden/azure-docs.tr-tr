---
author: mgoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 11/09/2018
ms.author: magoedte
ms.openlocfilehash: 6890c71ac7c265d46cc77751786fea4d0b228588
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505662"
---
### <a name="troubleshoot-azure-diagnostics"></a>Azure Tanılama Sorunlarını Giderme

Aşağıdaki hata iletisini alırsanız, Microsoft.insights kaynak sağlayıcısı kayıtlı değildir:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Kaynak sağlayıcısını kaydetmek için Azure portalında aşağıdaki adımları uygulayın:

1.  Sol gezinti bölmesinde *Abonelikler*’e tıklayın
2.  Hata iletisinde belirtilen aboneliği seçin
3.  *Kaynak Sağlayıcıları*’ne tıklayın
4.  *Microsoft.insights* sağlayıcısını bulun
5.  *Kaydet* bağlantısına tıklayın

![Microsoft.insights kaynak sağlayıcısını kaydetme](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

*Microsoft.insights* kaynak sağlayıcısı kaydedildikten sonra tanılama yapılandırmasını yeniden deneyin.


PowerShell'de aşağıdaki hata iletisini alırsanız, PowerShell sürümünüz güncelleştirmeniz gerekir:

`Set-AzDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Azure PowerShell sürümünüzü güncelleştirin, yönergeleri [Azure PowerShell yükleme](/powershell/azure/install-az-ps) makalesi.
