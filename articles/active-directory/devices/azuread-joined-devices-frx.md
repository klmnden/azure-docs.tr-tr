---
title: İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihazını ekleme | Microsoft Docs
description: Nasıl kullanıcıları Azure AD'ye katılımı ayarlama ilk çalıştırma deneyimi sırasında ayarlayabilirsiniz açıklayan bir konu.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1376f011d056aac33333f6ac31ee2eaadaf3ef4a
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415004"
---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihazını ekleme

İle cihaz Yönetimi Azure Active Directory'de (Azure AD) güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimine giriş](overview.md).

Windows 10 ile yeni bir cihaz ilk kez çalıştırma deneyimi (FRX) sırasında Azure AD'ye katılabilirsiniz.  
Bu, çalışanlarınıza veya Öğrenciler naylon cihazlara dağıtmak sağlar.

Windows 10 Professional veya Windows 10 Enterprise ' ın bir cihazda yüklü varsa deneyimi şirketinize ait cihazlar Kurulum işlemi varsayılan olarak.

Windows içinde *out-of-box deneyimini*, bir şirket içi Active Directory (AD) etki alanına katılma desteklenmiyor. Bilgisayarı, Kurulum sırasında bir AD etki alanına planlıyorsanız, bağlantı seçmelisiniz **Windows ile yerel bir hesap ayarlamak**. Ardından, etki alanından ayarları bilgisayarınızda katılabilirsiniz.
 


## <a name="before-you-begin"></a>Başlamadan önce

Windows 10 cihazı alanına katılmak için cihaz Kayıt Hizmeti'ni aygıtlarını kaydetmesini sağlamak için yapılandırılmalıdır. Azure AD kiracınıza cihazları katılma izni sahip olmaya ek olarak, daha az cihazları yapılandırılmış en fazla kayıtlı olması gerekir. Daha fazla bilgi için [cihaz ayarlarını yapılandırma](device-management-azure-portal.md#configure-device-settings).

Ayrıca, kiracınızın Federasyon kimlik sağlayıcınız WS-Federasyon ve WS-Trust uç noktası kullanıcı adı/parola desteklemesi gerekir. Bu sürüm, 1.3 veya 2005 olabilir. Bu protokol desteği, hem cihaz Azure AD'ye ve cihaza bir parola ile oturum açmak için gereklidir.

## <a name="joining-a-device"></a>Bir cihaz katılma

**Windows 10 cihaz FRX sırasında Azure AD'ye katılmak için:**


1. Yeni Cihazınızda ve kurulum işlemini görmelisiniz **alma hazır** ileti. Cihazınızı ayarlamak için yönergeleri izleyin.

2. Bölge ve dil özelleştirerek başlatın. Ardından, Microsoft Yazılımı Lisans koşulları kabul edin.
 
    ![Bölgeniz için özelleştirme](./media/azuread-joined-devices-frx/01.png)

3. Internet'e bağlanmak için kullanmak istediğiniz ağı seçin.

4. Tıklayın **bu cihaz kuruluşuma ait**. 

    ![Bu bilgisayar ekran Kime ait](./media/azuread-joined-devices-frx/02.png)

5. Size kuruluşunuz tarafından sağlanan kimlik bilgilerini girin ve ardından **oturum**.

    ![Oturum açma ekranı](./media/azuread-joined-devices-frx/03.png)

6. Cihaz, eşleşen bir kiracının Azure AD'de bulur. Bir Federasyon etki alanındaysa, şirket içi güvenli belirteç hizmeti (STS) sunucunuza, örneğin, Active Directory Federasyon Hizmetleri (AD FS) yönlendirilir.

7. Federasyon olmayan bir etki alanında bir kullanıcı varsa, doğrudan Azure AD tarafından barındırılan sayfasında kimlik bilgilerinizi girin. 

8. İçin çok faktörlü kimlik doğrulaması sınaması istenir. 
 
9. Azure AD, bir mobil cihaz Yönetimi kaydında gerekli olup olmadığını denetler.

10. Windows Azure AD'de kuruluşunuzun dizininde cihazı kaydeder ve uygunsa mobil cihaz Yönetimi'nde kaydeder.

11. Eğer:
    - Yönetilen bir kullanıcı Windows Masaüstü otomatik oturum açma işlemi aracılığıyla açılır.

    - Bir Federasyon kullanıcısı kimlik bilgilerinizi girmeniz için Windows oturum açma ekranına yönlendirilirsiniz.

## <a name="verification"></a>Doğrulama

Bir cihaz için Azure AD alanına olup olmadığını doğrulamak için gözden **işe veya okula erişim** Windows Cihazınızda iletişim. İletişim, Azure AD dizininize bağlandığını belirtmeniz gerekir.

![İşe veya okula erişim](./media/azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimine giriş](overview.md).

- Azure AD portalında cihazları yönetme hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md).
