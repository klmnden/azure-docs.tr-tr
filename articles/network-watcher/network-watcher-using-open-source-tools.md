---
title: Azure Ağ İzleyicisi ve açık kaynak araçlarla ağ trafiği desenlerini Görselleştirme | Microsoft Docs
description: Bu sayfa, Ağ İzleyicisi paket yakalama ile Capanalysis Vm'lerinizi gelen ve giden trafiği desenlerini görselleştirme için nasıl kullanılacağını açıklar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 3a0ae782d3fe97752ca8b9e786c3c2672f554277
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64936017"
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a>Açık kaynak araçlar kullanarak sanal makinelerinizi gelen ve giden ağ trafiği desenlerini Görselleştirme

Paket yakalama ağ inceleme ve derin paket incelemesi gerçekleştirmenize olanak sağlayan ağ verileri içerir. Kaynak araç ağınız hakkında Öngörüler elde etmek için paket yakalamaları çözümleme için kullanabileceğiniz birçok açılır vardır. CapAnalysis, bir açık kaynak paket yakalama görselleştirme aracı bu araçlardan biridir. Paket yakalama verileri görselleştirme, hızlı Öngörüler düzenleri ve anormallikleri ağınızdaki türetmek için faydalı bir yöntemdir. Görselleştirmeler, ayrıca buna benzer öngörüleri kullanımı kolay bir şekilde paylaşma olanağı sağlar.

Azure Ağ İzleyicisi, ağ üzerinde paket yakalamaları gerçekleştirmenize olanak vererek verilerini yakalama olanağı sağlar. Bu makalede, Ağ İzleyicisi ile CapAnalysis kullanarak görselleştirin ve paket verilerden Öngörüler elde etmeye yönelik bir kılavuzda yakalar sağlar.

## <a name="scenario"></a>Senaryo

Sahip olduğunuz açık kaynak Araçlar, ağ trafik akış desenlerini ve olası anomalileri hızlıca tanımlamak için görselleştirmek için kullanmak istediğiniz bir sanal makine azure'da dağıtılan basit bir web uygulaması. Ağ İzleyicisi ile ağ ortamınızın bir paket yakalama almak ve doğrudan depolama hesabınızda depolayın. CapAnalysis paket yakalaması depolama blobdan doğrudan alma ve içeriğini görselleştirin.

![senaryo][1]

## <a name="steps"></a>Adımlar

### <a name="install-capanalysis"></a>CapAnalysis yükleyin

