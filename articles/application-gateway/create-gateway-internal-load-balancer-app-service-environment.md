---
title: Azure ile ILB ASE içindeki uygulama ağ geçidi sorunlarını giderme | Microsoft Docs
description: Azure App Service ortamı ile bir iç Load Balancer'ı kullanarak uygulama ağ geçidi sorunlarını gidermeyi öğrenin
services: vpn-gateway
documentationCenter: na
author: genlin
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2018
ms.author: genli
ms.openlocfilehash: baed2b23a321c53a614303d3085fbb3a4bf6ad0b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831104"
---
# <a name="back-end-server-certificate-is-not-whitelisted-for-an-application-gateway-using-an-internal-load-balancer-with-an-app-service-environment"></a>Arka uç sunucu sertifikası bir App Service ortamı ile bir iç Load Balancer'ı kullanarak uygulama ağ geçidi için izin verilenler listesinde değil.

Bu makalede aşağıdaki sorunu giderir: Azure'da uçtan uca SSL kullanılırken arka uçtaki bir iç yük dengeleyici (ILB) bir App Service ortamı (ASE) ile birlikte kullanarak bir uygulama ağ geçidi oluşturduğunuzda bir sertifika izin verilenler listesinde değil.

## <a name="symptoms"></a>Belirtiler

Bir ILB ASE arka kullanarak uygulama ağ geçidi oluşturduğunuzda, arka uç sunucu sağlıksız hale gelebilir. Kimlik doğrulama sertifikası uygulama ağ geçidinin arka uç sunucuda yapılandırılan sertifikaya eşleşmiyorsa Bu sorun oluşur. Aşağıdaki senaryoya örnek olarak bkz.

**Uygulama ağ geçidi yapılandırması:**

- **Dinleyici:** Çok siteli
- **Bağlantı noktası:** 443
- **Ana bilgisayar adı:** test.appgwtestase.com
- **SSL sertifikası:** CN=test.appgwtestase.com
- **Arka uç havuzu:** IP adresi veya FQDN
- **IP adresi:** : 10.1.5.11
- **HTTP ayarları:** HTTPS
- **Bağlantı noktası:** : 443
- **Özel araştırma:** Ana bilgisayar adı – test.appgwtestase.com
- **Kimlik doğrulama sertifikası:** test.appgwtestase.com, .cer
- **Arka uç sistem durumu:** Sağlıksız – arka uç sunucu sertifikası ile Application Gateway izin verilenler listesinde değil.

**ASE yapılandırma:**

- **ILB IP:** 10.1.5.11
- **Etki alanı adı:** appgwtestase.com
- **App Service:** test.appgwtestase.com
- **SSL bağlaması:** SNI SSL – CN=test.appgwtestase.com

Application gateway'e erişmek, arka uç sunucu uygun olmadığından aşağıdaki hata iletisini alıyorsunuz:

**502-web sunucusu bir ağ geçidi veya proxy sunucu olarak çalışırken geçersiz yanıt aldı.**

## <a name="solution"></a>Çözüm

Bir HTTPS Web sitesine erişmek için bir ana bilgisayar adı kullanmayın, arka uç sunucu SNI devre dışı durumda varsayılan Web sitesinde yapılandırılan sertifikaya döndürür. ILB ASE, ILB sertifikası varsayılan sertifika gelir. ILB için yapılandırılmış hiçbir sertifika yoksa sertifika ASE uygulama sertifikası gelir.

ILB erişmek için bir tam etki alanı adı (FQDN) kullandığınızda, arka uç sunucu HTTP ayarlarında karşıya yüklenen sertifikanın doğru döndürür. Diğer bir deyişle Aksi halde, aşağıdaki seçenekleri göz önünde bulundurun:

- FQDN, ILB IP adresine işaret edecek şekilde uygulama ağ geçidinin arka uç havuzunda kullanın. Bu seçenek, yalnızca özel bir DNS bölgesi varsa veya bir özel DNS yapılandırılmış çalışır. Aksi takdirde, bir "A" kaydı için bir genel DNS oluşturmak gerekir.

- Karşıya yüklenen sertifikanın ILB veya HTTP ayarlarında varsayılan sertifika (ILB sertifikası) kullanın. Araştırma için ILB'nin IP eriştiğinde uygulama ağ geçidi sertifikası alır.

- Böylece tüm Web siteleri için sertifika ortak ILB ile arka uç sunucu üzerinde bir joker sertifikası kullanın. Ancak, bu çözüm yalnızca alt etki alanlarını olması durumunda mümkündür ve Web sitelerinin her farklı bir ana bilgisayar adları gerektirir durumunda gerekli değildir.

- NET **App service için kullanım** ILB IP adresi kullanıyorsanız application gateway için seçenek.

Ek yükü azaltmak için iş yoklama yolu yapmak için HTTP ayarlarında ILB sertifikası yükleyebilirsiniz. (Bu adım yalnızca Güvenilenler listesine eklenmek için aynıdır. Bu SSL iletişimi için kullanılmayacak.) HTTPS üzerinden tarayıcınızdan ILB IP adresiyle erişerek ILB sertifikası alabilir, ardından Base-64 olarak SSL sertifikasını dışarı aktarma kodlanmış CER biçiminde ve ilgili HTTP ayarlarında sertifika karşıya yükleniyor.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
