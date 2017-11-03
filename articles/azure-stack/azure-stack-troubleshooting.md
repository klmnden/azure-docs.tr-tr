---
title: "Microsoft Azure yığın sorunlarını giderme | Microsoft Docs"
description: "Azure yığını sorun giderme."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 3b40a657ee8eb391d14a38cb95acc0729a8dda21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure yığın sorunlarını giderme

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Bu belgede Azure yığını için genel sorun giderme bilgileri sağlar. 

Azure yığın teknik Geliştirme Seti değerlendirme ortamı olarak sunulan olduğundan, Microsoft Müşteri Destek Hizmetleri'nden resmi desteği yoktur.  Belgelenmemiştir bir sorun yaşıyorsanız, denetlediğinizden emin olun [Azure yığın MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) daha fazla Yardım ve bilgiler.  

Bu bölümde açıklanan sorunlarını giderme önerileri çeşitli kaynaklardan türetilmiş ve olabilir veya belirli sorunu çözebilir değil. Kod örnekleri olarak sağlanmıştır ve beklenen sonuçları garanti edilemez. Ürün geliştirmeleri uygulandığı şekilde bu bölüm sık düzenlemeleri ve güncelleştirmeleri tabi değildir.

## <a name="deployment"></a>Dağıtım
### <a name="deployment-failure"></a>Dağıtım hatası
Yükleme sırasında bir hatayla karşılaşırsanız, kullanabileceğiniz başarısız adımı dağıtımından yeniden başlatmak için dağıtım betiği yeniden çalıştır seçeneğini kullanın.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>PowerShell oturumu dağıtımını sonunda hala açıksa ve herhangi bir çıktı göstermiyor
Seçili olduğunda bu davranış büyük olasılıkla yalnızca bir PowerShell komut penceresi varsayılan davranışını sonucudur. Geliştirme Seti Dağıtımı gerçekte başarılı oldu ancak komut penceresi seçerken duraklatıldı. "Komut penceresinde titlebar içinde Seç" sözcüğünü bakarak böyledir doğrulayabilirsiniz.  Bunu seçimini kaldırmak için ESC tuşuna basın ve sonra tamamlama iletisi gösterilecek.

## <a name="templates"></a>Şablonlar
### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure şablonu Azure yığınına dağıtmayacaksanız
Olduğundan emin olun:

* Şablon kullanılabilir veya Azure yığınında önizlemede zaten bir Microsoft Azure hizmet kullanıyor olmanız gerekir.
* Belirli bir kaynak için kullanılan API'leri, yerel Azure yığın örneği tarafından desteklenir ve geçerli bir konum ("yerel" Azure yığın Geliştirme Seti, vs "Doğu ABD" veya "Güney Hindistan'da" Azure içinde) hedefleme.
* Gözden [bu makalede](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) Test-AzureRmResourceGroupDeployment cmdlet'leri hakkında hangi catch Azure Resource Manager sözdizimi küçük farklılıklar.

