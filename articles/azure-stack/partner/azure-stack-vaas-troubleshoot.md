---
title: Hizmet olarak Azure Stack doğrulama sorunlarını giderme | Microsoft Docs
description: Azure Stack için hizmet olarak doğrulama sorunlarını giderin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: fedfd7f83a35398586734fa647751e537b850bf8
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481205"
---
# <a name="troubleshoot-validation-as-a-service"></a>Hizmet olarak doğrulama sorunlarını giderme

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Yazılım sürümleri ve çözümleri için ilgisiz yaygın sorunlar aşağıda verilmiştir.

## <a name="local-agent"></a>Yerel aracı

### <a name="the-portal-shows-local-agent-in-debug-mode"></a>Portal, yerel aracı hata ayıklama modunda gösterir.

Aracı sinyal hizmete bir dengesiz bir ağ bağlantısı nedeniyle gönderemiyor olduğu için bu büyük olasılıkla budur. Beş dakikada bir sinyal gönderilmedi. Hizmet 15 dakika boyunca bir sinyal ulaşmazsa, hizmet Aracısı devre dışı olarak değerlendirir ve testleri artık üzerinde zamanlanacak. Hata iletisi iade *Agenthost.log* dizininde bulunan dosya nerede aracısı başlatıldı.

> [!Note]
> Zaten aracı üzerinde çalışan herhangi bir test çalışmasına devam eder ancak sinyal değilse, test sona ermeden önce geri sonra aracıyı test durumunu güncelleştirmek ya da günlükleri karşıya yükleme başarısız olur. Test olarak her zaman görünür **çalıştıran** ve iptal edilmesi gerekir.

### <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>Aracı makine üzerindeki işlem yürütülürken test kapatıldı. Neler?

Aracı işlemi ungracefully örneğin kapatılırsa, makine yeniden başlatıldı, işlem sonlandırıldı (CTRL + C aracı penceresinde kapatılmasını sayılır) üzerinde çalışan bir test olarak göstermeye devam eder sonra **çalıştıran**. Aracıyı yeniden başlatıldıktan sonra aracı test durumunu güncelleştirir **iptal**. Aracı olmayan yeniden başlatıldıktan sonra test olarak görünür **çalıştıran** ve el ile test iptal etmeniz gerekir.

> [!Note]
> Bir iş akışı içinde testleri, sırayla çalışmak üzere zamanlanır. **Bekleyen** çalıştırılmamış testler testlerinde kadar **çalıştıran** tam aynı iş akışı durum.

## <a name="vm-images"></a>VM görüntüleri

### <a name="handle-slow-network-connectivity"></a>Yavaş ağ bağlantısı işleme

Yerel veri merkezinizde bir paylaşıma PIR görüntü indirebilirsiniz. ' İ tıklatın ve ardından görüntüyü kontrol edebilirsiniz.

<!-- This is from the appendix to the Deploy local agent topic. -->

#### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>Yavaş ağ trafiğini durumunda Yerel paylaşım için PIR görüntü indirin

1. Azcopy'nin indirin: [vaasexternaldependencies(AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip)

2. AzCopy.zip ayıklayın ve AzCopy.exe içeren dizine geçin

3. Windows PowerShell'i yükseltilmiş isteminden açın. Aşağıdaki komutları çalıştırın:

```powershell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare, paylaşım yolu veya yerel yol değil.

#### <a name="verifying-pir-image-file-hash-value"></a>PIR resim dosya karma değeri doğrulanıyor

Kullanabileceğiniz **Get-HashFile** karma değeri, görüntü dosyalarını, resimleri bütünlüğünü kontrol etmek için indirilen ortak görüntü deposuna almak için cmdlet.

| Dosya Adı | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL.vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL.vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS.vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604-20170619.1.vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |

### <a name="failure-occurs-when-uploading-vm-image-in-the-vaasprereq-script"></a>Sanal makine görüntüsünü karşıya yüklenirken hata oluşması `VaaSPreReq` betiği

Önce ortamın sağlıklı olup olmadığını denetleyin:

1. DVM gelen / atlama kutusunu, yönetici kimlik bilgilerini kullanarak yönetim portalına başarıyla oturum açabildiğinizi kontrol edin.
1. Hiçbir uyarı veya uyarılar olduğundan emin olun.

Ortamın sağlıklı olup olmadığını el ile VaaS test çalıştırmaları için gereken 5 VM görüntülerini karşıya yükleyin:

1. Yönetim portalında Hizmet Yöneticisi olarak oturum açın. Yönetim portalında bulabilirsiniz ECE deposu veya damga bilgi dosyanızın URL. Yönergeler için [ortam parametrelerini](azure-stack-vaas-parameters.md#environment-parameters).
1. Seçin **diğer hizmetler** > **kaynak sağlayıcıları** > **işlem** > **VM görüntüleri**.
1. Seçin **+ Ekle** üst kısmındaki düğmeye **VM görüntüleri** dikey penceresi.
1. Değiştirebilir veya ilk VM görüntüsü için aşağıdaki alanların değerlerini kontrol edin:
    > [!IMPORTANT]
    > Tüm Varsayılanları, var olan bir Market öğesi için doğrudur.

    | Alan  | Değer  |
    |---------|---------|
    | Yayımcı | MicrosoftWindowsServer |
    | Sunduğu | WindowsServer |
    | İşletim Sistemi Türü | Windows |
    | SKU | 2012-R2-Datacenter |
    | Sürüm | 1.0.0 |
    | İşletim sistemi diski Blob URİ'si | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |

1. **Oluştur** düğmesini seçin.
1. Kalan VM görüntüleri için yineleyin.

Tüm 5 VM görüntüleri özelliklerini aşağıdaki gibidir:

| Yayımcı  | Sunduğu  | İşletim Sistemi Türü | SKU | Sürüm | İşletim sistemi diski Blob URİ'si |
|---------|---------|---------|---------|---------|---------|
| MicrosoftWindowsServer| WindowsServer | Windows | 2012-R2-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterFullBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter-Server-Core | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterCoreBYOL.vhd |
| Canonical | UbuntuServer | Linux | 14.04.3-LTS | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1404LTS.vhd |
| Canonical | UbuntuServer | Linux | 16.04-LTS | 16.04.20170811 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1604-20170619.1.vhd |

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [hizmet olarak doğrulama için sürüm notları](azure-stack-vaas-release-notes.md) değişikliklerin en son sürümlerde.