---
title: Azure ile ilgili en iyi uygulama, Kubernetes dağıtımları denetlemek
description: En iyi uygulamaları dağıtımlarınızı kube-Danışman'ı kullanarak Azure Kubernetes hizmeti uygulaması için denetlenecek hakkında bilgi edinin
services: container-service
author: seanmck
ms.service: container-service
ms.topic: troubleshooting
ms.date: 11/05/2018
ms.author: seanmck
ms.openlocfilehash: 29f98e334b0d2527b5159e1a5394109c5041024a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465377"
---
# <a name="checking-for-kubernetes-best-practices-in-your-cluster"></a>Kubernetes kümenizdeki en iyi uygulamaları denetleme

Kubernetes dağıtımlarınızı en iyi performans ve uygulamalarınız için dayanıklılık sağlamak için uygulamanız gereken birkaç en iyi yöntemler vardır. Bu önerilere şu olmayan dağıtımlar için aranacak kube-Danışman Aracı'nı kullanabilirsiniz.

## <a name="about-kube-advisor"></a>Kube-advisor hakkında

[Kube-Danışman aracı] [ kube-advisor-github] , kümenizde çalıştırılmak üzere tasarlanan tek bir kapsayıcıdır. Bu, dağıtımları hakkında daha fazla bilgi için Kubernetes API sunucusu sorgular ve iyileştirme önerileri kümesini döndürür.

> [!NOTE]
> Kube-Danışman aracı, bir en iyi çaba ilkesine göre Microsoft tarafından desteklenir. Sorunları ve önerileri, Github'da Dosyalanan.

## <a name="running-kube-advisor"></a>Çalışan kube-Danışman

Aracı için yapılandırılmış bir kümede çalıştırmak için [rol tabanlı erişim denetimi (RBAC)](azure-ad-integration.md), aşağıdaki komutları kullanarak. İlk komut, bir Kubernetes hizmeti hesabı oluşturur. İkinci komut, bu hizmet hesabı'nı kullanarak bir pod içinde Aracı'nı çalıştırır ve bu çıktıktan sonra pod silme işlemi için yapılandırır. 

```bash
kubectl apply -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml

kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never --overrides="{ \"apiVersion\": \"v1\", \"spec\": { \"serviceAccountName\": \"kube-advisor\" } }"
```

RBAC kullanmıyorsanız, komutu aşağıdaki gibi çalıştırabilirsiniz:

```bash
kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never
```

Birkaç saniye içinde bir tablo dağıtımlarınız için olası geliştirmeleri görmeniz gerekir.

![Advisor Kube çıkış](media/kube-advisor-tool/kube-advisor-output.png)

## <a name="checks-performed"></a>Gerçekleştirilen denetler

Aracı çeşitli Kubernetes en iyi uygulamalar, her biri kendi önerilen düzeltme doğrular.

### <a name="resource-requests-and-limits"></a>Kaynak isteklerini ve sınırlar

Kubernetes destekler tanımlama [kaynak ister ve pod özellikleri sınırlar][kube-cpumem]. İstek, en düşük CPU ve bellek kapsayıcıyı çalıştırmak için gerekli tanımlar. En fazla CPU ve izin verilmesi bellek sınırını tanımlar.

Varsayılan olarak, hiçbir istek veya sınırları pod belirtimlerine ayarlanır. Bu, overscheduled düğümlerin ve kapsayıcıların starved için açabilir. Kube-Danışman aracı, pod'ların istek göndermeden vurgular ve kümesi kısıtlar.

## <a name="cleaning-up"></a>Temizleme

Etkin RBAC kümeniz varsa, temizleyebilirsiniz `ClusterRoleBinding` aşağıdaki komutu kullanarak aracı çalıştırdıktan sonra:

```bash
kubectl delete -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml
```

RBAC-etkin olmayan bir küme karşı araç çalıştırıyorsanız, temizlik gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kubernetes hizmeti ile ilgili sorunları giderme](troubleshooting.md)

<!-- RESOURCES -->

[kube-cpumem]: https://github.com/Azure/azure-quickstart-templates
[kube-advisor-github]: https://github.com/azure/kube-advisor