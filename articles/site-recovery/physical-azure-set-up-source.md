---
title: (Fiziksel sunucuları azure'a) kaynak ortamı ayarlama | Microsoft Docs
description: Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları çoğaltmaya başlamak için şirket içi ortamınızı ayarlama açıklar.
services: site-recovery
documentationcenter: ''
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: anoopkv
ms.openlocfilehash: 00b09db97e597521de5c73eeefab77b0dfa1304d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38671031"
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a>(Fiziksel sunucudan azure'a) kaynak ortamı ayarlama

Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları çoğaltmaya başlamak için şirket içi ortamınızı ayarlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

Sahip olduğunuz varsayılır:
1. Bir kurtarma Hizmetleri kasası [Azure portalında](http://portal.azure.com "Azure portalında").
3. Yapılandırma sunucusunu yüklemek için fiziksel bir bilgisayar.

### <a name="configuration-server-minimum-requirements"></a>Yapılandırma sunucusunun en düşük gereksinimleri
Aşağıdaki tabloda, en düşük donanım, yazılım ve yapılandırma sunucusu için ağ gereksinimleri listelenmektedir.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Yapılandırma sunucusu tarafından HTTPS tabanlı Ara sunucular desteklenmez.

## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. Azure portalında Git **kurtarma Hizmetleri** dikey kasaları ve kasanızı seçin.
2. İçinde **kaynak** kasa menüsünü **Başlarken** > **Site Recovery** > **1. adım: altyapıyı hazırlama**   >  **Koruma hedefi**.

    ![Hedefleri seçme](./media/physical-azure-set-up-source/choose-goals.png)
3. İçinde **koruma hedefi**seçin **Azure'a** ve **sanallaştırılmamış/diğer**ve ardından **Tamam**.

    ![Hedefleri seçme](./media/physical-azure-set-up-source/physical-protection-goal.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

1. İçinde **kaynağı hazırla**,'a tıklayın, bir yapılandırma sunucusu yoksa, **+ yapılandırma sunucusu** eklemek için.

  ![Kaynağı ayarlama](./media/physical-azure-set-up-source/plus-config-srv.png)
2. İçinde **Sunucusu Ekle** dikey penceresinde bu maddeyi **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Birleşik Kurulum'u çalıştırdığınızda, kayıt anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/physical-azure-set-up-source/set-source2.png)
6. Yapılandırma sunucusu olarak kullandığınız makinede çalıştırma **Azure Site Recovery birleşik Kurulumu** yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu yüklemek için.

#### <a name="run-azure-site-recovery-unified-setup"></a>Çalıştırma Azure Site Recovery birleşik Kurulumu

> [!TIP]
> Bilgisayarınızın sistem saati yerel saat alanlarını beş dakikadan fazla ise yapılandırma sunucusu kaydı başarısız olur. Sistem saatinizi bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) yüklemeyi başlatmadan önce.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu, bir komut satırı yüklenebilir. Daha fazla bilgi için [komut satırı araçlarını kullanarak yükleme yapılandırma sunucusu](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Genel sorunlar

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım içerir [hedef ortamınızı ayarlarken](physical-azure-set-up-target.md) azure'da.
