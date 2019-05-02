---
title: Paket yakalama işlemlerini Azure Ağ İzleyicisi - Azure portal ile yönetme | Microsoft Docs
description: Azure portalını kullanarak Ağ İzleyicisi paket yakalama özelliğini yönetmeyi öğrenin.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: kumud
ms.openlocfilehash: 50092db9e2e3670168cbb3440b8cb99eb0c2ac20
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714721"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Portalı kullanarak Azure Ağ İzleyicisi ile paket yakalamayı yönetme

Ağ İzleyicisi paket yakalama, bir sanal makineye gelen ve giden trafiği izlemek için yakalama oturumu oluşturmanıza olanak sağlar. Sağlamak istediğiniz trafiği yakalamak yakalama oturumu için filtreler sağlanır. Paket yakalama ağ anomalileri öngörülebiliyorsa hem proaktif bir şekilde tanılamak için yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimi hata ayıklama ve çok daha fazlası hakkında bilgi sağlamasını ağ istatistiklerini toplama içerir. Paket uzaktan tetikleyebilmesi için olan yakalar, paket yakalama el ile bir istenen sanal değerli zaman kazandırır, makinede çalışan yükünü kolaylaştırır.

Bu makalede, Başlat, Durdur, indirmek ve bir paket yakalamasını Sil öğrenin. 

## <a name="before-you-begin"></a>Başlamadan önce

Paket yakalaması aşağıdaki bağlantı gerektirir:
* Bir depolama hesabı bağlantı noktası 443 üzerinden giden bağlantı.
* 169.254.169.254 numaralı gelen ve giden bağlantı
* 168.63.129.16 gelen ve giden bağlantı

Ağ arabirimiyle ya da ağ arabiriminin bulunduğu alt ağ için ağ güvenlik grubu ilişkiliyse, önceki bağlantı noktalarına izin verecek kuralları olmadığından emin olun. 

## <a name="start-a-packet-capture"></a>Paket Yakalamayı Başlat

