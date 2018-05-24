---
title: Sanal makineye gelen ve sanal makineden giden ağ trafiği akışını günlüğe kaydetme - öğretici - Azure portalı | Microsoft Docs
description: Ağ İzleyicisinin NSG akış günlükleri özelliğini kullanarak bir sanal makineye gelen ve sanal makineden giden ağ trafiği akışını günlüğe kaydetme hakkında bilgi edinin.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to log the network traffic to and from a VM so I can analyze it for anomalies.
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: f010bebcf1130b3061c60987ffbd4e706a030773
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-log-network-traffic-to-and-from-a-virtual-machine-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak sanal makineye gelen ve sanal makineden giden ağ trafiğini günlüğe kaydetme

Ağ güvenlik grubu (NSG), bir sanal makineye gelen trafiği ve sanal makineden giden trafiği filtrelemenize olanak sağlar. Ağ İzleyicisinin NSG akış günlüğü özelliği ile NSG aracılığıyla akan trafiği günlüğe kaydedebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağ güvenlik grubu ile sanal makine oluşturma
> * Ağ İzleyicisini etkinleştirme ve Microsoft.Insights sağlayıcısını kaydetme
> * Ağ İzleyicisinin NSG akış günlüğü özelliğini kullanarak NSG için bir trafik akışı günlüğünü etkinleştirme
> * Günlüğe kaydedilen verileri indirme
> * Günlüğe kaydedilen verileri görüntüleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **İşlem**'i seçin ve sonra **Windows Server 2016 Datacenter** veya **Ubuntu Server 17.10 VM** seçeneğini belirleyin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVm|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Yeni oluştur**’u seçin ve **myResourceGroup** değerini girin.|
    |Konum| **Doğu ABD**’yi seçin|

4. Sanal makine için bir boyut seçin ve **Seç** seçeneğini belirleyin.
5. **Ayarlar** bölümünde tüm varsayılanları kabul edin ve **Tamam**’ı seçin.
6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın. Sanal makinenin dağıtılması birkaç dakika sürer. Kalan adımlara devam etmeden önce sanal makinenin dağıtımı tamamlamasını bekleyin.

Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makinenin oluşturulması tamamlanmadan kalan adımlara devam etmeyin. Portal sanal makineyi oluştururken, **myVm-nsg** adıyla bir ağ güvenlik grubu da oluşturur ve bunu sanal makine için ağ arabirimiyle ilişkilendirir.

## <a name="enable-network-watcher"></a>Ağ İzleyicisini etkinleştirme

