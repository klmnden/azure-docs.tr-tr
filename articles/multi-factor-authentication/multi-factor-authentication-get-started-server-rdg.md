---
title: RADIUS kullanan RDG ve Azure MFA Sunucusu | Microsoft Belgeleri
description: "Bu, RADIUS kullanan Uzak Masaüstü (RD) Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: d0e35237b412d4c3bcc26c77d124f3ed2fef6403
ms.lasthandoff: 03/16/2017


---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu
Uzak Masaüstü (RD) Ağ Geçidi genellikle kullanıcıların kimliğini doğrulamak için yerel Ağ İlkesi Hizmetleri’ni (NPS) kullanır. Bu makale, Uzak Masaüstü Ağ Geçidi’ndeki RADIUS isteklerini (yerel NPS üzerinden) Multi-Factor Authentication Sunucusu’na yönlendirmeyi açıklar. Azure MFA ile RD Ağ Geçidinin birleşimi, güçlü kimlik doğrulaması ile kurumsal varlıkların kontrolü sizde kalırken kullanıcıların diledikleri yerden çalışma ortamlarına erişebileceği anlamına gelir. 

Server 2012 R2’de terminal hizmetleri için Windows Kimlik Doğrulaması desteklenmediğinden, MFA Sunucusu ile tümleştirmek için RD Ağ Geçidi ve RADIUS kullanın. 

Multi-Factor Authentication Sunucusu, daha sonra RADIUS isteğini Uzak Masaüstü Ağ Geçidi Sunucusu’ndaki NPS’ye sunduğu ayrı bir sunucuya yüklenmelidir. NPS kullanıcı adı ve parolayı doğruladıktan sonra, ağ geçidine bir sonuç döndürmeden önce kimlik doğrulamasının ikinci öğesini uygulayan Multi-Factor Authentication Sunucusu’na bir yanıt döndürür.

## <a name="prerequisites"></a>Ön koşullar

- Etki alanına katılmış bir Azure MFA Sunucusu. Henüz yüklü değilse, [Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama](multi-factor-authentication-get-started-server.md) bölümündeki adımları izleyin.
- Ağ İlkesi Hizmetleri’nde kimlik doğrulaması yapan bir Uzak Masaüstü Ağ Geçidi.

## <a name="configure-the-remote-desktop-gateway"></a>Uzak Masaüstü Ağ Geçidini yapılandırma
RD Ağ Geçidini, bir Azure Multi-Factor Authentication Sunucusu’na RADIUS kimlik doğrulaması gönderecek şekilde yapılandırın. 

1. RD Ağ Geçidi Yöneticisi’nde sunucu adına sağ tıklayıp **Özellikler**’i seçin.
2. **RD CAP Deposu** sekmesine gidin ve **NPS çalıştıran merkezi sunucu** öğesini seçin. 
3. Her bir sunucunun adını veya IP adresini girerek, RADIUS sunucuları olarak bir ya da daha fazla Azure Multi-Factor Authentication Sunucusu ekleyin. 
4. Her sunucu için paylaşılan gizlilik oluşturun.

## <a name="configure-nps"></a>NPS yapılandırma
RD Ağ Geçidi, Azure Multi-Factor Authentication’a RADIUS isteği göndermek için NPS kullanır. NPS’yi yapılandırmak için, RD Ağ Geçidinin iki adımlı doğrulama tamamlanmadan zaman aşımına uğramasını önlemek üzere ilk olarak zaman aşımı ayarlarını değiştirin. Ardından, MFA sunucunuzdan RADIUS kimlik doğrulamalarını almak için NPS’yi güncelleştirin. NPS’yi yapılandırmak için aşağıdaki yordamı kullanın:

### <a name="modify-the-timeout-policy"></a>Zaman aşımı ilkesini değiştirme

1. NPS’de, sol sütundaki **RADIUS İstemcileri ve Sunucu** menüsünü açın ve **Uzak RADIUS Sunucu Grupları**’nı seçin. 
2. **TS AĞ GEÇİDİ SUNUCU GRUBU**’nu seçin. 
3. **Yük Dengeleme** sekmesine gidin. 
4. “İsteği bırakılmış olarak kabul etmeden önce yanıt almadan beklenecek süre (saniye)” ve “Sunucunun kullanılamaz olduğu belirlendiğinde gönderilecek istekler arasındaki süre (saniye)” değerlerini 30 ile 60 saniye arasına ayarlayın. (Kimlik doğrulaması sırasında sunucu yine de zaman aşımına uğrarsa, buraya geri dönerek saniye sayısını artırabilirsiniz.)
5. **Kimlik Doğrulama/Hesap** sekmesine gidin ve belirtilen RADIUS bağlantı noktalarının, Multi-Factor Authentication Sunucusu’nun dinlediği bağlantı noktalarıyla eşleştiğinden emin olun.

