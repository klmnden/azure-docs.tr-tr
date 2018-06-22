---
title: Ağ trafiği desenlerini Azure Ağ İzleyicisi'ni ve açık kaynaklı araçları ile Görselleştirme | Microsoft Docs
description: Bu sayfayı Ağ İzleyicisi paket yakalama ile Capanalysis Vm'leriniz gelen ve giden trafik düzenlerini görselleştirmek için nasıl kullanılacağını açıklar.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 7b1e1383e8e244a7cdb30be1e08514a6a4dd7b14
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302242"
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a>Açık kaynaklı araçları kullanarak, Vm'lerde gelen ve giden ağ trafiği desenlerini Görselleştirme

Paket yakalama ağ hukuk ve derin paket incelemesi gerçekleştirmenize olanak sağlayan ağ verileri içerir. Kaynak araç ağınız hakkında Öngörüler elde etmek için paket yakalamaları çözümlemek için kullanabileceğiniz birçok açılır vardır. Böyle bir araç CapAnalysis, bir açık kaynak paket yakalama görselleştirme Aracı ' dir. Paket yakalama veri görselleştirme Öngörüler modelleri ve ağınızdaki anormallikleri hızlıca çıkarmaya değerli bir yoludur. Görselleştirmeleri de böyle Öngörüler apı'lerinizi bir şekilde paylaşma olanağı sağlar.

Azure'nın Ağ İzleyicisi ağınızdaki paket yakalamaları gerçekleştirmenizi sağlayarak verilerini yakalama olanağı sağlar. Bu makalede sağlar CapAnalysis ile Ağ İzleyicisi kullanılarak aracılığıyla ilerlemesi görselleştirmek ve paket ilişkin bilgiler elde etmek nasıl yakalar.

## <a name="scenario"></a>Senaryo

Akış desenleri ve tüm olası anormallikleri hızlı bir şekilde tanımlamak için ağ trafiğini görselleştirmek için açık kaynaklı araçları kullanmak istediğiniz bir Azure VM'de dağıtılan basit bir web uygulaması sahip. Ağ İzleyicisi ile ağ ortamınızın bir paket yakalama elde ve doğrudan depolama hesabınıza saklayın. CapAnalysis paket yakalama depolama blobda doğrudan alma ve içeriğini görselleştirin.

![senaryo][1]

## <a name="steps"></a>Adımlar

### <a name="install-capanalysis"></a>CapAnalysis yükleyin

Bir sanal makineye CapAnalysis yüklemek için buraya resmi yönergeleri başvurabilir https://www.capanalysis.net/ca/how-to-install-capanalysis.
CapAnalysis uzaktan erişim, yeni bir gelen güvenlik kuralı ekleyerek, VM 9877 noktasından açmanız gerekir. Ağ güvenlik grupları kuralları oluşturma hakkında daha fazla bilgi için bkz [içinde varolan bir NSG kuralları oluşturma](../virtual-network/manage-network-security-group.md#create-a-security-rule). Kural başarıyla eklendikten sonra gelen CapAnalysis erişebilir olmalıdır `http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a>Paket yakalama oturumunu başlatmak için Azure Ağ İzleyicisi'ni kullanma

Ağ İzleyicisi, bir sanal makine ve trafiği izlemek için paketler yakalamanızı sağlar. Kısmındaki yönergeleri başvurabilirsiniz [Yönet paket yakalar Ağ İzleyicisi ile](network-watcher-packet-capture-manage-portal.md) paket yakalama oturumunu başlatmak için. Paket yakalama CapAnalysis tarafından erişilecek bir depolama blob depolanabilir.

### <a name="upload-a-packet-capture-to-capanalysis"></a>Paket yakalama CapAnalysis için karşıya yükleme
Paket yakalama depolandığı "URL'den alma" sekmesini kullanarak ve depolama blobu bağlantı sağlayan Ağ İzleyicisi tarafından gerçekleştirilecek bir paket yakalama doğrudan yükleyebilirsiniz.

Bir bağlantı için CapAnalysis sağlarken, depolama blob URL'si için bir SAS belirteci eklenecek emin olun.  Bunu yapmak için paylaşılan erişim imzası depolama hesabından gidin, izin verilen izinleri atamak ve bir belirteç oluşturmak için SAS Oluştur düğmesine basın. Paket yakalama depolama blob URL'si SAS belirteci sonra ekleyebilirsiniz.

Sonuçta elde edilen URL aşağıdaki URL şöyle görünür: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Analiz etme paket yakalar

CapAnalysis paket yakalama, farklı açısından sağlayan her analizi görselleştirmek için çeşitli seçenekler sunar. Bu görsel özetler ile ağ trafiği eğilimleri anlamak ve hızlı bir şekilde tüm olağandışı nokta. Bu özelliklerden bazılarını aşağıdaki listede gösterilir:

1. Akış tabloları

    Bu tabloda paket verileri, akışları ile ilişkili zaman damgası ve akış yanı sıra ile kaynak ve hedef IP ilişkili çeşitli protokoller akışlarında listesi verilmektedir.

    ![capanalysis akış sayfası][5]

1. Protokolü'ne genel bakış

    Bu bölme, ağ trafiğinin dağıtımını çeşitli protokolleri ve coğrafi hızlı bir şekilde görmenize olanak sağlar.

    ![capanalysis Protokolü'ne genel bakış][6]

1. İstatistikler

    Bu bölme, ağ trafiğini istatistikleri – bayt görüntülemek için gönderilen ve kaynak ve hedef IP, Akışlar için her kaynak ve hedef IP, alınan Protokolü çeşitli akışları ve süre akışları için kullanılan sağlar.

    ![capanalysis istatistikleri][7]

1. geomap

    Bu bölme her ülkeden trafik hacmi ölçeklendirme renkler, ağ trafiğinizi olan bir harita görünümü sağlar. Gönderilen ve bu ülkede IP'leri alınan veriler oranını gibi ek akış istatistiklerini görüntülemek için vurgulanan ülkelerde seçebilirsiniz.

    ![geomap][8]

1. Filtreler

    CapAnalysis belirli paketlerin hızlı çözümleme için bir filtre kümesi sağlar. Örneğin, bu alt trafik üzerinde belirli serisidir protokolü tarafından verilerini filtrelemek seçebilirsiniz.

    ![filtreler][11]

    Ziyaret [ https://www.capanalysis.net/ca/#about ](https://www.capanalysis.net/ca/#about) tüm CapAnalysis özellikleri hakkında daha fazla bilgi edinmek için.

## <a name="conclusion"></a>Sonuç

Ağ İzleyicisi'nin paket yakalama özelliği, ağ hukuk gerçekleştirmek ve ağ trafiğinizi daha iyi anlamak gereken verileri yakalamanızı sağlar. Bu senaryoda, nasıl Ağ İzleyicisi paket görüntüleri kolayca açık kaynak görselleştirme araçları ile tümleştirilebilir gösterdi. Paket yakalamaları görselleştirmek için CapAnalysis gibi açık kaynaklı araçları kullanarak, derin paket incelemesi gerçekleştiren ve hızlı bir şekilde ağ trafiğinizi içinde eğilimleri belirlemek.

## <a name="next-steps"></a>Sonraki adımlar

NSG akış günlükleri hakkında daha fazla bilgi için [NSG akış günlükleri](network-watcher-nsg-flow-logging-overview.md)

Ziyaret ederek NSG akış günlüklerinizi Power BI ile görselleştirme öğrenin [görselleştirmek NSG akar Power BI ile günlükleri](network-watcher-visualize-nsg-flow-logs-power-bi.md)
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
