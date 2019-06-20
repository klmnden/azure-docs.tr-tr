---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/13/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: afd4836229c60ebef1536d4fa1ca4206a492e56d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188247"
---
Otomatik olarak imzalanan kök sertifika oluşturduktan sonra kök sertifika ortak anahtar .cer dosyasını (özel anahtarı değil) dışarı aktarın. Daha sonra bu dosya Azure'a yükler. Aşağıdaki adımları otomatik olarak imzalanan kök sertifikanız için .cer dosyasını dışarı aktarmanıza yardımcı olur:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Otomatik olarak imzalanan kök sertifikayı bulun (genellikle 'Certificates - Current User\Personal\Certificates' konumundadır) ve sağ tıklayın. **Tüm Görevler**’e tıklayın ve ardından **Dışarı Aktar**’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır. Geçerli kullanıcı\kişisel\sertifikalar'ın altında bir sertifika bulamazsanız, yanlışlıkla "Sertifikalar - Geçerli kullanıcı" yerine "Sertifikalar - yerel bilgisayar", açtığınız). PowerShell, yazdığınız kullanarak geçerli kullanıcı kapsamı içinde Sertifika Yöneticisi'ni açmak istiyorsanız *certmgr* konsol penceresinde.

   ![Dışarı Aktarma](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. Sihirbazda, **İleri**’ye tıklayın.

   ![Sertifikayı dışarı aktarma](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.

   ![Özel anahtarı dışarı aktarma](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

   ![Base-64 kodlamalı](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. İçin **dışarı aktarılan dosya**, **Gözat** sertifikasını dışarı aktarmak istediğiniz konum için. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.

   ![Göz at](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![Son](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. Sertifikanızı başarıyla dışarı aktarıldı.

   ![Başarılı](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. Dışarı aktarılan sertifika şuna benzer:

   ![Dışarı aktarılan](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Not Defteri'ni kullanarak dışarı aktarılan sertifikası açarsanız, bu örnektekine benzer bir şey görürsünüz. Mavi bölümünde Azure'a yüklenmiş bilgileri içerir. Sertifikanızı Not Defteri'nde açın ve buna benzer aramaz, genellikle Bunun anlamı vermeden değil kullanarak Base-64 kodlanmış X.509 (. CER) biçimi. Ayrıca, farklı bir metin Düzenleyicisi kullanmak istiyorsanız, bazı düzenleyiciler istenmeyen arka planda biçimlendirme çıkarabilir anlayın. Bu, bu sertifika metinden azure'a karşıya sorunlar oluşturabilir.

   ![Not Defteri ile açın](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
