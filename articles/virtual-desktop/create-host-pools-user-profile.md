---
title: Bir Windows sanal masaüstü Önizleme konak havuzu - Azure için bir kullanıcı profili paylaşımını ayarlama
description: Bir Windows sanal masaüstü Önizleme konak havuz için bir FSLogix profili kapsayıcısı ayarlama yapma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: f6516e37107a16d80c4d9eb9514782bdbcc44184
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925205"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Ana bilgisayar havuzu için kullanıcı profili paylaşımı ayarlama

Windows sanal masaüstü Önizleme hizmeti önerilen kullanıcı profili çözümü FSLogix profili kapsayıcılar sunar. Kullanım dışı olacak kullanıcı profili diski (UPD) çözümünü kullanarak gelecek sürümlerinde Windows sanal masaüstünün önerilmemektedir.

Bu bölümde bir ana makine havuzu için bir FSLogix profili kapsayıcı paylaşım kurma bildirir. FSLogix ilgili genel belgeler için bkz. [FSLogix site](https://docs.fslogix.com/).

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

1. Windows sanal masaüstü Active Directory Kullanıcıları için ekleme bir [Active Directory güvenlik grubu](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups). Bu güvenlik grubu, yeni oluşturduğunuz dosya paylaşımı sanal makineye Windows sanal masaüstü kullanıcıların kimliğini doğrulamak için kullanılır.
2. [Dosya Paylaşımı sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. Dosya Paylaşımı sanal makinede, üzerinde bir klasör oluşturun. **C sürücüsüne** profili paylaşımı olarak kullanılacak.
4. Yeni klasöre sağ tıklayın, **özellikleri**seçin **paylaşım**, ardından **Gelişmiş paylaşım...** .
5. Seçin **bu klasörü paylaş**seçin **izinler...** , ardından **Ekle...** .
6. Windows Sanal Masaüstü Kullanıcıları eklediğiniz için güvenlik grubu arayın, ardından bu gruba sahip olduğundan emin olun **tam denetim**.
7. Güvenlik grubuna ekledikten sonra klasöre sağ tıklayın, **özellikleri**seçin **paylaşım**, ardından aşağı kopyalayın **ağ yolu** için daha sonra kullanmak üzere.

İzinler hakkında daha fazla bilgi için bkz. [FSLogix belgeleri](https://docs.fslogix.com/display/20170529/Requirements%2B-%2BProfile%2BContainers).

## <a name="configure-the-fslogix-profile-container"></a>FSLogix profili kapsayıcısını yapılandırma

Sanal makineler ile FSLogix yazılım yapılandırmak için aşağıdaki ana havuzuna kayıtlı her makinede yapın:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. Bir internet tarayıcısı başlatın ve gidin [bu bağlantıyı](https://go.microsoft.com/fwlink/?linkid=2084562) FSLogix aracısını indirmek için. Windows sanal masaüstü genel Önizleme kapsamında FSLogix yazılım etkinleştirmek için bir lisans anahtarı alırsınız. Anahtar FSLogix aracı .zip dosyasına dahil LicenseKey.txt dosyasıdır.
3. Ya da gidin \\ \\Win32\\sürüm veya \\ \\X64\\.zip dosyası ve çalışma sürümde **FSLogixAppsSetup** FSLogix aracıyı yüklemek için.
4. Gidin **Program dosyaları** > **FSLogix** > **uygulamaları** yüklü bir aracıyı onaylamak için.
5. Başlat menüsünden çalıştırmak **RegEdit** yönetici olarak. Gidin **bilgisayar\\HKEY_LOCAL_MACHINE\\yazılım\\FSLogix**.
6. Adlı bir anahtar oluşturun **profilleri**.
7. Profilleri anahtarı için aşağıdaki değerleri oluşturun:

| Ad                | Tür               | Veri/değer                        |
|---------------------|--------------------|-----------------------------------|
| Enabled             | DWORD              | 1                                 |
| VHDLocations        | Çok dizeli değer | "Dosya paylaşımı için ağ yolu"     |

>[!IMPORTANT]
>Güvenliğini sağlamaya yardımcı olmak için azure'da Windows sanal masaüstü ortamınızı Vm'lerinizde gelen bağlantı noktası 3389 açmayın öneririz. Windows sanal masaüstü açık bir konak havuzun Vm'leri erişmek kullanıcılar için 3389 numaralı gelen bağlantı noktası gerektirmez. Sorun giderme amacıyla 3389 numaralı bağlantı noktası açmanız gerekiyorsa, kullanmanızı öneririz [tam zamanında VM erişimi](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).