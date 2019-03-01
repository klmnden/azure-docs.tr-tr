---
title: Azure Stack'te genel IP adresleri ekleme | Microsoft Docs
description: Daha fazla genel IP adresleri Azure Stack'e ekleme konusunda bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2019
ms.author: jeffgilb
ms.reviewer: scottnap
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 09805719262f0a1d30f3b38af4b5209667d25e5a
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57195379"
---
# <a name="add-public-ip-addresses"></a>Genel IP adresleri ekleme
*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*  

Daha fazla genel IP adresleri Azure Stack'e ekleme konusunda bilgi edinin.  Bu makalede, genel IP adresleri olarak dış adresleri diyoruz, ancak Azure Stack'te bu IP adresi blokları, dış ağa eklenmesi başvurmak için tasarlanmıştır.  Olup, dış ağ genel Internet yönlendirilebilir veya intranette yer ve özel adres alanınızı kullanır, bu makalenin amaçları önemli değildir.  Adımları aynıdır. 

## <a name="add-a-public-ip-address-pool"></a>Bir genel IP adres havuzu ekleme
Azure Stack sistemi ilk kez dağıtıldıktan sonra herhangi bir zamanda, genel IP adresleri, Azure Stack sisteminize ekleyebilirsiniz. İşlemlerini nasıl gerçekleştirebileceğinizi [görünümü genel IP adresi kullanımını](azure-stack-viewing-public-ip-address-consumption.md) hangi geçerli kullanım ve genel IP adresi görmek için Azure Stack kullanılabilirlik kümesidir.

Yüksek düzeyde, yeni bir genel IP adresi bloğu Azure Stack'e ekleme işlemini şu şekilde görünür:

 ![IP akışı Ekle](media/azure-stack-add-ips/flow.PNG)

## <a name="obtain-the-address-block-from-your-provider"></a>Sağlayıcınızdan adres bloğu
Yapmanız gereken ilk şey, Azure Stack için eklemek istediğiniz adres bloğu elde edilir.  Adresi bloğundan elde yere bağlı olarak, ne sağlama süresi olan ve bu, Azure Stack'te genel IP adresleri kullanan oranı karşı yönetme göz önünde bulundurmanız gerekir.  

> [!IMPORTANT]
> Azure Stack geçerli adresi bloğu ve Azure Stack'te var olan bir adres aralığı ile çakışmadığını sürece sağlayan, herhangi bir adres bloğu kabul eder.  Yönlendirilebilir ve Azure Stack bağlandığı dış ağ ile olmayan çakışan bir geçerli adresi bloğu elde emin olun.  Azure Stack'e aralığı ekledikten sonra kaldırılamaz.

## <a name="add-the-ip-address-range-to-azure-stack"></a>Azure Stack için IP adres aralığı Ekle

1. Bir Internet tarayıcısında yönetici portal panonuza gidin.  Bu örnekte, kullanacağız https://adminportal.local.azurestack.external.  
2.  Bir bulut işleci olarak Azure Stack yönetim portalında oturum açın.
3.  Varsayılan Panoda – bölge yönetim listesini bulmak ve yönetmek için bu örnekte, yerel istediğiniz bölgeyi seçin.
4.  Kaynak sağlayıcıları kutucuğu ve üzerinde ağ kaynak Sağlayıcısı'na tıklayın bulun.
5.  Genel IP tıklayarak kullanım kutucuğu havuzlara ayırır.
6.  Ekleme IP havuzu düğmesine tıklayın.
7.  IP havuzu için bir ad sağlayın.  Yalnızca istediğiniz gibi çağırabilmek IP havuzu kolayca belirlemenize izin vermesidir seçtiğiniz adı.  Adres aralığı ile aynı adı yapmak iyi bir uygulamadır, ancak bu gerekli değildir.
8.   CIDR gösteriminde eklemek istediğiniz adres bloğu girin.  Örneğin: 192.168.203.0/24
9.  Adres aralığı (CIDR bloğu) alanına başlangıç IP adresi geçerli bir CIDR aralığı sağladığınızda, bitiş IP adresi ve kullanılabilir IP adresi alanları otomatik olarak doldurulur.  Bunlar salt okunur ve bu adresi aralığı alanındaki değer değiştirmeden değiştiremezsiniz otomatik olarak oluşturulur.
10. Dikey penceresindeki bilgileri gözden geçirdikten ve her şeyi onayladıktan sonra görünen düzeltin, değişikliği kaydetmek ve adres aralığı Azure Stack'e ekleme için Tamam'a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar 
[Ölçek birimi düğüm eylemleri gözden geçirin](azure-stack-node-actions.md) 
