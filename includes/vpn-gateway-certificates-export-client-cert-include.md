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
ms.openlocfilehash: b657d54c3ebbe5afc20fc98c1348bb783410df60
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188245"
---
İstemci sertifikası oluşturma, sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. İstemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, oluşturduğunuz istemci sertifikasını dışarı aktarmanız gerekir.

1. Bir istemci sertifikasını dışarı aktarmak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Oluşturulan istemci sertifikaları 'Sertifikalar - Geçerli kullanıcı\kişisel\sertifikalar' varsayılan olarak, yer olan. ' I tıklatın, dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın **tüm görevler**ve ardından **dışarı** açmak için **Sertifika Verme Sihirbazı**.

   ![Dışarı Aktarma](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. Sertifika Dışarı Aktarma Sihirbazı'nda tıklatın **sonraki** devam etmek için.

   ![Sonraki](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. Seçin **Evet, özel anahtarı dışarı**ve ardından **sonraki**.

   ![özel anahtarı dışarı aktar](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında, varsayılan ayarları seçili bırakın. **Mümkünse sertifika yolundaki tüm sertifikaları ekle** seçeneğinin işaretli olduğundan emin olun. Bu ayar, ayrıca başarılı istemci kimlik doğrulaması için kök sertifika bilgileri verir. Güvenilen kök sertifika bir istemci olmadığı için bu olmadan, istemci kimlik doğrulaması başarısız olur. Ardından **İleri**'ye tıklayın.

   ![dışarı aktarma dosyası biçimi](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. **Güvenlik** sayfasında, özel anahtarı korumanız gerekir. Bir parola kullanmayı seçerseniz, bu sertifika için ayarladığınız parolayı kaydettiğinizden ya da unutmayacağınızdan emin olun. Ardından **İleri**'ye tıklayın.

   ![güvenlik](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.

   ![dışarı aktarılacak dosya](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![Son](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)