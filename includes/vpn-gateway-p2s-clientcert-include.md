---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: 01a62fe7abb8a79f9afc08c0ff707cdfbb97ddac
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44343230"
---
Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. İstemci sertifikası kök sertifikadan oluşturulur ve her bir istemci bilgisayara yüklenir. Geçerli bir istemci sertifikası yüklü değilse ve istemci sanal ağa bağlanmaya çalışırsa, kimlik doğrulaması başarısız olur.

Her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemci için aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, birden fazla istemci aynı istemci sertifikasını kullanıyorsa ve sertifikayı iptal etmeniz gerekirse, kimlik doğrulaması için bu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturup yüklemeniz gerekir.

Aşağıdaki yöntemleri kullanarak istemci sertifikaları oluşturabilirsiniz:

- **Kurumsal sertifika:**

  - Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialaniadi\kullaniciadi' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com' ad değer biçimiyle bir istemci sertifikası oluşturun.
  - İstemci sertifikasının, kullanım listesindeki ilk öğe olarak Akıllı Kart Oturumu, vb. yerine ‘İstemci Kimlik Doğrulaması’na sahip ‘Kullanıcı’ sertifikası şablonunu temel alarak hazırlandığından emin olun. İstemci sertifikasına sağ tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’nı görüntüleyerek sertifikayı denetleyebilirsiniz.

- **Otomatik olarak imzalanan sertifika:** Aşağıda bulunan P2S sertifika makalelerindeki adımları izlemeniz önemlidir. Aksi halde, oluşturduğunuz istemci sertifikaları P2S bağlantılarıyla uyumlu olmaz ve istemcileriniz bağlantı kurmaya çalıştığında bir hata alır. Aşağıdaki makalelerdeki adımlarla uyumlu bir istemci sertifikası oluşturabilirsiniz: 

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): Bu yönergeler, sertifika oluşturmak için Windows 10 ve PowerShell gerektirir. Oluşturulan sertifikalar tüm desteklenen P2S istemcilere yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifikaları oluşturmak için kullanabileceğiniz bir Windows 10 bilgisayarınızın yoksa MakeCert kullanın. MakeCert kullanım dışıdır, ancak sertifika oluşturmak için MakeCert kullanmaya devam edebilirsiniz. Oluşturulan sertifikalar tüm desteklenen P2S istemcilere yüklenebilir.
  * [Linux yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)

  Önceki yönergelerini kullanarak bir otomatik olarak imzalanan kök sertifikadan bir istemci sertifikası oluşturduğunuzda sertifika, sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. İstemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız sertifikayı tüm sertifika zinciriyle birlikte .pfx olarak dışarı aktarmanız gerekir. Bu, istemcinin kimliğini başarıyla doğrulamak için gerekli kök sertifika bilgilerini içeren bir .pfx dosyası oluşturur. Sertifikayı dışa aktarma adımları için bkz. [Sertifikalar - istemci sertifikasını dışarı aktarma](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).