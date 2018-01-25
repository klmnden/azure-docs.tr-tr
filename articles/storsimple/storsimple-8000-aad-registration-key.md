---
title: "Azure StorSimple 8000 Aygıt Yöneticisi'ni hizmet için yeni kimlik doğrulamasını kullan | Microsoft Docs"
description: "Hizmetiniz için AAD tabanlı kimlik doğrulaması kullanmak, yeni kayıt anahtarı oluşturun ve aygıtların el ile kayıt gerçekleştirmek açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: 37f44538d94ed78509bbcb09e726dc34a9e92e95
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>StorSimple için yeni kimlik doğrulama kullanın

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple cihazını bağlanır. Bugüne kadar StorSimple Aygıt Yöneticisi'ni bir erişim denetimi hizmeti (ACS) StorSimple Cihazınızı hizmete kimlik doğrulaması için kullanılan hizmet. ACS mekanizması yakında kullanım ve Azure Active Directory (AAD) kimlik doğrulaması tarafından değiştirildi. Daha fazla bilgi için aşağıdaki duyuruları ACS kullanımdan kaldırma ve AAD kimlik doğrulaması için gidin.

- [Azure ACS gelecekteki Azure Active Directory olduğu](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/12/the-future-of-azure-acs-is-azure-active-directory/)
- [Microsoft erişim denetimi hizmeti yaklaşan değişiklikleri](https://azure.microsoft.com/en-in/blog/acs-access-control-service-namespace-creation-restriction/)

Bu makalede ayrıntılarını AAD kimlik doğrulama ve ilişkili yeni hizmet kayıt anahtarını ve güvenlik duvarı kuralları StorSimple cihazlar için geçerli olarak yapılan değişiklikler açıklanmaktadır. Bu makalede yer alan bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir.

AAD kimlik doğrulaması güncelleştirme 5 veya sonraki sürümünü çalıştıran StorSimple 8000 serisi aygıtı oluşur. AAD kimlik giriş nedeniyle değişiklikler ortaya:

- Güvenlik duvarı kuralları için URL desenlerini.
- Hizmet kayıt anahtarı.

Bu değişiklikler, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

## <a name="url-changes-for-aad-authentication"></a>AAD kimlik doğrulaması için URL değişiklikleri

Hizmet AAD tabanlı kimlik doğrulaması kullandığından emin olmak için tüm kullanıcılar, kendi güvenlik duvarı kurallarında yeni kimlik doğrulaması URL'lerini eklemeniz gerekir.

StorSimple 8000 serisi kullanıyorsanız, aşağıdaki URL'yi güvenlik duvarı kurallarında bulunduğundan emin olun:

| URL deseni                         | Bulut | Bileşen/işlevi         |
|------------------------------------|-------|----------------------------------|
| `https://login.windows.net`        | Azure Public |AAD kimlik doğrulama hizmeti      |
| `https://login.microsoftonline.us` | ABD Devleti |AAD kimlik doğrulama hizmeti      |

StorSimple 8000 serisi cihazlar için tam bir URL listesi düzenleri için Git [güvenlik duvarı kuralları için URL desenlerini](storsimple-8000-system-requirements.md#url-patterns-for-firewall-rules).

Kimlik doğrulaması URL'sini kullanımdan kaldırma tarihi ötesinde güvenlik duvarı kuralları dahil değilse ve aygıt güncelleştirme 5 çalıştığından, kullanıcılar bir URL uyarısı bakın. Kullanıcılar yeni kimlik doğrulaması URL'sini eklemeniz gerekir. Cihaz güncelleştirme 5 önce bir sürümünü çalıştırıyorsa, kullanıcıların bir sinyal uyarısı bakın. Her durumda, StorSimple cihazı hizmetiyle kimlik doğrulaması yapamaz ve Hizmet aygıtla iletişim mümkün değil.

## <a name="device-version-and-authentication-changes"></a>Cihaz sürümü ve kimlik doğrulama değişiklikleri

StorSimple 8000 serisi cihaz kullanıyorsanız, uygulamanız gereken hangi eylemini kullanmakta olduğunuz cihaz yazılım sürümlerine göre belirlemek için aşağıdaki tabloyu kullanın.

| Cihazınızı çalışıyorsa| Aşağıdaki adımları uygulayın                                    |
|--------------------------|------------------------|--------------------|--------------------------------------------------------------|
| Güncelleştirme 5 veya sonraki sürümünü ve cihaz çevrimdışı. <br> URL Güvenilenler listesine değil bir uyarı görürsünüz.| Güvenlik duvarı kurallarını kimlik doğrulaması URL'sini içerecek şekilde değiştirin.<br> Bkz: [kimlik doğrulaması URL'lerini](#url-changes-for-aad-authentication). |
| Güncelleştirme 5 veya sonraki sürümünü ve cihaz çevrimiçi.| Hiçbir eylem gerekli değildir.                                       |
| Güncelleştirme 4 veya önceki ve cihaz çevrimdışı. | Güvenlik duvarı kurallarını kimlik doğrulaması URL'sini içerecek şekilde değiştirin.<br>[Güncelleştirme 5 katalog sunucusundan karşıdan](storsimple-8000-install-update-5.md#download-updates-for-your-device).<br>[Güncelleştirme 5 düzeltme yöntemini uygulama](storsimple-8000-install-update-5.md#install-update-5-as-a-hotfix). <br> [Hizmetinden AAD kayıt anahtarını alın](#aad-based-registration-keys). <br> [StorSimple 8000 serisi aygıt Windows PowerShell arabirimine bağlamak](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). <br>Kullanım `Redo-DeviceRegistration` cmdlet'ini Windows PowerShell üzerinden cihazı kaydedin. Önceki adımda aldığınız anahtarını belirtin.|
| Güncelleştirme 4 veya önceki ve cihaz çevrimiçi. |Güvenlik duvarı kurallarını kimlik doğrulaması URL'sini içerecek şekilde değiştirin.<br> Güncelleştirme 5 Azure portalı üzerinden yükleyin.              |
| Bir sürüme güncelleştirme 5 önce Fabrika.      |Eski yazılım cihaz çalışırken portal AAD tabanlı kayıt anahtarı gösterir. Önceki senaryoda cihaz güncelleştirme 4 veya önceki çalışırken için adımları izleyin.              |

## <a name="aad-based-registration-keys"></a>AAD tabanlı kayıt anahtarları

Güncelleştirme 5 StorSimple 8000 serisi cihazlar, yeni AAD tabanlı kayıt anahtarları kullanılan başlangıcı. StorSimple cihaz Yöneticisi hizmetiniz cihazı kaydetmek için kayıt tuşlarını kullanın.

(Daha eski bir aygıtı artık etkinleştirilmekte olan içerir) güncelleştirme 4 veya önceki bir sürümü çalıştıran bir StorSimple 8000 serisi cihaz kullanıyorsanız, yeni AAD hizmet Kayıt anahtarlarını kullanamazsınız.
Bu senaryoda, hizmet kayıt anahtarını yeniden oluşturmak gerekebilir. Anahtar yeniden sonra yeni anahtar sonraki tüm cihazları kaydetmek için kullanılır. Eski anahtarı artık geçerli değil.

- Yeni AAD kayıt anahtarı 3 gün sonra süresi dolar.
- AAD Kayıt anahtarlarını güncelleştirme 5 çalışan StorSimple 8000 serisi cihazlar ile çalışmak veya üzeri.
- AAD Kayıt anahtarlarını karşılık gelen ACS Kayıt anahtarlarını uzun.

Bir AAD hizmet kayıt anahtarını oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-generate-the-aad-service-registration-key"></a>AAD hizmet kayıt anahtarını oluşturmak için

1. İçinde **StorSimple Aygıt Yöneticisi'ni**gidin **Yönetim &gt;**  **anahtarları**. Aramak için arama çubuğunu kullanabilirsiniz _anahtarları_.
    
2. Tıklatın **anahtar üret**.

    ![Yeniden tıklatın](./media/storsimple-8000-aad-registration-key/aad-click-generate-registration-key.png)

3. Yeni anahtarı kopyalayın. Eski anahtar artık çalışmaz.

    ![Yeniden onaylayın](./media/storsimple-8000-aad-registration-key/aad-registration-key2.png)

    > [!NOTE] 
    > StorSimple 8000 serisi cihazınız kayıtlı hizmet üzerinde bir StorSimple bulut uygulaması oluşturuyorsanız, oluşturma işlemi devam ederken bir kayıt anahtarı oluşturmaz. Oluşturmayı tamamlayın ve ardından kayıt anahtarını oluşturmak bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl dağıtılacağı hakkında daha fazla bilgi [StorSimple 8000 serisi aygıt](storsimple-8000-deployment-walkthrough-u2.md).