1. Tarayıcınızda gidin [Azure portalında](https://portal.azure.com) seçip **tüm hizmetleri**ve ardından **Ağ İzleyicisi** içinde **ağ bölümü**.
2. Seçin **paket yakalaması** altında **Ağ Tanılama Araçları**. Var olan bir paket yakalamaları durumlarını bağımsız olarak listelenir.
3. Seçin **Ekle** paket yakalaması oluşturmak için. Aşağıdaki özelliklerin değerlerini seçebilirsiniz:
   - **Abonelik**: Paket oluşturmak istediğiniz sanal makineyi yakalama için abonelik bileşenidir.
   - **Kaynak grubu**: Sanal makinenin kaynak grubu.
   - **Hedef sanal makine**: Paket yakalaması için oluşturmak istediğiniz sanal makine.
   - **Paket yakalama adı**: Paket yakalaması için bir ad.
   - **Depolama hesabı veya dosya**: Seçin **depolama hesabı**, **dosya**, veya her ikisini de. Seçerseniz **dosya**, sanal makinenin içindeki bir yol yakalama yazılır.
   - **Yerel dosya yolu**: Paket yakalaması nereye kaydedileceği sanal makinede yerel yol (yalnızca geçerli olduğunda *dosya* seçili). Yol geçerli bir yol olmalıdır. Linux sanal makinesi kullanıyorsanız, yol ile başlamalıdır */var/yakalar*.
   - **Depolama hesapları**: Seçtiyseniz, mevcut bir depolama hesabını seçin *depolama hesabı*. Bu seçenek yalnızca seçtiyseniz, kullanılabilir **depolama**.
   
     > [!NOTE]
     > Paket depolama yakalar için premium depolama hesapları şu anda desteklenmemektedir.

   - **Paket başına en fazla bayt**: Her paket yakalanan bayt sayısı. Tüm baytları boş bırakılırsa yakalanır.
   - **Oturum başına en fazla bayt**: Yakalanan bayt sayısı. Değer, paket yakalama durakları ulaşıldıktan sonra.
   - **Süre (saniye)**: Paket yakalaması durdurulmadan önce zaman sınırını. Varsayılan 18, 000 saniyedir.
   - (İsteğe bağlı) filtrelemeyi. Seçin **+ Filtre Ekle**
     - **Protokol**: Paket yakalaması için filtre uygulamak için protokol. TCP, UDP ve değerleri kullanılabilir.
     - **Yerel IP adresi**: Paket yakalaması paketler için yerel IP adresi bu değer eşleştiği filtreler.
     - **Yerel bağlantı noktası**: Paket yakalaması paketler için yerel bağlantı noktası bu değer eşleştiği filtreler.
     - **Uzak IP adresi**: Paket yakalaması paketler için uzak IP adresi bu değer eşleştiği filtreler.
     - **Uzak bağlantı noktası**: Paket yakalaması paketler için Uzak bağlantı noktası bu değer eşleştiği filtreler.
    
     > [!NOTE]
     > Bağlantı noktası ve IP adresi değerleri tek bir değer, değer aralığı ve bağlantı noktası için 80-1024 gibi bir aralığı olabilir. Gereksinim duyduğunuz kadar çok filtreleri tanımlayabilirsiniz.

4. **Tamam**’ı seçin.

Paket yakalama süresi sınırını süresi dolduktan sonra paket yakalaması durdurulur ve incelenebilir. Paket yakalama oturumu el ile de durdurabilirsiniz.

> [!NOTE]
> Portal otomatik olarak:
>  * Bölge zaten bir Ağ İzleyicisi yoksa, seçtiğiniz sanal makinenin var bölge ile aynı bölgede Ağ İzleyicisi oluşturur.
>  * Ekler *AzureNetworkWatcherExtension* [Linux](../virtual-machines/linux/extensions-nwa.md) veya [Windows](../virtual-machines/windows/extensions-nwa.md) zaten yüklü değilse sanal makine, sanal makine uzantısı.

## <a name="delete-a-packet-capture"></a>bir paket yakalamasını Sil

1. Paket yakalama Görünümü'nde seçin **...**  paketin sağ taraftaki yakalama veya var olan bir paket yakalaması sağ tıklayıp seçin **Sil**.
2. Paket yakalamasını silmek istediğinizi onaylamanız istenir. Seçin **Evet**.

> [!NOTE]
> Paket yakalaması siliniyor yakalama dosyasını depolama hesabına veya sanal makine silinmez.

## <a name="stop-a-packet-capture"></a>Paket Yakalamayı Durdur

Paket yakalama Görünümü'nde seçin **...**  paketin sağ taraftaki yakalama veya var olan bir paket yakalaması sağ tıklayıp seçin **Durdur**.

## <a name="download-a-packet-capture"></a>Paket yakalaması indirin

Paket yakalama oturumunuz tamamladıktan sonra yakalama dosyasını blob depolama veya sanal makinede yerel bir dosya karşıya yüklendi. Paket yakalaması depolama konumunu paket yakalaması oluşturulurken tanımlanır. Yapabilecekleriniz Microsoft Azure Depolama Gezgini, bir depolama hesabına kayıtlı yakalama dosyalara erişmek için uygun bir araç olan [indirme](https://storageexplorer.com/).

Bir depolama hesabı belirttiyseniz, paket yakalama dosyaları şu konumda bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

Seçtiyseniz **dosya** yakalama oluştururken görüntüleyebilir veya sanal makinede yapılandırılmış yolundan dosyasını indirin.

## <a name="next-steps"></a>Sonraki adımlar

- Paket yakalama ile sanal makine uyarıları otomatik hale getirmek öğrenmek için bkz [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md).
- Belirli bir trafik içine veya dışına bir sanal makine izin verilip verilmediğini belirlemek için bkz: [bir sanal makine ağ trafik filtresi sorununu tanılama](diagnose-vm-network-traffic-filtering-problem.md).
