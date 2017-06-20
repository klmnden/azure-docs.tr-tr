---
title: "RADIUS Kimlik Doğrulaması ve Azure MFA Sunucusu | Microsoft Belgeleri"
description: "Bu, RADIUS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 20afeb3ba290ddf728d2b52c076c7a57fadc77c6
ms.openlocfilehash: e696b95c9db86b062440f0c4fd788bf97223317a
ms.contentlocale: tr-tr
ms.lasthandoff: 02/28/2017

---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>RADIUS kimlik doğrulamasını ve Azure Multi-Factor Authentication Sunucusuyla tümleştirme
RADIUS kimlik doğrulamasını etkinleştirmek ve yapılandırmak için Azure MFA Sunucusu’nun RADIUS Kimlik Doğrulaması bölümünü kullanın. RADIUS, kimlik doğrulama isteklerini kabul etmek ve bu istekleri işlemek için standart bir protokoldür. Azure Multi-Factor Authentication Sunucusu bir RADIUS sunucusu işlevi görür. Azure Multi-Factor Authentication eklemek istiyorsanız MFA Sunucusu’nu RADIUS istemciniz (VPN gereci) ile Active Directory (AD), LDAP dizini ya da başka bir RADIUS sunucusu olabilecek kimlik doğrulama hedefiniz arasına ekleyin. Azure Multi-Factor Authentication’ın (MFA) çalışması için Azure MFA Sunucusu’nu hem istemci sunucuları hem de kimlik doğrulama hedefi ile iletişim kurabilecek şekilde yapılandırmalısınız. Azure MFA Sunucusu, RADIUS istemcisinden gelen istekleri kabul eder, kimlik doğrulama hedefine göre kimlik bilgilerini doğrular, Azure Multi-Factor Authentication ekler ve RADIUS istemcisine bir yanıt döndürür. Kimlik doğrulama isteği yalnızca hem birincil kimlik doğrulamasının hem de Azure Multi-Factor Authentication’ın başarılı olması durumunda başarılı olur.

> [!NOTE]
> MFA sunucusu, RADIUS sunucusu olarak hareket ederken, yalnızca PAP (parola kimlik doğrulama protokolü) ve MSCHAPv2 (Microsoft Karşılıklı Kimlik Doğrulama Protokolü ) RADIUS protokollerini destekler.  MFA sunucusu bu protokolü destekleyen başka bir RADIUS sunucusu için RADIUS proxy işlevi görüyorsa, EAP (genişletilebilir kimlik doğrulama protokolü) gibi diğer protokoller kullanılabilir.
>
> Bu yapılandırmada, MFA Sunucusu alternatif protokolleri kullanarak başarılı bir RADIUS Sınama yanıtı başlatamadığından, tek yönlü SMS ve OATH belirteçleri çalışmaz.