Ayrıca zaten sağlanan Azure yığın şablonları kullanabilirsiniz [GitHub deposunu](http://aka.ms/AzureStackGitHub/) başlamanıza yardımcı olacak.

## <a name="virtual-machines"></a>Sanal makineler
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Sanal makineleri Azure yığınında dağıtmadan önce bir Windows Server görüntüsü ve galeri öğesi eklemeniz gerekir.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>My Azure yığın ana bilgisayar yeniden başlatıldıktan sonra bazı sanal makineleri otomatik olarak başlatılamayabilir.
Ana makine yeniden başlatıldıktan sonra Azure yığın Hizmetleri hemen kullanılamaz olduğunu görebilirsiniz.  Azure yığın olmasıdır [altyapı VM'ler](azure-stack-architecture.md#virtual-machine-roles) ve RPs tutarlılık denetimi için çok az bit etkinleştirilir, ancak sonunda otomatik olarak başlatılacak.

Ayrıca, sanal makineleri bir Azure yığın Geliştirme Seti ana bilgisayarı yeniden başlatmadan sonra otomatik olarak başlatma Kiracı fark edebilirsiniz.  Bu bilinen bir sorundur ve yalnızca çevrimiçi duruma getirmek için birkaç el ile yapılacak adımlar gerektirir:

1.  Azure yığın Geliştirme Seti konakta başlatmak **yük devretme kümesi Yöneticisi** Başlat menüsünden.
2.  Kümeyi seçin **S Cluster.azurestack.local**.
3.  Seçin **rolleri**.
4.  Kiracı VM'ler görünür bir *kaydedilen* durumu.  Tüm altyapı VM'ler çalıştıran sonra Kiracı Vm'lerinizin sağ tıklatın ve seçin **Başlat** VM sürdürmek için.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Bazı sanal makineler silinmiş, ancak hala disk üzerinde VHD dosyalarını bakın. Bu davranış beklenir?
Evet, bu beklenen davranıştır. Çünkü, bu şekilde tasarlanmıştır:

* Bir VM sildiğinizde, VHD silinmez. Diskler, kaynak grubundaki ayrı kaynaklardır.
* Bir depolama hesabı silindiğinde, bu silme işlemini hemen Azure Resource Manager aracılığıyla (portal, PowerShell) görünür ancak çöp toplama çalıştırılana kadar içeriyor olabilir diskleri depolama hala tutulur.

"Artık" VHD'ler görürseniz, silinmiş olan bir depolama hesabı için klasör parçası olup olmadığını bilmek önemlidir. Depolama hesabı silinemedi, hala var oldukları normaldir.

Daha fazla bilgiyi bekletme eşiği ve isteğe bağlı geri kazanma içinde yapılandırma hakkında daha fazla [depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Depolama
### <a name="storage-reclamation"></a>Depolama geri kazanma
Portalda görünmesi kapasite iadesi on dört saate kadar sürebilir. Alan geri kazanma blok blob Mağazası'nda iç kapsayıcı dosyaları kullanım yüzdesi gibi çeşitli etkenlere bağlıdır. Bu nedenle, ne kadar veri silinmiş bağlı olarak, garantisi yoktur Atık toplayıcısının çalışacağı yükleyen iadesi alanı üzerinde.

## <a name="powershell"></a>PowerShell
### <a name="resource-providers-not-registered"></a>Kaynak sağlayıcıları kayıtlı değil
Kiracı abonelik PowerShell ile bağlanırken, kaynak sağlayıcıları değil otomatik olarak kaydedilir fark edeceksiniz. Kullanım [Bağlan Modülü](https://github.com/Azure/AzureStack-Tools/tree/master/Connect), veya Powershell'den aşağıdaki komutu çalıştırın (çalıştırdıktan sonra [yüklemek ve bağlamak](azure-stack-connect-powershell.md) Kiracı olarak): 
  
       Get-AzureRMResourceProvider | Register-AzureRmResourceProvider

## <a name="cli"></a>CLI

* CLI etkileşimli mod yani `az interactive` komutu Azure yığınında henüz desteklenmiyor.
* Sanal makine görüntülerini Azure yığınında kullanılabilir listesini almak için kullanmak `az vm images list --all` komutu yerine `az vm image list` komutu. Belirtme `--all` yanıtı yalnızca Azure yığın ortamınızda kullanılabilir görüntüleri döndürür emin olur. 
* Mevcut olan sanal makine görüntüsü diğer adlar Azure yığını için geçerli olmayabilir. Sanal makine görüntülerini kullanırken, tüm URN parametresini kullanmanız gerekir (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) yerine görüntü diğer adı. Ve bu URNmust eşleşen görüntü belirtimleri türetilmiş gibi `az vm images list` komutu.
* Varsayılan olarak, CLI 2.0 "Standard_DS1_v2" varsayılan sanal makine görüntü boyutunu kullanır. Ancak, bu boyutu değil henüz Azure yığınında kullanılabilir, bu nedenle, belirtmeniz gerekir `--size` açıkça bir sanal makine oluşturulurken parametre. Kullanarak Azure yığınında kullanılabilir sanal makine boyutlarının listesi elde edebilirsiniz `az vm list-sizes --location <locationName>` komutu.


## <a name="windows-azure-pack-connector"></a>Windows Azure Pack Bağlayıcısı
* Azure yığın Geliştirme Seti dağıttıktan sonra azurestackadmin hesabının parolasını değiştirirseniz, artık birden çok bulut modunu yapılandırabilirsiniz. Bu nedenle, hedef Windows Azure Pack ortamına bağlamak mümkün olmayacaktır.
* Birden çok bulut Modu'nu ayarla sonra:
    * Yalnızca bunlar portal ayarlarını sıfırladıktan sonra bir kullanıcı Pano görebilirsiniz. (Kullanıcı Portalı'nda (dişli simgesine sağ üst köşesinde) portal ayarları simgesine tıklayın. Altında **varsayılan ayarları geri**, tıklatın **Uygula**.)
    * Pano başlıklarını görünmeyebilir. Bu sorun oluşursa, el ile bunları geri eklemeniz gerekir.
    * İlk bunları panoya eklediğinizde bazı kutucuklar doğru şekilde göstermeyebilir. Bu sorunu gidermek için tarayıcıyı yenileyin.