### <a name="prepare-nps-to-receive-authentications-from-the-mfa-server"></a>NPS’yi MFA Sunucusundan kimlik doğrulamaları almaya hazırlama

1. Sol sütundaki RADIUS İstemcileri ve Sunucuları altında bulunan **RADIUS İstemcileri**’ne sağ tıklayıp **Yeni**’yi seçin.
2. RADIUS istemcisi olarak Azure Multi-Factor Authentication Sunucusu ekleme Kolay bir ad seçin ve paylaşılan gizlilik belirtin.
3. Sol sütundaki **İlkeler** menüsünü açıp **Bağlantı İsteği İlkeleri**’ni seçin. RD Ağ Geçidi yapılandırıldığında oluşturulan TS AĞ GEÇİDİ KİMLİK DOĞRULAMA İLKESİ adlı bir ilke görürsünüz. Bu ilke RADIUS isteklerini Multi-Factor Authentication Sunucusu’na iletir.
4. **TS AĞ GEÇİDİ KİMLİK DOĞRULAMA İLKESİ**’ne sağ tıklayıp **İlkeyi Yinele**’yi seçin. 
5. Yeni ilkeyi açıp **Koşullar** sekmesine gidin.
6. Azure Multi-Factor Authentication Sunucusu RADIUS istemcisi için 2. adımda ayarladığınız Kolay adla eşleşen bir İstemci Kolay Adı koşulu ekleyin. 
7. **Ayarlar** sekmesine gidin ve **Kimlik Doğrulaması**’nı seçin.
8. Kimlik Doğrulama Sağlayıcısı’nı **Bu sunucu üzerindeki isteklerin kimliğini doğrula** olarak değiştirin. Bu ilke, NPS Azure MFA Sunucusu’ndan bir RADIUS isteği aldığında, döngü koşuluna neden olacak şekilde RADIUS isteğini Azure Multi-Factor Authentication Sunucusu’na geri göndermek yerine, kimlik doğrulamasının yerel olarak gerçekleştirilmesini sağlar. 
9. Döngü koşulunu önlemek için yeni ilkenin **Bağlantı İsteği İlkeleri** bölmesindeki özgün ilkenin ÜZERİNDE bulunduğundan emin olun.

## <a name="configure-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı yapılandırma

Azure Multi-Factor Authentication Sunucusu, RD Ağ Geçidi ile NPS arasında bir RADIUS proxy olarak yapılandırılmıştır.  RD AĞ Geçidi sunucusundan ayrı bir etki alanına katılmış sunucuya yüklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın.

1. Azure Multi-Factor Authentication Sunucusu’nu açın ve RADIUS Kimlik Doğrulaması simgesini seçin. 
2. **RADIUS kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin.
3. İstemciler sekmesinde, bağlantı noktalarının NPS’de yapılandırılanlarla eşleştiğinden emin olun ve **Ekle** düğmesini seçin.
4. RD Ağ Geçidi sunucusu IP adresi, uygulama adı (isteğe bağlı) ve paylaşılan gizlilik ekleyin. Paylaşılan gizliliğin Azure Multi-Factor Authentication Sunucusu’nda ve RD Ağ Geçidinde aynı olması gerekir.
3. **Hedef** sekmesine gidin ve **RADIUS sunucuları** radyo düğmesini seçin.
4. **Ekle**’yi seçip IP adresi, paylaşılan gizlilik ve NPS sunucusu bağlantı noktalarını girin. Merkezi NPS kullanmadığınız sürece, RADIUS istemcisi ile RADIUS hedefi aynıdır. Paylaşılan gizliliğin, NPS sunucusunun RADIUS istemcisi bölümündekiyle eşleşmesi gerekir.

![Radius Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure MFA ve [IIS web uygulamalarını](multi-factor-authentication-get-started-server-iis.md) tümleştirme

- [Azure Multi-Factor Authentication SSS](multi-factor-authentication-faq.md) bölümünde sorularınızın yanıtlarını alın

