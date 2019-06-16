---
title: Yeni bir Azure Kubernetes Service (AKS) kümesini izleme | Microsoft Docs
description: Yeni bir Azure Kubernetes Service (AKS) kümesi için Azure İzleyici ile kapsayıcıları abonelik için izlemeyi etkinleştirme hakkında bilgi edinin.
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
ms.openlocfilehash: d73ab2d5cca4f20f954a0b0e972111d3f395c3c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077538"
---
# <a name="enable-monitoring-of-a-new-azure-kubernetes-service-aks-cluster"></a>Yeni bir Azure Kubernetes Service (AKS) kümesini izlemeyi etkinleştir

Bu makalede, kapsayıcılar için Azure üzerinde barındırılan yönetilen Kubernetes kümesini izlemek için İzleyici ayarlanacağı açıklanır [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/) , aboneliğinizde dağıtmak hazırlanıyor.

Desteklenen yöntemlerden birini kullanarak bir AKS kümesi izleme etkinleştirebilirsiniz:

* Azure CLI
* Terraform

## <a name="enable-using-azure-cli"></a>Azure CLI kullanarak etkinleştirin

Azure CLI ile oluşturulan yeni bir AKS kümesi izlemeyi etkinleştirmek için hızlı başlangıç makalesinde bölümünde adım izleyin [oluşturma AKS kümesi](../../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.59 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

## <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirin

Eğer [Terraform kullanan yeni bir AKS kümesi dağıtma](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), profilinde gerekli bağımsız değişkenleri belirtmeniz [Log Analytics çalışma alanı oluşturmak için](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) değil seçerseniz, mevcut bir belirtmek. 

>[!NOTE]
>Terraform kullanmayı seçerseniz, Terraform'u Azure RM sağlayıcısı sürümü 1.17.0 çalıştırmalıdır veya üzeri.

Azure İzleyici kapsayıcılar için çalışma alanına eklemek için bkz [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) ve dahil ederek profilini tamamlayın [ **addon_profile** ](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) belirtin **oms_agent**. 

İzleme etkin ve tüm yapılandırma görevleri başarıyla tamamlandıktan sonra iki yöntemden biriyle kümenizin performansı izleyebilirsiniz:

* Seçerek doğrudan AKS kümesinde **sistem durumu** sol bölmesinde.
* Seçerek **İzleyici kapsayıcı öngörüleri** AKS kümesi sayfasında seçilen küme için bir kutucuk. Azure İzleyicisi'nde sol bölmede seçin **sistem durumu**. 

  ![AKS, kapsayıcılar için Azure İzleyici seçmek için seçenekleri](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="verify-agent-and-solution-deployment"></a>Aracı ve çözüm dağıtımı doğrulama
Aracı sürümü ile *06072018* veya daha sonra hem aracıyı hem de çözüm başarıyla dağıtıldı doğrulayabilirsiniz. Aracıyı önceki sürümleriyle birlikte, yalnızca aracı dağıtımı doğrulayabilirsiniz.

### <a name="agent-version-06072018-or-later"></a>Aracı 06072018 veya sonraki bir sürümü
Aracı başarıyla dağıtıldığını doğrulamak için aşağıdaki komutu çalıştırın. 

```
kubectl get ds omsagent --namespace=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Çözümün dağıtımı doğrulamak için aşağıdaki komutu çalıştırın:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Aracı sürümü 06072018 öncesi

Log Analytics aracı sürümü önce kullanıma sunduğunu doğrulamak için *06072018* düzgün bir şekilde aşağıdaki komutu çalıştırarak dağıtılır:  

```
kubectl get ds omsagent --namespace=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>CLI ile yapılandırmayı görüntüle
Kullanım `aks show` etkin çözüm olduğu gibi ayrıntılarını almak için komut, Log Analytics çalışma alanı ResourceId nedir ve küme hakkında özet ayrıntıları.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Birkaç dakika sonra komut tamamlanır ve JSON biçimli çözümü hakkında bilgi döndürür.  Komutun sonuçlarını izleme eklenti profilini göstermesi gerekir ve aşağıdaki örnek çıktıya benzer:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="next-steps"></a>Sonraki adımlar

* Çözüm ekleme girişimi sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](container-insights-troubleshoot.md)

* AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
