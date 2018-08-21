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
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 4372da2f1057c06761dd6abaf18c26df6e33640e
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235142"
---
# <a name="troubleshoot-validation-as-a-service"></a>Hizmet olarak doğrulama sorunlarını giderme

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Yazılım sürümleri ve çözümleri için ilgisiz yaygın sorunlar aşağıda verilmiştir.

## <a name="the-portal-shows-local-agent-in-debug-mode"></a>Portal, yerel aracı hata ayıklama modunda gösterir.

Aracı sinyal hizmete bir dengesiz bir ağ bağlantısı nedeniyle gönderemiyor olduğu için bu büyük olasılıkla budur. Beş dakikada bir sinyal gönderilmedi. Hizmet 15 dakika boyunca bir sinyal ulaşmazsa, hizmet Aracısı devre dışı olarak değerlendirir ve testleri artık üzerinde zamanlanacak. Hata iletisi iade *Agenthost.log* dizininde bulunan dosya nerede aracısı başlatıldı.

> [!Note] 
> Zaten aracı üzerinde çalışan herhangi bir test çalışmasına devam eder ancak sinyal değilse, test sona ermeden önce geri sonra aracıyı test durumunu güncelleştirmek ya da günlükleri karşıya yükleme başarısız olur. Test olarak her zaman görünür **çalıştıran** ve iptal edilmesi gerekir.

## <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>Aracı makine üzerindeki işlem yürütülürken test kapatıldı. Neler?

Aracı işlemi ungracefully örneğin kapatılırsa, makine yeniden başlatıldı, işlem sonlandırıldı (CTRL + C aracı penceresinde kapatılmasını sayılır) üzerinde çalışan bir test olarak göstermeye devam eder sonra **çalıştıran**. Aracıyı yeniden başlatıldıktan sonra aracı test durumunu güncelleştirir **iptal**. Aracı olmayan yeniden başlatıldıktan sonra test olarak görünür **çalıştıran** ve el ile test iptal etmeniz gerekir

> [!Note] 
> Bir iş akışı içinde testleri, sırayla çalışmak üzere zamanlanır. **Bekleyen** çalıştırılmamış testler testlerinde kadar **çalıştıran** tam aynı iş akışı durum.

## <a name="handle-slow-network-connectivity"></a>Yavaş ağ bağlantısı işleme

Yerel veri merkezinizde bir paylaşıma PIR görüntü indirebilirsiniz. ' İ tıklatın ve ardından görüntüyü doğrulayabilirsiniz.

<!-- This is from the appendix to the Deploy local agent topic. -->

### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>Yavaş ağ trafiğini durumunda Yerel paylaşım için PIR görüntü indirin

1. Azcopy'nin indirin: [vaasexternaldependencies(AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip)

2. AzCopy.zip ayıklayın ve AzCopy.exe içeren dizine geçin

3. Windows PowerShell'i yükseltilmiş isteminden açın. Aşağıdaki komutları çalıştırın:

```PowerShell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare, paylaşım yolu veya yerel yol değil.

### <a name="verifying-pir-image-file-hash-value"></a>PIR resim dosya karma değeri doğrulanıyor

Kullanabileceğiniz **Get-HashFile** karma değeri, görüntü dosyalarını, resimleri bütünlüğünü kontrol etmek için indirilen ortak görüntü deposuna almak için cmdlet.

| Dosya Adı | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL.vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL.vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS.vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604 20170619.1.vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
