---
title: "İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihaz katılma | Microsoft Docs"
description: "Nasıl kullanıcıları Azure AD'ye katılımı ilk çalıştırma deneyimi sırasında ayarlayabilirsiniz açıklayan bir konu."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: d02994a4fba64605c79edae3f68db36dc8935b28
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihaz katılma

Azure Active Directory'de (Azure AD) ile cihaz yönetimi, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla ayrıntı için bkz: [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md).

Windows 10 ile ilk çalıştırma deneyimi (FRX) sırasında Azure AD için yeni bir cihaz birleştirebilirsiniz.  
Bu çalışanlarınıza veya Öğrenciler naylon cihazlara dağıtmanızı sağlar.

Windows 10 Professional veya Windows 10 Enterprise bir aygıtta yüklü varsa, deneyimi Kurulum işlemi şirkete ait cihazlar için varsayılan olarak ayarlanır.

Windows *out-of-box deneyimi*, bir şirket içi Active Directory (AD) etki alanına katılma desteklenmiyor. Bir bilgisayarın AD etki alanı için Kurulum sırasında katılmak planlıyorsanız, bağlantıyı seçmelisiniz **Windows'u yerel hesapla ayarlamak**. Ardından, bilgisayarınızda etki alanı ayarlarından birleştirebilirsiniz.
 


## <a name="before-you-begin"></a>Başlamadan önce

Windows 10 cihazına katılmak için cihaz Kayıt Hizmeti'ni aygıtlarını kaydetmesini sağlamak için yapılandırılmalıdır. Azure AD kiracınızda aygıtları katılma iznine sahip ek olarak, daha az aygıtlarının yapılandırılmış en büyük değerden daha kayıtlı olmalıdır. Daha fazla ayrıntı için bkz: [aygıt ayarlarını yapılandır](device-management-azure-portal.md#configure-device-settings).

## <a name="joining-a-device"></a>Bir aygıtı birleştirme

**Windows 10 cihazına FRX sırasında Azure AD katılmak için:**


1. Yeni aygıtınızı açın ve kurulum işlemini başlatamazsa, görmelisiniz **alma hazır** ileti. Cihazınızı ayarlamak için istemleri izleyin.

2. Bölge ve dil özelleştirerek başlatın. Ardından, Microsoft Yazılımı Lisans koşulları kabul edin.
 
    ![Bölgeniz için özelleştirme](./media/device-management-azuread-joined-devices-frx/01.png)

3. Internet'e bağlanmak için kullanmak istediğiniz ağı seçin.

4. Tıklatın **bu cihaz kuruluşuma ait**. 

    ![Bu bilgisayar ekran Kime ait](./media/device-management-azuread-joined-devices-frx/02.png)

5. Kuruluşunuz tarafından size sağlanan kimlik bilgilerini girin ve ardından **oturum**.

    ![Oturum açma ekranı](./media/device-management-azuread-joined-devices-frx/03.png)

6. Cihaz, Azure AD içinde eşleşen bir kiracı bulur. Federe bir etki alanındaysa, şirket içi güvenli belirteç hizmeti (STS) sunucunuza, örneğin, Active Directory Federasyon Hizmetleri (AD FS) yönlendirilir.

7. Federasyon olmayan bir etki alanındaki bir kullanıcı varsa, doğrudan Azure AD tarafından barındırılan sayfasında kimlik bilgilerinizi girin. 

8. Çok faktörlü kimlik doğrulaması sınaması için istenir. 
 
9. Azure AD, mobil cihaz Yönetimi'nde bir kayıt gerekli olup olmadığını denetler.

10. Windows Azure AD'de kuruluşunuzun dizininde cihazı kaydeder ve uygulanabilirse mobil cihaz Yönetimi'nde kaydeder.

11. Varsa:
    - Yönetilen bir kullanıcı Windows, Masaüstü otomatik oturum açma işlemi boyunca sürer.

    - Bir Federasyon kullanıcısı kimlik bilgilerinizi girmeniz için Windows oturum açma ekranına yönlendirilir.

## <a name="verification"></a>Doğrulama

Bir aygıt için Azure AD alanına katılıp katılmadığını doğrulamak için gözden **erişim iş veya Okul** iletişim, Windows Cihazınızda. İletişim kutusu, Azure AD dizininizi bağlandığını belirtmeniz gerekir.

![Erişim iş veya Okul](./media/device-management-azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla ayrıntı için bkz: [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md).

- Azure AD portalında cihazları yönetme hakkında daha fazla ayrıntı için bkz: [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md).
