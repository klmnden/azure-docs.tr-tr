---
title: Java CA deposuna sertifika Ekle
description: Java CA sertifika (cacerts) deposuna Twilio hizmet veya Azure hizmet veri yolu için bir sertifika yetkilisi (CA) sertifikası eklemeyi öğrenin.
services: ''
documentationcenter: java
author: rmcmurray
manager: mbaldwin
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/11/2018
ms.author: robmcm
ms.openlocfilehash: 1c432aa9da9637675262313d935edd38bd7b698b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Bir sertifika Java CA sertifika depolama alanına ekleme
Aşağıdaki adımlar Java CA sertifikası (cacerts) depoya bir sertifika yetkilisi (CA) sertifikası ekleme gösterir. Twilio hizmeti için gereken CA sertifikası için kullanılan örnek verilebilir. Daha sonra konu başlığı altında sağlanan bilgiler, Azure hizmet veri yolu için CA sertifikası yüklemek açıklar. 

Keytool, JDK sıkıştırma ve Azure projenizin için ekleme önce CA sertifika eklemek için kullanabileceğiniz **approot** klasör veya sertifika eklemek için keytool kullanan bir Azure başlangıç görevi çalıştırabilir. (Bu örnekte, JDK sıkıştırma önce bir CA sertifikası eklemekte olduğunuz varsayılır.) Ayrıca, belirli bir CA sertifikası örnekte kullanılır, ancak farklı bir CA sertifikası alma ve cacerts deposuna içeri aktarma adımları benzer olacaktır.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Bir sertifika cacerts deposuna eklemek için
1. Bir yönetici komut isteminde, JDK için 's ayarlanan **jdk\jre\lib\security** klasörü, hangi sertifika yüklü görmek için aşağıdaki komutu çalıştırın:
   
    `keytool -list -keystore cacerts`
   
    Mağaza parolası istenir. Varsayılan parola **değiştirme**. (Parolayı değiştirmek istiyorsanız, keytool belgelerine bakın <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Bu örnek varsayar MD5 sahip sertifika parmak izi 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 listede ve onu (Twilio API hizmeti tarafından bu belirli sertifika gereklidir) içeri aktarmak istediğiniz.
2. Adresinde listelenmiş sertifikalar listesine sertifika almak [GeoTrust kök sertifikalarını](http://www.geotrust.com/resources/root-certificates/). Sertifikanın seri numarası 35:DE:F4:CF ile bağlantıyı sağ tıklatın ve kaydetmesi **jdk\jre\lib\security** klasör. Bu örneğin amaçları doğrultusunda adlı bir dosyada kaydedilmiş olan **Equifax\_güvenli\_sertifika\_Authority.cer**.
3. Aşağıdaki komut aracılığıyla sertifikasını içeri aktarın:
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    Bir MD5 parmak izi 67:CB:9 D, sahip bir sertifika güven istenirse: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, yazarak yanıtlayın **y**.
4. CA sertifikasını başarıyla içe aktarıldığından emin olmak için aşağıdaki komutu çalıştırın:
   
    `keytool -list -keystore cacerts`
5. JDK zip ve Azure projenizin için ekleme **approot** klasör.

Keytool hakkında daha fazla bilgi için bkz: <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure kök sertifikaları
Azure Hizmetleri (örneğin, Azure Service Bus) kullanan, uygulamaları Baltimore CyberTrust kök sertifikasına güvenmesi gerekir. (15 Nisan 2013 başlayan Azure GTE CyberTrust genel kökünden Baltimore CyberTrust kök geçirme başlamıştır. Bu geçiş tamamlamak için birkaç ay sürdü.)

Bu nedenle sertifika önceden yüklenmiş olmalıdır cacerts deponuzda Baltimore unutmayın çalıştırmak **keytool-liste** ilk zaten olup olmadığını görmek için komutu.

Baltimore CyberTrust Root eklemeniz gerekiyorsa, seri numarası 02:00:00:b9 ve SHA1 parmak izi d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c onu vardır: 78:db:28:52:ca:e4:74. Adresten yüklenebilir <https://cacert.omniroot.com/bc2025.crt>, yerel bir dosya uzantısına sahip kaydedilmiş **.cer**ve kullanılarak içe **keytool** önceki adımlarda gösterildiği gibi.

## <a name="next-steps"></a>Sonraki adımlar
Azure tarafından kullanılan kök sertifikalar hakkında daha fazla bilgi için bkz: [Azure kök sertifika geçiş](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Java hakkında daha fazla bilgi için bkz: [Java geliştiriciler için Azure](/java/azure).

