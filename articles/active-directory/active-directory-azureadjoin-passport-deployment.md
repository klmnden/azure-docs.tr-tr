---
title: Microsoft iş için Windows Hello kuruluşunuzda etkinleştirme | Microsoft Docs
description: Microsoft Passport, kuruluşunuzda etkinleştirmek için dağıtım yönergeleri.
services: active-directory
documentationcenter: ''
keywords: Microsoft Passport, Microsoft Windows Hello iş dağıtım için yapılandırma
author: MarkusVi
manager: mtillman
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 0aa16e3466b36b6d1d83308cf37623aa15d61fcb
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2018
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Microsoft iş için Windows Hello kuruluşunuzda etkinleştir
Sonra [Windows 10 etki alanına katılmış cihazları Azure Active Directory ile bağlanma](active-directory-azureadjoin-devices-group-policy.md), Microsoft iş için Windows Hello kuruluşunuzda etkinleştirmek için aşağıdakileri yapın:

1. System Center Configuration Manager dağıtma  
2. İlke ayarlarını yapılandırın
3. Sertifika profili yapılandırma  

## <a name="deploy-system-center-configuration-manager"></a>System Center Configuration Manager dağıtma
Windows Hello üzerinde iş tuşları tabanlı kullanıcı sertifikaları dağıtmak için aşağıdakiler gerekir:

