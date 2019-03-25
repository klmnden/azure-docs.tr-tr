---
title: Bir Windows sanal masaüstü Önizleme konak havuzu - Azure için bir kullanıcı profili paylaşımını ayarlama
description: Bir Windows sanal masaüstü Önizleme konak havuz için bir FSLogix profili kapsayıcısı ayarlama yapma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: c9c2ca2cc27c5fa757b8ff6846e0a6a8f7087875
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403723"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Bir ana makine havuzu için bir kullanıcı profili paylaşımını ayarlama

Windows sanal masaüstü Önizleme hizmeti önerilen kullanıcı profili çözümü FSLogix profili kapsayıcılar sunar. Kullanıcı profili diski (UPD) çözümü kullanımı önerilmemektedir ve Windows sanal masaüstü gelecek sürümlerde kaldırılacak.

Bu bölümde bir ana makine havuzu için bir FSLogix profili kapsayıcı paylaşım kurma bildirir.

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Bir dosya paylaşımı olarak görev yapacak yeni bir sanal makine oluşturma

Sanal makine oluştururken ya da aynı sanal ağ üzerinde konak havuzu sanal makineler veya sanal makineleri barındıracak havuzu bağlantısı olan bir sanal ağ üzerindeki yerleştirdiğinizden emin olun. Birden çok yolla bir sanal makine oluşturabilirsiniz:

- [Bir Azure Galerisi görüntüsünü bir sanal makine oluşturun](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Yönetilen bir görüntüden sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Yönetilmeyen bir görüntüden sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

Sanal makineyi oluşturduktan sonra aşağıdaki işlemleri yaparak etki alanına katılmadan:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. Sanal makinede başlatın **Denetim Masası** seçip **sistem**.
3. Seçin **bilgisayar adı**seçin **ayarlarını değiştir**ve ardından **Değiştir...**
4. Seçin **etki alanı** ve ardından sanal ağ üzerinde Active Directory etki alanını girin.
5. Makine etki alanına katılım ayrıcalıkları olan bir etki alanı hesabıyla kimlik doğrulaması.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Kullanıcı profilleri için dosya paylaşımı olarak görev yapacak bir sanal makine hazırlama

Kullanıcı profilleri için dosya paylaşımı olarak görev yapacak bir sanal makine hazırlama hakkında genel yönergeler şunlardır:

1. Oturum ana bilgisayarı sanal makinelere katılmak bir [Active Directory güvenlik grubu](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups). Bu güvenlik grubu, yeni oluşturduğunuz dosya paylaşımı sanal makine oturumu ana bilgisayar sanal makinelere kimliğini doğrulamak için kullanılır.
2. [Dosya Paylaşımı sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. Dosya Paylaşımı sanal makinede, üzerinde bir klasör oluşturun. **C sürücüsüne** profili paylaşımı olarak kullanılacak.
4. Yeni klasöre sağ tıklayın, **özellikleri**seçin **paylaşım**, ardından **Gelişmiş paylaşım...** .
5. Seçin **bu klasörü paylaş**seçin **izinler...** , ardından **Ekle...** .
6. Oturumu konak sanal makinelerin eklediğiniz için güvenlik grubu arayın, ardından bu gruba sahip olduğundan emin olun **tam denetim**.
7. Güvenlik grubuna ekledikten sonra klasöre sağ tıklayın, **özellikleri**seçin **paylaşım**, ardından aşağı kopyalayın **ağ yolu** için daha sonra kullanmak üzere.

İçin en iyi uygulamalar, izinler, aşağıdaki bakın [FSLogix belgeleri](https://support.fslogix.com/index.php/forum-main/faqs/84-best-practices#120).

## <a name="configure-the-fslogix-profile-container"></a>FSLogix profili kapsayıcısını yapılandırma

Sanal makineler ile FSLogix yazılım yapılandırmak için aşağıdaki ana havuzuna kayıtlı her makinede yapın:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. Bir internet tarayıcısı başlatın ve aşağıdaki Git [bağlantı](https://go.microsoft.com/fwlink/?linkid=2084562) FSLogix aracısını indirmek için. Windows sanal masaüstü genel Önizleme kapsamında FSLogix yazılım etkinleştirmek için bir lisans anahtarı alırsınız. Anahtar FSLogix aracı .zip dosyasına dahil LicenseKey.txt dosyasıdır.
3. FSLogix aracıyı yükleyin.
4. Gidin **Program dosyaları** > **FSLogix** > **uygulamaları** yüklü bir aracıyı onaylamak için.
5. Başlat menüsünden çalıştırmak **RegEdit** yönetici olarak. Gidin **bilgisayar\\HKEY_LOCAL_MACHINE\\yazılım\\FSLogix\\profilleri**
6. Aşağıdaki değerleri oluşturun:

| Ad                | Type               | Veri/değer                        |
|---------------------|--------------------|-----------------------------------|
| Etkin             | DWORD              | 1                                 |
| VHDLocations        | Çok dizeli değer | "Dosya paylaşımı için ağ yolu" |
| VolumeType          | String             | VHDX                              |
| SizeInMBs           | DWORD              | "profil boyutu tamsayı"     |
| Isdynamic           | DWORD              | 1                                 |
| LockedRetryCount    | DWORD              | 1                                 |
| LockedRetryInterval | DWORD              | 0                                 |
