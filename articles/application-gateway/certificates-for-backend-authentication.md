---
title: Azure Application Gateway beyaz arka uçları için gerekli sertifikalar
description: Bu makalede, Azure Application Gateway beyaz arka uç örnekler için gerekli olan kimlik doğrulama sertifikasını ve güvenilen kök sertifika için bir SSL sertifikasını nasıl dönüştürülebilir örnekler sağlar
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/14/2019
ms.author: absha
ms.openlocfilehash: 72ee9123ad959c0c7240d4f7a906adc1a4dd1a93
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831733"
---
# <a name="create-certificates-for-whitelisting-backend-with-azure-application-gateway"></a>Azure Application Gateway ile beyaz arka uç için sertifikaları oluşturma

Uçtan uca SSL gerçekleştirmek için kimlik doğrulaması ve güvenilen kök sertifikaları karşıya yükleyerek Güvenilenler listesine eklenmek için arka uç örnekleriyle uygulama ağ geçidi gerektirir. Sertifikaları beyaz listeye eklemek için v1 SKU’da kimlik doğrulama sertifikaları, v2 SKU’da ise güvenilen kök sertifikaları gereklidir

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> - Sertifikadan bir arka uç (v1 SKU) kimlik doğrulama sertifikasını dışarı aktarma
> - Sertifikadan bir arka uç (v2 SKU) güvenilen kök sertifikayı dışarı aktarma

## <a name="prerequisites"></a>Önkoşullar

Kimlik doğrulama sertifikaları ya da uygulama ağ geçidi arka uç örnekleriyle beyaz listeye ekleme için gerekli güvenilen kök sertifikalar oluşturmak için var olan bir arka uç sertifikası gerektirir. Arka uç sertifikayı SSL sertifikası ile aynı veya daha fazla güvenlik için farklı olabilir. Uygulama ağ geçidi veya bir SSL sertifikası satın alma mekanizması sağlamaz. Sınama amacıyla, otomatik olarak imzalanan bir sertifika oluşturabilir ancak, bu üretim iş yükleri için kullanmamanız gerekir. 

## <a name="export-authentication-certificate-for-v1-sku"></a>Kimlik doğrulama sertifikasını dışarı aktarma (v1 SKU için)

Kimlik doğrulama sertifikası, uygulama ağ geçidi v1 beyaz arka uç örneklerine gerekli SKU. Kimlik doğrulama sertifikası, ortak anahtarı arka uç sunucusu sertifikalarının Base-64 ile kodlanmış X.509 (. CER) biçimi. Bu örnekte biz arka uç sertifika için bir SSL sertifikası kullanmak ve kimlik doğrulama sertifikası kullanılacak ortak anahtarını dışarı aktarın. Ayrıca, bu örnekte, Windows Sertifika Yöneticisi aracı gerekli sertifikaları vermek için kullanacağız. Size kolaylık sağlamak göre herhangi bir aracı kullanmayı da tercih edebilirsiniz.

SSL sertifikanızı, ortak anahtar .cer dosyasını (özel anahtarı değil) dışarı aktarın. Aşağıdaki adımları .cer dışarı Yardım dosyasında Base-64 ile kodlanmış X.509 (. Sertifikanızı CER) biçimi:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Sertifika genellikle 'Sertifikalar - Geçerli kullanıcı\kişisel\sertifikalar' bulun ve sağ tıklayın. **Tüm Görevler**’e tıklayın ve ardından **Dışarı Aktar**’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır. Geçerli kullanıcı\kişisel\sertifikalar'ın altında bir sertifika bulamazsanız, yanlışlıkla "Sertifikalar - Geçerli kullanıcı" yerine "Sertifikalar - yerel bilgisayar", açtığınız). PowerShell, yazdığınız kullanarak geçerli kullanıcı kapsamı içinde Sertifika Yöneticisi'ni açmak istiyorsanız *certmgr* konsol penceresinde.

   ![Dışarı Aktarma](./media/certificates-for-backend-authentication/export.png)

2. Sihirbazda, **İleri**’ye tıklayın.

   ![Sertifikayı dışarı aktarma](./media/certificates-for-backend-authentication/exportwizard.png)

3. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.

   ![Özel anahtarı dışarı aktarma](./media/certificates-for-backend-authentication/notprivatekey.png)

4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

   ![Base-64 kodlamalı](./media/certificates-for-backend-authentication/base64.png)

