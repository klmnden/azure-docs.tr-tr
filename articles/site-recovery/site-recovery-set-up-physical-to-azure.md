---
title: "Kaynak ortamı (fiziksel sunucuları azure'a) ayarlama | Microsoft Docs"
description: "Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları çoğaltıyor başlatmak için şirket içi ortamınızı ayarlayın açıklar."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: anoopkv
ms.openlocfilehash: d242954eb62a0d7325cc4222a54f2581967fdc19
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a>Kaynak ortamı (Azure fiziksel sunucuya) ayarlama
> [!div class="op_single_selector"]
> * [Vmware’den Azure’a](./site-recovery-set-up-vmware-to-azure.md)
> * [Azure için fiziksel](./site-recovery-set-up-physical-to-azure.md)

Bu makalede, Azure'da Windows veya Linux çalıştıran fiziksel sunucuları çoğaltıyor başlatmak için şirket içi ortamınızı ayarlayın açıklar.

## <a name="prerequisites"></a>Ön koşullar

Makale, zaten sahip olduğunuz varsayılmaktadır:
1. Kurtarma Hizmetleri kasası [Azure portal](http://portal.azure.com "Azure portal").
3. Yapılandırma sunucusu yüklemek için fiziksel bir bilgisayar.

### <a name="configuration-server-minimum-requirements"></a>Yapılandırma sunucusunun en düşük gereksinimleri
Aşağıdaki tabloda, en düşük donanım, yazılım ve yapılandırma sunucusu için ağ gereksinimleri listelenmektedir.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS tabanlı proxy sunucuları yapılandırma sunucusu tarafından desteklenmez.

## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. Azure portalında Git **kurtarma Hizmetleri** dikey kasaları ve kasanızı seçin.
2. İçinde **kaynak** kasasının menüsünü **Başlarken** > **Site Recovery** > **1. adım: altyapıyı hazırlama** > **koruma hedefi**.

    ![Hedefleri seçme](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. İçinde **koruma hedefi**seçin **için Azure** ve **değil sanallaştırılmış/diğer**ve ardından **Tamam**.

    ![Hedefleri seçme](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

1. İçinde **kaynağı**,'ı tıklatın, bir yapılandırma sunucusu yoksa **+ yapılandırma sunucusu** eklemesini.

  ![Kaynağı ayarlama](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. İçinde **Sunucu Ekle** dikey penceresinde denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda, kayıt anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. Yapılandırma sunucusu olarak kullandığınız makine üzerinde çalışan **Azure Site Recovery birleşik Kurulumu** yapılandırma sunucusuna işlem sunucusu ve ana hedef sunucusu yüklemek için.

#### <a name="run-azure-site-recovery-unified-setup"></a>Kurulumu çalıştırma Azure Site Recovery birleşik

> [!TIP]
> Bilgisayarınızın sistem saati beş dakikadan yerel saat dışına ise yapılandırma sunucusu kaydı başarısız olur. Sistem saatinizi bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) yüklemeyi başlatmadan önce.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu bir komut satırı yüklenebilir. Daha fazla bilgi için bkz: [komut satırı araçlarını kullanarak yükleme yapılandırma sunucusu](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Genel sorunlar

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım içerir [hedef ortamınızı ayarlarken](./site-recovery-prepare-target-physical-to-azure.md) azure'da.
