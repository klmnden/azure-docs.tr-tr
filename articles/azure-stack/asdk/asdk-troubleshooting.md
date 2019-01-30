---
title: Microsoft Azure Stack sorunlarını giderme | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (sorun giderme bilgileri ASDK).
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
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 2111fe6a70f45559faeb3e0f8096548dcc7b48bc
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238950"
---
# <a name="microsoft-azure-stack-development-kit-asdk-troubleshooting"></a>Microsoft Azure Stack geliştirme Seti'ni (ASDK) sorunlarını giderme
Bu belge ASDK için genel sorun giderme bilgileri sağlar. Belgelenmemiş bir sorunla karşılaşıyorsanız, kontrol ettiğinizden emin olun [Azure Stack MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) ilişkin daha fazla Yardım ve bilgileri.  

> [!IMPORTANT]
> ASDK değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla sunulan resmi desteği yoktur.

Bu bölümde açıklanan sorunları gidermek için öneriler, çeşitli kaynaklardan elde edilen ve olabilir veya belirli sorununuzu çözebilir değil. Kod örnekleri "olduğu gibi" sağlanmıştır ve beklenen sonuçları garanti edilemez. Bu bölüm, ürün geliştirmeleri uygulandığı şekilde sık düzenlemeleri ve güncelleştirmeleri tabi değildir.

## <a name="deployment"></a>Dağıtım
### <a name="deployment-failure"></a>Dağıtım hatası
Yükleme sırasında bir hatayla karşılaşırsanız, başarısız olan adımda dağıtımdan kullanarak başlatabilirsiniz dağıtım betiği aşağıdaki örnekte olduğu gibi yeniden çalıştırma seçeneği:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Dağıtımın sonundaki PowerShell oturumu hala açıksa ve herhangi bir çıktı göstermiyor
Seçili olduğunda bu davranış büyük olasılıkla yalnızca bir PowerShell komut penceresi varsayılan davranışını sonucudur. Geliştirme Seti dağıtım başarılı oldu ancak komut penceresi seçerken duraklatıldı. "Komut penceresinde başlık çubuğu seçin" sözcüğünü bakarak Kurulum Tamamlandı doğrulayabilirsiniz. Bunu seçimini kaldırmak için ESC tuşuna basın ve sonra tamamlama ileti gösterilecek.

## <a name="virtual-machines"></a>Sanal makineler
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Azure stack'teki Vm'leri dağıtmadan önce bir Windows Server görüntüsünü ve galeri öğesi eklenmesi gerekir.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>My Azure Stack ana bilgisayar yeniden başlatıldıktan sonra bazı VM'ler otomatik olarak başlatılamayabilir.
Ana bilgisayar yeniden başlatıldıktan sonra Azure Stack Hizmetleri hemen kullanılamaz fark edebilirsiniz. Azure Stack olmasıdır [altyapı Vm'leri](asdk-architecture.md#virtual-machine-roles) ve bazı tutarlılık denetimi zaman ancak sonunda otomatik olarak başlatılacak RPs gerçekleştirin.

Ayrıca, bu Kiracı Vm'leri otomatik olarak Azure Stack Geliştirme Seti konağın yeniden başlatmanın ardından başlatma fark edebilirsiniz. Bu bilinen bir sorundur ve yalnızca çevrimiçi duruma getirmek için birkaç adımı gerektirir:

1.  Azure Stack Geliştirme Seti konağında başlatın **yük devretme kümesi Yöneticisi** Başlat menüsünden.
2.  Kümeyi seçin **S Cluster.azurestack.local**.
3.  Seçin **rolleri**.
4.  Kiracı VM görünür bir *kaydedilmiş* durumu. Tüm altyapı Vm'lere çalışmaya sonra Kiracı Vm'lere sağ tıklayıp **Başlat** VM sürdürmek için.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Bazı sanal makineler silinmiş, ancak disk üzerinde VHD dosyalarını görmeye devam ediyor. Bu davranış beklenmektedir?
Evet, bu beklenen bir davranıştır. Çünkü, bu şekilde tasarlanmıştır:

* Bir VM'yi sildiğinizde VHD'ler silinmez. Diskleri, kaynak grubundaki ayrı kaynaklardır.
* Bir depolama hesabı silindiğinde, silme işlemini Azure Resource Manager aracılığıyla hemen görünür, ancak çöp toplama çalışana kadar içerebilir diskleri yine de depolama alanında tutulur.

"Artık" VHD'ler görürseniz, klasör, silinen bir depolama hesabı için bir parçası olup olmadığını bilmek önemlidir. Depolama hesabı silindi, hala orada mısınız normal bir durumdur.

Daha fazla bekletme eşiği ve isteğe bağlı kazanma yapılandırma hakkında daha fazla [depolama hesaplarını yönetme](../azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Depolama
### <a name="storage-reclamation"></a>Depolama geri kazanma
Bu portalda gösterilmesi, geri kazanılan Kapasite 14 saate kadar sürebilir. Alan geri kazanma bloğu blob Deposu'nda kullanım yüzdesi iç kapsayıcı dosyaları gibi çeşitli etkenlere bağlıdır. Bu nedenle, ne kadar veri silinmiş bağlı olarak, garantisi yoktur Atık toplayıcısının çalışacağı kazanılacak alanı miktarı.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack destek forumunu ziyaret edin](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)
