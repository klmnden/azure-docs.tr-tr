---
title: Windows sanal masaüstü Önizleme - Azure GPU yapılandırın
description: GPU hızlandırmalı işleme ve Windows sanal masaüstü önizlemede kodlama elverişli hale getirme.
services: virtual-desktop
author: gundarev
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/06/2019
ms.author: denisgun
ms.openlocfilehash: a6a67c89253a1b16f9266d7917655d1b1104022e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159578"
---
# <a name="configure-graphics-processing-unit-gpu-acceleration-for-windows-virtual-desktop-preview"></a>Grafik işlem birimi (GPU) hızlandırma Windows sanal masaüstü Önizleme için yapılandırma

Windows sanal masaüstü önizlemesi, GPU hızlandırmalı işleme ve Gelişmiş uygulama performans ve ölçeklenebilirlik için kodlama destekler. GPU hızlandırma grafik kullanımı yoğun uygulamalar için özellikle önemlidir.

GPU hızlandırma oluşturma ve kodlama için kullanılacak GPU için iyileştirilmiş bir Azure sanal makinesi oluşturun, konak havuzunuza ekleme ve yapılandırmak için bu makaledeki yönergeleri izleyin. Bu makalede, yapılandırılmış bir Windows sanal masaüstü kiracısına zaten sahip varsayılır.

## <a name="select-a-gpu-optimized-azure-virtual-machine-size"></a>GPU için iyileştirilmiş bir Azure sanal makine boyutu seçin

Azure'un sunduğu birçok [GPU için iyileştirilmiş sanal makine boyutları](/azure/virtual-machines/windows/sizes-gpu). Konak havuzunuz için doğru seçenek belirli uygulama iş yükleri, kullanıcı deneyimi ve maliyet istenen kalite dahil çeşitli Etkenler sayısına bağlıdır. Genel olarak, daha büyük ve daha yetenekli Gpu'lar verili kullanıcı yoğunluğunu en iyi bir kullanıcı deneyimi sunar.

## <a name="create-a-host-pool-provision-your-virtual-machine-and-configure-an-app-group"></a>Bir konak havuzu oluşturun, sanal makinenize kaynak sağlamak ve bir uygulama grubu yapılandırma

Seçilen boyutta bir VM kullanarak yeni bir konak havuzu oluşturun. Yönergeler için [Öğreticisi: Azure Marketi ile konak havuz oluşturma](/azure/virtual-desktop/create-host-pools-azure-marketplace).

Windows sanal masaüstü önizlemesi, GPU hızlandırmalı işleme ve aşağıdaki işletim sistemlerinde kodlamayı destekler:

* Windows 10 sürüm 1511 veya daha yeni
* Windows Server 2016 veya daha yenisi

Ayrıca bir uygulama grubu yapılandırma veya yeni bir ana makine havuzu oluşturduğunuzda, otomatik olarak oluşturulan ("Masaüstü uygulama grubu" olarak adlandırılır) varsayılan masaüstü uygulaması gruplandırma gerekir. Yönergeler için [Öğreticisi: Windows sanal masaüstü Önizleme için uygulama gruplarını yönetme](/azure/virtual-desktop/manage-app-groups).

>[!NOTE]
>Windows sanal masaüstü önizlemesi, GPU özellikli konak havuzlar için yalnızca "Masaüstü" uygulama grubu türü destekler. Uygulama grupları "RemoteApp" türündeki GPU özellikli konak havuzlar için desteklenmiyor.

## <a name="install-supported-graphics-drivers-in-your-virtual-machine"></a>Sanal makinenizi desteklenen grafik sürücüleri yükleyin

Windows sanal masaüstü önizlemede Azure N serisi VM'ler, GPU özelliklerine yararlanmak için NVIDIA grafik sürücüleri yüklemeniz gerekir. Konumundaki yönergeleri [yükleme NVIDIA GPU sürücüleri Windows çalıştıran N serisi vm'lerde](/azure/virtual-machines/windows/n-series-driver-setup) sürücüleri ya da el ile yüklemek için veya bu adı kullanıyor [NVIDIA GPU sürücüsünün uzantısı](/azure/virtual-machines/extensions/hpccompute-gpu-windows).

