---
title: Azure Resource Manager vCPU Kotayı artırmak istekleri | Microsoft Docs
description: Azure Resource Manager vCPU kota artışı isteği
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: c22a6dde0067385a1bf8d889cc76178bb44dd0ac
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Resource Manager vCPU kota artışı isteği

Resource Manager vCPU kotaları bölge düzeyi ve SKU ailesi düzeyinde uygulanır.
Nasıl kotaları üzerinde uygulanır hakkında daha fazla bilgi [Azure aboneliği ve hizmet sınırları](http://aka.ms/quotalimits) sayfası.
SKU ailesi hakkında daha fazla bilgi için maliyet ve performans üzerinde karşılaştırmak [sanal makineler fiyatlandırma](http://aka.ms/pricingcompute) sayfası.

Artırma isteğinde bulunmak için Azure portalında Vcpu'lar bir kota destek servis talebi oluşturma [ https://portal.azure.com ](https://portal.azure.com).

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
