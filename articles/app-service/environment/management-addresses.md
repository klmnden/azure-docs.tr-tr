---
title: App Service ortamı yönetim adresleri - Azure
description: App Service ortamı komutu için kullanılan yönetim adresleri listeler
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 7fb39886b19a2229188821eb39d4fb8a5928bb43
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276698"
---
# <a name="app-service-environment-management-addresses"></a>App Service ortamı yönetim adresleri

App Service ortamı (ASE), Azure App Service, Azure sanal ağı (VNet) bir alt ağa dağıtımıdır.  ASE Azure App Service tarafından kullanılan yönetim düzlemi erişilebilir olması gerekir.  Bu ASE yönetim trafiği, kullanıcı tarafından denetlenen ağ erişir. Bu trafiğin engellendiği veya misrouted, ASE askıya alınır. ASE ağ bağımlılıklar hakkında daha fazla bilgi için okuma [önemli noktalar ve App Service ortamı ağ][networking]. İle başlayabilirsiniz ASE ile ilgili genel bilgiler için [App Service ortamı giriş][intro].

Bu belge, yönetim trafiği ASE için App Service kaynak adreslerini listeler ve iki önemli amaca hizmet eder.  

1. Bu adresler, gelen trafiği kilitlemek için ağ güvenlik grupları oluşturmak için kullanabilirsiniz.  
2. Zorlamalı tünel dağıtımlarını desteklemek için bu adresleri ile yollar oluşturabilirsiniz. Burada giden trafik gönderilir şirket içi bir ortamda çalışmak üzere ASE'nizi yapılandırma hakkında daha fazla ayrıntı için okuma [zorlamalı tünel ile ASE'nizi yapılandırma][forcedtunnel]

Tüm Ase'ler hangi yönetim trafiğini salesforce'taki genel bir VIP vardır. Bu adreslerden gelen yönetim trafiği 454 ve 455 bağlantı noktalarına ASE'nizi üzerindeki genel VIP geldiği.  

## <a name="list-of-management-addresses"></a>Yönetim adresleri listesi ##

| Bölge | Adresler |
|--------|-----------|
| Tüm genel bölgelerde | 70.37.57.58 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246 |
| Microsoft Azure kamu (Fairfax veya MAG) | 23.97.29.209 13.72.53.37, 13.72.180.105 23.97.0.17, 23.97.16.184 |

## <a name="get-your-management-addresses-from-api"></a>API Yönetimi adreslerinizi Al ##

Aşağıdaki API çağrısı ile ASE'nizi için eşleşen yönetim adreslerine listeleyebilirsiniz.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

API, tüm gelen adreslerini ASE'NİZİN içeren bir JSON belgesini döndürür. Adresleri listesi, ASE'nizi ve ASE alt ağ adres aralığı kendisi tarafından kullanılan VIP yönetim adresleri içerir.  

API ile çağrılacak [armclient](https://github.com/projectkudu/ARMClient) aşağıdaki komutları kullanırsınız, ancak, abonelik kimliği, kaynak grubu ve ASE adını değiştirin.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
