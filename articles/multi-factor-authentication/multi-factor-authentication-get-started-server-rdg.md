---
title: "RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu"
description: "Bu, RADIUS kullanan Uzak Masaüstü (RD) Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ee868c5ba1a8429a733633edbc7efaa74e512135


---
# <a name="remote-desktop-gateway-and-azure-multifactor-authentication-server-using-radius"></a>RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu
Çoğu durumda, Uzak Masaüstü Ağ Geçidi kullanıcıların kimliklerini doğrulamak yerel NPS kullanır. Bu belge, Uzak Masaüstü Ağ Geçidi’ndeki RADIUS isteğini (yerel NPS üzerinden) Multi-Factor Authentication Sunucusu’na yönlendirmeyi açıklar.

Multi-Factor Authentication Sunucusu, daha sonra RADIUS isteğini Uzak Masaüstü Ağ Geçidi Sunucusu’ndaki NPS’ye sunacağı ayrı bir sunucuya yüklenmelidir. NPS kullanıcı adı ve parolayı doğruladıktan sonra, ağ geçidine bir sonuç döndürmeden önce kimlik doğrulamasının ikinci öğesini uygulayan Multi-Factor Authentication Sunucusu’na bir yanıt döndürür.

## <a name="configure-the-rd-gateway"></a>RD Ağ Geçidini yapılandırma
RD Ağ Geçidi bir Azure Multi-Factor Authentication Sunucusu’na RADIUS kimlik doğrulaması gönderecek şekilde yapılandırılmalıdır. RD Ağ Geçidi yüklendikten, yapılandırıldıktan ve çalışmaya başladıktan sonra, RD Ağ Geçidi özelliklerine gidin. RD CAP Deposu sekmesine gidin ve NPS çalıştıran Yerel sunucu yerine NPS çalıştıran Merkez sunucusu kullanacak şekilde değiştirin. RADIUS sunucuları olarak bir ya da daha fazla Azure Multi-Factor Authentication Sunucusu ekleyin ve her sunucu için paylaşılan gizlilik belirtin.

## <a name="configure-nps"></a>NPS yapılandırma
RD Ağ Geçidi, Azure Multi-Factor Authentication’a RADIUS isteği göndermek için NPS kullanır. Multi-factor authentication tamamlanmadan önce RD Ağ Geçidi’nin zaman aşımını önleyecek şekilde zaman aşımı değiştirilmelidir. NPS’yi yapılandırmak için aşağıdaki yordamı kullanın:

1. NPS’de, sol sütunda RADIUS İstemcileri’ni ve Sunucu menüsünü genişletin ve Uzak RADIUS Sunucu Grupları’na tıklayın. TS AĞ GEÇİDİ SUNUCU GRUBU özelliklerine gidin. Görüntülenen RADIUS Sunucularını düzenleyin ve Yük Dengeleme sekmesine gidin. “İsteği bırakılmış olarak kabul etmeden önce yanıt almadan beklenecek süre (saniye):” ve “Sunucunun kullanılamaz olduğu belirlendiğinde gönderilecek istekler arasındaki süre (saniye):” değerlerini 30-60 saniye olarak değiştirin. Kimlik Doğrulama/Hesap sekmesine tıklayın ve belirtilen RADIUS bağlantı noktalarının Multi-Factor Authentication Sunucusu’nun dinleyeceği bağlantı noktalarıyla eşleştiğinden emin olun. 
2. NPS ayrıca Azure Multi-Factor Authentication Sunucusu’ndan gelen RADIUS kimlik doğrulamalarını da alacak şekilde yapılandırılmalıdır. Soldaki menüde RADIUS İstemcileri’ne tıklayın. RADIUS istemcisi olarak Azure Multi-Factor Authentication Sunucusu ekleme Kolay bir ad seçin ve paylaşılan gizlilik belirtin.
3. Sol gezinme bölmesindeki İlkeler bölümünü genişletin ve Bağlantı İsteği İlkeleri’ne tıklayın. Bu, RD Ağ Geçidi yapılandırıldığında oluşturulmuş, TS AĞ GEÇİDİ KİMLİK DOĞRULAMASI İLKESİ adlı bir Bağlantı İsteği İlkesi içermelidir. Bu ilke RADIUS isteklerini Multi-Factor Authentication Sunucusu’na iletir.
4. Yeni bir tane oluşturmak için bu ilkeyi kopyalayın. Yeni ilkede, Azure Multi-Factor Authentication Sunucusu RADIUS istemcisi için yukarıda 2. adımda ayarladığınız Kolay adla eşleşen bir İstemci Kolay Adı koşulu ekleyin. Kimlik Doğrulama Sağlayıcısı’nı Yerel Bilgisayar olarak değiştirin. Bu ilke, Azure Multi-Factor Authentication Sunucusu’ndan bir RADIUS isteği alındığında, döngü koşuluna neden olacak şekilde RADIUS isteğini Azure Multi-Factor Authentication Sunucusu’na geri göndermek yerine, kimlik doğrulamasının yerel olarak gerçekleştirilmesini sağlar. Döngü koşulunu önlemek için, bu yeni ilke Multi-Factor Authentication Sunucusu’na ileten özgün ilkenin ÜZERİNDE sıralanmalıdır.

## <a name="configure-azure-multifactor-authentication"></a>Azure Multi-Factor Authentication’ı yapılandırma
- - -
Azure Multi-Factor Authentication Sunucusu, RD Ağ Geçidi ile NPS arasında bir RADIUS proxy olarak yapılandırılmıştır.  RD AĞ Geçidi sunucusundan ayrı bir etki alanına katılmış sunucuya yüklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın.

1. Azure Multi-Factor Authentication Sunucusu’nu açın ve RADIUS Kimlik Doğrulaması simgesine tıklayın. RADIUS kimlik doğrulamasını etkinleştir onay kutusunu işaretleyin.
2. İstemciler sekmesinde, bağlantı noktalarının NPS’de yapılandırılanlarla eşleştiğinden emin olun ve Ekle düğmesine... tıklayın. RD Ağ Geçidi sunucusu IP adresi, uygulama adı (isteğe bağlı) ve paylaşılan gizlilik ekleyin. Paylaşılan gizliliğin Azure Multi-Factor Authentication Sunucusu’nda ve RD Ağ Geçidi’nde aynı olması gerekir.
3. Hedef sekmesine tıklayın ve RADIUS sunucuları radyo düğmesini seçin.
4. Ekle düğmesine... tıklayın. IP adresi, paylaşılan gizlilik ve NPS sunucusu bağlantı noktalarını girin. Merkezi NPS kullanmadığınız sürece, RADIUS istemcisi ve RADIUS hedefi aynı olacaktır. Paylaşılan gizliliğin, NPS sunucusunun RADIUS istemcisi bölümündekiyle eşleşmesi gerekir.

![Radius Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-rdg/radius.png)




<!--HONumber=Nov16_HO2-->


