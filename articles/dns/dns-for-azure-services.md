---
title: "Azure DNS kullanarak diğer Azure hizmetleriyle | Microsoft Docs"
description: "Azure DNS diğer Azure hizmetleriyle adı çözümlemek için nasıl kullanılacağını anlama"
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: kumud
ms.openlocfilehash: 6d052bc82c35aa3f2fdf5b5820e3901bd5c4080d
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS diğer Azure hizmetleriyle nasıl çalışır?

Azure DNS bir barındırılan DNS Yönetimi ve ad çözümleme hizmetidir. Bu, diğer uygulamalar ve hizmetler dağıttığınız ortak DNS adlarını Azure'da oluşturmanıza olanak sağlar. Özel etki alanınızda bir Azure hizmeti için bir ad oluşturma, hizmetiniz için doğru türde bir kayıt ekleme olarak kadar basittir.

* Dinamik olarak ayrılan IP adresleri için hizmetiniz için oluşturulan Azure DNS adıyla eşleşen bir DNS CNAME kaydı oluşturmanız gerekir. DNS standartları için bölge tepesinde CNAME kaydı kullanmanızı engelleyebilir.
* Statik olarak ayrılan IP adresleri için herhangi bir ad kullanarak bir DNS A kaydı oluşturabilirsiniz dahil olmak üzere bir *naked etki alanı* bölge tepesinde adresindeki adı.

Aşağıdaki tabloda, çeşitli Azure Hizmetleri için kullanılan desteklenen kayıt türleri özetlenmektedir. Bu tablodan görebileceğiniz gibi Azure DNS yalnızca Internet'e dönük ağ kaynakları için DNS kayıtlarını destekler. Azure DNS, iç, özel adresler ad çözümlemesi için kullanılamaz.

| Azure Hizmeti | Ağ Arabirimi | Açıklama |
| --- | --- | --- |
| Application Gateway |[Ön uç genel IP](dns-custom-domain.md#public-ip-address) |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. |
| Load Balancer |[Ön uç genel IP](dns-custom-domain.md#public-ip-address)  |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. Yük Dengeleyici dinamik olarak atanmış bir IPv6 genel IP adresine sahip olabilir. Bu nedenle, bir IPv6 adresi için bir CNAME kaydı oluşturmanız gerekir. |
| Traffic Manager |Ortak ad |Yalnızca trafik Yöneticisi profilinize atanan trafficmanager.net adıyla eşleşen bir CNAME oluşturabilirsiniz. Daha fazla bilgi için bkz: [trafik Yöneticisi nasıl çalışır](../traffic-manager/traffic-manager-overview.md#traffic-manager-example). |
| Bulut Hizmeti |[Genel IP](dns-custom-domain.md#public-ip-address) |Statik olarak ayrılan IP adresleri için bir DNS A kaydı oluşturabilirsiniz. Dinamik olarak ayrılan IP adresleri için eşleşen bir CNAME kaydı oluşturmanız gerekir *cloudapp.net* adı.|
| App Service | [Dış IP](dns-custom-domain.md#app-service-web-apps) |Dış IP adresleri için bir DNS A kaydı oluşturabilirsiniz. Aksi takdirde azurewebsites.net adıyla eşleşen bir CNAME kaydı oluşturmanız gerekir. Daha fazla bilgi için bkz: [bir Azure uygulaması için bir özel etki alanı adı eşleme](../app-service/app-service-web-tutorial-custom-domain.md) |
| Kaynak Yöneticisi Vm'leri |[Genel IP](dns-custom-domain.md#public-ip-address) |Kaynak Yöneticisi Vm'leri genel IP adresleri olabilir. Bir genel IP adresine sahip bir VM, bir yük dengeleyicinin arkasına de olabilir. Ortak adres için bir DNS A veya CNAME kaydı oluşturabilirsiniz. Bu özel ad, yük dengeleyici VIP atlamak için kullanılabilir. |
| Klasik VM'ler |[Genel IP](dns-custom-domain.md#public-ip-address) |PowerShell kullanarak Klasik VM'ler oluşturulduktan veya CLI dinamik veya statik (ayrılmış) sahip sanal bir adresi yapılandırılabilir. Bir DNS CNAME veya bir kayıt sırasıyla oluşturabilirsiniz. |
