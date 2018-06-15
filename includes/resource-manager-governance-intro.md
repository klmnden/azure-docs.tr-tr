---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2c16e82ccf259a4cc5ae8fcf35b2dd6b5d50ee2d
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
ms.locfileid: "29528346"
---
Kaynakları Azure'a dağıtırken, hangi tür kaynaklara dağıtmak için nerede olurlarsa olsunlar ve bunları nasıl ayarlanacağını karar verirken inanılmaz esneklik vardır. Ancak, bu esneklik, kuruluşunuzda izin vermek istiyor musunuz olandan daha fazla seçenek açabilirsiniz. Kaynaklar için Azure dağıtmayı göz önünde bulundurun gibi merak ediyor:

* Bazı ülkelerde veri egemenliği için yasal gereksinimlere nasıl karşılar?
* Maliyetleri nasıl denetlerim?
* Birisi yanlışlıkla bir kritik sistem değiştirmemesini nasıl emin olursunuz?
* Nasıl kaynak maliyetleri izlemek ve doğru şekilde faturalandırmak?

Bu makalede bu soruları yanıtlamaya yöneliktir. Özellikle:

> [!div class="checklist"]
* Kullanıcıları rollere atamak ve kullanıcıların beklenen Eylemler ancak olmayan diğer eylemler gerçekleştirmek için izni şekilde bir kapsama rolleri atayın.
* Kaynakları için kuralları belgenizdeki ilkeleri uygulanır.
* Sisteminiz için kritik olan kaynaklar kilitleyin.
* Böylece, kuruluşunuz için anlamlı değerlere göre izleyebilirsiniz kaynakları etiketleyin.

Bu makalede idare uygulamak için gerçekleştirmeniz görevlerde odaklanır. Kavramlar daha geniş bir tartışma için bkz: [azure'da idare](../articles/security/governance-in-azure.md). 
