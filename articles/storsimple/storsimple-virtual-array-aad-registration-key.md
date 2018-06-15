---
title: StorSimple sanal diziler için yeni kimlik doğrulama | Microsoft Docs
description: Hizmetiniz için AAD tabanlı kimlik doğrulaması kullanmak, yeni kayıt anahtarı oluşturun ve aygıtların el ile kayıt gerçekleştirmek açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: 8d033cc09de8e115324067d7bbdf052751730d63
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28030957"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>StorSimple için yeni kimlik doğrulama kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple sanal diziler bağlanır. Bugüne kadar StorSimple Aygıt Yöneticisi'ni bir erişim denetimi hizmeti (ACS) StorSimple Cihazınızı hizmete kimlik doğrulaması için kullanılan hizmet. ACS mekanizması yakında kullanım ve Azure Active Directory (AAD) kimlik doğrulaması tarafından değiştirildi.

Her iki StorSimple 1200 Serisi sanal diziler için yalnızca bu makalede yer alan bilgileri geçerli. Bu makalede ayrıntılarını AAD kimlik doğrulama ve ilişkili yeni hizmet kayıt anahtarını ve güvenlik duvarı kuralları StorSimple cihazlar için geçerli olarak yapılan değişiklikler açıklanmaktadır.

StorSimple sanal diziler (modeli 1200) AAD kimlik doğrulaması gerçekleşir güncelleştirme 1 veya sonrasını çalıştırıyor.

AAD kimlik giriş nedeniyle değişiklikler ortaya:

- Güvenlik duvarı kuralları için URL desenlerini.
- Hizmet kayıt anahtarı.

Bu değişiklikler, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

## <a name="url-changes-for-aad-authentication"></a>AAD kimlik doğrulaması için URL değişiklikleri

Hizmet AAD tabanlı kimlik doğrulaması kullandığından emin olmak için tüm kullanıcılar, kendi güvenlik duvarı kurallarında yeni kimlik doğrulaması URL'lerini eklemeniz gerekir.

StorSimple sanal dizinin kullanıyorsanız, aşağıdaki URL'yi güvenlik duvarı kurallarında bulunduğundan emin olun:

| URL deseni                         | Bulut | Bileşen/işlevi         |
|------------------------------------|-------|---------------------------------|
| `https://login.windows.net`        | Azure Public |AAD kimlik doğrulama hizmeti      |
| `https://login.microsoftonline.us` | ABD Devleti |AAD kimlik doğrulama hizmeti      |

StorSimple sanal diziler için tam bir URL listesi düzenleri için Git [güvenlik duvarı kuralları için URL desenlerini](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).

Kimlik doğrulaması URL'sini kullanımdan kaldırma tarihi ötesinde güvenlik duvarı kurallarında dahil edilmezse, kullanıcılar, StorSimple cihazını hizmetiyle kimlik doğrulaması yapamıyor kritik bir uyarı bakın. Hizmet aygıtla iletişim kuramıyor olmaz. Kullanıcılar bu uyarıyı görürseniz, yeni kimlik doğrulaması URL'sini eklemeniz gerekir. Uyarı hakkında daha fazla bilgi için Git [StorSimple Cihazınızı izlemek için uyarıları kullanın](storsimple-virtual-array-manage-alerts.md#networking-alerts).

## <a name="device-version-and-authentication-changes"></a>Cihaz sürümü ve kimlik doğrulama değişiklikleri

Bir StorSimple sanal dizisi kullanıyorsanız, uygulamanız gereken hangi eylemini kullanmakta olduğunuz cihaz yazılım sürümlerine göre belirlemek için aşağıdaki tabloyu kullanın.

| Cihazınızı çalışıyorsa  | Aşağıdaki adımları uygulayın                                    |
|----------------------------|--------------------------------------------------------------|
| Güncelleştirme 1.0 veya üstü ve çevrimdışı. <br> URL Güvenilenler listesine değil bir uyarı görürsünüz.| Güvenlik duvarı kurallarını kimlik doğrulaması URL'sini içerecek şekilde değiştirin. Bkz: [kimlik doğrulaması URL'lerini](#url-changes-for-aad-authentication). |
| Güncelleştirme 1.0 veya üstü ve cihaz çevrimiçi.| Hiçbir eylem gerekli değildir.                                       |
| Güncelleştirme 0,6 veya önceki bir sürümünü ve cihaz çevrimdışı. | [Katalog sunucusundan güncelleştirme 1.0 indirmek](storsimple-virtual-array-install-update-1.md#download-the-update-or-the-hotfix).<br>[Yerel web kullanıcı Arabirimi güncelleştirme 1.0 uygulamak](storsimple-virtual-array-install-update-1.md#install-the-update-or-the-hotfix). <br> [Hizmetinden AAD kayıt anahtarını alın](#aad-based-registration-keys). <br> 1-5 adımlarını gerçekleştirmek [sanal dizinin Windows PowerShell arabirimi Bağlan](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br> Kullanım `Invoke-HcsReRegister` cmdlet'ini Windows PowerShell üzerinden cihazı kaydedin. Önceki adımda aldığınız anahtarını belirtin.|
| Güncelleştirme 0,6 veya önceki bir sürümünü ve aygıtın çevrimiçi | Güvenlik duvarı kurallarını kimlik doğrulaması URL'sini içerecek şekilde değiştirin.<br> Güncelleştirme 1.0 Azure portalı üzerinden yükleyin. |

## <a name="aad-based-registration-keys"></a>AAD tabanlı kayıt anahtarları

Güncelleştirme 1.0 StorSimple sanal diziler için yeni AAD tabanlı kayıt anahtarları kullanılan başlangıcı. StorSimple cihaz Yöneticisi hizmetiniz cihazı kaydetmek için kayıt tuşlarını kullanın.

StorSimple sanal güncelleştirme 0,6 veya önceki sürümleri çalıştıran diziler kullanıyorsanız, yeni AAD hizmeti kayıt anahtarları kullanamazsınız. Hizmet kayıt anahtarını yeniden oluşturulması gerekir. Anahtar yeniden sonra yeni anahtar sonraki tüm cihazları kaydetmek için kullanılır. Eski anahtarı artık geçerli değil.

- Yeni AAD kayıt anahtarı 3 gün sonra süresi dolar.
- AAD kayıt anahtarları çalışmak yalnızca StorSimple 1200 Serisi sanal çalışan Update 1 veya sonrasını dizi. AAD kayıt anahtarını bir StorSimple 8000 serisi aygıttan çalışmaz.
- AAD Kayıt anahtarlarını karşılık gelen ACS Kayıt anahtarlarını uzun.

Bir AAD hizmet kayıt anahtarını oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-generate-the-aad-service-registration-key"></a>AAD hizmet kayıt anahtarını oluşturmak için

1. İçinde **StorSimple Aygıt Yöneticisi'ni**gidin **Yönetim &gt;**  **anahtarları**.
    
    ![Anahtarları gidin](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key1.png)

2. Tıklatın **anahtar üret**.

    ![Yeniden tıklatın](./media/storsimple-virtual-array-aad-registration-key/aad-click-generate-registration-key.png)

3. Yeni anahtarı kopyalayın. Eski anahtar artık çalışır.

    ![Yeniden onaylayın](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key2.png)

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl dağıtılacağı hakkında daha fazla bilgi [StorSimple sanal dizinin](storsimple-virtual-array-deploy1-portal-prep.md)
