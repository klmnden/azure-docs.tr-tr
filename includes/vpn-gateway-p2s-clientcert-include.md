---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.openlocfilehash: 7ae3886db6391836cd8d281e44c95c5253cc8dd5
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323890"
---
Noktadan siteye bağlantı ile sanal ağdan sanal ağa bağlanan her istemci bilgisayara bir istemci sertifikası yüklü olmalıdır. Kök sertifikadan oluşturur ve her istemci bilgisayara yükleyin. Geçerli bir istemci sertifikası yüklemezseniz, istemci sanal ağa bağlanmaya çalıştığında, kimlik doğrulaması başarısız olur.

Her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemci için aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, kimliğini doğrulamak için birden çok istemci aynı istemci sertifikasını kullanmak ve sertifikayı iptal, oluşturmanız ve bu sertifikayı kullanan her istemci için yeni sertifikalar yüklemeniz gerekir.

Aşağıdaki yöntemleri kullanarak istemci sertifikaları oluşturabilirsiniz:

- **Kurumsal sertifika:**

  - Kurumsal bir sertifika çözümü kullanıyorsanız, ortak ad değer biçimiyle bir istemci sertifikası oluşturma *name@yourdomain.com*. Yerine bu biçimi kullanmak *etki alanı adı\kullanıcı adı* biçimi.
  - İstemci sertifikasına sahip bir kullanıcı sertifikası şablonunu temel emin *istemci kimlik doğrulaması* kullanıcı listesindeki ilk öğe olarak listelenir. Çift ve görüntüleyerek sertifikayı işaretleyin **Gelişmiş anahtar kullanımı** içinde **ayrıntıları** sekmesi.

- **Otomatik olarak imzalanan kök sertifika:** Oluşturduğunuz istemci sertifikaları, P2S bağlantılarıyla uyumlu olacak şekilde aşağıdaki P2S sertifika makalelerinde bulunan adımları izleyin. Aşağıdaki makalelerdeki adımlarla uyumlu bir istemci sertifikası oluşturabilirsiniz: 

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): Bu yönergeler, sertifikalarını oluşturmak Windows 10 ve PowerShell gerektirir. Oluşturulan sertifikalar tüm desteklenen P2S istemcilere yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifikaları oluşturmak için bir Windows 10 bilgisayarına erişim yoksa MakeCert kullanın. MakeCert kullanım dışı bırakılmış olsa da, bu sertifikaları oluşturmak için kullanmaya devam edebilirsiniz. Oluşturulan sertifikalar tüm desteklenen P2S istemcilere yükleyebilirsiniz.
  * [Linux yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)

  Otomatik olarak imzalanan kök sertifikadan istemci sertifikası oluşturma, sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. Bir istemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, tüm sertifika zinciriyle birlikte .pfx dosyası olarak dışarı aktarın. Bir .pfx dosyası oluşturur, bunun yapılması, istemcinin kimliğini doğrulamak için gerekli kök sertifika bilgileri içerir. Sertifikayı dışarı aktarma adımları için bkz. [noktadan siteye sertifikaları oluşturma ve dışarı aktarma PowerShell kullanarak](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).