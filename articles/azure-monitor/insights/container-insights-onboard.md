---
title: Kapsayıcılar için Azure İzleyici'ı etkinleştirme | Microsoft Docs
description: Bu makalede, nasıl olanak açıklanmaktadır ve kapsayıcınızı nasıl performans gösterdiğini ve performans ile ilgili sorunları tanımlanan anlayabilmeniz kapsayıcılar için Azure İzleyicisi'ni yapılandırabilirsiniz.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: 5e149fa96e0a62656804906b52adf10273321d17
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65521910"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici'ı etkinleştirme  

Bu makalede Kubernetes ortamlara dağıtılmış ve barındırılan iş yüklerinin performansını izlemek için Azure İzleyici'kapsayıcıları için kurulumu için kullanılabilir seçenekleri'ne genel bakış sağlar [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/).

Kapsayıcılar için Azure İzleyici yeni etkinleştirilebilir ya da bir veya daha fazla var olan dağıtımları AKS aşağıdakileri kullanarak desteklenen yöntemleri:

* Azure portalı, Azure PowerShell veya Azure CLI ile
* Kullanarak [Terraform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar 
Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- **Log Analytics çalışma alanı.** Yeni AKS kümesini izlemeyi etkinleştirin veya AKS kümesi aboneliğin varsayılan kaynak grubunda bir varsayılan çalışma alanı oluşturma ekleme deneyimi sağlar, oluşturabilirsiniz. Kendiniz oluşturmayı seçerseniz, üzerinden oluşturabilirsiniz [Azure Resource Manager](../platform/template-workspace-configuration.md)temellidir [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../learn/quick-create-workspace.md).
- Üyesi olduğunuz **Log Analytics katkıda bulunan rolü** kapsayıcı izlemeyi etkinleştirmek için. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).
- Üyesi olduğunuz **[sahibi](../../role-based-access-control/built-in-roles.md#owner)** AKS kümesi Kaynak rolü. 

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Bileşenler 

Yeteneğinizi performansını izlemek için Azure İzleyici kapsayıcılar için özel olarak geliştirilmiş bir Linux kapsayıcı bir Log Analytics aracısını kullanır. Bu özel aracı kümedeki tüm düğümlerden performans ve olay verilerini toplar ve aracıyı otomatik olarak dağıtılır ve dağıtım sırasında belirtilen Log Analytics çalışma alanı ile kayıtlı. Aracı sürümü microsoft, / oms:ciprod04202018 ya da sonraki ve bir tarihi şu biçimde temsil edilir: *mmddyyyy*. 

>[!NOTE]
>AKS için Windows Server desteği Önizleme sürümü ile Windows Server düğümleri ile bir AKS kümesi veri toplamak ve iletmek için Azure İzleyici için bir aracı yok. Bunun yerine, otomatik olarak standart dağıtımının bir parçası olarak kümede dağıtılan bir Linux düğümü toplar ve kümedeki tüm düğümlerin Windows Azure İzleyici için veri adınıza iletir.  
>

Aracıyı yeni bir sürümü yayımlandığında, Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilir. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). 

>[!NOTE] 
>Bir AKS kümesi zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi Azure CLI'yı veya sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silmek, yeniden dağıtın veya aracıyı dağıtın. Şablon, aynı kaynak grubunda kümesi olarak dağıtılması gerekiyor.

Kapsayıcılar için Azure İzleyici aşağıdaki tabloda açıklanan aşağıdaki yöntemlerden birini kullanarak etkinleştirin.

| Dağıtım durumu | Yöntem | Açıklama | 
|------------------|--------|-------------| 
| Yeni AKS kümesi | [Azure CLI kullanarak küme oluşturma](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Azure CLI ile oluşturduğunuz yeni bir AKS kümesi izlemeyi etkinleştirebilirsiniz. | 
| | [Terraform kullanarak küme oluşturma](container-insights-enable-new-cluster.md#enable-using-terraform)| Terraform açık kaynak aracını kullanarak oluşturduğunuz yeni bir AKS kümesi izlemeyi etkinleştirebilirsiniz. | 
| Mevcut bir AKS kümesi | [Azure CLI kullanarak etkinleştirin](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Azure CLI kullanarak zaten dağıtılmış bir AKS kümesi izlemeyi etkinleştirebilirsiniz. | 
| |[Terraform kullanarak etkinleştirin](container-insights-enable-existing-clusters.md#enable-using-terraform) | Bir AKS kümesi açık kaynaklı araç Terraform kullanarak dağıtılmış izleme etkinleştirebilirsiniz. | 
| | [Azure İzleyici'den etkinleştir](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Azure İzleyici AKS çoklu küme sayfasından zaten dağıtılmış bir veya daha fazla AKS küme izleme etkinleştirebilirsiniz. | 
| | [AKS kümesi etkinleştir](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Azure portalında AKS kümesi doğrudan izlemeyi etkinleştirebilirsiniz. | 
| | [Bir Azure Resource Manager şablonu kullanarak etkinleştirin](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Önceden yapılandırılmış bir Azure Resource Manager şablonu ile bir AKS kümesi izlemeyi etkinleştirebilirsiniz. | 

## <a name="next-steps"></a>Sonraki adımlar

* AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
