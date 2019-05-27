---
title: Azure Red Hat OpenShift sorunlarını giderme | Microsoft Docs
description: Azure Red Hat OpenShift ile sık karşılaşılan sorunları çözmek ve giderme
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: 82678091c1d0b71e6209f6d03e9d1a0ca60fe03e
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65992167"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift için sorun giderme

Bu makalede, oluşturma veya Microsoft Azure Red Hat OpenShift kümelerini yönetirken karşılaşılan bazı yaygın sorunlar açıklanmaktadır.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Başarısız bir küme oluşturma yeniden deneniyor

Bir Azure Red Hat OpenShift oluşturma kullanarak küme varsa `az` CLI komutu başarısız olursa, yeniden deneniyor oluşturma devam başarısız.
Kullanım `az openshift delete` başarısız kümeyi silmek için ardından tamamen yeni bir küme oluşturun.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Gizli Azure Red Hat OpenShift küme kaynağı grubu

Şu anda `Microsoft.ContainerService/openShiftManagedClusters` Azure CLI tarafından otomatik olarak oluşturulan kaynak (`az openshift create` komut) Azure portalında gizlenir. İçinde **kaynak grubu** görünümü, onay **gizli türleri Göster** kaynak grubunu görüntülemek için.

![Portalda gizli türü onay kutusunun ekran görüntüsü](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Küme kaynak sağlayıcısı bulunamadı hatası sonuçları oluşturma

Bir küme sonuçları, bir hata oluşturuyorsanız `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview`, önizlemesinin bir parçası olan ve artık gerek [satın alma Azure sanal makine örnekleri ayrılmış](https://aka.ms/openshift/buy) sunuldu ürünü kullanmak için. Rezervasyon azaltır, tam olarak yönetilen Azure Hizmetleri için önceden ödeme yaparak ayırın. Başvurmak [ *Azure ayırmaları nelerdir* ](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) ayırmaları ve nasıl bunlar tasarruf hakkında daha fazla bilgi edinmek için.

## <a name="next-steps"></a>Sonraki adımlar

- Deneyin [Red Hat OpenShift Yardım Merkezi](https://help.openshift.com/) OpenShift sorun giderme daha fazla açık.

- Bul yanıtlar [Azure Red Hat OpenShift hakkında sık sorulan sorular](openshift-faq.md).
