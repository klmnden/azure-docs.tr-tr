---
title: StorSimple sanal diziler için yeni kimlik doğrulaması | Microsoft Docs
description: Hizmetiniz için temel AAD kimlik doğrulaması kullanması, yeni kayıt anahtarı oluşturun ve cihazların el ile kayıt gerçekleştirmek açıklanmaktadır.
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
ms.date: 06/28/2018
ms.author: alkohli
ms.openlocfilehash: 080f49ca1078858462264f229e9acfee6fad17d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61387665"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>Yeni kimlik doğrulaması için StorSimple'ınızı kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure'da çalışan ve birden çok StorSimple sanal dizisi bağlanır. Bugüne kadar StorSimple cihaz Yöneticisi hizmeti bir erişim denetimi hizmeti (ACS) StorSimple Cihazınızı hizmete kimlik doğrulaması için kullandı. ACS mekanizması yakında kullanımdan kaldırıldı ve bir Azure Active Directory (AAD) kimlik doğrulaması tarafından değiştirildi.

Bu makalede yer alan bilgileri, her iki StorSimple 1200 Serisi sanal diziler için yalnızca geçerlidir. Bu makalede, AAD kimlik doğrulamasını ve ilişkili yeni hizmet kayıt anahtarını ve güvenlik duvarı kuralları olarak StorSimple cihazlar için geçerli bir değişiklik ayrıntılarını açıklar.

StorSimple sanal diziler (1200 model) AAD kimlik doğrulaması gerçekleşir güncelleştirme 1 veya sonrasını çalıştırıyor.

AAD kimlik doğrulamasını tanıtılması nedeniyle, değişiklikler ortaya:

- URL desenleri için güvenlik duvarı kuralları.
- Hizmet kayıt anahtarı.

Bu değişiklikler aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

## <a name="url-changes-for-aad-authentication"></a>AAD kimlik doğrulaması için URL değişiklikleri

Hizmet AAD tabanlı kimlik doğrulaması kullandığından emin olmak için tüm kullanıcılar kendi güvenlik duvarı kurallarında yeni kimlik doğrulaması URL'lerini içermelidir.

StorSimple sanal dizisi kullanıyorsanız, aşağıdaki URL'yi güvenlik duvarı kurallarının eklendiğinden emin olun:

| URL deseni                         | Bulut | Bileşen/işlevi         |
|------------------------------------|-------|---------------------------------|
| `https://login.windows.net`        | Azure kamu |AAD kimlik doğrulama hizmeti      |
| `https://login.microsoftonline.us` | ABD Devleti |AAD kimlik doğrulama hizmeti      |

URL tam listesi için StorSimple sanal dizilerine desenleri için Git [URL desenleri için güvenlik duvarı kuralları](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).

