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
ms.openlocfilehash: 7e19837c1d16ddeea185f340305a0c9c52ce23ff
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
Otomatik olarak imzalanan bir sertifika oluşturduktan sonra kök sertifika ortak anahtar .cer dosyasını (özel anahtarı değil) verin. Ayrıca, Azure için daha sonra bu dosyayı karşıya yükler. Aşağıdaki adımlar, otomatik olarak imzalanan kök sertifika .cer dosyasını dışarı yardımcı:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Otomatik olarak imzalanan kök sertifikayı bulun (genellikle 'Certificates - Current User\Personal\Certificates' konumundadır) ve sağ tıklayın. **Tüm Görevler**’e tıklayın ve ardından **Dışarı Aktar**’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır.

  ![Dışarı Aktarma](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. Sihirbazda, **İleri**’ye tıklayın.

  ![Sertifika verme](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.

  ![Özel anahtarı verme](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

  ![Base-64 kodlanmış](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. İçin **dışarı aktarılan dosya**, **Gözat** sertifika vermek istediğiniz konuma. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.

  ![Göz at](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

  ![Son](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Sertifikanızı başarıyla dışarı aktarıldı.

  ![Başarılı](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Dışarı aktarılan sertifikayı şuna benzer:

  ![Dışa Aktarıldı](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Not Defteri'ni kullanarak verilen sertifika açarsanız, bu örneğe benzer bir şey görürsünüz. Mavi bölümünde Azure'a karşıya bilgileri içerir. Sertifikanızı Not Defteri'nde açın ve buna benzer aramaz, genellikle Bunun anlamı, dışa değil kullanarak Base-64 olarak kodlanmış X.509 (. CER) biçimi. Ayrıca, farklı bir metin Düzenleyicisi kullanmak istiyorsanız, bazı düzenleyicileri istenmeyen arka planda biçimlendirme getirebilir anlayın. Bu, bu sertifika metinden Azure karşıya sorunlar oluşturabilir.

  ![Not Defteri ile Aç](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)