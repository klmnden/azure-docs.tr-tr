---
title: Azure Application Gateway ile çok kiracılı genel bakış sona erer
description: Bu sayfada, Application Gateway’in çok kiracılı arka uç desteği için genel bir bakış sunulmuştur.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/1/2018
ms.author: victorh
ms.openlocfilehash: 8e5f48e42a2a677622dae0b733f9d5af484d1c51
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605815"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Çok kiracılı arka uçlar için Application Gateway desteği

Azure Application Gateway destekleyen sanal makine ölçek kümeleri, ağ arabirimleri, ortak/özel IP veya tam etki alanı adı (FQDN), kendi arka uç havuzlarının bir parçası olarak. Varsayılan olarak, application gateway istemciden gelen HTTP ana bilgisayar üst bilgisini değiştirmez ve üst bilgiyi değiştirilmemiş halde arka uca gönderir. Gibi çok sayıda hizmet vardır [Azure App Service](../app-service/overview.md) doğası gereği çok kiracılı ve özel ana bilgisayar üst bilgisini veya SNI uzantısını doğru uç noktaya çözümlemek için kullanır. Application Gateway, arka uç HTTP ayarlarına göre gelen HTTP ana bilgisayar üst bilgisini üzerine yazmak kullanıcılar için artık desteklemektedir. Bu özellik Azure Web Apps ve API Management uygulamalarında çok kiracılı arka uçlar için destek sağlar. Bu özellik hem standart hem de WAF SKU’sunda kullanılabilir. Çok kiracılı arka uç desteği, SSL sonlandırma ve uçtan uca SSL senaryoları ile de çalışır.

> [!NOTE]
> Kimlik doğrulama sertifikası Kurulumu güvenilir, Azure Web Apps gibi Azure Hizmetleri için gerekli değildir.

![web uygulaması senaryosu](./media/application-gateway-web-app-overview/scenario.png)

Bir ana bilgisayar geçersiz kılma işlemi belirtme olanağı HTTP ayarlarında tanımlanır ve kural oluşturma sırasında herhangi bir arka uç havuzuna uygulanabilir. Çok kiracılı arka uçlar, ana bilgisayar üst bilgisini ve SNI uzantısını aşağıdaki iki yöntemle geçersiz kılmayı destekler.

1. Ana bilgisayar adını HTTP ayarlarında sabit bir değere ayarlama. Bu özellik, ana bilgisayar üstbilgisi burada HTTP ayarlarının uygulandığı arka uç havuzu için tüm trafik için şu değer kılınır sağlar. Uçtan uca SSL kullanılırken, geçersiz kılınan bu ana bilgisayar adı SNI uzantısında kullanılır. Bu özellik, burada bir arka uç havuzu grubunun gelen müşteri ana bilgisayar üst bilgisinden farklı bir ana bilgisayar üst bilgisi beklediği senaryolara olanak sağlar.

2. IP veya FQDN arka uç havuzu üyelerinin ana bilgisayar adı türetme olanağını. HTTP ayarları ayrıca ana bilgisayar adı FQDN'den bir arka uç havuzu üyesinin bir bağımsız arka uç havuzu üyesinden ana bilgisayar adı türetme seçeneğiyle yapılandırdıysanız seçmek için bir seçenek sağlar. Uçtan uca SSL kullanılırken, bu ana bilgisayar adı FQDN’den türetilir ve SNI uzantısında kullanılır. Bu özellik, burada bir arka uç havuzunun Azure web apps gibi iki veya daha fazla çok kiracılı PaaS hizmetine sahip olabilir ve her üyesine isteğin ana bilgisayar üstbilgisi, FQDN'den türetilmiş ana bilgisayar adını içeren senaryolara olanak sağlar.

> [!NOTE]
> Önceki örneklerin her ikisinde de, ayarlar yalnızca canlı trafik davranışını etkiler ve sistem durumu araştırma davranışını etkilemez. Özel araştırmalar, araştırma yapılandırmasında bir ana bilgisayar üst bilgisi belirtme olanağını zaten desteklemektedir. Özel araştırmalar artık o anda yapılandırılmış HTTP ayarlarından ana bilgisayar üst bilgisi davranışını türetme olanağını da desteklemektedir. Bu yapılandırma, araştırma yapılandırmasındaki `PickHostNameFromBackendHttpSettings` parametresi kullanılarak belirtilebilir. Uçtan uca işlevselliğin çalışması için hem araştırma hem de HTTP ayarları doğru yapılandırmayı yansıtacak şekilde değiştirilmelidir.

Bu özellik sayesinde müşteriler, HTTP ayarları ve özel araştırmalardaki seçenekleri uygun yapılandırmaya göre belirtebilir. Bu ayar daha sonra bir dinleyici ve arka uç havuzu için bir kural kullanarak bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

Application gateway web uygulaması ile arka uç havuzu üyesi olarak ziyaret ayarlama işlemleri gerçekleştirmeyi öğreneceksiniz: [Application Gateway ile App Service web apps'ı yapılandırma](application-gateway-web-app-powershell.md)
