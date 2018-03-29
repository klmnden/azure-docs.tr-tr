---
title: Azure Resource Manager vCPU Kotayı artırmak istekleri | Microsoft Docs
description: Azure Resource Manager vCPU kota artışı isteği
author: ganganarayanan
ms.author: gangan
ms.date: 3/15/2018
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: a42fa8e4e8dae140db4fcc8977bda335455b97a1
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Resource Manager vCPU kota artışı isteği

Resource Manager vCPU kotaları bölge düzeyi ve SKU ailesi düzeyinde uygulanır.
Nasıl kotaları üzerinde uygulanır hakkında daha fazla bilgi [Azure aboneliği ve hizmet sınırları](http://aka.ms/quotalimits) sayfası.
SKU ailesi hakkında daha fazla bilgi için maliyet ve performans üzerinde karşılaştırmak [sanal makineler fiyatlandırma](http://aka.ms/pricingcompute) sayfası.

Artırma isteğinde bulunmak için Azure portalında Vcpu'lar bir kota destek servis talebi oluşturma [ https://portal.azure.com ](https://portal.azure.com).

> [!NOTE]
> Bilgi edinmek için nasıl [bir destek isteği oluşturmak](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) Azure portalında

1. Seçin **abonelikleri**.

   ![Abonelikler](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Artan bir kota gereken aboneliği seçin.

   ![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seçin **kullanım + kotaları**

   ![Kullanım ve kotaları seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşedeki seçin **isteği artış**.

   ![Artışı isteği](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seçin **çekirdek** Teklif türü. 

   ![Formu doldurun](./media/resource-manager-core-quotas-request/forms.png)