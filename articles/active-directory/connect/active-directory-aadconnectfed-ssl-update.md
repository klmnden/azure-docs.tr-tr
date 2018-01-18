---
title: "Azure AD Connect - güncelleştirme SSL sertifikası için bir AD FS grubunu | Microsoft Docs"
description: "Bu belge ayrıntıları Azure AD Connect kullanarak AD FS grubunda SSL sertifikasını güncelleştirmek için adımları izleyin."
services: active-directory
keywords: "Azure ad connect, adfs ssl güncelleştirme, adfs sertifika güncelleştirme, değişiklik adfs sertifika, yeni adfs sertifika, adfs sertifika ssl sertifikası, güncelleştirme ssl sertifikası adfs, adfs ssl sertifikası, adfs, ssl, sertifika, adfs hizmet iletişimi sertifikasına, güncelleştirme Federasyon yapılandırma, güncelleştirme adfs Federasyon yapılandırma, aad bağlanma"
authors: anandyadavmsft
manager: mtillman
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: anandy
ms.custom: seohack1
ms.openlocfilehash: b31a4d178d287eba275a0072936b4222a2c84346
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="update-the-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Bir Active Directory Federasyon Hizmetleri (AD FS) grubu için SSL sertifikasını güncelleştir

## <a name="overview"></a>Genel Bakış
Bu makalede, bir Active Directory Federasyon Hizmetleri (AD FS) grubu için SSL sertifikası güncelleştirmek için Azure AD Connect nasıl kullanabileceğinizi açıklar. Seçili kullanıcı oturum açma yöntemi, AD FS olmasa bile, SSL sertifikası AD FS grubu için kolayca güncelleştirmek için Azure AD Connect aracını kullanabilirsiniz.

Tüm Federasyon ve üç basit adımda Web uygulaması Ara sunucusu (WAP) sunucuları arasında AD FS grubu için SSL sertifikası güncelleştirme tüm işlem gerçekleştirebilirsiniz:

