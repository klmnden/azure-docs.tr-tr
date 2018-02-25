---
title: "Microsoft Azure yığın sorunlarını giderme | Microsoft Docs"
description: "Azure yığını sorun giderme."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 799a7f7ed7e2373e4cf819a34d5deb362c9e6a3f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure yığın sorunlarını giderme

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Bu belgede Azure yığını için genel sorun giderme bilgileri sağlar. 

Azure yığın teknik Geliştirme Seti değerlendirme ortamı olarak sunulan olduğundan, Microsoft Müşteri Destek Hizmetleri'nden resmi desteği yoktur. Belgelenmemiştir bir sorun yaşıyorsanız, denetlediğinizden emin olun [Azure yığın MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) daha fazla Yardım ve bilgiler.  

Bu bölümde açıklanan sorunlarını giderme önerileri çeşitli kaynaklardan türetilmiş ve olabilir veya belirli sorunu çözebilir değil. Kod örnekleri olarak sağlanmıştır ve beklenen sonuçları garanti edilemez. Ürün geliştirmeleri uygulandığı şekilde bu bölüm sık düzenlemeleri ve güncelleştirmeleri tabi değildir.

## <a name="deployment"></a>Dağıtım
### <a name="deployment-failure"></a>Dağıtım hatası
Yükleme sırasında bir hatayla karşılaşırsanız, başarısız adımı dağıtımından kullanarak yeniden başlatabilirsiniz - yeniden çalıştır seçeneğini dağıtım komut dosyası.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>PowerShell oturumu dağıtımını sonunda hala açıksa ve herhangi bir çıktı göstermiyor
Seçili olduğunda bu davranış büyük olasılıkla yalnızca bir PowerShell komut penceresi varsayılan davranışını sonucudur. Geliştirme Seti Dağıtımı gerçekte başarılı oldu ancak komut penceresi seçerken duraklatıldı. "Komut penceresinde titlebar içinde Seç" sözcüğünü bakarak kurulumu tamamlandı doğrulayabilirsiniz.  Bunu seçimini kaldırmak için ESC tuşuna basın ve sonra tamamlama iletisi gösterilecek.

## <a name="virtual-machines"></a>Sanal makineler
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Bir Windows Server görüntüsü ve galeri öğesi Azure yığınında VM'ler dağıtmadan önce eklenmesi gerekir.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>My Azure yığın ana bilgisayar yeniden başlatıldıktan sonra bazı sanal makineleri otomatik olarak başlatılamayabilir.
Ana makine yeniden başlatıldıktan sonra Azure yığın Hizmetleri hemen kullanılamaz olduğunu görebilirsiniz.  Azure yığın olmasıdır [altyapı VM'ler](azure-stack-architecture.md#virtual-machine-roles) ve RPs tutarlılık denetimi için çok az bit etkinleştirilir, ancak sonunda otomatik olarak başlatılacak.

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

Daha fazla bilgiyi bekletme eşiği ve isteğe bağlı geri kazanma içinde yapılandırma hakkında daha fazla [depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Depolama
### <a name="storage-reclamation"></a>Depolama geri kazanma
Portalda görünmesi kapasite iadesi on dört saate kadar sürebilir. Alan geri kazanma blok blob Mağazası'nda iç kapsayıcı dosyaları kullanım yüzdesi gibi çeşitli etkenlere bağlıdır. Bu nedenle, ne kadar veri silinmiş bağlı olarak, garantisi yoktur Atık toplayıcısının çalışacağı yükleyen iadesi alanı üzerinde.

## <a name="windows-azure-pack-connector"></a>Windows Azure Pack Bağlayıcısı
* Azure yığın Geliştirme Seti dağıttıktan sonra azurestackadmin hesabının parolasını değiştirirseniz, artık birden çok bulut modunu yapılandırabilirsiniz. Bu nedenle, hedef Windows Azure Pack ortamına bağlamak mümkün olmayacaktır.
* Birden çok bulut Modu'nu ayarla sonra:
    * Yalnızca bunlar portal ayarlarını sıfırladıktan sonra bir kullanıcı Pano görebilirsiniz. (Kullanıcı Portalı'nda (dişli simgesine sağ üst köşesinde) portal ayarları simgesine tıklayın. Altında **varsayılan ayarları geri**, tıklatın **Uygula**.)
    * Pano başlıklarını görünmeyebilir. Bu sorun oluşursa, el ile bunları geri eklemeniz gerekir.
    * İlk bunları panoya eklediğinizde bazı kutucuklar doğru şekilde göstermeyebilir. Bu sorunu gidermek için tarayıcıyı yenileyin.



