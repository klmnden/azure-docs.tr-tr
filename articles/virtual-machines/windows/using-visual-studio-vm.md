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
ms.openlocfilehash: 599a890be4d014d22bae899be4cf6e281c4109d4
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a id="top"></a> Azure visual Studio görüntülerde
Önceden yapılandırılmış Azure sanal makinede (VM) çalıştıran Visual Studio kullanarak herhangi bir şey için bir yukarı ve çalışan geliştirme ortamında gitmek için kolay ve en hızlı yoludur.  Sistem görüntüleri farklı Visual Studio yapılandırmalarla kullanılabilir [Azure Marketi](https://portal.azure.com/). Yalnızca bir VM önyükleme ve gidin.

Azure’da yeni misiniz? [Ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Hangi yapılandırmaları ve sürümleri kullanılabilir?
Azure Marketi'nde en son ana sürümleri için görüntüleri bulma: Visual Studio 2017 ve Visual Studio 2015.  Her ana sürümü için özgün olarak yayımlanmış görürsünüz (aka ' RTW') sürümü ve "son" güncelleştirilmiş sürümleri.  Her bu farklı sürümleri için Visual Studio Enterprise ve Visual Studio Community sürümleri bulun.  Bu görüntüler en az her ay en son Visual Studio ve Windows güncelleştirmeleri içerecek şekilde güncelleştiriyoruz.  Görüntüleri adları aynı kalsa da her görüntünün açıklaması yüklü ürün sürümü ve görüntünün 'itibariyle' tarihini içerir.

|               Yayın sürümü              |          Sürümleri            |     Ürün sürümü:     |
|:------------------------------------------:|:----------------------------:|:-----------------------:|
| Visual Studio 2017 - son (sürüm 15,5) |    Enterprise, Community     |      Sürüm 15.5.3     |
|         Visual Studio 2017 - RTW           |    Enterprise, Community     |      Sürüm 15.0.7     |
|   Visual Studio 2015 - son (güncelleştirme 3)   |    Enterprise, Community     |  Sürüm 14.0.25431.01  |
|         Visual Studio 2015 - RTW           |              Hiçbiri            | (Bakım için süresi) |

> [!NOTE]
> İlke, özgün olarak yayımlanmış bakım Microsoft göre (aka ' RTW') Visual Studio 2015 sürümü, bakım için doldu.  Bu nedenle, Visual Studio 2015 güncelleştirme 3 için Visual Studio 2015 ürün satır sunulan yalnızca kalan sürümüdür.

Daha fazla bilgi için bkz: [Visual Studio Hizmet İlkesi](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Hangi özelliklerin yüklü mü?
Her görüntü önerilen özellik için Visual Studio sürümü kümesini içerir.  Genellikle, yüklemesi içerir:

* Bu iş yükü önerilen isteğe bağlı bileşenler dahil olmak üzere tüm kullanılabilir iş yükleri
* .NET 4.6.2 ve 4.7 SDK'lar, hedefleme paketleri ve geliştirici araçları
* Visual F#
* Visual Studio için GitHub uzantısı
* LINQ-SQL araçları

Bu görüntüleri oluştururken Visual Studio'yu yüklemek için kullanırız komut satırı.

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

Görüntüleri gerektiren bir Visual Studio özelliği eklemezseniz, geri bildirim Aracı (sayfanın sağ üst köşesinde) aracılığıyla geri bildirim sağlayın.

## <a name="what-size-vm-should-i-choose"></a>VM boyutu seçmem gerekir?
Yeni bir sanal makine sağlama kolaydır ve Azure eksiksiz bir sanal makine boyutlarını sunar.  Tüm donanım edinme olduğu gibi ile performans maliyeti karşı dengelemek istiyorsunuz.  Visual Studio güçlü çok iş parçacıklı bir uygulama olduğundan, en az 2 işlemciler ve 7 GB bellek içeren bir VM boyutu istiyor.  Visual Studio görüntüleri için önerilen VM boyutları şunlardır:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
En son makine boyutları hakkında daha fazla bilgi için bkz: [boyutları için Windows Azure sanal makineleri](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Azure ile ilk çekme ile sınırlı değilsiniz – VM boyutlandırarak ilk seçiminiz yeniden dengelemeniz.  Daha uygun bir boyutu ile yeni bir VM ya da sağlayabilir veya farklı temel alınan donanım, var olan VM yeniden boyutlandırabilirsiniz.  Daha fazla bilgi için bkz: [Windows VM yeniden boyutlandırma](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-i-get-the-vm-running-then-what"></a>Sonra ardından ne VM çalışma alıyorum?
Visual Studio Azure "kendi lisansını getir" modelinde izler.  Bu nedenle, benzer şekilde özel donanım üzerinde bir yükleme için ilk adımlarından biri Visual Studio yüklemenizin lisans.  Visual Studio imzalama ya da Visual Studio abonelikle ilişkili bir Microsoft hesabıyla oturum açabilir veya Visual Studio ürün anahtarı ile ilk satın alma işleminiz kilidini açabilir.  Daha fazla bilgi için bkz: [Visual Studio'da oturum açma](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) ve [Visual Studio kilidini açma](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="after-i-build-out-the-dev-vm-how-do-i-save-capture-it-for-future-or-team-use"></a>Sonra VM geliştirme yapımı nasıl yedeklerim (yakalama) gelecek için veya takım kullanımı?

Geliştirme ortamlarını yelpazesini çok büyük olduğundan ve var gerçek maliyet daha karmaşık ortamları oluşturmaya ile ilişkili.  Ancak, ortamınıza ait yapılandırma bağımsız olarak Azure kaydetme/kusursuz şekilde yapılandırılmış VM bir 'temel görüntü' kendiniz için ve/veya diğer üyeleri için takımınızın gelecekte kullanılmak – yakalama tarafından bu yatırım kolay koruma sağlar.  Ardından, yeni bir VM önyüklemesi sırasında bu Market görüntüsü yerine temel görüntü sağlayın.

Kısa bir Özet, sysprep ve kapanışında çalışan VM sonra gerekir *yakalama (Şekil 1)* Azure portal'ın kullanıcı Arabirimi aracılığıyla bir görüntü olarak VM.  Azure kaydeder `.vhd` seçtiğiniz depolama hesabı resmi içeren dosya.  Ardından, yeni görüntüyü kaynakları aboneliğinizin listesinde bir görüntü kaynağı olarak görünür.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal’s UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Şekil 1) Azure portal'ın kullanıcı Arabirimi aracılığıyla bir görüntüsünü yakalayın.*</center>

Daha fazla bilgi için bkz: [bir VM görüntü yakalama](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

  **Anımsatıcı:** sysprep VM unutmayın!  Bu adımı atlarsanız, Azure VM görüntüden sağlayamazsınız.

> [!NOTE]
> Hala yansımaları depolama için bazı maliyetler doğurur, artımlı maliyet olasıdır ancak bu önemsiz bir VM gereksinim duyan her kişi için takımınızdaki VM – sıfırdan yeniden manpower maliyetleri karşılaştırılan.  Örneğin, oluşturmak ve tüm ekibinizin üyeleri tarafından yeniden kullanılabilir bir ay için 127 GB görüntü depolamak için birkaç dolar maliyetleri.  Ancak, bu maliyetleri saatler dışarı yapı ve kendi kullanımları için doğru yapılandırılmış geliştirme kutusunu doğrulamak için her çalışan invests karşılaştırılan anlamsız değildir.

Ayrıca, geliştirme görevleri veya teknolojileri – geliştirme yapılandırmaları ve birden fazla makine yapılandırmaları çeşitleri gibi daha fazla ölçek gerekebilir.  Azure DevTest Labs oluşturmak için kullanabileceğiniz _tarif_ 'altın görüntünün' yapım otomatikleştirmek ve ekibiniz için ilkelerini yönetmek için sanal makineleri çalıştıran.  [Geliştiriciler için Azure DevTest Labs kullanarak](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) DevTest Labs hakkında daha fazla bilgi için en iyi bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Önceden yapılandırılmış Visual Studio görüntüler hakkında bildiğinize göre sonraki adım yeni bir VM oluşturmaktır:

* [Azure Portalı aracılığıyla bir VM oluşturma](quick-create-portal.md)
* [Windows sanal makinelere genel bakış](overview.md)
