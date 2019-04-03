---
title: İstemci Ip'lerine - Azure App Service kısıtlamak | Microsoft Docs
description: Azure App Service ile erişim kısıtlamaları kullanma
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
ms.openlocfilehash: 805de614246028bc75268e83991fa7831b990325
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882336"
---
# <a name="azure-app-service-static-access-restrictions"></a>Azure App Service'e statik erişim kısıtlamaları #

Erişim kısıtlamalarını sıralı uygulamanıza erişmek için izin verilen IP adreslerini izin verme/reddetme listesi öncelikli tanımlamanızı sağlar. IPv4 ve IPv6 adresleri izin verilenler listesine ekleyebilirsiniz. Bir veya daha fazla olduğunda, örtük olarak reddetmek sonra listenin en sonunda bulunan tüm yoktur.

Erişim kısıtlamaları özelliği içeren iş yükleri, App Service barındırılan tümüyle çalışır; Web apps, API apps, Linux uygulamaları, Linux kapsayıcı uygulamaları ve işlevleri.

Uygulamanız için bir istek yapıldığında, gelen IP adresi erişim sınırlamaları listesine göre değerlendirilir. Adres listesinde yer alan kurallara göre erişim izin verilmiyorsa, hizmet ile yanıtlar bir [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) durum kodu.

Erişim kısıtlamaları özelliği, kodunuzun çalıştığı çalışan ana Yukarı Akış olan App Service ön uç rollerinde gerçekleştirilir. Bu nedenle, erişim kısıtlamaları etkili bir şekilde ağ ACL'leri altındadır.  

![erişim kısıtlamaları akışı](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Bir süre için erişim kısıtlamaları özelliği portalında IPSecurity özelliği IIS üzerinde bir katman oluştu. Geçerli erişim kısıtlamaları özelliği farklıdır. İçinde uygulamanın web.config IPSecurity hala yapılandırabilirsiniz, ancak herhangi bir trafik IIS ulaşmadan önce ön uç tabanlı erişim kısıtlamalarını kuralları uygulanır.

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Ekleme ve erişimi kısıtlama kuralları Portalı'nda düzenleme ##

Uygulamanız için bir erişim kısıtlama kuralı eklemek, açmak için menü kullanın **ağ**>**erişim kısıtlamalarını** tıklayın **erişim kısıtlamalarını yapılandırma**

![App Service ağ seçenekleri](media/app-service-ip-restrictions/ip-restrictions.png)  

Erişim kısıtlamaları Arabiriminden uygulamanız için tanımlanan erişim kısıtlama kuralları listesini gözden geçirebilirsiniz.

![Liste erişim kısıtlamaları](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Bu resimde göründüğü gibi kuralları yapılandırıldıysa, uygulamanız yalnızca 131.107.159.0/24 gelen trafiği kabul eder ve diğer bir IP adresinden reddedilir.

Tıklayabilirsiniz **[+] Ekle** yeni bir erişim kısıtlama kuralı eklemek için. Bir kural eklediğinizde, hemen geçerli olur. Kurallar öncelik sırasına göre yukarı ve en düşük sayıdan başlayan uygulanır. Örtük Reddet tek bir kural eklediğinizde, geçerli tüm yoktur.

![bir erişim kısıtlama Kuralı Ekle](media/app-service-ip-restrictions/ip-restrictions-add.png)

IP adresi gösterimi, IPv4 ve IPv6 adresleri CIDR gösteriminde belirtilmelidir. Bir tam adresini belirtmek için burada IP adresiniz ilk dört sekizlik tabanda temsil eder ve özelliğini/32 maske 1.2.3.4/32 gibi kullanabilirsiniz. Tüm adresler için IPv4 CIDR gösteriminde 0.0.0.0/0 ' dir. CIDR gösterimi hakkında daha fazla bilgi edinebilirsiniz [sınıfsız etki alanları arası yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

Mevcut bir erişim kısıtlama kuralı düzenlemek için herhangi bir satıra tıklayabilirsiniz. Düzenlemeler hemen öncelik sıralamada değişiklikleri içeren son derece etkilidir.

![bir erişim kısıtlama kuralını Düzenle](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Bir kuralı silmek için tıklayın **...**  kural ve ardından **Kaldır**.

![erişimi kısıtlama kuralını Sil](media/app-service-ip-restrictions/ip-restrictions-delete.png)

Sonraki sekme dağıtım erişimi de kısıtlayabilirsiniz. Ekleme/düzenleme/silme için her kural, yukarıdakilerle aynı adımı izleyin.

![Liste erişim kısıtlamaları](media/app-service-ip-restrictions/ip-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programsal olarak erişim kısıtlama kuralları ##

Şu anda herhangi bir CLI veya PowerShell yeni erişim kısıtlamaları özelliği için olmakla birlikte değerlerini el ile Kaynak Yöneticisi'nde uygulama yapılandırması üzerindeki PUT işlemi sırasında ayarlanabilir. Örneğin, resources.azure.com kullanın ve gerekli JSON eklemek için ipSecurityRestrictions bloğu düzenleyin.

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
