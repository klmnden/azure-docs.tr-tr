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
ms.date: 01/16/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 39ab31cd06707dbd488914da248941ab6d174c29
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388766"
---
# <a name="app-service-environment-management-addresses"></a>App Service ortamı yönetim adresleri

App Service ortamı (ASE), Azure App Service, Azure sanal ağı (VNet) bir alt ağa dağıtımıdır.  ASE Azure App Service tarafından kullanılan yönetim düzlemi erişilebilir olması gerekir.  Bu ASE yönetim trafiği, kullanıcı tarafından denetlenen ağ erişir. Bu trafiğin engellendiği veya misrouted, ASE askıya alınır. ASE ağ bağımlılıklar hakkında daha fazla bilgi için okuma [önemli noktalar ve App Service ortamı ağ][networking]. İle başlayabilirsiniz ASE ile ilgili genel bilgiler için [App Service ortamı giriş][intro].

Tüm Ase'ler hangi yönetim trafiğini salesforce'taki genel bir VIP vardır. Bu adreslerden gelen yönetim trafiği 454 ve 455 bağlantı noktalarına ASE'nizi üzerindeki genel VIP geldiği. Bu belge, yönetim trafiği ASE için App Service kaynak adreslerini listeler. Bu hizmet etiketi AppServiceManagement adlı adresleridir.

Gelen yönetim trafiği, ASE için kilitlemek için ağ güvenlik gruplarında AppServiceManagement adlı hizmet etiketini kullanabilirsiniz.  

