---
title: "Etkinleştirmek veya devre dışı Azure içerik teslim ağı özel etki alanı üzerinde HTTPS | Microsoft Docs"
description: "Etkinleştirme veya HTTPS ile özel bir etki alanı Azure CDN uç noktanız devre dışı bırakma hakkında bilgi edinin."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: c92f1e20acf55b8bd791fad43f17e162a5cb3847
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-or-disable-https-on-an-azure-content-delivery-network-custom-domain"></a>Etkinleştirmek veya HTTPS Azure içerik teslim ağı özel etki alanı üzerinde devre dışı bırakma

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Microsoft Azure içerik teslim ağı (CDN) özel etki alanları için HTTPS desteği aktarım sırasında verilerin güvenliğini artırmak için kendi etki alanı adınızı kullanarak SSL üzerinden güvenli içerik teslim etmenizi sağlar. Özel etki alanınız için HTTPS'yi etkinleştirmek için uçtan uca iş akışı tek tıklatmayla etkinleştirme, eksiksiz bir sertifika yönetimi ve tüm ek ücret ödemeden ile aracılığıyla basitleştirilmiştir.

Gizlilik ve tüm web uygulamaları hassas verilerinizi aktarım sırasında veri bütünlüğünü sağlamak önemlidir. HTTPS protokolünü kullanarak Internet üzerinden gönderildiğinde hassas verilerinizi şifrelenmesini sağlar. Sağladığı güven, kimlik doğrulaması ve web uygulamalarınızın saldırılara karşı korur. Şu anda Azure CDN bir CDN uç noktası HTTPS destekler. Örneğin, HTTPS (https://contoso.azureedge.net gibi) Azure CDN bir CDN uç noktası oluşturursanız, varsayılan olarak etkindir. Şimdi, özel etki alanı ile HTTPS, güvenli teslimat özel bir etki alanı (örneğin, https://www.contoso.com) için de etkinleştirebilirsiniz. 

HTTPS özelliğinin en önemli özelliklerinden bazıları şunlardır:

- Ek ücret ödemeden: sertifika edinme veya yenileme için hiçbir maliyetleri ve HTTPS trafiği için ek ücret ödemeden vardır. Yalnızca CDN GB çıkışı için ödeme.

- Basit etkinleştirme: bir tıklatın sağlama edinilebilir [Azure portal](https://portal.azure.com). Özelliği etkinleştirmek için REST API veya diğer geliştirici araçları kullanabilirsiniz.

- Sertifika yönetimi tamamlamak: tüm tedarik sertifika ve yönetim sizin için işlenir. Sertifikaları otomatik olarak sağlanan ve sona erme önce yenilendi. Bu sertifikanın sona ermesinden sonucu olarak hizmet kesintisi riskleri tamamen kaldırır.

>[!NOTE] 
>HTTPS desteği etkinleştirilmeden önce önceden oluşturulmuş gereken bir [Azure CDN özel etki alanı](./cdn-map-content-to-custom-domain.md).

## <a name="enabling-https"></a>HTTPS etkinleştirme

HTTPS'yi etkinleştirmek için aşağıdaki adımları izleyin:

### <a name="step-1-enable-the-feature"></a>1. adım: özelliğini etkinleştir 

1. İçinde [Azure portal](https://portal.azure.com), Verizon standart veya premium CDN profilinizi göz atın.

2. Uç noktalar listesinde, özel etki alanınızı içeren uç noktasına tıklayın.

3. HTTPS etkinleştirmek istediğiniz özel etki alanını tıklatın.

    ![Uç nokta dikey penceresi](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Tıklatın **üzerinde** HTTPS'yi etkinleştirmek için ardından **Uygula**.

    ![Özel HTTPS iletişim](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>2. adım: etki alanı doğrulanamıyor

>[!IMPORTANT] 
>HTTPS etkin özel etki alanınızda önce etki alanı doğrulama tamamlamanız gerekir. Etki alanı onaylamak için altı iş günü vardır. Altı iş günü içinde onaylanmamış istekleri otomatik olarak iptal edilir. 

Özel etki alanınızda HTTPS etkinleştirdikten sonra bizim HTTPS sertifika sağlayıcısı DigiCert etki alanınızın sahipliğini etki alanınız için registrant başvurarak etki alanının göre doğrular [WHOIS](http://whois.domaintools.com/) registrant bilgi. Telefon numarası WHOIS kaydında listelenen veya kişi (varsayılan) e-posta adresi yapılır. 

![WHOIS kaydı](./media/cdn-custom-ssl/whois-record.png)

Ayrıca, DigiCert aşağıdaki adresler olarak doğrulama e-posta gönderir. WHOIS registrant bilgi özel ise, doğrudan bu adreslerden birinden onaylayabilirsiniz doğrulayın:

Yönetici @&lt;etki alanı name.com bilgisayarınızı&gt;  
Yönetici @&lt;etki alanı name.com bilgisayarınızı&gt;  
@ yayımlanması&lt;etki alanı name.com bilgisayarınızı&gt;  
@ hostmaster&lt;etki alanı name.com bilgisayarınızı&gt;  
@ Yöneticisi&lt;etki alanı name.com bilgisayarınızı&gt;  

Sizden isteği onaylamak için birkaç dakika cinsinden aşağıdaki örneğe benzer bir e-posta alacaksınız. İstenmeyen posta Filtresi kullanıyorsanız, ekleyin admin@digicert.com kendi güvenilir listeye. 24 saat içinde bir e-posta almazsanız, Microsoft Destek'e başvurun.
    
![Özel HTTPS iletişim](./media/cdn-custom-ssl/domain-validation-email.png)

Onay bağlantısına tıkladığınızda, aşağıdaki çevrimiçi onay formun yönlendirilir: 
    
![Özel HTTPS iletişim](./media/cdn-custom-ssl/domain-validation-form.png)

Formdaki yönergeleri izleyin; iki doğrulama seçeneğiniz vardır:

- Aynı kök etki alanı için aynı hesabı aracılığıyla verilen tüm gelecekteki siparişler onaylayabilirsiniz; Örneğin, contoso.com. Bu, ek özel etki alanlarını gelecekte aynı kök etki alanı eklemek planlıyorsanız önerilen yaklaşımdır.

- Yalnızca bu istekte kullanılan belirli ana bilgisayar adı onaylayabilirsiniz. Ek onay sonraki istekleri için gerekli olacaktır.

Onay sonrasında konu alternatif adları (SAN) sertifika için özel etki alanı adınızı DigiCert ekleyin. Sertifika bir yıl için geçerlidir ve otomatik-sona ermeden önce yenilenmesi.

### <a name="step-3-wait-for-propagation"></a>3. adım: Bekleme süresi yayma

Etki alanı adı doğrulandıktan sonra özel etki alanı HTTPS özelliğin etkinleştirilmesi 6-8 saate kadar sürebilir. İşlem tamamlandığında, Azure Portalı'ndaki "Özel HTTPS" durumu "Etkin" olarak ayarlanır ve özel etki alanı HTTPS dikey dört işlemi adımlarda tamamlandı olarak işaretlenir. Özel etki alanınızı artık HTTPS kullanmak üzere hazırdır.

![HTTPS iletişim etkinleştir](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>İşlem devam ediyor

Aşağıdaki tabloda, HTTPS etkinleştirdiğinizde oluşan işlemi ilerleme durumunu gösterir. HTTPS etkinleştirdikten sonra dört işlem adımları özel etki alanı HTTPS dikey penceresinde görünür. Her adım etkin olduğu gibi ek ayrıntılar ilerledikçe adımı altında görüntülenir. Bir adım başarıyla tamamlandıktan sonra bunun yanında yeşil bir onay işareti görünür. 

| İşlemi adım | İşlem adım ayrıntıları | 
| --- | --- |
| 1 gönderme isteği | İsteği gönderiliyor |
| | HTTPS isteği gönderiliyor. |
| | HTTPS isteği başarıyla gönderildi. |
| 2 etki alanı doğrulama | Etki alanı sahipliği doğrulamak için isteyen bir e-posta gönderdik. Onayınız bekleniyor. |
| | Etki alanı sahipliğini başarıyla doğruladı. |
| | Etki alanı sahipliği doğrulama isteğinin süresi doldu (büyük olasılıkla müşteri kaydetmedi yanıt 6 gün içinde). HTTPS, etki alanınızda etkin değil. * |
| | Etki alanı sahipliği doğrulama isteği müşteri tarafından reddedildi. HTTPS, etki alanınızda etkin değil. * |
| 3 sertifika sağlama | Sertifika yetkilisi şu anda, etki alanınızda HTTPS'yi etkinleştirmek için gereken sertifikayı veren. |
| | Sertifika verildikten ve CDN ağa şu an dağıtılıyor. Bu 6 saate kadar sürebilir. |
| | Sertifika CDN ağa başarıyla dağıtıldı. |
| 4 tamamlandı | HTTPS, etki alanınızda başarıyla etkinleştirildi. |

\*Bir hata oluştu sürece bu ileti görünmez. 

İstek gönderilmeden önce bir hata oluşursa, aşağıdaki hata iletisini görürsünüz:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>HTTPS devre dışı bırakma

HTTPS etkinleştirdikten sonra daha sonra onu devre dışı bırakabilirsiniz. HTTPS devre dışı bırakmak için şu adımları izleyin:

### <a name="step-1-disable-the-feature"></a>1. adım: özelliği devre dışı 

1. İçinde [Azure portal](https://portal.azure.com), Verizon standart veya premium CDN profilinizi göz atın.

2. Uç noktalar listesinde, özel etki alanınızı içeren uç noktasına tıklayın.

3. HTTPS devre dışı bırakmak istediğiniz özel etki alanını tıklatın.

    ![Uç nokta dikey penceresi](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Tıklatın **kapalı** HTTPS devre dışı bırakmak için ardından **Uygula**.

    ![Özel HTTPS iletişim](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>2. adım: Bekleme süresi yayma

Özel etki alanı HTTPS özelliği devre dışı bırakıldıktan sonra 6-8 saate kadar etkili olması için alabilir. İşlem tamamlandığında, Azure Portalı'ndaki "Özel HTTPS" Durum "disabled" olarak ayarlanır ve özel etki alanı HTTPS dikey üç işlemi adımlarda tamamlandı olarak işaretlenir. Özel etki alanınızı artık HTTPS kullanabilirsiniz.

![HTTPS iletişim devre dışı bırak](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>İşlem devam ediyor

Aşağıdaki tabloda, HTTPS devre dışı bıraktığınızda oluşan işlem ilerlemesini gösterir. HTTPS devre dışı bıraktıktan sonra üç işlem adımları özel etki alanı HTTPS dikey penceresinde görünür. Her adım etkin olduğu gibi ek ayrıntılar altında adımı görüntülenir. Bir adım başarıyla tamamlandıktan sonra bunun yanında yeşil bir onay işareti görünür. 

| İşlem devam ediyor | İşlem ayrıntıları | 
| --- | --- |
| 1 gönderme isteği | İsteğiniz gönderiliyor |
| 2 sertifika sağlama kaldırma | Sertifikası siliniyor |
| 3 tamamlandı | Sertifika silindi |

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

1. *Sertifika Sağlayıcısı ve ne tür bir sertifika kullanılan kim?*

    DigiCert tarafından sağlanan konu alternatif adları (SAN) sertifika kullanırız. Bir SAN sertifika bir sertifika ile birden fazla tam etki alanı adları güvenliğini sağlayabilirsiniz.

2. *My ayrılmış sertifika kullanabilir miyim?*
    
    Şu anda değil ancak yol haritası üzerinde değil.

3. *DigiCert ne etki alanı doğrulama e-posta alıyorum mu?*

    24 saat içinde bir e-posta almazsanız, Microsoft Destek'e başvurun.

4. *Ayrılmış bir sertifika az güvenli bir SAN sertifikası kullanıyor?*
    
    Bir SAN sertifika aynı şifreleme ve güvenlik standartları bir ayrılmış sertifika olarak izler. Verilen tüm SSL sertifikaları SHA-256 Gelişmiş server güvenliği için kullanıyorsunuz.

5. *Akamai'den Azure CDN ile özel etki alanı HTTPS kullanabilir miyim?*

    Şu anda bu özellik yalnızca verizon'dan Azure CDN ile kullanılabilir. Bu özellik akamai'den Azure CDN ile önümüzdeki aylarda desteklemeye çalışıyoruz.


## <a name="next-steps"></a>Sonraki adımlar

- Nasıl ayarlanacağını öğrenin bir [Azure CDN uç noktanız özel etki alanında](./cdn-map-content-to-custom-domain.md)


