---
title: "Bir Azure küme kaynaklarını silip | Microsoft Docs"
description: "Tamamen silmek için Service Fabric küme nasıl küme içeren kaynak grubunu silme veya kaynakları seçmeli silme tarafından öğrenin."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 7672aa12421fbe4ad86e7315d6a7a06c2ff5124d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Azure ve kaynaklarını Service Fabric kümesini Sil
Service Fabric kümesi, diğer Azure birçok kaynakları küme kaynağı yanı sıra oluşur. Bu nedenle, bir Service Fabric kümesini tamamen silmek için onu oluşturan tüm kaynakları da silmeniz gerekir.
İki seçeneğiniz vardır: ya da küme olan kaynak grubunu silin (silen küme kaynağı ve kaynak grubundaki kaynaklar) veya özel küme kaynağı siler ve ilişkili kaynakları (ancak diğer kaynakları değil kaynak grubu).

> [!NOTE]
> Küme kaynağı silme **yok** Service Fabric kümesi oluşan diğer tüm kaynakları silin.
> 
> 

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Service Fabric kümesinin tüm kaynak grubu (RG) silme
Bu, küme kaynak grubu dahil olmak üzere, ilişkili tüm kaynakları silmek emin olmak için en kolay yoludur. PowerShell kullanarak kaynak grubunu silebilirsiniz veya Azure portalı üzerinden. Kaynak grubunuz için Service fabric kümesi ilgili olmayan kaynakları varsa, belirli kaynaklara silebilirsiniz.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Azure PowerShell kullanarak kaynak grubunu silme
Ayrıca, aşağıdaki Azure PowerShell cmdlet'lerini çalıştırarak kaynak grubunu silebilirsiniz. Büyük bilgisayarınızda yüklü veya Azure PowerShell 1.0 emin olun. Daha önceden yapmadıysanız özetlenen adımları izleyin [nasıl yükleneceği ve Azure PowerShell yapılandırın.](/powershell/azure/overview)

Bir PowerShell penceresi açın ve aşağıdaki PS cmdlet'leri çalıştırın:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Kullanmadıysanız, silme işlemini onaylamak için bir istem alırsınız *-Force* seçeneği. Onay RG ve içerdiği tüm kaynaklar silinir.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Azure portalında bir kaynak grubu Sil
1. Oturum açma [Azure portal](https://portal.azure.com).
2. Silmek istediğiniz Service Fabric kümesine gidin.
3. Küme essentials sayfasında kaynak grubu adını tıklatın.
4. Bu işlem sonrasında **kaynak grubu Essentials** sayfası.
5. **Sil**'e tıklayın.
6. Kaynak grubunun silinmesi tamamlamak için bu sayfadaki yönergeleri izleyin.

![Kaynak Grubu Sil][ResourceGroupDelete]

## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Küme kaynağı ve kaynaklarını, ancak diğer kaynakları değil kaynak grubundaki Sil
Ardından, silmek istediğiniz Service Fabric kümesi ilişkili kaynakları kaynak grubunuz varsa, tüm kaynak grubunu silmek daha kolay olur. Kaynak grubunuzun kaynakları tek tek seçerek silmek istiyorsanız, aşağıdaki adımları izleyin.

Portalı kullanarak veya Şablon Galerisi'nden Service Fabric Resource Manager şablonları birini kullanarak kümenizi dağıtılmışsa, küme kullanan tüm kaynakları aşağıdaki iki etiketle etiketlenir. Silmek istediğiniz hangi kaynaklara karar vermek için bunları kullanabilirsiniz.

***#1 etiketi:*** anahtarı clusterName, değer = 'küme adı' =

***#2 etiketi:*** anahtarı KaynakAdı, değer = ServiceFabric =

### <a name="delete-specific-resources-in-the-azure-portal"></a>Azure Portalı'ndaki belirli kaynakları silin
1. Oturum açma [Azure portal](https://portal.azure.com).
2. Silmek istediğiniz Service Fabric kümesine gidin.
3. Git **tüm ayarları** essentials dikey.
4. Tıklayın **etiketleri** altında **kaynak yönetimi** ayarlar dikey penceresinde.
5. Aşağıdakilerden birini tıklatın **etiketleri** bu etikete sahip tüm kaynakların bir listesini almak için etiketleri dikey penceresinde.
   
    ![Kaynak Etiketleri][ResourceTags]
6. Etiketli kaynaklar listesine sahip olduktan sonra her bir kaynağın'ı tıklatın ve silebilirsiniz.
   
    ![Etiketli kaynakları][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Azure PowerShell kullanarak kaynakları silin
Aşağıdaki Azure PowerShell cmdlet'lerini çalıştırarak kaynakları tek tek silebilirsiniz. Büyük bilgisayarınızda yüklü veya Azure PowerShell 1.0 emin olun. Daha önceden yapmadıysanız özetlenen adımları izleyin [nasıl yükleneceği ve Azure PowerShell yapılandırın.](/powershell/azure/overview)

Bir PowerShell penceresi açın ve aşağıdaki PS cmdlet'leri çalıştırın:

```powershell
Login-AzureRmAccount
```
Her bir kaynağın silmek şu komutu çalıştırın istediğiniz:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Küme kaynağı silmek için şu komutu çalıştırın:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Sonraki adımlar
Ayrıca Küme yükseltme ve Hizmetleri bölümleme hakkında bilgi edinmek için aşağıdaki okuyun:

* [Küme yükseltme hakkında bilgi edinin](service-fabric-cluster-upgrade.md)
* [En fazla ölçek için durum bilgisi olan hizmetler bölümleme hakkında bilgi edinin](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
