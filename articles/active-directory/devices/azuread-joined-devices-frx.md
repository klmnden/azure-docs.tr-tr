---
title: İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihazının katılımını sağlama | Microsoft Docs
description: Kullanıcıların ilk çalıştırma deneyimi sırasında Azure AD’ye Katılma özelliğini nasıl ayarlayabileceklerinin açıklandığı konu başlığıdır.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/03/2019
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: a79c5f89b14d15ffe4f3c582ac7e1e4cabbdc611
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521559"
---
# <a name="tutorial-join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Öğretici: İlk çalıştırma sırasında Azure AD ile yeni bir Windows 10 cihazını ekleme

Azure Active Directory’de (Azure AD) cihaz yönetimi ile, kullanıcılarınızın güvenlik ve uyumluluk açısından standartlarınızı karşılayan cihazlardan kaynaklarınıza eriştiğinden emin olabilirsiniz. Daha fazla bilgi için, bkz. [Azure Active Directory’de cihaz yönetimine giriş](overview.md).

Windows 10 ile, ilk çalıştırma deneyimi (FRX) sırasında Azure AD’ye yeni bir cihazı katabilirsiniz.  
Bu, çalışanlarınıza veya öğrencilerinize kutulu ve ambalajlı cihazlar dağıtmanıza olanak sağlar.

Bir cihazda Windows 10 Professional veya Windows 10 Enterprise yüklüyse deneyim, varsayılan olarak şirkete ait cihazlar için kurulum işlemine ayarlanır.

Windows *ilk çalıştırma deneyiminde* şirket içi Active Directory (AD) etki alanının katılımı desteklenmez. Kurulum sırasında bir bilgisayarı AD etki alanına katmak istiyorsanız **Yerel bir hesapla Windows'u Kur** bağlantısını seçmeniz gerekir. Ardından bilgisayarınızdaki ayarlardan etki alanına katılım sağlayabilirsiniz.
 
Bu öğreticide ilk çalıştırma deneyimi sırasında bir cihazı nasıl Azure AD'ye katacağınızı öğrenirsiniz:
 > [!div class="checklist"]
> * Önkoşullar
> * Bir cihazı katma
> * Doğrulama

## <a name="prerequisites"></a>Önkoşullar

Windows 10 cihazını katmak için, cihaz kayıt hizmetinin cihazları kaydedebileceğiniz şekilde yapılandırılması gerekir. Azure AD kiracınızda cihazları katma iznini almanıza ek olarak, kayıtlı cihazların sayısının yapılandırılan maksimum değerden daha az olması gerekir. Daha fazla bilgi için, bkz. [cihaz ayarlarını yapılandır](device-management-azure-portal.md#configure-device-settings).

Ayrıca kiracınız federasyonsa Kimlik sağlayıcınızın WS-Fed ve WS-Trust kullanıcı adı/parola uç noktasını desteklemesi GEREKİR. Bu, sürüm 1.3 veya 2005 olabilir. Bu protokol desteğinin hem cihazı Azure AD'ye katması hem de bir parola ile cihazda oturum açması gerekir.

## <a name="joining-a-device"></a>Bir cihazı katma

**İlk çalıştırma deneyimi sırasında bir Windows 10 cihazını katmak için:**


1. Yeni cihazınızı açıp kurulum işlemini başlattığınızda **Hazırlanıyor** mesajı görüntülenecektir. Cihazınızın kurulumunu gerçekleştirmek için istemleri uygulayın.

2. Bölge ve dil ayarlarını özelleştirerek başlayın. Ardından Microsoft Yazılımı Lisans Koşulları'nı kabul edin.
 
    ![Bölgeniz için özelleştirme](./media/azuread-joined-devices-frx/01.png)

3. İnternete bağlanmak için kullanmak istediğiniz ağı seçin.

4. **Bu cihaz kuruluşuma ait** seçeneğine tıklayın. 

    ![Bu bilgisayar kime ait ekranı](./media/azuread-joined-devices-frx/02.png)

5. Kuruluşunuzun sağladığı kimlik bilgilerini girin ve ardından **Oturum aç** düğmesine tıklayın.

    ![Oturum açma ekranı](./media/azuread-joined-devices-frx/03.png)

6. Cihazınız Azure AD'de eşleşen bir kiracı bulur. Federasyon etki alanındaysanız şirket içi Güvenli Belirteç Hizmeti (STS) sunucunuza, örneğin Active Directory Federasyon Hizmetleri (AD FS) sunucusuna yönlendirilirsiniz.

7. Federasyon olmayan bir etki alanındaki bir kullanıcıysanız kimlik bilgilerinizi doğrudan Azure AD'de barındırılan sayfaya girin. 

8. Çok faktörlü kimlik doğrulaması sınaması istenir. 
 
9. Azure AD mobil cihaz yönetimine kayıt gerekip gerekmediğini belirler.

10. Windows kuruluşun dizinindeki cihazı Azure AD'ye kaydeder ve uygun olan durumlarda mobil cihaz yönetimine kaydeder.

11. Şu anda:
    - Yönetilen bir kullanıcıysanız Windows otomatik oturum açma işlemi ile sizi masaüstüne yönlendirir.

    - Federasyon kullanıcıysanız kimlik bilgilerinizi girmek üzere Windows oturum açma ekranına yönlendirilirsiniz.

## <a name="verification"></a>Doğrulama

Bir cihazın Azure AD'nize katılıp katılmadığını doğrulamak için Windows cihazınızda **İş veya okul hesabına erişim** iletişim kutusunu inceleyin. İletişim kutusunun, Azure AD dizininize bağlı olduğunuzu belirtmesi gerekir.

![İş veya okul hesabına erişme](./media/azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için, bkz. [Azure Active Directory’de cihaz yönetimine giriş](overview.md).

- Azure AD portalında cihazları yönetme hakkında daha fazla bilgi için, bkz. [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md).