* **System Center Configuration Manager geçerli dalının** -sürüm 1606 ya da daha iyi yüklemeniz gerekir. Daha fazla bilgi için bkz: [System Center Configuration Manager belgeleri](https://technet.microsoft.com/library/mt346023.aspx) ve [System Center Configuration Manager ekip blogu](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
* **Ortak anahtar altyapısı (PKI)** - Microsoft iş için Windows Hello kullanıcı sertifikaları kullanarak etkinleştirmek için bir PKI yerinde olması gerekir. Yoksa veya kullanıcı sertifikaları için kullanmak istemiyorsanız, Windows Server 2016 yapı 10551 (veya üzeri) yüklü olan yeni bir etki alanı denetleyicisi dağıtabilirsiniz. Adımlarını izleyin [varolan bir etki alanında etki alanı Denetleyicisi'nin çoğaltmasını yükleme](https://technet.microsoft.com/library/jj574134.aspx) veya [yeni bir ortam oluşturuyorsanız yeni bir Active Directory ormanı yüklemek](https://technet.microsoft.com/library/jj574166). (ISO'ları indirmek için kullanılabilir olan [Signiant medya Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)

## <a name="configure-policy-settings"></a>İlke ayarlarını yapılandırın
Microsoft için Windows Hello iş ilke ayarlarını yapılandırmak için iki seçeneğiniz vardır:

* Active Directory'de Grup İlkesi 
* System Center Configuration Manager 

Grup İlkesi kullanarak Active Directory, Microsoft Windows Hello iş ilke ayarlarını yapılandırmak için önerilen yöntemdir. 

Ayrıca sertifikaları dağıtmak için kullandığınız zaman System Center Configuration Manager kullanarak tercih edilen yöntemdir. Bu senaryo:

* Yeni dağıtım senaryoları ile uyumluluk sağlar
* İstemci tarafında Windows 10 sürüm 1607 ya da daha iyi gerektirir.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Microsoft iş için Windows Hello Active Directory'de Grup İlkesi aracılığıyla yapılandırın
**Adımları**:

1. Sunucu Yöneticisi'ni açın ve gidin **Araçları** > **Grup İlkesi Yönetimi**.
2. Grup İlkesi Yönetimi'nden Azure AD katılım etkinleştirmek istediğiniz etki alanı için karşılık gelen etki alanı düğümü gidin.
3. Sağ **Grup İlkesi nesneleri**seçip **yeni**. Grup İlkesi nesnesi, örneğin, etkinleştirmek için Windows Hello iş adlandırın. **Tamam**’a tıklayın.
4. Yeni Grup İlkesi nesneniz sağ tıklayın ve ardından **Düzenle**.
5. Gidin **Bilgisayar Yapılandırması** > **ilkeleri** > **Yönetim Şablonları** > **Windows bileşenleri** > **iş için Windows Hello**.
6. Sağ **etkinleştirmek için Windows Hello iş**ve ardından **Düzenle**.
7. Seçin **etkin** seçenek düğmesine ve ardından **Uygula**. **Tamam**’a tıklayın.
8. Bu gibi durumlarda, Grup İlkesi nesnesi artık bir konuma bağlayabilirsiniz. Tüm kuruluşunuzda etki alanına katılmış Windows 10 cihazları için bu ilkeyi etkinleştirmek için Grup İlkesi etki alanına bağlayın. Örneğin:
   * Windows 10 etki alanına katılmış bilgisayarları nerede bulunacağı Active Directory'de belirli kuruluş birimi (OU)
   * Azure AD ile kayıtlı otomatik olarak Windows 10 etki alanına katılmış bilgisayarları içeren belirli bir güvenlik grubu

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>System Center Configuration Manager kullanarak iş için Windows Hello yapılandırın
**Adımları**:

1. Açık **System Center Configuration Manager**ve ardından gidin **varlıklar ve uyumluluk > uyumluluk ayarları > Şirket kaynağına erişim > İş profilleri için Windows Hello**.
   
    ![İş için Windows Hello yapılandırın](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. Üstteki araç çubuğunda tıklatın **oluşturma iş için Windows Hello profili**.
   
    ![İş için Windows Hello yapılandırın](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. Üzerinde **genel** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![İş için Windows Hello yapılandırın](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    a. İçinde **adı** metin kutusuna, bu gibi bir durumda profil için bir ad yazın **WHfB Profilim**.
   
    b. **İleri**’ye tıklayın.
4. Üzerinde **desteklenen platformlar** iletişim kutusunda, ile iş profili için Windows Hello sağlanacak ve ardından platformları seçin **sonraki**.
   
    ![İş için Windows Hello yapılandırın](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. Üzerinde **ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![İş için Windows Hello yapılandırın](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    a. Olarak **yapılandırmak için Windows Hello iş**seçin **etkin**.
   
    b. Olarak **bir Güvenilir Platform Modülü (TPM) kullanmak**seçin **gerekli**. 
   
    c. Olarak **kimlik doğrulama yöntemini**seçin **sertifika tabanlı**.
   
    d. **İleri**’ye tıklayın.
6. Üzerinde **Özet** iletişim kutusunda, tıklatın **sonraki**.
7. Üzerinde **tamamlama** iletişim kutusunda, tıklatın **Kapat**.
8. Üstteki araç çubuğunda tıklatın **dağıtma**.
   
    ![İş için Windows Hello yapılandırın](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-the-certificate-profile"></a>Sertifika profili yapılandırma
Şirket içi kimlik doğrulaması için sertifika tabanlı kimlik doğrulaması kullanıyorsanız, yapılandırmak ve bir sertifika profili dağıtmak gerekir. Bu görev bir NDES sunucusu ve sertifika kayıt noktası site rolü System Center Configuration Manager'ın oluşturmanızı gerektirir. Daha fazla ayrıntı için bkz: [Configuration Manager'da sertifika profilleri için Önkoşullar](https://technet.microsoft.com/library/dn261205.aspx).

1. Açık **System Center Configuration Manager**ve ardından gidin **varlıklar ve uyumluluk > uyumluluk ayarları > Şirket kaynağına erişim > sertifika profilleri**.
2. Akıllı kart oturum açma genişletilmiş anahtar kullanımı (EKU) sahip bir şablon seçin.

Üzerinde **SCEP kaydı** sayfa sertifika profili seçmeniz gerekir. **yükle, yoksa başarısız iş için Passport** olarak **anahtar depolama sağlayıcısı**.

## <a name="next-steps"></a>Sonraki adımlar
* [Kurumlar için Windows 10: Cihazları iş için kullanmanın yolları](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory Join ile bulut işlevlerini Windows 10 cihazlarına genişletme](active-directory-azureadjoin-user-upgrade.md)
* [Parolasız kimlik doğrulama](active-directory-azureadjoin-passport.md)
* [Azure AD Katılımı kullanım senaryoları hakkında bilgi edinin](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 deneyiminden faydalanmak için etki alanına katılan cihazları Azure AD'ye bağlama](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD'ye Katılım ayarlama](active-directory-azureadjoin-setup.md)