Doğu ABD bölgesinde etkinleştirilmiş bir ağ izleyiciniz zaten varsa [Insights sağlayıcısını kaydetme](#register-insights-provider) bölümüne atlayın.

1. Portalda **Tüm hizmetler**’i seçin. **Filtre kutusu**’na *Ağ İzleyicisi* yazın. **Ağ İzleyicisi**, sonuçlarda görüntülendiğinde onu seçin.
2. **Bölgeler**’i seçip genişletin ve sonra aşağıdaki resimde gösterildiği gibi **Doğu ABD**’nin sağındaki **...** öğesini seçin:

    ![Ağ İzleyicisini etkinleştirme](./media/network-watcher-nsg-flow-logging-portal/enable-network-watcher.png)

3. **Ağ izleyicisini etkinleştirme**’yi seçin.

## <a name="register-insights-provider"></a>Insights sağlayıcısını kaydetme

NSG akış günlüğü kaydı için **Microsoft.Insights** sağlayıcısı gerekir. Sağlayıcıyı kaydetmek için aşağıdaki adımları tamamlayın:

1. Portalın sol üst köşesinde **Tüm hizmetler**’i seçin. Filtre kutusuna *Abonelikler* yazın. **Abonelikler**, arama sonuçlarında görüntülendiğinde onu seçin.
2. Abonelikler listesinden sağlayıcısını etkinleştirmek istediğiniz aboneliği seçin.
3. **AYARLAR** bölümünden **Kaynak sağlayıcıları**’nı seçin.
4. **microsoft.insights** sağlayıcısı için **DURUM** değerinin, aşağıdaki resimde gösterildiği gibi **Kayıtlı** olduğunu onaylayın. Durum **Kaydedilmedi** ise sağlayıcının sağındaki **Kaydet**’i seçin.

    ![Sağlayıcıyı kaydetme](./media/network-watcher-nsg-flow-logging-portal/register-provider.png)

## <a name="enable-nsg-flow-log"></a>NSG akış günlüğünü etkinleştirme

1. NSG akış günlüğü verileri bir Azure Depolama hesabına yazılır. Azure Depolama hesabı oluşturmak için, portalın sol üst köşesinden **+ Kaynak oluştur**’u seçin.
2. **Depolama**’yı ve sonra **Depolama hesabı - blob, dosya, tablo, kuyruk** öğesini seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin.

    | Ayar        | Değer                                                        |
    | ---            | ---   |
    | Adı           | 3-24 karakter uzunluğundadır. Yalnızca küçük harfler ve rakamlar içerebilir ve tüm Azure Depolama hesapları arasında benzersiz olmalıdır.                                                               |
    | Konum       | **Doğu ABD**’yi seçin                                           |
    | Kaynak grubu | **Var olanı kullan**’ı seçin ve sonra **myResourceGroup** seçeneğini belirleyin |

    Depolama hesabının oluşturulması yaklaşık bir dakika sürebilir. Depolama hesabı oluşturulmadan kalan adımlara devam etmeyin. Yenisini oluşturmak yerine mevcut bir depolama hesabını kullanırsanız, depolama hesabı için **AYARLAR** bölümünde **Güvenlik duvarları ve sanal ağlar** için **Tüm ağlar** (varsayılan) seçeneği belirlenmiş bir depolama hesabı seçtiğinizden emin olun.
4. Portalın sol üst köşesinde **Tüm hizmetler**’i seçin. **Filtre** kutusuna *Ağ İzleyicisi* yazın. **Ağ İzleyicisi**, arama sonuçlarında görüntülendiğinde onu seçin.
5. **GÜNLÜKLER** bölümünde, aşağıdaki resimde gösterildiği gibi **NSG akış günlükleri**’ni seçin:

    ![NSG'ler](./media/network-watcher-nsg-flow-logging-portal/nsgs.png)

6. NSG listesinden **myVm-nsg** adlı NSG’yi seçin.
7. **Akış günlükleri ayarları** bölümünde **Açık** seçeneğini belirleyin.
8. 3. adımda oluşturduğunuz depolama hesabını seçin.
9. **Bekletme (gün)** değerini 5 olarak ayarlayın ve **Kaydet**’i seçin.

## <a name="download-flow-log"></a>Akış günlüğünü indirme

1. Ağ İzleyicisinden, portalda **GÜNLÜKLER** bölümündeki **NSG akış günlükleri**’ni seçin.
2. Aşağıdaki resimde gösterildiği gibi **Yapılandırılan depolama hesaplarından akış günlüklerini indirebilirsiniz** seçeneğini belirleyin:

  ![Akış günlüklerini indirme](./media/network-watcher-nsg-flow-logging-portal/download-flow-logs.png)

3. [NSG akış günlüğünü etkinleştirme](#enable-nsg-flow-log) bölümünün 2. adımında yapılandırdığınız depolama hesabını seçin.
4. **Kapsayıcılar**’ı seçin, aşağıdaki resimde gösterildiği gibi **BLOB HİZMETİ** bölümünde **insights-logs-networksecuritygroupflowevent** kapsayıcısını seçin:

    ![Kapsayıcı seçme](./media/network-watcher-nsg-flow-logging-portal/select-container.png)
5. Klasör hiyerarşisine gidin ve aşağıdaki resimde gösterildiği gibi bir PT1H.json dosyasına ulaşın:

    ![Günlük dosyası](./media/network-watcher-nsg-flow-logging-portal/log-file.png)

    Günlük dosyaları, şu ad kuralını izleyen bir klasör hiyerarşisine yazılır:  https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json

6. PT1H.json dosyasının sağındaki **...** öğesini seçin ve **İndir** seçeneğini belirleyin.

## <a name="view-flow-log"></a>Akış günlüğünü görüntüleme

Aşağıdaki json, verileri günlüğe kaydedilen her akış için PT1H.json dosyasında göreceklerinize bir örnektir:

```json
{
    "time": "2018-05-01T15:00:02.1713710Z",
    "systemId": "<Id>",
    "category": "NetworkSecurityGroupFlowEvent",
    "resourceId": "/SUBSCRIPTIONS/<Id>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MYVM-NSG",
    "operationName": "NetworkSecurityGroupFlowEvents",
    "properties": {
        "Version": 1,
        "flows": [{
            "rule": "UserRule_default-allow-rdp",
            "flows": [{
                "mac": "000D3A170C69",
                "flowTuples": ["1525186745,192.168.1.4,10.0.0.4,55960,3389,T,I,A"]
            }]
        }]
    }
}
```

Önceki çıktıda yer alan **mac** değeri, sanal makine oluşturulurken oluşturulan ağ arabiriminin MAC adresidir. **flowTuples** için virgülle ayrılmış bilgiler aşağıdaki gibidir:

| Örnek veriler | Verilerin temsil ettiği   | Açıklama                                                                              |
| ---          | ---                    | ---                                                                                      |
| 1525186745   | Zaman damgası             | Akışın oluştuğu zamanın UNIX EPOCH biçiminde zaman damgası. Önceki örnekte tarih, 1 Mayıs 2018, 14:59:05 GMT olarak dönüştürülür.                                                                                    |
| 192.168.1.4  | Kaynak IP adresi      | Akışın geldiği kaynak IP adresi.
| 10.0.0.4     | Hedef IP adresi | Akışın hedeflendiği hedef IP adresi. 10.0.0.4, [Sanal makine oluşturma](#create-a-vm) bölümünde oluşturduğunuz sanal makinenin özel IP adresidir.                                                                                 |
| 55960        | Kaynak bağlantı noktası            | Akışın geldiği kaynak bağlantı noktası.                                           |
| 3389         | Hedef bağlantı noktası       | Akışın hedeflendiği hedef bağlantı noktası. Trafik, 3389 numaralı bağlantı noktasını hedeflediğinden, günlük dosyasında **UserRule_default-allow-rdp** adlı kural, akışı işlemiştir.                                                |
| T            | Protokol               | Akış protokolünün TCP (T) mi yoksa UDP (U) mi olduğu.                                  |
| I            | Yön              | Trafiğin gelen (I) mi yoksa giden (O) mi olduğu.                                     |
| A            | Eylem                 | Trafiğe izin mi verildiği (A) yoksa trafiğin ret mi edildiği (D).                                           |

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, NSG için NSG akış günlüğünün nasıl etkinleştirildiğini öğrendiniz. Ayrıca bir dosyada günlüğe kaydedilen verilerin nasıl indirileceğini ve görüntüleneceğini de öğrendiniz. Json dosyasındaki işlenmemiş verilerin yorumlanması güç olabilir. Verileri görselleştirmek için, Ağ İzleyicisi [trafik analizini](traffic-analytics.md), Microsoft [PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md) ve diğer araçları kullanabilirsiniz.