Kimlik doğrulama URL'si kullanımdan kaldırma tarihi ötesinde güvenlik duvarı kuralları yer almayan kullanıcıların StorSimple cihazını hizmeti ile doğrulanamadı kritik bir uyarı görürsünüz. Hizmet aygıtıyla iletişim kurmak mümkün olmayacaktır. Kullanıcılar bu uyarıyı görürseniz, bunlar yeni kimlik doğrulama URL'si eklemeniz gerekir. Uyarı hakkında daha fazla bilgi için Git [StorSimple aygıtınızı izleme için uyarıları kullanın](storsimple-virtual-array-manage-alerts.md#networking-alerts).

## <a name="device-version-and-authentication-changes"></a>Cihaz sürümü ve kimlik doğrulaması değişiklikleri

StorSimple Virtual Array kullanıyorsa, yapmanız gereken hangi eylemin kullanmakta olduğunuz cihaz yazılım sürüme göre belirlemek için aşağıdaki tabloyu kullanın.

| Cihazınızı çalışıyorsa  | Aşağıdaki eylemi gerçekleştirin                                    |
|----------------------------|--------------------------------------------------------------|
| Güncelleştirme 1.0 veya üstü ve çevrimdışı. <br> URL izin verilenler listesinde değil bir uyarı görürsünüz.| 1. Güvenlik duvarı kurallarını, kimlik doğrulama URL'si içerecek şekilde değiştirin. Bkz: [kimlik doğrulaması URL'lerini](#url-changes-for-aad-authentication). <br> 2. [Hizmet AAD kayıt anahtarını almanız](#aad-based-registration-keys). <br> 3. 1-5 adımlarını gerçekleştirmek [sanal diziyi Windows PowerShell arabiriminde Bağlan](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br> 4. Kullanım `Invoke-HcsReRegister` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet'i. Önceki adımda aldığınız anahtarını belirtin.|
| Güncelleştirme 1.0 veya üstü ve cihaz çevrimiçi.| İşlem yapmanız gerekmez.                                       |
| Güncelleştirme 0.6 veya daha önceki ve cihaz çevrimdışı. | 1. [Katalog sunucusundan güncelleştirme 1.0 indirmek](storsimple-virtual-array-install-update-1.md#download-the-update-or-the-hotfix).<br>2. [Güncelleştirme 1.0 yerel web kullanıcı Arabirimi uygulama](storsimple-virtual-array-install-update-1.md#install-the-update-or-the-hotfix).<br>3. [Hizmet AAD kayıt anahtarını almanız](#aad-based-registration-keys). <br>4. 1-5 adımlarını gerçekleştirmek [sanal diziyi Windows PowerShell arabiriminde Bağlan](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br>5. Kullanım `Invoke-HcsReRegister` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet'i. Önceki adımda aldığınız anahtarını belirtin.|
| Güncelleştirme 0.6 veya daha önceki ve cihazın çevrimiçi | Güvenlik duvarı kurallarını, kimlik doğrulama URL'si içerecek şekilde değiştirin.<br> Güncelleştirme 1.0 Azure Portalı aracılığıyla yükleyin. |

## <a name="aad-based-registration-keys"></a>AAD tabanlı kayıt anahtarları

StorSimple sanal diziler için yeni bir AAD tabanlı kayıt anahtarlarının kullanılması güncelleştirme 1.0 başlangıcı. Cihazı ile StorSimple cihaz Yöneticisi hizmetine kaydetmek için kayıt anahtarları kullanın.

StorSimple sanal güncelleştirme 0.6 veya önceki bir sürümü çalıştıran dizilere kullanıyorsanız yeni AAD hizmet Kayıt anahtarlarına kullanamazsınız. Hizmet kayıt anahtarını yeniden oluşturmak gerekir. Anahtarı yeniden sonra yeni anahtar sonraki tüm cihazları kaydetmek için kullanılır. Eski anahtar artık geçerli değil.

- Yeni bir AAD kayıt anahtarı, 3 gün sonra süresi dolar.
- AAD kayıt anahtarları iş yalnızca StorSimple 1200 Serisi sanal ile çalışan Update 1 veya sonrasını dizi. StorSimple 8000 serisi cihaz AAD kayıt anahtarı, çalışmaz.
- AAD kayıt anahtarlarının karşılık gelen ACS kayıt anahtarları uzun.

Bir AAD hizmet kayıt anahtarı oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-generate-the-aad-service-registration-key"></a>AAD hizmet kayıt anahtarını oluşturmak için

1. İçinde **StorSimple cihaz Yöneticisi**Git **Yönetim &gt;**  **anahtarları**.
    
    ![Anahtarları gidin](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key1.png)

2. Tıklayın **anahtar üret**.

    ![Regenerate tıklayın](./media/storsimple-virtual-array-aad-registration-key/aad-click-generate-registration-key.png)

3. Yeni anahtarı kopyalayın. Eski anahtar artık çalışmaz.

    ![Regenerate onaylayın](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key2.png)

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl dağıtılacağı hakkında daha fazla bilgi [StorSimple sanal dizisi](storsimple-virtual-array-deploy1-portal-prep.md)