![Üç adımı](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
>AD FS tarafından kullanılan sertifikalar hakkında daha fazla bilgi için bkz: [AD FS tarafından kullanılan sertifikaları anlama](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Önkoşullar

* **AD FS grubu**: AD FS grubunuzu tabanlı Windows Server 2012 R2 veya sonrası olduğundan emin olun.
* **Azure AD Connect**: Azure AD Connect sürümü 1.1.553.0 olduğundan emin olun ya da daha yüksek. Görev kullanacağınız **güncelleştirme AD FS SSL sertifikası**.

![Güncelleştirme SSL görevi](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>1. adım: AD FS grubu bilgileri sağlayın

Azure AD Connect tarafından otomatik olarak AD FS grubu hakkında bilgi edinmek çalışır:
1. AD FS (Windows Server 2016 veya sonraki) grubu bilgileri sorgulama.
2. Bilgi önceki dosyadan başvuran, hangi Azure AD Connect ile yerel olarak depolanır.

Ekleyerek veya kaldırarak AD FS grubunu geçerli yapılandırmasını yansıtacak şekilde sunucuları görüntülenen sunucularının listesini değiştirebilirsiniz. Sunucu bilgilerini sağlanan hemen Azure AD Connect bağlantısı ve geçerli SSL sertifikası durumu görüntüler.

![AD FS sunucu bilgisi](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

Listede artık AD FS grubunun parçası olan bir sunucu varsa tıklatın **kaldırmak** sunucu, AD FS grubunuzdaki sunucuların listesinden silmek için.

![Çevrimdışı sunucu listesinde](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Bir sunucu için bir AD FS grubunu Azure AD CONNECT'te sunucuların listesinden kaldırmak, yerel bir işlemdir ve Azure AD Connect yerel olarak tutar AD FS grubu bilgilerini güncelleştirir. Azure AD Connect değişikliği yansıtmak için AD FS yapılandırmasına değiştirmeyen.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>2. adım: yeni bir SSL sertifikası sağlayın

AD FS grubu sunucuları hakkında bilgi onaylanıp sonra Azure AD Connect için yeni SSL sertifikası ister. Yüklemeye devam etmek için bir parola korumalı PFX sertifikası sağlayın.

![SSL sertifikası](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

Sertifika verdikten sonra Azure AD Connect Önkoşullar bir dizi aracılığıyla gider. Sertifika AD FS grubu için doğru olduğundan emin olmak için sertifika doğrulayın:

-   Sertifikanın konu adı/alternatif konu adı ya da Federasyon Hizmeti adıyla aynı olduğundan veya bir joker sertifikası.
-   Sertifika 30 günden fazla bir süre için geçerlidir.
-   Sertifika güven zinciri geçerli değil.
-   Parola korumalı sertifikasıdır.

## <a name="step-3-select-servers-for-the-update"></a>3. adım: güncelleştirmeyi sunucuları seçin

Sonraki adımda, güncelleştirilmiş SSL sertifikasına sahip olmasını gerektiren sunucuları seçin. Çevrimdışı sunucuları güncelleştirmesi seçilemez.

![Güncelleştirme için sunucuları seçin](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

Yapılandırmayı tamamladıktan sonra Azure AD Connect güncelleştirme durumunu gösterir ve AD FS oturum açma doğrulamak için bir seçenek sağlayan iletisi görüntülenir.

![Yapılandırma tamamlandı](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>SSS

* **Hangi sertifikanın konu adı için yeni AD FS SSL sertifikası olmalı?**

    Azure AD Connect, sertifika konu adı/alternatif konu adı Federasyon Hizmeti adı içerip içermediğini denetler. Örneğin, Federasyon Hizmeti adınız fs.contoso.com ise, konu adı/alternatif konu adı fs.contoso.com olmalıdır.  Joker karakterli sertifikalar da kabul edilir.

* **Neden kimlik bilgilerini WAP sunucusu sayfasında yeniden soruluyor?**

    AD FS sunucularına bağlanmak için verdiğiniz kimlik bilgileri WAP sunucuları yönetmek için ayrıcalığına sahip değilseniz, Azure AD Connect WAP sunucularında yönetici ayrıcalıklarına sahip kimlik bilgilerini ister.

* **Sunucu çevrimdışı olarak gösterilir. Ne yapmalıyım?**

    Sunucu çevrimdışı ise, azure AD Connect hiçbir işlem gerçekleştirilemiyor. AD FS grubunun parçası sunucusuysa sunucusuyla bağlantıyı denetleyin. Sorunun giderilmiş sonra Sihirbazı'nda durumunu güncelleştirmek için Yenile simgesine basın. Parçası sunucusuysa grubu daha önce ancak şimdi artık yok.'ye tıklayın **kaldırmak** bu Azure AD Connect sunucuların listesinden silmek için korur. Azure AD Connect listesinden sunucusunun kaldırılması, AD FS yapılandırmasını değiştirmez. AD FS Windows Server 2016 veya sonraki sürümlerde, yapılandırma ayarlarında sunucu kalır kullanıyorsanız ve yeniden sonraki açışınızda gösterilecek görev çalıştırılır.

* **Yeni SSL sertifikası ile bir alt grup Sunucularım kümesini güncelleştirebilir miyim?**

    Evet. Görev her zaman çalıştırabilirsiniz **güncelleştirme SSL sertifikası** yeniden geri kalan sunucular güncelleştirmek için. Üzerinde **Select sunucuları için SSL sertifika güncelleştirme** sayfasında, üzerinde sunucularının listesini sıralayabilirsiniz **SSL sona erme tarihi** kolayca henüz güncelleştirilmemiş sunuculara erişmek için.

* **Sunucunun önceki çalışmada kaldırdım, ancak bunu hala çevrimdışı ve listelenen olarak AD FS sunucuları sayfasında gösteriliyor. Neden kaldırdım daha sonra çevrimdışı sunucu hala var mı?**

    Azure AD Connect listesinden sunucusunun kaldırılması, AD FS yapılandırması kaldırmaz. Azure AD Connect grubu hakkındaki tüm bilgileri için AD FS (Windows Server 2016 veya daha yüksek) başvuruyor. Sunucu AD FS yapılandırmasında hala varsa, geri listesinde listelenir.  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Connect ve federasyon](active-directory-aadconnectfed-whatis.md)
- [Active Directory Federasyon Hizmetleri Yönetimi ve Azure AD Connect ile özelleştirme](active-directory-aadconnect-federation-management.md)
