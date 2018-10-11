---
title: Öğretici - Azure Service Fabric Mesh kaynaklarını temizleme | Microsoft Docs
description: Artık kullanmadığınız kaynaklara ücret ödememek için Azure Service Fabric Mesh kaynaklarını kaldırmayı öğrenin.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: fb7a444c54a57e7f2c38d941eb99f2fea7eebcef
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993366"
---
# <a name="tutorial-remove-azure-resources"></a>Öğretici: Azure kaynaklarını kaldırma

Bu öğretici bir serinin beşinci bölümüdür ve uygulamayla uygulamanın kaynaklarına ücret ödememeniz için bunları nasıl sileceğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Uygulama tarafından kullanılan kaynakları temizlerseniz bunlar için ücretlendirilmezsiniz.

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Visual Studio’da Service Fabric Mesh uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Yerel geliştirme kümenizde çalışan bir Service Fabric Mesh uygulamasının hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Service Fabric Mesh uygulaması dağıtma](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * [Service Fabric Mesh uygulamasını yükseltme](service-fabric-mesh-tutorial-upgrade.md)
> * Service Fabric Mesh kaynaklarını temizleme

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* To-Do uygulamasını dağıtmadıysanız, [Service Fabric Mesh web uygulamasını yayımlama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md) başlığı altında verilen yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticinin sonuna geldiniz. Oluşturduğunuz kaynaklarla işiniz bittiğinde, bunları silin. Böylece artık kullanmadığınız kaynaklar için ödeme yapmazsınız. Mesh saniye başına faturalandırılan sunucusuz bir hizmet olduğundan, bu özellikle önemlidir. Mesh fiyatlandırması hakkında daha fazla bilgi edinmek için https://aka.ms/sfmeshpricing bağlantısına bakın.

Azure'un sağladığı kolaylıklardan biri de, belirli bir kaynak grubuyla ilişkilendirilmiş kaynaklar oluşturduğunuzda, kaynak grubunu silmenin ilişkili tüm kaynakların da silinmesine neden olmasıdır. Bu yolla, bunları birer birer silmek zorunda kalmazsınız.

To-Do uygulamasını oluşturmak amacıyla yeni bir kaynak grubu oluşturduğunuz için, bu kaynak grubunu silerek ilgili tüm kaynakların da silinmesini sağlayabilirsiniz.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Alternatif olarak **sfmeshTutorial1RG** kaynak grubunu [portaldan](../azure-resource-manager/resource-group-portal.md#delete-resource-group-or-resources) silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Azure'da bir Service Fabric Mesh uygulaması yayımlamayı tamamladığınıza göre aşağıdakileri deneyebilirsiniz:

* Hizmetler arası iletişimin farklı bir örneğini görmek için [Oy verme uygulaması örneğini](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) inceleyin.
* Service Fabric Kaynak Modeli hakkında daha fazla bilgi için bkz. [Service Fabric Mesh Kaynak Modeli](service-fabric-mesh-service-fabric-resources.md).
* Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
* [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) hakkında bilgi edinin