![Radius Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>RADIUS istemcisi ekleme
RADIUS kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Bir Active Directory ortamınız varsa, sunucu ağ içindeki etki alanına eklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın:

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde RADIUS Kimlik Doğrulaması simgesine tıklayın.
2. **RADIUS kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin.
3. Azure MFA RADIUS hizmetinin RADIUS isteklerini standart olmayan bağlantı noktalarında dinlemesi gerekiyorsa, İstemciler sekmesinden Kimlik Doğrulama ve Hesap Oluşturma bağlantı noktalarını değiştirin.
4. **Ekle**'ye tıklayın.
5. Azure Multi-Factor Authentication Sunucusu için kimlik doğrulaması yapacak gerecin/sunucunun IP adresini, bir uygulama adı (isteğe bağlı) ve paylaşılan bir gizli dizi girin.

  Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.

  Paylaşılan gizli dizinin Azure Multi-Factor Authentication Sunucusu’nda ve gereçte/sunucuda aynı olması gerekir.

6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, **Multi-Factor Authentication İste kullanıcı eşleme** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.
7. Mobil doğrulama uygulamalarınızdan edindiğiniz OATH parolalarını bant dışı telefon araması, SMS veya anında iletme bildirimlerinin yedeği olarak kullanmak istiyorsanız **Yedek OATH belirtecini etkinleştir** kutusunu işaretleyin.
8. **Tamam** düğmesine tıklayın.

4 adımdan 8 adıma kadar yapılan işlemleri tekrarlayarak dilediğiniz kadar RADIUS istemcisi ekleyebilirsiniz.

## <a name="configure-your-radius-client"></a>RADIUS istemcinizi yapılandırma

1. **Hedef** sekmesine tıklayın.
2. Azure MFA Sunucusu Active Directory ortamında etki alanına katılmış bir sunucuda yüklüyse, Windows etki alanını seçin.
3. Kullanıcılara LDAP dizinine göre kimlik doğrulaması uygulanması gerekiyorsa **LDAP bağlaması**’nı seçin.

  LDAP bağlamasını kullanmak istiyorsanız Sunucu’nun dizininize bağlanabilmesi için Dizin Tümleştirme simgesine tıklayın ve Ayarlar sekmesinde LDAP yapılandırmasını düzenleyin. LDAP yapılandırma yönergeleri [LDAP Proxy yapılandırma kılavuzunda](multi-factor-authentication-get-started-server-ldap.md) bulunabilir.

4. Kullanıcılara başka bir RADIUS sunucusuna göre kimlik doğrulaması yapılması gerekiyorsa, RADIUS sunucularını seçin.
5. Azure MFA Sunucusu’nun RADIUS istekleri için proxy olarak kullanacağı sunucuyu yapılandırmak için **Ekle**’ye tıklayın.
6. RADIUS Sunucusu Ekle iletişim kutusuna RADIUS sunucusunun IP adresi İle paylaşılan bir gizli dizi girin.

  Paylaşılan gizli dizinin Azure Multi-Factor Authentication Sunucusu’nda ve RADIUS sunucusunda aynı olması gerekir. RADIUS sunucusu tarafından farklı bağlantı noktaları kullanılıyorsa, Kimlik Doğrulama bağlantı noktasını ve Hesap bağlantı noktasını değiştirin.

7. **Tamam** düğmesine tıklayın.
8. Azure MFA Sunucusu’ndan gönderilen erişim isteklerini işleyebilmesi için Azure MFA Sunucusu’nu başka bir RADIUS sunucusunda RADIUS istemcisi olarak ekleyin. Azure Multi-Factor Authentication Sunucusu’nda yapılandırılanla aynı paylaşılan gizli diziyi kullanın.

Bu adımları tekrarlayarak daha fazla RADIUS sunucusu ekleyebilir; **Yukarı Taşı** ve **Aşağı Taşı** düğmeleriyle Azure MFA Sunucusu’nun bunları çağıracağı sırayı yapılandırabilirsiniz.

Bu Azure Multi-Factor Authentication Sunucusu yapılandırmasını tamamlar. Sunucu artık yapılandırılan istemcilerden gelen RADIUS erişim istekleri için yapılandırılan bağlantı noktalarını dinler.   

## <a name="radius-client-configuration"></a>RADIUS İstemcisi yapılandırması
RADIUS istemcisini yapılandırmak için yönergeleri kullanın:

* Gerecinizi/sunucunuzu, RADIUS sunucusu olarak hareket edecek Azure Multi-Factor Authentication Sunucusu’nun IP adresi için RADIUS aracılığıyla kimlik doğrulaması yapacak şekilde yapılandırın.
* Daha önce yapılandırılanla aynı paylaşılan gizli diziyi kullanın.
* Kullanıcının kimlik bilgilerini doğrulama, iki aşamalı kimlik doğrulaması gerçekleştirme, bunların yanıtını alma ve sonra RADIUS erişim isteğini yanıtlamaya yetecek kadar zaman olması için RADIUS zaman aşımını 30-60 saniye olarak yapılandırın.