Aşağıda belirtildiği adresleri bir yol tablosu içinde yapılandırılabilir. Bu, burada yoksa bir asimetrik yönlendirme sorununu olabilir VNet tünelli bir zorla ASE'NİZDE işletim zaman önemlidir. Burada giden trafik gönderilir şirket içi bir ortamda çalışmak üzere ASE'nizi yapılandırma hakkında daha fazla ayrıntı için okuma [zorlamalı tünel ile ASE'nizi yapılandırma][forcedtunnel]

## <a name="list-of-management-addresses"></a>Yönetim adresleri listesi ##

| Bölge | Adresler |
|--------|-----------|
| Tüm genel bölgelerde | 70.37.57.58, 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246, 52.225.177.153, 65.52.172.237, 52.151.25.45, 40.124.47.188 |
| Microsoft Azure Kamu | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="configuring-a-network-security-group"></a>Bir ağ güvenlik grubu yapılandırma

Ağ güvenlik grupları ile tek tek adreslerini ya da kendi yapılandırma bakımını yapma konusunda endişelenmeniz gerekmez. Güncel tüm adreslerini tutulur AppServiceManagement adlı bir IP hizmet etiketi yoktur. Bu IP hizmet etiketi, NSG'de kullanmak için portala gidin ve ağ güvenlik grupları kullanıcı Arabirimi açın gelen güvenlik kuralları'nı seçin. Varsa gelen yönetim trafiği için önceden var olan bir kural bu düzenleyin. Bu NSG ile ASE'nizi oluşturulmamışsa ya da tüm yeni ise seçip **Ekle**. Kaynak açılan altında seçin **hizmet etiketi**.  Kaynak hizmet etiketi altında seçin ** AppServiceManagement **. Kaynak bağlantı noktası aralıkları kümesine \*, hedefe **herhangi**, hedef bağlantı noktası aralıkları için **454 455**, protokol **TCP**ve eyleme **izin ver** . Kural yapıyorsanız önceliğini ayarlamak gerekir. 

![Hizmet etiketi ile bir NSG oluşturma][1]

## <a name="configuring-a-route-table"></a>Bir yol tablosu yapılandırma

Yönetim adreslerini bir sonraki internet atlaması olan tüm gelen yönetim trafiğinin aynı yol ile geri dönmek erişebildiğinden emin olmak için bir rota tablosuyla yerleştirilebilir. Yapılandırma zorlamalı tünel, bu yollar gereklidir. Yol tablosu oluşturmak için portalı, PowerShell veya Azure CLI'yı kullanabilirsiniz.  Bir PowerShell isteminden Azure CLI kullanarak bir yönlendirme tablosu oluşturmak için komutları aşağıda verilmiştir. 

    $rg = "resource group name"
    $rt = "route table name"
    $location = "azure location"
    az network route-table create --name $rt --resource-group $rg --location $location
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.57.58 --next-hop-type Internet --address-prefix 70.37.57.58/32 
    az network route-table route create -g $rg --route-table-name $rt -n 157.55.208.185 --next-hop-type Internet --address-prefix 157.55.208.185/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.174.22.21 --next-hop-type Internet --address-prefix 52.174.22.21/32 
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.149.179 --next-hop-type Internet --address-prefix 13.94.149.179/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.143.126 --next-hop-type Internet --address-prefix 13.94.143.126/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.141.115 --next-hop-type Internet --address-prefix 13.94.141.115/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.195.197 --next-hop-type Internet --address-prefix 52.178.195.197/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.190.65 --next-hop-type Internet --address-prefix 52.178.190.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.184.149 --next-hop-type Internet --address-prefix 52.178.184.149/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.177.147 --next-hop-type Internet --address-prefix 52.178.177.147/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.75.127.117 --next-hop-type Internet --address-prefix 13.75.127.117/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.125.161 --next-hop-type Internet --address-prefix 40.83.125.161/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.121.56 --next-hop-type Internet --address-prefix 40.83.121.56/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.120.64 --next-hop-type Internet --address-prefix 40.83.120.64/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.56.50 --next-hop-type Internet --address-prefix 52.187.56.50/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.37 --next-hop-type Internet --address-prefix 52.187.63.37/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.59.251 --next-hop-type Internet --address-prefix 52.187.59.251/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.19 --next-hop-type Internet --address-prefix 52.187.63.19/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.158.140 --next-hop-type Internet --address-prefix 52.165.158.140/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.152.214 --next-hop-type Internet --address-prefix 52.165.152.214/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.154.193 --next-hop-type Internet --address-prefix 52.165.154.193/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.153.122 --next-hop-type Internet --address-prefix 52.165.153.122/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.255 --next-hop-type Internet --address-prefix 104.44.129.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.134.255 --next-hop-type Internet --address-prefix 104.44.134.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.243 --next-hop-type Internet --address-prefix 104.44.129.243/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.141 --next-hop-type Internet --address-prefix 104.44.129.141/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.188.65 --next-hop-type Internet --address-prefix 23.102.188.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 191.236.154.88 --next-hop-type Internet --address-prefix 191.236.154.88/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.64.115.203 --next-hop-type Internet --address-prefix 13.64.115.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.193.203 --next-hop-type Internet --address-prefix 65.52.193.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.89.222 --next-hop-type Internet --address-prefix 70.37.89.222/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.224.105.172 --next-hop-type Internet --address-prefix 52.224.105.172/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.135.246 --next-hop-type Internet --address-prefix 23.102.135.246/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.225.177.153 --next-hop-type Internet --address-prefix 52.225.177.153/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.172.237 --next-hop-type Internet --address-prefix 65.52.172.237/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.151.25.45 --next-hop-type Internet --address-prefix 52.151.25.45/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.124.47.188 --next-hop-type Internet --address-prefix 40.124.47.188/32

Yol tablosu oluşturulduktan sonra ASE alt ağınız ayarlamanız gerekir.  

## <a name="get-your-management-addresses-from-api"></a>API Yönetimi adreslerinizi Al ##

Aşağıdaki API çağrısı ile ASE'nizi için eşleşen yönetim adreslerine listeleyebilirsiniz.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

API, tüm gelen adreslerini ASE'NİZİN içeren bir JSON belgesini döndürür. Adresleri listesi, ASE'nizi ve ASE alt ağ adres aralığı kendisi tarafından kullanılan VIP yönetim adresleri içerir.  

API ile çağrılacak [armclient](https://github.com/projectkudu/ARMClient) aşağıdaki komutları kullanırsınız, ancak, abonelik kimliği, kaynak grubu ve ASE adını değiştirin.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!--IMAGES-->
[1]: ./media/management-addresses/managementaddr-nsg.png

<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