5. İçin **dışarı aktarılan dosya**, **Gözat** sertifikasını dışarı aktarmak istediğiniz konum için. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.

   ![Göz at](./media/certificates-for-backend-authentication/browse.png)

6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![Son](./media/certificates-for-backend-authentication/finish.png)

7. Sertifikanızı başarıyla dışarı aktarıldı.

   ![Başarılı](./media/certificates-for-backend-authentication/success.png)

   Dışarı aktarılan sertifika şuna benzer:

   ![Dışarı aktarılan](./media/certificates-for-backend-authentication/exported.png)

8. Not Defteri'ni kullanarak dışarı aktarılan sertifikası açarsanız, bu örnektekine benzer bir şey görürsünüz. Mavi bölümünde, uygulama ağ geçidine karşıya bilgileri içerir. Sertifikanızı Not Defteri'nde açın ve buna benzer aramaz, genellikle Bunun anlamı vermeden değil kullanarak Base-64 kodlanmış X.509 (. CER) biçimi. Ayrıca, farklı bir metin Düzenleyicisi kullanmak istiyorsanız, bazı düzenleyiciler istenmeyen arka planda biçimlendirme çıkarabilir anlayın. Bu, bu sertifika metinden azure'a karşıya sorunlar oluşturabilir.

   ![Not Defteri ile açın](./media/certificates-for-backend-authentication/format.png)

## <a name="export-trusted-root-certificate-for-v2-sku"></a>Güvenilen kök sertifikasını dışarı aktarma (v2 SKU için)

Güvenilen kök sertifika uygulama ağ geçidi v2'de beyaz arka uç örneklerine gerekli SKU. Kök sertifikayı Base-64 ile kodlanmış X.509 olduğu (. CER) biçimi kök sertifikadan arka uç sunucusu sertifikalarının. Bu örnekte, eder arka uç sertifika için bir SSL sertifikası kullanmak, kendi ortak anahtarını dışarı aktarmak ve güvenilen CA'ın kök sertifikasının güvenilen kök sertifikayı almak için base64 olarak kodlanmış biçimde ortak anahtardan dışarı aktarma. 

Aşağıdaki adımları sertifikanız için .cer dosyasını dışarı aktarmanıza yardımcı olur:

1. 1-9 bölümünde belirtilen adımları **(v1 SKU için) arka uç sertifika verme kimlik doğrulama sertifikasından** arka uç sertifikanızı ortak anahtarını dışarı aktarmak için yukarıdaki.

2. Ortak anahtarı dışarı sonra dosyayı açın.

   ![Açık yetkilendirme sertifikası](./media/certificates-for-backend-authentication/openAuthcert.png)

   ![Sertifika hakkında](./media/certificates-for-backend-authentication/general.png)

3. Sertifika yetkilisi sertifika yolu görünüm için Görünüm taşıyın.

   ![Sertifika ayrıntıları](./media/certificates-for-backend-authentication/certdetails.png)

4. Kök sertifikayı seçin ve tıklayın **Sertifikayı Görüntüle**.

   ![Sertifika yolu](./media/certificates-for-backend-authentication/rootcert.png)

   Kök Sertifika ayrıntılarını görüntülemek olması gerekir.

   ![Sertifika bilgisi](./media/certificates-for-backend-authentication/rootcertdetails.png)

5. Taşı **ayrıntıları** görüntülemek ve **dosyaya Kopyala...**

   ![kopya kök sertifikası](./media/certificates-for-backend-authentication/rootcertcopytofile.png)

6. Bu noktada, arka uç sertifikası kök sertifika ayrıntılarını ayıkladınız. Göreceğiniz **Sertifika Verme Sihirbazı**. 2-9 bölümünde belirtilen kullanma adımları artık **(v1 SKU için) arka uç sertifika verme kimlik doğrulama sertifikasından** yukarıda dışarı güvenilen kök sertifikayı Base-64 kodlanmış X.509 (. CER) biçimi.

## <a name="next-steps"></a>Sonraki adımlar

Artık kimlik doğrulama sertifikasını ve güvenilen kök sertifikayı Base-64 kodlanmış X.509 (. CER) biçimi. Bu uygulama ağ geçidine beyaz liste için arka uç sunucuları için uçtan uca SSL şifrelemesini ekleyebilirsiniz. Bkz: [uçtan uca SSL şifrelemesini yapılandırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).