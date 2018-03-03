---
title: Bir Azure sanal makinede Visual Studio kullanarak | Microsoft Docs
description: Bir Azure sanal makinede Visual Studio kullanarak.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: sacalla
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 01/30/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: a77fa83ba4fe3f6d7e7ab1ea35929ae610566129
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="visual-studio-images-on-azure"></a>Azure üzerinde Visual Studio görüntüleri
Önceden yapılandırılmış Azure sanal makinede (VM) Visual Studio kullanarak herhangi bir şey için bir yukarı ve çalışan geliştirme ortamı gitmek için hızlı ve kolay bir yoludur. Sistem görüntüleri farklı Visual Studio yapılandırmalarla kullanılabilir [Azure Marketi](https://portal.azure.com/).

Azure’da yeni misiniz? [Ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Hangi yapılandırmaları ve sürümleri kullanılabilir?
Görüntüleri en son ana sürüm, Visual Studio 2017 ve Visual Studio 2015 için Azure Marketi'nde bulunabilir. Her ana sürümü için yayınlanmış (RTW) sürümü ve en son güncelleştirilmiş sürümlerine bakın. Bu sürümlerin her biri, Visual Studio Enterprise ve Visual Studio Community sürümleri sunar. Bu görüntüler en az her ay en son Visual Studio ve Windows güncelleştirmeleri içerecek şekilde güncelleştirildi. Görüntüleri adları aynı kalsa da her görüntünün açıklaması yüklü ürün sürümü ve görüntünün "itibariyle" tarihini içerir.

| Yayın sürümü              | Sürümleri            | Ürün sürümü     |
|:------------------------------------------:|:----------------------------:|:-----------------------:|
| Visual Studio 2017: En son (sürüm 15,5) |    Enterprise, Community     |      Sürüm 15.5.3     |
|         Visual Studio 2017: RTW           |    Enterprise, Community     |      Sürüm 15.0.7     |
|   Visual Studio 2015: En son (güncelleştirme 3)   |    Enterprise, Community     |  Sürüm 14.0.25431.01  |
|         Visual Studio 2015: RTW           |              None            | (Bakım için süresi) |

> [!NOTE]
> İlke bakım Microsoft uygun olarak, bakım için Visual Studio 2015 yayınlanan özgün (RTW) sürümünün süresi sona erdi. Visual Studio 2015 güncelleştirme 3, Visual Studio 2015 ürün çizgi sunulan yalnızca kalan sürümüdür.

Daha fazla bilgi için bkz: [Visual Studio Hizmet İlkesi](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Hangi özelliklerin yüklü mü?
Her görüntü önerilen özellik için Visual Studio sürümü kümesini içerir. Genellikle, yüklemesi içerir:

* Tüm kullanılabilir iş yükleri, her iş yükü de dahil olmak üzere isteğe bağlı bileşenler önerilir
* .NET 4.6.2 ve 4.7 SDK'lar, hedefleme paketleri ve geliştirici araçları
* Visual F#
* Visual Studio için GitHub uzantısı
* LINQ-SQL araçları

Görüntüleri oluştururken Visual Studio'yu yüklemek için kullanılan komut satırı aşağıdaki gibidir:

```
    vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
       add Microsoft.Net.Component.4.7.SDK ^
       add Microsoft.Net.Component.4.7.TargetingPack ^ 
       add Microsoft.Net.Component.4.6.2.SDK ^
       add Microsoft.Net.Component.4.6.2.TargetingPack ^
       add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
       add Microsoft.VisualStudio.Component.FSharp ^
       add Component.GitHub.VisualStudio ^
       add Microsoft.VisualStudio.Component.LinqToSql
```

Görüntüleri gerektiren bir Visual Studio özelliği eklemezseniz, sayfanın sağ üst köşesindeki geri bildirim araçla geri bildirim sağlayın.

## <a name="what-size-vm-should-i-choose"></a>VM boyutu seçmem gerekir?
Azure sanal makine boyutlarının sunmaktadır. Visual Studio güçlü çok iş parçacıklı bir uygulama olduğundan, en az iki işlemcileri ve 7 GB bellek içeren bir VM boyutu istiyor. Visual Studio görüntüler için aşağıdaki VM boyutları öneririz:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
En son makine boyutları hakkında daha fazla bilgi için bkz: [boyutları için Windows Azure sanal makineleri](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Azure ile VM boyutlandırarak ilk seçiminiz yeniden dengelemeniz. Daha uygun bir boyutu ile yeni bir VM sağlamak veya farklı temel alınan donanım için mevcut VM'yi yeniden boyutlandırın. Daha fazla bilgi için bkz: [Windows VM yeniden boyutlandırma](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-the-vm-is-running-whats-next"></a>VM çalışmaya başladığında, sonraki nedir?
Visual Studio Azure "kendi lisansını getir" modelinde izler. Bir yüklemede gibi özel donanım ile ilk adımlarından biri Visual Studio yüklemenizin lisans. Visual Studio ya da kilidini açmak için:
- Visual Studio abonelikle ilişkili bir Microsoft hesabıyla oturum açın 
- İlk satın alma işleminiz ile birlikte gelen ürün anahtarı ile Visual Studio kilidini açma

Daha fazla bilgi için bkz: [Visual Studio'ya oturum](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) ve [Visual Studio kilidini açma](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Nasıl ı gelecek için VM geliştirme kaydedin veya takım kullanıyor musunuz?

Geliştirme ortamlarını yelpazesini çok büyük olduğundan ve var gerçek maliyet daha karmaşık ortamları oluşturmaya ile ilişkili. Ortamınızı ait yapılandırma bağımsız olarak kaydetmek veya yakalama, bir "temel görüntü olarak" gelecekte kullanım için veya diğer ekibinizin üyeleri için yapılandırılan VM. Ardından, yeni bir VM önyüklemesi sırasında bunu Azure Market görüntüsü yerine temel görüntü sağlayın.

Hızlı bir özeti: Sistem Hazırlama (Sysprep) aracını ve çalışan VM kapatma ve ardından yakalama *(Şekil 1)* Azure portalında kullanıcı Arabirimi aracılığıyla bir görüntü olarak VM. Azure kaydeder `.vhd` seçtiğiniz depolama hesabı resmi içeren dosya. Yeni görüntüyü sonra aboneliğiniz listesinde kaynakların bir görüntü kaynağı olarak görünür.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Şekil 1) Azure portal UI aracılığıyla bir görüntüsünü yakalayın.*</center>

Daha fazla bilgi için bkz: [bir yönetilen genelleştirilmiş bir VM görüntüsü oluşturma](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

> [!IMPORTANT]
> VM hazırlamak üzere Sysprep kullanmayı unutmayın. Bu adımı atlarsanız, Azure VM görüntüden sağlayamazsınız.

> [!NOTE]
> Hala görüntülerinin depolama için bazı maliyetler doğurur, ancak bir gereksinim duyan her ekip üyesi için en baştan VM yeniden oluşturmak için genel giderlerinden karşılaştırılan artımlı maliyet Önemsiz olabilir. Örneğin, oluşturmak ve tüm ekibiniz tarafından yeniden kullanılabilir bir ay için 127 GB görüntü depolamak için birkaç dolar maliyetleri. Ancak, bu maliyetleri saatler dışarı yapı ve kendi kullanımları için doğru yapılandırılmış geliştirme kutusunu doğrulamak için her çalışan invests karşılaştırılan anlamsız değildir.

Ayrıca, geliştirme görevleri veya teknolojileri geliştirme yapılandırmaları ve birden fazla makine yapılandırmaları çeşitleri gibi daha fazla ölçek gerekebilir. Azure DevTest Labs oluşturmak için kullanabileceğiniz _tarif_ , "altın görüntü." yapımı otomatikleştirme DevTest Labs, ekibinizin çalışan VM'ler için ilkelerini yönetmek için de kullanabilirsiniz. [Geliştiriciler için Azure DevTest Labs kullanarak](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) DevTest Labs hakkında daha fazla bilgi için en iyi bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Önceden yapılandırılmış Visual Studio görüntüler hakkında bildiğinize göre sonraki adım yeni bir VM oluşturmaktır:

* [Azure Portalı aracılığıyla bir VM oluşturma](quick-create-portal.md)
* [Windows sanal makinelere genel bakış](overview.md)
