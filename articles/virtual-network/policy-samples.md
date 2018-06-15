---
title: İlke şablonu örnekleri | Microsoft Docs
description: Sanal Ağ için Azure ilke şablonu örnekleri.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 05/02/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 2b8766a5353b015030872176e9032034afb7cb9d
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779592"
---
# <a name="azure-policy-sample-templates-for-virtual-network"></a>Sanal ağ için Azure ilke şablonu örnekleri

Aşağıdaki tabloda örnek [Azure İlkesi](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) şablonlarının bağlantıları verilir. Örnekler, [Azure İlkesi örnek deposunda](https://github.com/Azure/azure-policy) bulunur.

| | |
|---|---|
|**Ağ**||
| [Her NIC üzerinde NSG X](../azure-policy/scripts/nsg-on-nic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Her sanal ağ arabirimiyle belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [Her alt ağ üzerinde NSG X](../azure-policy/scripts/nsg-on-subnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Her sanal alt ağ ile belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [Yol tablosu yok](../azure-policy/scripts/no-user-def-route-table.md?toc=%2fazure%2fvirtual-network%2ftoc.json)  |Sanal ağların bir yol tablosuyla dağıtılmasını yasaklar. |
| [VM ağ arabirimlerinde onaylanan alt ağı kullan](../azure-policy/scripts/use-approved-subnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Ağ arabirimlerinin onaylı bir alt ağ kullanmasını gerektirir. Onaylanan alt ağın kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan sanal ağı kullan](../azure-policy/scripts/use-approved-vnet-vm-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz. |
|**İzleme**||
| [Tanılama ayarını denetle](../azure-policy/scripts/audit-diag-setting.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Belirtilen kaynak türlerinde tanılama ayarlarının etkinleştirilip etkinleştirilmediğini denetler. Tanılama ayarlarının etkin olup olmadığını denetlemek için bir kaynak türü dizisi belirtirsiniz. |
|**Ad ve metin kuralları**||
| [Birden çok adı desenine izin ver](../azure-policy/scripts/allow-multiple-name-patterns.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Birçok ad deseninden birinin kaynaklarda kullanılmasına izin verir. |
| [Benzer desen gerektir](../azure-policy/scripts/enforce-like-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Kaynak adlarının bir desen için *benzer* koşuluna uyduğundan emin olun. |
| [Eşleşme deseni gerektir](../azure-policy/scripts/enforce-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Kaynak adlarının belirli bir adlandırma deseniyle eşleştiğinden emin olun. |
| [Etiket eşleştirme deseni gerektir](../azure-policy/scripts/enforce-tag-match-pattern.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Bir etiket değerinin metin deseniyle eşleştiğinden emin olun. |
|**Etiketler**||
| [Fatura etiketleri ilkesi girişimi](../azure-policy/scripts/billing-tags-policy-init.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir. Gerekli etiketleri uygulamak ve zorlamak için yerleşik ilkeleri kullanır. Etiketler için gereken değerleri belirtin.  |
| [Etiket ve etiket değerini kaynak gruplara zorla](../azure-policy/scripts/enforce-tag-rg.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Kaynak grubunda bir etiket ve değer gerektirir. Gerekli etiket adını ve değerini belirtirsiniz.  |
| [Etiketi ve değerini zorla](../azure-policy/scripts/enforce-tag-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Belirli bir etiket adı ve değeri gerektirir. Zorlamak için etiket adını ve değerini belirtirsiniz.  |
| [Etiketi ve varsayılan değerini uygula](../azure-policy/scripts/apply-tag-def-val.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | İlgili etiket sağlanmadıysa belirli bir etiket adı ve değerini ekler. Uygulanacak etiket adını ve değerini belirtirsiniz.  |
|**Genel**||
| [İzin verilen konumlar](../azure-policy/scripts/allowed-locs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Tüm kaynakların uygun konumlara dağıtılmasını gerektirir. Onaylanan bir konum dizisi belirtirsiniz.  |
| [İzin verilen kaynak türleri](../azure-policy/scripts/allowed-res-types.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Yalnızca onaylanan kaynak türlerinin dağıtılmasını güvence altına alır. İzin verilen kaynak türü dizisini belirtirsiniz.  |
| [İzin verilmeyen kaynak türleri](../azure-policy/scripts/not-allowed-res-type.md?toc=%2fazure%2fvirtual-network%2ftoc.json) | Belirtilen kaynak türlerinin dağıtımını yasaklar. Engellenen kaynak türü dizisini belirtirsiniz.  |