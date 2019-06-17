---
title: Microsoft Azure Site Recovery sağlayıcısı yükseltme sorunlarını giderme | Microsoft Docs
description: Anlama ve
author: vDonGlover
manager: jarrettr
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 02/05/2019
ms.author: v-doglov
ms.openlocfilehash: fc50be2a960784895947f3f154a0251f41716fc7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565710"
---
# <a name="troubleshoot-microsoft-azure-site-recovery-provider-upgrade-failures"></a>Microsoft Azure Site Recovery Sağlayıcısını yükseltme hatalarını giderme

Bu makalede yükseltme sırasında bir Microsoft Azure Site Recovery sağlayıcısı hatalara neden olabilen sorunları çözmenize yardımcı olur.

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>En son Site Recovery sağlayıcısı zaten yüklü olduğunu bildirdiği yükseltme başarısız

Microsoft Azure Site kurtarma sağlayıcısı (DRA) yükseltme yaparken, birleşik Kurulum'u yükseltme başarısız olur ve hata iletisi verir:

Yazılımı daha yüksek bir sürümü zaten yüklü olarak yükseltme desteklenmez.

Yükseltmek için aşağıdaki adımları kullanın:

1. İndirme Microsoft Azure Site Recovery birleşik Kurulumu:
   1. "Şu anda desteklenen güncelleştirme paketlerini bağlantılar" bölümünde [hizmet güncelleştirmeleri Azure Site recovery'de](service-updates-how-to.md##links-to-currently-supported-update-rollups) makalesi, kendisine yükselttiğiniz bir sağlayıcıyı seçin.
   2. Toplama sayfasında bulun **güncelleştirme bilgileri** bölümü ve Microsoft Azure Site Recovery birleşik kurulumu için güncelleştirme paketi yükleyin.

2. Bir komut istemi açın ve birleşik kurulum dosyasını karşıdan yüklediğiniz klasöre gidin. Kurulum dosyalarını kullanarak aşağıdaki komut, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q x: indirilerek ayıklamak&lt;ayıklanan dosyaları için klasör yolu&gt;.
    
    Örnek komut:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

3. Komut isteminde, dosyaları ayıkladığınız klasöre gidin ve aşağıdaki yükleme komutları çalıştırın:
   
    CX_THIRDPARTY_SETUP. EXE /VERYSILENT /SUPPRESSMSGBOXES/NORESTART UCX_SERVER_SETUP. EXE /VERYSILENT /SUPPRESSMSGBOXES/NORESTART / UPGRADE

1. Birleşik Kurulum yüklediğiniz klasöre geri dönmek ve MicrosoftAzureSiteRecoveryUnifiedSetup.exe yükseltmeyi tamamlamak için çalıştırın. 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>Yükseltme hatası nedeniyle adlandırılan 3. taraf klasörü

Yükseltmenin başarılı olması için 3. taraf klasörü kaydedilmelidir değil.

Bu sorunu çözmek için.

1. Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın ve HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\Installed Products\10 dalı açın.
1. İnceleme `Build_Version` anahtar değeri. En son sürüme olarak ayarlanırsa sürüm numarası azaltın. Örneğin, en son sürümü 9.22 ise. \* ve `Build_Version` anahtar değeri kümesine, sonra için 9.21 azaltın.\*.
1. En son Microsoft Azure Site Recovery birleşik Kurulumu indirme:
   1. "Şu anda desteklenen güncelleştirme paketlerini bağlantılar" bölümünde [hizmet güncelleştirmeleri Azure Site recovery'de](service-updates-how-to.md##links-to-currently-supported-update-rollups) makalesi, kendisine yükselttiğiniz bir sağlayıcıyı seçin.
   2. Toplama sayfasında bulun **güncelleştirme bilgileri** bölümü ve Microsoft Azure Site Recovery birleşik kurulumu için güncelleştirme paketi yükleyin.
1. Bir komut istemi açın ve için indirdiğiniz birleşik kurulum dosyası ve ayıklama klasöre kurulum dosyalarını kullanarak aşağıdaki komut, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q x: indirilerek gidin&lt;için klasör yolu Ayıklanan dosyaları&gt;.

    Örnek komut:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

1. Komut isteminde, dosyaları ayıkladığınız klasöre gidin ve aşağıdaki yükleme komutları çalıştırın:
   
    CX_THIRDPARTY_SETUP. EXE /VERYSILENT /SUPPRESSMSGBOXES/NORESTART

1. Yüklemenin ilerleme durumunu izlemek için Görev Yöneticisi'ni kullanın. Zaman CX_THIRDPARTY_SETUP işlemi. EXE artık Görev Yöneticisi'nde göründüğünden, sonraki adıma geçin.
1. C:\thirdparty var olduğundan ve klasörün RRD kitaplıkları içerdiğini doğrulayın.
1. Birleşik Kurulum yüklediğiniz klasöre geri dönmek ve MicrosoftAzureSiteRecoveryUnifiedSetup.exe yükseltmeyi tamamlamak için çalıştırın. 