Bir sanal makinede CapAnalysis yüklemek için resmi yönergeleri burada başvurabilir https://www.capanalysis.net/ca/how-to-install-capanalysis.
CapAnalysis uzaktan erişim, yeni bir gelen güvenlik kuralı ekleyerek vm'nizde 9877 bağlantı noktası açmanız gerekir. Ağ güvenlik gruplarında kuralları oluşturma hakkında daha fazla bilgi için bkz [mevcut bir NSG'de kurallar oluşturma](../virtual-network/manage-network-security-group.md#create-a-security-rule). Kural başarıyla eklendikten sonra gelen CapAnalysis erişebilir olmalıdır `http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a>Paket yakalama oturumu başlatmak için Azure Ağ İzleyicisi'ni kullanın

Ağ İzleyicisi, bir sanal makine ve giden trafiği izlemek için paket yakalamanızı sağlar. Bölümündeki yönergeleri başvurabilirsiniz [Yönet paket yakalar Ağ İzleyicisi ile](network-watcher-packet-capture-manage-portal.md) bir paket yakalama oturumu başlatmak için. Paket yakalaması CapAnalysis tarafından erişilecek bir depolama blobu depolanabilir.

### <a name="upload-a-packet-capture-to-capanalysis"></a>Paket yakalaması CapAnalysis için karşıya yükleme
Paket yakalaması depolandığı "URL'den içeri aktar" sekmesini kullanarak ve depolama blobu bağlantı sağlayan Ağ İzleyicisi tarafından gerçekleştirilen bir paket yakalama doğrudan yükleyebilirsiniz.

Bir bağlantı için CapAnalysis sağlanırken bir SAS belirteci depolama blob URL'ye emin olun.  Bunu yapmak için paylaşılan erişim imzası için depolama hesabından gidin, izin verilen izinleri belirlemek ve bir belirteç oluşturmak için SAS Oluştur düğmesine basın. Ardından, paket yakalama depolama blobu URL'si için bir SAS belirteci ekleyebilirsiniz.

Çıkan URL, aşağıdaki URL aşağıdakine benzer: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Çözümleme paket yakalar.

CapAnalysis paket yakalama, farklı bir bakış açısı sağlayan her analiz görselleştirmek için çeşitli seçenekler sunar. Bu görsel özetler ile ağ trafiğini eğilimlerini ve hızlı bir şekilde hiç olağan dışı etkinlik saptayın. Bu özelliklerin birkaçını aşağıda gösterilmiştir:

1. Akış tabloları

    Bu tablo, paket verileri, iş akışlarıyla ilişkili zaman damgası ve akış, yanı sıra kaynak ve hedef IP ilişkili çeşitli protokoller akışların listesini sağlar.

    ![capanalysis akış sayfası][5]

1. Protokolüne genel bakış

    Bu bölme, ağ trafiğinin dağıtımını çeşitli protokoller ve coğrafyalar hızlıca görmenize olanak sağlar.

    ![capanalysis protokolüne genel bakış][6]

1. İstatistikler

    Bu bölme, ağ trafiğini istatistiklerini – bayt görüntülemek için gönderilen ve alınan kaynak ve hedef Ip'lerini, her kaynak ve hedef Ip'lerini, bir Akış Protokolü çeşitli akışlar ve akışlar süresi için kullanılan sağlar.

    ![capanalysis istatistikleri][7]

1. Coğrafi haritayı

    Bu bölme her ülke/bölgeden trafik hacmi ölçeklendirme renklerle ile ağ trafiğinizin bir harita görünümünü sağlar. Gönderilen ve bu ülke/bölge içinde Ip'lerden alınan veriler oranı gibi ek akış istatistiklerini görüntülemek için vurgulanan ülkeler/bölgeler seçebilirsiniz.

    ![Coğrafi haritayı][8]

1. Filtreler

    CapAnalysis hızlı analiz belirli bir paket için bir filtre kümesi sağlar. Örneğin, bu trafiğin alt dayalı belirli Öngörüler elde etmek için protokolü tarafından verilere filtre uygulamak seçebilirsiniz.

    ![filtreler][11]

    Ziyaret [ https://www.capanalysis.net/ca/#about ](https://www.capanalysis.net/ca/#about) tüm CapAnalysis özellikleri hakkında daha fazla bilgi edinmek için.

## <a name="conclusion"></a>Sonuç

Ağ İzleyicisi'nin paket yakalama özelliğini ağ adli gerçekleştirmek ve ağ trafiğinizi daha iyi anlamak gereken verileri yakalamanıza olanak sağlar. Bu senaryoda nasıl paket yakalamaları Ağ İzleyicisi'nden kolayca açık kaynaklı görselleştirme araçları ile tümleştirilebilir gösterdi. Paket yakalamaları görselleştirmek için CapAnalysis gibi açık kaynak araçları kullanarak derin paket incelemesi gerçekleştiren ve ağ trafiğinizi içinde eğilimleri hızla tanımlayın.

## <a name="next-steps"></a>Sonraki adımlar

NSG akış günlükleri hakkında daha fazla bilgi edinmek için [NSG akış günlükleri](network-watcher-nsg-flow-logging-overview.md)

Ziyaret ederek, NSG akış günlüklerini Power BI ile görselleştirmeyi öğrenmek [görselleştirme NSG akış günlükleri Power BI ile](network-watcher-visualize-nsg-flow-logs-power-bi.md)
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
