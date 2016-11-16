---
title: "Kullanıcılarınız için Azure AD&quot;ye Katılımı ayarlama | Microsoft Belgeleri"
description: "Yöneticilerin, şirket içi dizin ve cihaz kaydı için Azure AD Katılımını nasıl ayarlayacakları açıklanmaktadır."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/27/2016
ms.author: femila
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aaa52cdecc09adb3b7ca53e0c0283d4203b21810


---
# <a name="setting-up-azure-ad-join-in-your-organization"></a>Kuruluşunuzda Azure AD'ye Katılımı ayarlama
Azure Active Directory Katılımını (Azure AD Katılımı) ayarlamadan önce kullanıcıların şirket içi dizinini buluta eşitlemeniz veya Azure AD'de yönetilen hesaplar oluşturmanız gerekir.

Şirket içi kullanıcılarınızı Azure AD'ye eşitlemeye ilişkin ayrıntılı yönergeler için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

Azure AD'de el ile kullanıcı oluşturmak ve yönetmek için bkz. [Azure AD'de kullanıcı yönetimi](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Cihaz kaydı oluşturma
1. Azure portalında yönetici olarak oturum açın.
2. Sol bölmede **Active Directory**'yi seçin.
3. **Directory (Dizin)** sekmesinde dizininizi seçin.
4. **Configure (Yapılandır)** sekmesini seçin.
5. **Devices (Cihazlar)** bölümüne gidin.
6. **Devices (Cihazlar)** sekmesinde şunları ayarlayın:  
   * **MAXIMUM NUMBER OF DEVICES PER USER (KULLANICI BAŞINA MAKSİMUM CİHAZ SAYISI)**: Kullanıcıların Azure AD'de sahip olabileceği maksimum cihaz sayısını belirleyin.  Bu kotayı dolduran bir kullanıcı, var olan cihazlarının bir veya birden fazlasını kaldırmadan yeni cihaz ekleyemez.
   * **REQUIRE MULTI-FACTOR AUTH TO JOIN DEVICES (CİHAZLARIN KATILIMI İÇİN MULTI-FACTOR AUTHENTICATION'I GEREKLİ KIL)**: Kullanıcıların cihazlarını Azure AD'ye eklemeleri için ikinci bir kimlik doğrulama faktörü sağlamalarının gerekip gerekmediğini belirleyin. Azure Multi-Factor Authentication hakkında daha fazla bilgi edinmek için bkz. [Bulutta Azure Multi-Factor Authentication ile çalışmaya başlama](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
   * **USERS MAY AZURE AD JOIN DEVICES (KULLANICILAR AZURE AD'YE CİHAZ KATABİLİR)**: Cihazlarını Azure AD'ye ekleme izni olan kullanıcıları ve grupları seçin.
   * **ADDITIONAL ADMINISTRATORS ON AZURE AD JOINED DEVICES (AZURE AD'YE KATILAN CİHAZLAR İÇİN EK YÖNETİCİLER)**: Azure AD Premium veya Enterprise Mobility Suite (EMS) ile hangi kullanıcılara cihaz için yerel yönetici haklarının verileceğini belirleyebilirsiniz. Varsayılan olarak genel yöneticilere ve cihaz sahiplerine yerel yönetici hakları verilir.

<center>![Cihaz kaydı ayarlama](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>

Kullanıcılarınız için Azure AD'ye Katılımı ayarladığınızda kurumsal veya kişisel cihazları üzerinden Azure AD'ye bağlanabilirler.

Kullanıcılarınızın Azure AD Katılımını ayarlamalarını sağlamak üzere şu üç senaryoyu kullanabilirsiniz:

* Kullanıcılar, şirketlerine ait cihazları doğrudan Azure AD'ye ekler.
* Kullanıcılar, şirketlerine ait bir cihazı şirket içi Active Directory'deki etki alanına ekler ve ardından cihazı Azure AD'ye genişletir.
* Kullanıcılar, kişisel bir cihazdan Windows'a iş veya okul hesabı ekler.

## <a name="additional-information"></a>Ek bilgiler
* [Kurumlar için Windows 10: Cihazları iş için kullanmanın yolları](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory Join ile bulut işlevlerini Windows 10 cihazlarına genişletme](active-directory-azureadjoin-user-upgrade.md)
* [Azure AD Katılımı kullanım senaryoları hakkında bilgi edinin](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 deneyiminden faydalanmak için etki alanına katılan cihazları Azure AD'ye bağlama](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD'ye Katılım ayarlama](active-directory-azureadjoin-setup.md)




<!--HONumber=Nov16_HO2-->


