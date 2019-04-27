---
title: İlke şablonu örnekleri | Microsoft Docs
description: Sanal Ağ için Azure ilke şablonu örnekleri.
services: virtual-network
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: b0b3b65ee2360a5cec32235f2ee5bf8d4839cc8b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679621"
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Sanal ağ için Azure ilke şablonu örnekleri

Aşağıdaki tablo bağlantılarını içerir [Azure İlkesi](../governance/policy/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) örnekleri. Örnekler, [Azure İlkesi örnek deposunda](https://github.com/Azure/azure-policy) bulunur.

| | |
|---|---|
|**Ağ**||
| [Her NIC üzerinde NSG X](../governance/policy/samples/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Her sanal ağ arabirimiyle belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [Her alt ağ üzerinde NSG X](../governance/policy/samples/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Her sanal alt ağ ile belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [Yol tablosu yok](../governance/policy/samples/no-user-defined-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json)  |Sanal ağların bir yol tablosuyla dağıtılmasını yasaklar. |
| [VM ağ arabirimlerinde onaylanan alt ağı kullan](../governance/policy/samples/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Ağ arabirimlerinin onaylı bir alt ağ kullanmasını gerektirir. Onaylanan alt ağın kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan sanal ağı kullan](../governance/policy/samples/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz. |
|**İzleme**||
| [Tanılama ayarını denetle](../governance/policy/samples/audit-diagnostic-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Belirtilen kaynak türlerinde tanılama ayarlarının etkinleştirilip etkinleştirilmediğini denetler. Tanılama ayarlarının etkin olup olmadığını denetlemek için bir kaynak türü dizisi belirtirsiniz. |
|**Ad ve metin kuralları**||
| [Birden çok adı desenine izin ver](../governance/policy/samples/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Birçok ad deseninden birinin kaynaklarda kullanılmasına izin verir. |
| [Benzer desen gerektir](../governance/policy/samples/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Kaynak adlarının bir desen için *benzer* koşuluna uyduğundan emin olun. |
| [Eşleşme deseni gerektir](../governance/policy/samples/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Kaynak adlarının belirli bir adlandırma deseniyle eşleştiğinden emin olun. |
| [Etiket eşleştirme deseni gerektir](../governance/policy/samples/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Bir etiket değerinin metin deseniyle eşleştiğinden emin olun. |
|**Etiketler**||
| [Fatura etiketleri ilkesi girişimi](../governance/policy/samples/billing-tags-policy-initiative.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir. Gerekli etiketleri uygulamak ve zorlamak için yerleşik ilkeleri kullanır. Etiketler için gereken değerleri belirtin.  |
| [Etiket ve etiket değerini kaynak gruplara zorla](../governance/policy/samples/enforce-tag-on-resource-groups.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Kaynak grubunda bir etiket ve değer gerektirir. Gerekli etiket adını ve değerini belirtirsiniz.  |
| [Etiketi ve değerini zorla](../governance/policy/samples/enforce-tag-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Belirli bir etiket adı ve değeri gerektirir. Zorlamak için etiket adını ve değerini belirtirsiniz.  |
| [Etiketi ve varsayılan değerini uygula](../governance/policy/samples/apply-tag-default-value.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | İlgili etiket sağlanmadıysa belirli bir etiket adı ve değerini ekler. Uygulanacak etiket adını ve değerini belirtirsiniz.  |
|**Genel**||
| [İzin verilen konumlar](../governance/policy/samples/allowed-locations.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Tüm kaynakların uygun konumlara dağıtılmasını gerektirir. Onaylanan bir konum dizisi belirtirsiniz.  |
| [İzin verilen kaynak türleri](../governance/policy/samples/allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Yalnızca onaylanan kaynak türlerinin dağıtılmasını güvence altına alır. İzin verilen kaynak türü dizisini belirtirsiniz.  |
| [İzin verilmeyen kaynak türleri](../governance/policy/samples/not-allowed-resource-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Belirtilen kaynak türlerinin dağıtımını yasaklar. Engellenen kaynak türü dizisini belirtirsiniz.  |