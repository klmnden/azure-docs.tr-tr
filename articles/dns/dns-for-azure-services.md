---
title: Diğer Azure Hizmetleri ile Azure DNS kullanma | Microsoft Docs
description: Azure DNS'yi diğer Azure Hizmetleri için ad çözümlemede kullanmak nasıl anlama
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
ms.openlocfilehash: 5d9886e16aa1921963f3d91d0fbd4da9287f7e54
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989153"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS, diğer Azure hizmetleriyle nasıl çalışır?

Azure DNS bir barındırılan DNS Yönetimi ve ad çözümleme hizmetidir. Bu, Azure'da Genel DNS adları için diğer uygulamalar ve dağıttığınız hizmetler oluşturmanıza olanak sağlar. Özel etki alanınızı bir Azure hizmeti için bir ad oluşturmak, hizmetiniz için doğru türde bir kayıt eklemek kadar kolaydır.

* Dinamik olarak ayrılan IP adresleri için Azure hizmetiniz için oluşturulan DNS adına eşleyen bir DNS CNAME kaydı oluşturabilirsiniz. DNS standartları için bölge tepesinde CNAME kaydı kullanarak engellemek, ancak bunun yerine bir diğer ad kaydı kullanın. Daha fazla bilgi için [öğretici: Azure genel IP adresine başvurmak için bir diğer ad kaydı yapılandırmak](tutorial-alias-pip.md).
* Statik olarak ayrılan IP adresleri için herhangi bir ad kullanarak bir DNS A kaydı oluşturabilirsiniz dahil olmak üzere bir *çıplak etki* bölgenin tepesindeki adı.

Aşağıdaki tabloda, çeşitli Azure Hizmetleri için kullanılabilir desteklenen kayıt türleri özetlenmektedir. Bu tabloda görebileceğiniz gibi Azure DNS yalnızca Internet'e yönelik ağ kaynakları için DNS kayıtlarını destekler. Azure DNS ad çözümlemesi için dahili, özel adres kullanılamaz.

| Azure Hizmeti | Ağ Arabirimi | Açıklama |
| --- | --- | --- |
| Application Gateway |[Ön uç genel IP](dns-custom-domain.md#public-ip-address) |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. |
| Load Balancer |[Ön uç genel IP](dns-custom-domain.md#public-ip-address)  |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. Yük Dengeleyici, dinamik olarak atanmış bir IPv6 genel IP adresi olabilir. Bu nedenle, bir IPv6 adresi için bir CNAME kaydı oluşturmanız gerekir. |
| Traffic Manager |Ortak ad |Traffic Manager profilinizin atanan trafficmanager.net adıyla eşleşen bir diğer ad kaydı oluşturabilirsiniz. Daha fazla bilgi için [öğretici: bir diğer ad kaydı apex etki alanı adları ile Traffic Manager'ı destekleyecek şekilde yapılandırmak](tutorial-alias-tm.md). |
| Bulut Hizmeti |[Genel IP](dns-custom-domain.md#public-ip-address) |Statik olarak ayrılan IP adresleri için DNS A kaydı oluşturabilirsiniz. Dinamik olarak ayrılan IP adresleri için eşleyen bir CNAME kaydı oluşturmanız gerekir *cloudapp.net* adı.|
| App Service | [Dış IP](dns-custom-domain.md#app-service-web-apps) |Dış IP adresleri için DNS A kaydı oluşturabilirsiniz. Aksi takdirde, azurewebsites.net adına eşleyen bir CNAME kaydı oluşturmanız gerekir. Daha fazla bilgi için [bir özel etki alanı adını bir Azure uygulamasına eşleme](../app-service/app-service-web-tutorial-custom-domain.md) |
| Resource Manager Vm'lerinde |[Genel IP](dns-custom-domain.md#public-ip-address) |Resource Manager Vm'lerinde genel IP adresleri olabilir. Genel IP adresine sahip bir VM, yük dengeleyici arkasında da olabilir. DNS A, CNAME veya genel adresi için diğer ad kaydı oluşturabilirsiniz. Bu özel ad yük dengeleyicide VIP'ye atlamak için kullanılabilir. |
| Klasik VM'ler |[Genel IP](dns-custom-domain.md#public-ip-address) |PowerShell kullanarak Klasik VM'ler oluşturulamaz veya CLI dinamik veya statik (ayrılmış) sanal adres ile yapılandırılabilir. Bir DNS CNAME veya bir kayıt sırasıyla oluşturabilirsiniz. |