Not yalnızca [NVIDIA GRID sürücüleri](/azure/virtual-machines/windows/n-series-driver-setup#nvidia-grid-drivers) Azure tarafından dağıtılmış Windows sanal masaüstü Önizleme için desteklenmektedir.

Sürücü yükleme sonrasında, VM yeniden başlatma gereklidir. Doğrulama adımları, grafik sürücüleri başarıyla yüklendiğini doğrulamak için yukarıdaki yönergeleri kullanın.

## <a name="configure-gpu-accelerated-app-rendering"></a>Uygulama GPU hızlandırmalı işleme yapılandırın

Varsayılan olarak, uygulamalara ve masaüstlerine çoklu oturum yapılandırmasında çalışan ve CPU işlenir ve işleme için mevcut Gpu'lar yararlanıyor mu. GPU hızlandırmalı işleme olanağı oturumu ana bilgisayarı için Grup İlkesi yapılandırın:

1. Yerel yönetici ayrıcalıklarına sahip bir hesap kullanarak VM masaüstüne bağlanın.
2. "Grup İlkesi Düzenleyicisi'ni açmak için Başlat menüsü ve türü gpedit.msc" açın.
3. Ağaç için Git **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri**  >   **Uzak Masaüstü Hizmetleri** > **Uzak Masaüstü oturumu konağı** > **uzak oturumu ortam**.
4. İlkeyi seçin **donanım varsayılan grafik bağdaştırıcısının tüm Uzak Masaüstü Hizmetleri oturumlarına kullanın** ve bu ilke kümesine **etkin** uzak oturumu de GPU işleme olanağı.

## <a name="configure-gpu-accelerated-frame-encoding"></a>GPU hızlandırmalı çerçeve kodlama yapılandırın

Uzak Masaüstü (CPU veya GPU ile işlenmiş olup olmadığını) uygulamaları ve masaüstü bilgisayarlar tarafından işlenen tüm grafik iletilmesi için Uzak Masaüstü istemcilerini kodlar. Varsayılan olarak, Uzak Masaüstü, kullanılabilir GPU'ları bu kodlama için yararlanmaz. GPU hızlandırmalı çerçeve kodlamasını etkinleştirmek oturumu ana bilgisayarı için Grup İlkesi'ni yapılandırın. Yukarıdaki adımları etmeden:

1. İlkeyi seçin **Uzak Masaüstü bağlantıları için öncelik H.264/AVC 444 grafik modu** ve bu ilke kümesine **etkin** H.264/AVC 444 codec uzak oturumu zorlamak için.
2. İlkeyi seçin **yapılandırma H.264/AVC donanım Uzak Masaüstü bağlantıları için kodlama** ve bu ilke kümesine **etkin** donanım uzak oturumu için AVC/H.264 kodlamasını etkinleştirmek için.

    >[!NOTE]
    >Windows Server 2016'da seçenek kümesi **tercih AVC donanım kodlama** için **her zaman**.

3. Grup İlkeleri düzenlendi, bir Grup İlkesi güncelleştirmesini zorla. Komut istemi açıp şunu yazın:

    ```batch
    gpupdate.exe /force
    ```

4. Uzak Masaüstü oturumundan oturumu kapatın.

## <a name="verify-gpu-accelerated-app-rendering"></a>Uygulama GPU hızlandırmalı işleme doğrulayın

Uygulamaları GPU işleme için kullandığını doğrulamak için aşağıdakilerden birini deneyin:

* Kullanım `nvidia-smi` bölümünde anlatıldığı gibi bir yardımcı programı [sürücü yüklemesini doğrulama](/azure/virtual-machines/windows/n-series-driver-setup#verify-driver-installation) uygulamalarınızı çalıştırırken için GPU kullanımını denetlemek için.
* Desteklenen işletim sistemi sürümlerinde, GPU kullanımı denetlemek için Görev Yöneticisi'ni kullanabilirsiniz. GPU uygulamaları GPU kullandığını görmek için "Performans" sekmesini seçin.

## <a name="verify-gpu-accelerated-frame-encoding"></a>GPU hızlandırmalı çerçeve kodlama doğrulayın

Uzak Masaüstü GPU hızlandırmalı kodlama kullandığını doğrulamak için:

1. Windows Sanal Masaüstü İstemcisi'ni kullanarak VM masaüstüne bağlanın.
2. Olay Görüntüleyicisi'ni başlatın ve aşağıdaki düğümüne gidin: **Uygulama ve hizmet günlükleri** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreTS**  >  **İşletimsel**
3. GPU hızlandırmalı kodlama kullanılıyorsa belirlemek için olay kimliği 170 için bakın. Portalın "AVC donanım Kodlayıcı etkin: 1" sonra GPU kodlama kullanılır.
4. AVC 444 mod kullanılıp kullanılmadığını belirlemek için olay kimliği 162 için bakın. Portalın "AVC kullanılabilir: 1 ilk profili: 2048" ardından AVC 444 kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu yönergeler bir oturumla konakta VM GPU hızlandırmalı ve çalışıyor olmalıdır. Daha büyük bir konak havuzunda GPU ivmesini etkinleştirmek için bazı ek hususlar:

* Kullanmayı [NVIDIA GPU sürücüsünün uzantısı](/azure/virtual-machines/extensions/hpccompute-gpu-windows) sürücü yükleme ve güncelleştirme VM sayısı arasında basitleştirmek için.
* Grup İlkesi yapılandırması VM sayısı arasında basitleştirmek için Active Directory Grup İlkesi'ni kullanmayı düşünün. Grup İlkesi Active Directory etki alanında dağıtma hakkında daha fazla bilgi için bkz: [Grup İlkesi nesneleri ile çalışma](https://go.microsoft.com/fwlink/p/?LinkId=620889).
