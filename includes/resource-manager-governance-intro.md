---
title: "include dosyası"
description: "include dosyası"
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 001bdf20f1d8756e63f15c68141aa415c000070e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
Kaynakları Azure'a dağıtırken, hangi tür kaynaklara dağıtmak için nerede olurlarsa olsunlar ve bunları nasıl ayarlanacağını karar verirken inanılmaz esneklik vardır. Ancak, bu esneklik, kuruluşunuzda izin vermek istiyor musunuz olandan daha fazla seçenek açabilirsiniz. Kaynaklar için Azure dağıtmayı göz önünde bulundurun gibi merak ediyor:

* Bazı ülkelerde veri egemenliği için yasal gereksinimlere nasıl karşılar?
* Maliyetleri nasıl denetlerim?
* Birisi yanlışlıkla bir kritik sistem değiştirmemesini nasıl emin olursunuz?
* Nasıl kaynak maliyetleri izlemek ve doğru şekilde faturalandırmak?

Bu makalede bu soruları yanıtlamaya yöneliktir. Özellikle:

* Kullanıcıları rollere atamak ve kullanıcıların beklenen Eylemler ancak olmayan diğer eylemler gerçekleştirmek için izni şekilde bir kapsama rolleri atayın.
* Böylece, kuruluşunuz için anlamlı değerlere göre izleyebilirsiniz kaynakları etiketleyin.
* Kaynakları için kuralları belgenizdeki ilkeleri uygulanır.
* Sisteminiz için kritik olan kaynaklar kilitleyin.

Bu makalede idare uygulamak için gerçekleştirmeniz görevlerde odaklanır. Kavramlar daha geniş bir tartışma için bkz: [azure'da idare](../articles/security/governance-in-azure.md). 
