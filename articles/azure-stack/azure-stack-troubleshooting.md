---
title: Microsoft Azure Stack sorunlarını giderme | Microsoft Docs
description: Azure Stack sorunlarını giderme.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: a74fb749e130b565c44c637bfc16ff09e3314a05
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857174"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure Stack sorunlarını giderme

Bu belge, Azure Stack için genel sorun giderme bilgileri sağlar. 

> [!NOTE]
> Azure Stack teknik geliştirme Seti'ni (ASDK) bir değerlendirme ortamı olarak sunulur çünkü Microsoft Müşteri Destek Hizmetleri resmi desteği yoktur. Bir sorun yaşıyorsanız, kontrol ettiğinizden emin olun [Azure Stack MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) ilişkin daha fazla Yardım ve bilgileri.  

Bu bölümde açıklanan sorunları gidermek için öneriler, çeşitli kaynaklardan elde edilen ve olabilir veya belirli sorununuzu çözebilir değil. Kod örnekleri olarak sağlanır ve beklenen sonuçları garanti edilemez. Bu bölüm, ürün geliştirmeleri uygulandığı şekilde sık düzenlemeleri ve güncelleştirmeleri tabi değildir.

## <a name="deployment"></a>Dağıtım
### <a name="general-deployment-failure"></a>Genel Dağıtım hatası
Yükleme sırasında bir hatayla karşılaşırsanız, başarısız olan adımda dağıtımdan kullanarak başlatabilirsiniz dağıtım betiği yeniden çalıştırma seçeneği.  

### <a name="at-the-end-of-asdk-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>ASDK dağıtım sonunda, PowerShell oturumunu hala açıksa ve herhangi bir çıktı göstermez.
Seçili olduğunda bu davranış büyük olasılıkla yalnızca bir PowerShell komut penceresi varsayılan davranışını sonucudur. Geliştirme Seti dağıtım başarılı oldu ancak komut penceresi seçerken duraklatıldı. "Komut penceresinde başlık çubuğu seçin" sözcüğünü bakarak Kurulum Tamamlandı doğrulayabilirsiniz.  Bunu seçimini kaldırmak için ESC tuşuna basın ve sonra tamamlama ileti gösterilecek.

### <a name="deployment-fails-due-to-lack-of-external-access"></a>Dış erişimi alınamadığından nedeniyle dağıtım başarısız
Dış erişim gerekli olduğu aşamalarda dağıtım başarısız olduğunda, aşağıdaki örnekte olduğu gibi bir özel durum döndürülür:

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
Bu hata ortaya çıkarsa emin olmak için gözden geçirerek en düşük ağ gereksinimlerin karşılandığından denetleyin [dağıtım ağ trafiği belgeleri](deployment-networking.md). Bir ağ Denetleyicisi aracı ayrıca iş ortağı Araç Seti parçası olarak iş ortakları için kullanılabilir.

Dağıtım hatalarını yukarıdaki durumla Internet üzerindeki kaynaklara bağlanma konusunda sorunlar genellikle kaynaklanır

Bu sorunu olduğunu doğrulamak için aşağıdaki adımları gerçekleştirebilirsiniz:

1. PowerShell'i açın
2. WAS01 veya ERCs VM'lerin girin-PSSession
3. Komutunu çalıştırın: Test-NetConnection login.windows.net -port 443

Bu komut başarısız olursa, TOR anahtarını ve diğer ağ cihazları için yapılandırılan doğrulayın [ağ trafiğine izin veren](azure-stack-network.md).

## <a name="virtual-machines"></a>Sanal makineler
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Azure stack'teki Vm'leri dağıtmadan önce bir Windows Server görüntüsünü ve galeri öğesi eklenmesi gerekir.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>My Azure Stack ana bilgisayar yeniden başlatıldıktan sonra bazı VM'ler otomatik olarak başlatılamayabilir.
Ana bilgisayar yeniden başlatıldıktan sonra Azure Stack Hizmetleri hemen kullanılamaz fark edebilirsiniz.  Azure Stack olmasıdır [altyapı Vm'leri](../azure-stack/asdk/asdk-architecture.md#virtual-machine-roles) ve kaynak sağlayıcıları tutarlılık denetimi için biraz zaman alabilir ama sonunda otomatik olarak başlatılacak.

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

Daha fazla bekletme eşiği ve isteğe bağlı kazanma yapılandırma hakkında daha fazla [depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Depolama
### <a name="storage-reclamation"></a>Depolama geri kazanma
Bu portalda gösterilmesi, geri kazanılan Kapasite 14 saate kadar sürebilir. Alan geri kazanma bloğu blob Deposu'nda kullanım yüzdesi iç kapsayıcı dosyaları gibi çeşitli etkenlere bağlıdır. Bu nedenle, ne kadar veri silinmiş bağlı olarak, garantisi yoktur Atık toplayıcısının çalışacağı kazanılacak alanı miktarı.

