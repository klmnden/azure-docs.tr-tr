---
title: Diğer Azure Hizmetleri ile Azure DNS kullanma | Microsoft Docs
description: Azure DNS'yi diğer Azure Hizmetleri için adlarını çözümlemek için kullanmayı öğrenme
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: victorh
ms.openlocfilehash: dcf209d2036d2686bea0b51380db3cd2473d04a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293206"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS, diğer Azure hizmetleriyle nasıl çalışır?

Azure DNS bir barındırılan DNS Yönetimi ve ad çözümleme hizmetidir. Azure'da diğer uygulama ve dağıttığınız hizmetler için Genel DNS adları oluşturmak için kullanabilirsiniz. Özel etki alanınızı bir Azure hizmeti için bir ad oluşturmak kolaydır. Yalnızca hizmetiniz için doğru türde bir kayıt ekleyin.

* Dinamik olarak ayrılan IP adresleri için Azure hizmetiniz için oluşturulan DNS adına eşleyen bir DNS CNAME kaydı oluşturabilirsiniz. DNS standartları için bölge tepesinde CNAME kaydı kullanarak engeller. Bunun yerine bir diğer ad kaydı kullanabilirsiniz. Daha fazla bilgi için [Öğreticisi: Bir Azure genel IP adresi için başvuruda bulunmak için bir diğer ad kaydı yapılandırmak](tutorial-alias-pip.md).
* Statik olarak ayrılan IP adresleri için DNS A kaydı içeren herhangi bir ad kullanarak oluşturabileceğiniz bir *çıplak etki* bölgenin tepesindeki adı.

Aşağıdaki tabloda, çeşitli Azure Hizmetleri için kullanabileceğiniz desteklenen kayıt türleri özetlenmektedir. Tablonun gösterdiği gibi Azure DNS, Internet'e yönelik ağ kaynakları için yalnızca DNS kayıtları destekler. Azure DNS ad çözümlemesi için dahili, özel adres kullanılamaz.

| Azure hizmeti | Ağ arabirimi | Açıklama |
| --- | --- | --- |
| Azure Application Gateway |[Ön uç genel IP](dns-custom-domain.md#public-ip-address) |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. |
| Azure Load Balancer |[Ön uç genel IP](dns-custom-domain.md#public-ip-address) |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. Dinamik olarak atanan bir IPv6 genel IP adresini yük dengeleyici olabilir. Bir IPv6 adresi için bir CNAME kaydı oluşturun. |
| Azure Traffic Manager |Ortak ad |Traffic Manager profilinizin atanan trafficmanager.net adıyla eşleşen bir diğer ad kaydı oluşturabilirsiniz. Daha fazla bilgi için [Öğreticisi: Apex etki alanı adları ile Traffic Manager'ı desteklemek için bir diğer ad kaydı yapılandırmak](tutorial-alias-tm.md). |
| Azure Cloud Services |[Genel IP](dns-custom-domain.md#public-ip-address) |Statik olarak ayrılan IP adresleri için DNS A kaydı oluşturabilirsiniz. Dinamik olarak ayrılan IP adresleri için eşleyen bir CNAME kaydı oluşturmanız gerekir *cloudapp.net* adı.|
| Azure App Service | [Dış IP](dns-custom-domain.md#app-service-web-apps) |Dış IP adresleri için DNS A kaydı oluşturabilirsiniz. Aksi takdirde, azurewebsites.net adına eşleyen bir CNAME kaydı oluşturmanız gerekir. Daha fazla bilgi için [bir özel etki alanı adını bir Azure uygulamasına eşleme](../app-service/app-service-web-tutorial-custom-domain.md). |
| Azure Resource Manager Vm'lerinde |[Genel IP](dns-custom-domain.md#public-ip-address) |Resource Manager Vm'lerinde genel IP adresleri olabilir. Bir VM'nin genel IP adresine sahip yük dengeleyici arkasında da olabilir. DNS A, CNAME veya genel adresi için diğer ad kaydı oluşturabilirsiniz. Yük dengeleyicide VIP'ye atlamak için bu özel ad kullanabilirsiniz. |
| Klasik VM'ler |[Genel IP](dns-custom-domain.md#public-ip-address) |PowerShell veya CLI kullanılarak oluşturulan Klasik Vm'leri dinamik veya statik (ayrılmış) sanal adresi ile yapılandırılabilir. Bir DNS CNAME veya bir A kaydı sırasıyla oluşturabilirsiniz. |
