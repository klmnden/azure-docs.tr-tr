---
title: "Azure Resource Manager çekirdek Kotayı artırmak istekleri | Microsoft Docs"
description: "Azure Resource Manager çekirdek Kotayı artırmak istekleri"
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: cb6c5b3e86f126d4110d1cd29d8c9891e356e414
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resource-manager-core-quota-increase-requests"></a>Resource Manager çekirdek Kotayı artırmak istekleri

Resource Manager çekirdek kotalarını bölge düzeyi ve SKU ailesi düzeyinde uygulanır.
Nasıl kotaları üzerinde uygulanır hakkında daha fazla bilgi [Azure aboneliği ve hizmet sınırları](http://aka.ms/quotalimits) sayfası.
SKU ailesi hakkında daha fazla bilgi için maliyet ve performans üzerinde karşılaştırmak [sanal makineler fiyatlandırma](http://aka.ms/pricingcompute) sayfası.

Artırma isteğinde bulunmak için bir kota destek çekirdeği Azure portalında çalışmasının [https://portal.azure.com](https://portal.azure.com).

> [!NOTE]
> Bilgi edinmek için nasıl [bir destek isteği oluşturmak](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) Azure portalında

1. Sorun türü "Kota" olarak ve kota türü "Çekirdek" olarak yeni destek isteği sayfasındaki seçin.

    ![Kota temel bilgileri dikey penceresi](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. "Kaynak Yöneticisi" olarak dağıtım modeli seçin ve bir konum seçin.

    ![Kota sorun dikey penceresi](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Artışı gerektiren SKU ailesi seçin.

    ![Seçili SKU serisi](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. İstediğiniz yeni sınırları abonelikte girin.

    ![SKU yeni kota isteği](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Bir satırı kaldırmak için SKU SKU ailesi açılan kutusunun işaretini kaldırın veya atma "x" simgesine tıklayın.
Her SKU ailesi için istenen kota girdikten sonra destek isteği oluşturma işlemiyle devam etmek sorun adım sayfasında "İleri" yi tıklatın.
