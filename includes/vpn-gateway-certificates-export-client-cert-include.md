---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 517ebbf739c64c0364dc21386fee86ebc740e997
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
Bir istemci sertifikası oluşturduğunuzda, onu oluşturmak için kullanılan bilgisayarda otomatik olarak yüklenir. İstemci sertifikası başka bir istemci bilgisayara yüklemek istiyorsanız, oluşturulan istemci sertifikasını dışarı aktarmanız gerekmez.

1. Bir istemci sertifikasını dışarı aktarmak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Oluşturulan istemci sertifikalarını varsayılan olarak, 'Sertifikalar - Geçerli User\Personal\Certificates' bulunan. İstediğiniz dışarı aktarmak için istemci sertifikasına sağ **tüm görevler**ve ardından **verme** açmak için **Sertifika Verme Sihirbazı**.

  ![Dışarı Aktarma](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. Sertifika Dışarı Aktarma Sihirbazı'nda tıklatın **sonraki** devam etmek için.

  ![Sonraki](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. Seçin **Evet, özel anahtarı dışarı**ve ardından **sonraki**.

  ![özel anahtarı dışarı aktar](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında, varsayılan ayarları seçili bırakın. **Mümkünse sertifika yolundaki tüm sertifikaları ekle** seçeneğinin işaretli olduğundan emin olun. Bu ayar ayrıca başarılı istemci kimlik doğrulaması için kök sertifika bilgileri verir. Güvenilen Kök Sertifika istemcisi bulunmadığından, istemci kimlik doğrulaması başarısız olur. Ardından **İleri**'ye tıklayın.

  ![dışarı aktarma dosyası biçimi](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. **Güvenlik** sayfasında, özel anahtarı korumanız gerekir. Bir parola kullanmayı seçerseniz, bu sertifika için ayarladığınız parolayı kaydettiğinizden ya da unutmayacağınızdan emin olun. Ardından **İleri**'ye tıklayın.

  ![güvenlik](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.

  ![dışa aktarılacak dosya](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

  ![son](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)