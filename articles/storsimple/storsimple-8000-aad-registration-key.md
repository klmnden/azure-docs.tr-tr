---
title: Azure StorSimple 8000 cihaz Yöneticisi hizmeti için yeni kimlik doğrulamasını kullan | Microsoft Docs
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
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: 01d36188c1684eae8303cb20ba0fd0c708ff91ba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60309880"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>Yeni kimlik doğrulaması için StorSimple'ınızı kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure'da çalışan ve birden çok StorSimple aygıtına bağlanır. Bugüne kadar StorSimple cihaz Yöneticisi hizmeti bir erişim denetimi hizmeti (ACS) StorSimple Cihazınızı hizmete kimlik doğrulaması için kullandı. ACS mekanizması yakında kullanımdan kaldırıldı ve bir Azure Active Directory (AAD) kimlik doğrulaması tarafından değiştirildi. Daha fazla bilgi için ACS kullanımdan kaldırma ve AAD kimlik doğrulaması için aşağıdaki duyuruları gidin.

- [Azure ACS Azure Active Directory geleceğidir](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/12/the-future-of-azure-acs-is-azure-active-directory/)
- [Microsoft Access Control Service yaklaşan değişiklikleri](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/)

Bu makalede, AAD kimlik doğrulamasını ve ilişkili yeni hizmet kayıt anahtarını ve güvenlik duvarı kuralları olarak StorSimple cihazlar için geçerli bir değişiklik ayrıntılarını açıklar. Bu makalede yer alan bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir.

AAD kimlik doğrulamasını güncelleştirme 5 veya üzerini çalıştıran StorSimple 8000 serisi Cihazınızı içinde gerçekleşir. AAD kimlik doğrulamasını tanıtılması nedeniyle, değişiklikler ortaya:

- URL desenleri için güvenlik duvarı kuralları.
- Hizmet kayıt anahtarı.

Bu değişiklikler aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

## <a name="url-changes-for-aad-authentication"></a>AAD kimlik doğrulaması için URL değişiklikleri

Hizmet AAD tabanlı kimlik doğrulaması kullandığından emin olmak için tüm kullanıcılar kendi güvenlik duvarı kurallarında yeni kimlik doğrulaması URL'lerini içermelidir.

StorSimple 8000 serisi kullanıyorsanız, aşağıdaki URL'yi güvenlik duvarı kurallarının eklendiğinden emin olun:

| URL deseni                         | Bulut | Bileşen/işlevi         |
|------------------------------------|-------|----------------------------------|
| `https://login.windows.net`        | Azure kamu |AAD kimlik doğrulama hizmeti      |
| `https://login.microsoftonline.us` | ABD Devleti |AAD kimlik doğrulama hizmeti      |

