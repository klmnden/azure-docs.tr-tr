---
title: Microsoft Azure yığın sorunlarını giderme | Microsoft Docs
description: Azure yığın Geliştirme Seti (sorun giderme bilgileri ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 6c715f07f75c9196b7cf2cc8659c6e541e1260da
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30167912"
---
# <a name="microsoft-azure-stack-development-kit-asdk-troubleshooting"></a>Microsoft Azure yığın Geliştirme Seti (ASDK) sorunlarını giderme
Bu belge ASDK için genel sorun giderme bilgileri sağlar. Belgelenmemiştir bir sorun yaşıyorsanız, denetlediğinizden emin olun [Azure yığın MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) daha fazla Yardım ve bilgiler.  

> [!IMPORTANT]
> ASDK bir değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri'ne (CSS) aracılığıyla sunulan resmi desteği yoktur.

Bu bölümde açıklanan sorunlarını giderme önerileri çeşitli kaynaklardan türetilmiş ve olabilir veya belirli sorunu çözebilir değil. Kod örnekleri "olduğu gibi" sağlanmıştır ve beklenen sonuçları garanti edilemez. Ürün geliştirmeleri uygulandığı şekilde bu bölüm sık düzenlemeleri ve güncelleştirmeleri tabi değildir.

## <a name="deployment"></a>Dağıtım
### <a name="deployment-failure"></a>Dağıtım hatası
Yükleme sırasında bir hatayla karşılaşırsanız, başarısız adımı dağıtımından kullanarak yeniden başlatabilirsiniz - yeniden çalıştır seçeneğini aşağıdaki örnekteki gibi dağıtım komut dosyası:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>PowerShell oturumu dağıtımını sonunda hala açıksa ve herhangi bir çıktı göstermiyor
Seçili olduğunda bu davranış büyük olasılıkla yalnızca bir PowerShell komut penceresi varsayılan davranışını sonucudur. Geliştirme Seti Dağıtımı başarılı oldu ancak komut penceresi seçerken duraklatıldı. "Komut penceresinde titlebar içinde Seç" sözcüğünü bakarak kurulumu tamamlandı doğrulayabilirsiniz. Bunu seçimini kaldırmak için ESC tuşuna basın ve sonra tamamlama iletisi gösterilecek.

## <a name="virtual-machines"></a>Sanal makineler
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Bir Windows Server görüntüsü ve galeri öğesi Azure yığınında VM'ler dağıtmadan önce eklenmesi gerekir.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>My Azure yığın ana bilgisayar yeniden başlatıldıktan sonra bazı sanal makineleri otomatik olarak başlatılamayabilir.
Ana makine yeniden başlatıldıktan sonra Azure yığın Hizmetleri hemen kullanılamaz olduğunu görebilirsiniz. Azure yığın olmasıdır [altyapı VM'ler](asdk-architecture.md#virtual-machine-roles) ve bazı tutarlılık denetimi zaman ancak sonunda otomatik olarak başlatılacak RPs gerçekleştirin.

Ayrıca, sanal makineleri bir Azure yığın Geliştirme Seti ana bilgisayarı yeniden başlatmadan sonra otomatik olarak başlatma Kiracı fark edebilirsiniz. Bu bilinen bir sorundur ve yalnızca çevrimiçi duruma getirmek için birkaç el ile yapılacak adımlar gerektirir:

1.  Azure yığın Geliştirme Seti konakta başlatmak **yük devretme kümesi Yöneticisi** Başlat menüsünden.
2.  Kümeyi seçin **S Cluster.azurestack.local**.
3.  Seçin **rolleri**.
4.  Kiracı VM'ler görünür bir *kaydedilen* durumu. Tüm altyapı VM'ler çalıştıran sonra Kiracı Vm'lerinizin sağ tıklatın ve seçin **Başlat** VM sürdürmek için.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Bazı sanal makineler silinmiş, ancak hala disk üzerinde VHD dosyalarını bakın. Bu davranış beklenir?
Evet, bu beklenen davranıştır. Çünkü, bu şekilde tasarlanmıştır:

* Bir VM sildiğinizde, VHD silinmez. Diskler, kaynak grubundaki ayrı kaynaklardır.
* Bir depolama hesabı silindiğinde, bu silme işlemini hemen Azure Resource Manager aracılığıyla görünür, ancak çöp toplama çalıştırılana kadar içeriyor olabilir diskleri depolama hala tutulur.

"Artık" VHD'ler görürseniz, silinmiş olan bir depolama hesabı için klasör parçası olup olmadığını bilmek önemlidir. Depolama hesabı silinemedi, hala var oldukları normaldir.

Daha fazla bilgiyi bekletme eşiği ve isteğe bağlı geri kazanma içinde yapılandırma hakkında daha fazla [depolama hesaplarını yönetme](.\.\azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Depolama
### <a name="storage-reclamation"></a>Depolama geri kazanma
Portalda görünmesi kapasite iadesi 14 saate kadar sürebilir. Alan geri kazanma blok blob Mağazası'nda iç kapsayıcı dosyaları kullanım yüzdesi gibi çeşitli etkenlere bağlıdır. Bu nedenle, ne kadar veri silinmiş bağlı olarak, garantisi yoktur Atık toplayıcısının çalışacağı yükleyen iadesi alanı üzerinde.

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın destek forumunu ziyaret edin](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)

