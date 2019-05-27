---
title: 'Hızlı başlangıç: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma | Microsoft Docs'
description: PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b3b23509197dd1d91dbd74c57b3c732151c51a97
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66142979"
---
# <a name="quickstart-create-a-server---powershell"></a>Hızlı Başlangıç: Sunucu oluşturma - PowerShell

Bu hızlı başlangıç, Azure aboneliğinizde bir Azure Analysis Services sunucusu oluşturmak için komut satırından PowerShell kullanmayı anlatmaktadır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**: Ziyaret [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/) hesap oluşturmak için.
- **Azure Active Directory**: Aboneliğinizin Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir ve bu dizinde bir hesabınızın olması gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).
- **Azure PowerShell**. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

## <a name="import-azanalysisservices-module"></a>Az.AnalysisServices modülünü içeri aktarın

Aboneliğinizde bir sunucu oluşturmak için kullandığınız [Az.AnalysisServices](/powershell/module/az.analysisservices) modülü. Az.AnalysisServices modülü PowerShell oturumunuza yükleyin.

```powershell
Import-Module Az.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

Azure aboneliğinizi kullanarak oturum açın [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) komutu. Ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

[Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md), Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sunucunuzu oluşturduğunuzda, aboneliğinizde bir kaynak grubu belirtmeniz gerekir. Bir kaynak grubu zaten yoksa kullanarak yeni bir tane oluşturabilirsiniz [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Aşağıdaki örnekte Batı ABD bölgesinde `myResourceGroup` adında bir kaynak grubu oluşturulmaktadır.

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

## <a name="create-a-server"></a>Sunucu oluşturma

Kullanarak yeni bir sunucu oluşturma [yeni AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver) komutu. Aşağıdaki örnek, Batı ABD bölgesinde D1 (ücretsiz) katmanında myResourceGroup’ta myServer adlı bir sunucu oluşturur ve sunucu yöneticisi olarak philipc@adventureworks.com adresini belirtir.

```powershell
New-AzAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location WestUS -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanarak sunucuyu aboneliğinizden kaldırabilirsiniz [Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver) komutu. Bu koleksiyondaki diğer hızlı başlangıçları ve öğreticileri kullanacaksanız sunucunuzu kaldırmayın. Aşağıdaki örnekte önceki adımda oluşturduğunuz sunucu kaldırılır.


```powershell
Remove-AzAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta PowerShell kullanarak Azure aboneliğinizde sunucu oluşturmayı öğrendiniz. Sunucuyu oluşturduğunuza göre bir sunucu güvenlik duvarı yapılandırarak (isteğe bağlı) güvenliğinin sağlanmasına yardımcı olacaksınız. Sunucunuza doğrudan portaldan örnek bir veri modeli de ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenme konusunda yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Sunucu güvenlik duvarını - Portal yapılandırma](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza bir örnek model ekleme](analysis-services-create-sample-model.md)
