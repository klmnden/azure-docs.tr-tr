---
title: İstemci Ip'lerine - Azure App Service kısıtlamak | Microsoft Docs
description: IP kısıtlamaları, Azure App Service ile kullanma
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/30/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: a152efb3979b4ffe3402ed668c0f683f5e9cc651
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337164"
---
# <a name="azure-app-service-static-ip-restrictions"></a>Azure App Service'e statik IP kısıtlamaları #

IP kısıtlamaları sıralı uygulamanıza erişmek için izin verilen IP adreslerini izin verme/reddetme listesi öncelikli tanımlamanızı sağlar. IPv4 ve IPv6 adresleri izin verilenler listesine ekleyebilirsiniz. Bir veya daha fazla olduğunda, örtük olarak reddetmek sonra listenin en sonunda bulunan tüm yoktur. 

IP kısıtlamaları özelliği içeren iş yükleri, App Service barındırılan tümüyle çalışır; Web apps, API apps, linux uygulamaları, linux kapsayıcı uygulamaları ve işlevleri. 

Uygulamanız için bir istek yapıldığında, gelen IP adresi IP kısıtlamaları listesine göre değerlendirilir. Adres listesinde yer alan kurallara göre erişim izin verilmiyorsa, hizmet ile yanıtlar bir [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) durum kodu.

IP kısıtlamaları özelliği, kodunuzun çalıştığı çalışan ana Yukarı Akış olan App Service ön uç rollerinde gerçekleştirilir. Bu nedenle, IP kısıtlamaları, etkili bir şekilde ağ ACL'leri altındadır.  

![IP kısıtlamaları akışı](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Bir süre için portalda IP kısıtlamaları özelliği IIS'de IPSecurity özelliği üzerinde bir katman oluştu. Geçerli IP kısıtlamaları özelliği farklıdır. İçinde uygulamanın web.config IPSecurity hala yapılandırabilirsiniz, ancak herhangi bir trafik IIS ulaşmadan önce tabanlı ön uç IP kısıtlamaları kuralları uygulanır.

## <a name="adding-and-editing-ip-restriction-rules-in-the-portal"></a>Ekleme ve düzenleme portalında IP kısıtlaması kuralları ##

Uygulamanız için bir IP kısıtlama kuralı eklemek, açmak için menü kullanın **ağ**>**IP kısıtlamaları** tıklayın **IP kısıtlamalarını Yapılandır**

![App Service ağ seçenekleri](media/app-service-ip-restrictions/ip-restrictions.png)  

IP kısıtlamaları Arabiriminden uygulamanız için tanımlanan IP kısıtlama kurallar listesini gözden geçirebilirsiniz.

![Liste IP kısıtlamaları](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Bu resimde göründüğü gibi kuralları yapılandırıldıysa, uygulamanız yalnızca 131.107.159.0/24 gelen trafiği kabul eder ve diğer bir IP adresinden reddedilir.

Tıklayabilirsiniz **[+] Ekle** yeni bir IP kısıtlama kuralı eklemek için. Bir kural eklediğinizde, hemen geçerli olur. Kurallar öncelik sırasına göre yukarı ve en düşük sayıdan başlayan uygulanır. Örtük Reddet tek bir kural eklediğinizde, geçerli tüm yoktur. 

![bir IP kısıtlama Kuralı Ekle](media/app-service-ip-restrictions/ip-restrictions-add.png)

IP adresi gösterimi, IPv4 ve IPv6 adresleri CIDR gösteriminde belirtilmelidir. Bir tam adresini belirtmek için burada IP adresiniz ilk dört sekizlik tabanda temsil eder ve özelliğini/32 maske 1.2.3.4/32 gibi kullanabilirsiniz. Tüm adresler için IPv4 CIDR gösteriminde 0.0.0.0/0 ' dir. CIDR gösterimi hakkında daha fazla bilgi edinebilirsiniz [sınıfsız etki alanları arası yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

Mevcut bir IP kısıtlama kuralı düzenlemek için herhangi bir satıra tıklayabilirsiniz. Düzenlemeler hemen öncelik sıralamada değişiklikleri içeren son derece etkilidir.

![bir IP kısıtlama kuralını Düzenle](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Bir kuralı silmek için tıklayın **...**  kural ve ardından **Kaldır**. 

![IP kısıtlama kuralını Sil](media/app-service-ip-restrictions/ip-restrictions-delete.png)

## <a name="programmatic-manipulation-of-ip-restriction-rules"></a>Programsal olarak işlenmesini IP kısıtlama kuralları ##

Şu anda herhangi bir CLI veya PowerShell için yeni IP kısıtlamaları özelliği olmakla birlikte değerlerini el ile Kaynak Yöneticisi'nde uygulama yapılandırması üzerindeki PUT işlemi sırasında ayarlanabilir. Örneğin, resources.azure.com kullanın ve gerekli JSON eklemek için ipSecurityRestrictions bloğu düzenleyin. 

Bu bilgiler Kaynak Yöneticisi'nde konumudur:

Management.Azure.com/subscriptions/**abonelik kimliği**/resourceGroups/**kaynak grupları**/providers/Microsoft.Web/sites/**web uygulaması adı**  /config/web? api sürümü 2018-02-01 =

Önceki örnek JSON sözdizimi aşağıdaki gibidir:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],
