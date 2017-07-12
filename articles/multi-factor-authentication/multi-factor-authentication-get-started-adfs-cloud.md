---
title: "Azure MFA ve AD FS ile bulut kaynaklarını güvenli hale getirme | Microsoft Docs"
description: "Bu, bulutta nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 6cf4ec4f777ea1f2b852945ab82da2547946f378
ms.contentlocale: tr-tr
ms.lasthandoff: 06/08/2017

---

<a id="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs" class="xliff"></a>

# Azure Multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme
Kuruluşunuz Azure Active Directory ile birleştiriliyorsa, Azure AD’nin erişebildiği kaynakları güvenli hale getirmek için Azure Multi-Factor Authentication ya da Active Directory Federation Services (AD FS) kullanın. Azure Active Directory kaynaklarını Azure Multi-Factor Authentication ya da Active Directory Federasyon Hizmetleri ile güvenli hale getirmek için aşağıdaki yordamları kullanın.

<a id="secure-azure-ad-resources-using-ad-fs" class="xliff"></a>

## AD FS kullanarak Azure AD kaynaklarını güvenli hale getirme
Bulut kaynağınızın güvenliğini sağlamak için, kullanıcı iki adımlı doğrulamayı başarıyla gerçekleştirdiğinde Active Directory Federation Services tarafından multipleauthn talebinin yayılması için bir talep kuralı oluşturun. Bu talep Azure AD'ye aktarılır. İlerlemek için bu yordamı izleyin:


1. AD FS Yönetimi'ni açın.
2. Solda, **Bağlı Olan Taraf Güvenleri**’ni seçin.
3. **Microsoft Office 365 Kimlik Platformu**'na sağ tıklayın ve **Talep Kurallarını Düzenle**'yi seçin.

   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.

   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde **Gelen Talep için Geçiş ya da Filtre**’yi seçin ve **İleri**’ye tıklayın.

   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Kuralınıza bir ad verin. 
7. Gelen talep türü olarak **Kimlik Doğrulama Yöntemleri Başvuruları**’nı seçin.
8. **Tüm talep değerlerini geçir**’i seçin.
    ![Dönüşüm Talep Kuralı Ekleme Sihirbazı](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. **Son**'a tıklayın. AD FS Yönetim Konsolu'nu kapatın.

<a id="trusted-ips-for-federated-users" class="xliff"></a>

## Federasyon kullanıcıları için Güvenilen IP'ler
Güvenilen IP'ler yöneticilerin, belirli IP adresleri ya da kendi intranetlerinden kaynaklanan taleplere sahip federasyon kullanıcıları için iki aşamalı doğrulamayı atlamasına izin verir. Aşağıdaki bölümlerde, federasyon kullanıcıları intranetinden gelen talepler için Azure Multi-Factor Authentication Güvenilen IP’lerinin federasyon kullanıcılarıyla yapılandırılması ve iki aşamalı doğrulamanın atlanması açıklanmıştır. Bu, AD FS’nin bir geçiş kullanacak ya da Kurumsal Ağ İçinde talep türü ile gelen bir talep şablonunu filtreleyecek şekilde yapılandırılmasıyla gerçekleştirilir.

Bu örnekte Güvenilen Taraf Güvenlerimiz için Office 365 kullanılmıştır.

<a id="configure-the-ad-fs-claims-rules" class="xliff"></a>

### AD FS talep kurallarını yapılandırma
Yapmamız gereken ilk şey, AD FS taleplerini yapılandırmaktır. Biri Kurumsal Ağ İçinde talep türü için, diğeriyse kullanıcıların oturumunu açık şekilde tutmak için olmak üzere iki talep kuralı oluşturun.

1. AD FS Yönetimi'ni açın.
2. Solda, **Bağlı Olan Taraf Güvenleri**’ni seçin.
3. **Microsoft Office 365 Kimlik Platformu**’na sağ tıklayın ve **Talep Kurallarını Düzenle…** seçeneğini belirleyin.
   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.
   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde **Gelen Talep için Geçiş ya da Filtre**’yi seçin ve **İleri**’ye tıklayın.
   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Talep kuralı adının yanındaki kutuda kuralınıza bir ad verin. Örneğin: InsideCorpNet.
7. Gelen talep türü’nün yanındaki açılır menüde, **Kurumsal Ağ İçinde** seçeneğini belirleyin.
   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. **Son**'a tıklayın.
9. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.
10. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüden **Talepleri Özel Bir Kural Kullanarak Gönder**’i seçin ve **İleri**’ye tıklayın.
11. Talep kuralı adı: altındaki kutuya *Kullanıcıların Oturumlarını Açık Tut* ifadesini girin.
12. Özel kural kutusuna şunu girin:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. **Finish (Son)** düğmesine tıklayın.
14. **Uygula**'ya tıklayın.
15. **Tamam**’a tıklayın.
16. AD FS Yönetimi'ni kapatın.

<a id="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users" class="xliff"></a>

### Azure Multi-Factor Authentication Güvenilen IP’leri Federasyon Kullanıcıları ile Yapılandırma
Talepler yapıldığına göre, artık güvenilen IP’leri yapılandırabiliriz.

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Solda, **Active Directory**'ye tıklayın.
3. Dizin altında, güvenilen IP'leri ayarlamak istediğiniz dizini seçin.
4. Seçtiğiniz Dizinde **Yapılandır**'a tıklayın.
5. Çok faktörlü kimlik doğrulaması bölümünde, **Hizmet ayarlarını yönet**'e tıklayın.
6. Hizmet Ayarları sayfasındaki güvenilen IP'ler altında bulunan **İntranetimde bulunan şirket dışındaki kullanıcıların istekleri için çok öğeli kimlik doğrulamayı atla** seçeneğini belirleyin.  

   ![Bulut](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. **Kaydet**’e tıklayın.
8. Güncelleştirmeler uygulandıktan sonra **Kapat**'a tıklayın.

Bu kadar! Bu noktada, birleştirilmiş Office 365 kullanıcıları yalnızca talep kurumsal intranet dışından kaynaklandığı zaman MFA kullanmalıdır.