StorSimple 8000 serisi cihazlar için tam bir URL listesi desenleri için Git [URL desenleri için güvenlik duvarı kuralları](storsimple-8000-system-requirements.md#url-patterns-for-firewall-rules).

Kimlik doğrulama URL'si kullanımdan kaldırma tarihi ötesinde güvenlik duvarı kuralları yer almayan ve cihaz güncelleştirme 5 çalıştığından, kullanıcılar bir URL uyarı görür. Yeni kimlik doğrulama URL'si eklemek kullanıcıların gerekir. Cihaz güncelleştirme 5'den önceki bir sürümü çalıştırıyorsa, kullanıcılar bir sinyal uyarısını görür. Her durumda, StorSimple Cihazınızı hizmete kimlik doğrulaması yapamaz ve hizmet cihazla iletişim kuramıyor.

## <a name="device-version-and-authentication-changes"></a>Cihaz sürümü ve kimlik doğrulaması değişiklikleri

StorSimple 8000 serisi cihaz kullanıyorsanız, yapmanız gereken hangi eylemin kullanmakta olduğunuz cihaz yazılım sürüme göre belirlemek için aşağıdaki tabloyu kullanın.

| Cihazınızı çalışıyorsa| Aşağıdaki eylemi gerçekleştirin                                    |
|--------------------------|------------------------|
| Güncelleştirme 5 veya üzeri ve cihaz çevrimdışı. <br> URL izin verilenler listesinde değil bir uyarı görürsünüz.|1. Güvenlik duvarı kurallarını, kimlik doğrulama URL'si içerecek şekilde değiştirin. Bkz: [kimlik doğrulaması URL'lerini](#url-changes-for-aad-authentication).<br>2. [Hizmet AAD kayıt anahtarını almanız](#aad-based-registration-keys).<br>3. [StorSimple 8000 serisi cihaz Windows PowerShell arabirimine bağlanma](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).<br>4. Kullanım `Redo-DeviceRegistration` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet'i. Önceki adımda aldığınız anahtarını belirtin.|
| Güncelleştirme 5 veya üzeri ve cihaz çevrimiçi.| İşlem yapmanız gerekmez.                                       |
| Güncelleştirme 4 veya önceki sürümlerini ve cihaz çevrimdışı. |1. Güvenlik duvarı kurallarını, kimlik doğrulama URL'si içerecek şekilde değiştirin.<br>2. [Güncelleştirme 5 katalog sunucusuna indirin](storsimple-8000-install-update-5.md#download-updates-for-your-device).<br>3. [Güncelleştirme 5 ile düzeltme yöntemi uygulamak](storsimple-8000-install-update-5.md#install-update-5-as-a-hotfix).<br>4. [Hizmet AAD kayıt anahtarını almanız](#aad-based-registration-keys).<br>5. [StorSimple 8000 serisi cihaz Windows PowerShell arabirimine bağlanma](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). <br>6. Kullanım `Redo-DeviceRegistration` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet'i. Önceki adımda aldığınız anahtarını belirtin.|
| Güncelleştirme 4 veya önceki sürümlerini ve cihaz çevrimiçi. |Güvenlik duvarı kurallarını, kimlik doğrulama URL'si içerecek şekilde değiştirin.<br> Güncelleştirme 5 Azure Portalı aracılığıyla yükleyin.              |
| Güncelleştirme 5 önce bir sürüme sıfırlama üreteci.      |Portal, cihaz eski yazılım çalışırken bir AAD tabanlı kayıt anahtarını gösterir. Cihaz güncelleştirme 4 veya önceki çalışırken için önceki senaryoda adımları izleyin.              |

## <a name="aad-based-registration-keys"></a>AAD tabanlı kayıt anahtarları

Güncelleştirme 5, StorSimple 8000 serisi cihazlar, yeni bir AAD tabanlı kayıt anahtarlarının kullanılması başlangıcı. Cihazı ile StorSimple cihaz Yöneticisi hizmetine kaydetmek için kayıt anahtarları kullanın.

Update 4 veya önceki bir sürümü (şimdi etkinleştirilmekte olan eski bir cihaz içerir) çalıştıran StorSimple 8000 serisi cihaz kullanıyorsanız, yeni AAD hizmet Kayıt anahtarlarına kullanamazsınız.
Bu senaryoda, hizmet kayıt anahtarını yeniden oluşturmak gerekir. Anahtarı yeniden sonra yeni anahtar sonraki tüm cihazları kaydetmek için kullanılır. Eski anahtar artık geçerli değil.

- Yeni bir AAD kayıt anahtarı, 3 gün sonra süresi dolar.
- AAD kayıt anahtarları yalnızca StorSimple 8000 serisi cihazlar güncelleştirme 5 çalışan ile iş veya üzeri.
- AAD kayıt anahtarlarının karşılık gelen ACS kayıt anahtarları uzun.

Bir AAD hizmet kayıt anahtarı oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-generate-the-aad-service-registration-key"></a>AAD hizmet kayıt anahtarını oluşturmak için

1. İçinde **StorSimple cihaz Yöneticisi**Git **Yönetim &gt;**  **anahtarları**. Aramak için arama çubuğuna kullanabilirsiniz _anahtarları_.
    
2. Tıklayın **anahtar üret**.

    ![Regenerate tıklayın](./media/storsimple-8000-aad-registration-key/aad-click-generate-registration-key.png)

3. Yeni anahtarı kopyalayın. Eski anahtar artık çalışmayacak.

    ![Regenerate onaylayın](./media/storsimple-8000-aad-registration-key/aad-registration-key2.png)

    > [!NOTE] 
    > StorSimple bulut Gereci StorSimple 8000 serisi Cihazınızı kayıtlı hizmeti oluşturuyorsanız, oluşturma işlemi devam ederken bir kayıt anahtarı oluşturmaz. Oluşturmayı tamamlayın ve ardından kayıt anahtarını oluşturmak bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl dağıtılacağı hakkında daha fazla bilgi [StorSimple 8000 serisi cihaz](storsimple-8000-deployment-walkthrough-u2.md).

