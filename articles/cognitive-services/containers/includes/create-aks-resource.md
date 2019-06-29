---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Bir azure kubernetes (AKS) kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1d68c08f6dfca74c38973af1686d614f3f10cc28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455081"
---
## <a name="create-an-azure-kubernetes-service-aks-cluster-resource"></a>Bir Azure Kubernetes Service (AKS) kümesi kaynağı oluşturun

1. Git [oluşturma](https://ms.portal.azure.com/#create/microsoft.aks) Kubernetes Hizmetleri.

1. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Uygun aboneliği seçin|
    |Kaynak grubu|Kullanılabilir kaynak grubu seçin|
    |Kubernetes küme adı|İstenen ad (küçük)|
    |Bölge|Yakın bir konum seçin|
    |Kubernetes sürümü|1.12.8 (varsayılan)|
    |DNS adı ön eki|Otomatik olarak oluşturulur, ancak isteğe bağlı olarak geçersiz kılabilirsiniz|
    |Düğüm boyutu|Standart DS2 v2'de:<br>`2 vCPUs`, `7 GB`|
    |Düğüm sayısı|Kaydırıcı varsayılan değerde bırakın|

1. Üzerinde **ölçek** sekmesinde *sanal düğümü* ve *sanal makine ölçek kümeleri* varsayılan değerler.
1. Üzerinde **kimlik doğrulaması** sekmesinde *hizmet sorumlusu* ve *etkinleştirme RBAC* varsayılan değerler.
1. Üzerinde **ağ** sekmesinde, aşağıdaki seçimleri girin:

    |Ayar|Değer|
    |--|--|
    |HTTP uygulaması yönlendirme|Hayır|
    |Ağ yapılandırması|Temel|

1. Üzerinde **izleme** sekmesinde, emin *kapsayıcı izlemeyi etkinleştir* ayarlanır **Evet** bırakıp *Log Analytics çalışma alanı* olarak kendi Varsayılan değer
1. Üzerinde **etiketleri** sekmesinde, ad/değer çiftleri şimdilik boş bırakın.
1. Tıklayın **gözden geçir ve Oluştur**.
1. Doğrulama denetimini geçtikten tıklayın **Oluştur**.

> [!NOTE]
> Doğrulama başarısız olursa, son olabilir bir *hizmet sorumlusu* hata. Geri gidin *kimlik doğrulaması* sekmesine ve ardından tekrar *gözden geçir + Oluştur* nerede doğrulama yürütün ve sonra geçirin.
