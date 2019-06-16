---
title: Azure Ağ İzleyicisi ile paket incelemesi | Microsoft Docs
description: Bu makalede, bir VM'den toplanan derin paket incelemesi gerçekleştirmek için Ağ İzleyicisi'ni kullanmayı açıklar
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 7f3fc69bbfd881a26ceb25705852558b66c60153
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64716909"
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Azure Ağ İzleyicisi ile paket incelemesi

Ağ İzleyicisi paket yakalama özelliğini kullanarak, Şirket portalı, PowerShell, CLI ve REST API'si ve SDK'sı aracılığıyla program aracılığıyla Azure sanal makinelerinize yakalamaları oturumlarını yönetmek ve başlatmak. Paket yakalaması kullanılabilir bir biçimde bilgi sağlayarak paket düzeyinde veri gerektiren senaryolara olanak tanır. Verileri incelemek için ücretsiz Araçlar yararlanarak, iletişimler için ve sanal makinelerinize inceleyin ve ağ trafiğiniz hakkında Öngörüler elde edebilirsiniz. Paket yakalama veri bazı örnek kullanımları şunlardır: ağ veya uygulama sorunlarını araştırma, ağ kötüye kullanım ve yetkisiz erişim girişimlerini algılama ve yasal uyumluluk bakımını yapma. Bu makalede, Ağ İzleyicisi tarafından sağlanan bir paket yakalama dosyasını açmak nasıl bir popüler açık kaynak aracını kullanarak göstereceğiz. Bağlantı gecikmesini hesaplamak, olağan dışı trafiği tanımlamak ve ağ istatistiklerini İnceleme gösteren örnekler de sağlayacağız.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede daha önce çalıştırılmış olan bir paket yakalama önceden yapılandırılmış bazı senaryolar üzerinden gider. Bu senaryolar, bir paket yakalama gözden geçirerek erişilebilir özellikleri gösterilmektedir. Bu senaryo kullanan [WireShark](https://www.wireshark.org/) paket yakalaması incelemek için.

Bu senaryo bir sanal makinede bir paket yakalaması zaten çalıştırıldığı varsayılır. Bir paket yakalama ziyaret oluşturma hakkında bilgi edinmek için [portal ile yönetme paket yakalar](network-watcher-packet-capture-manage-portal.md) veya REST ederek [REST API'si ile yönetme paket yakalar](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Senaryo

Bu senaryoda:

* Paket yakalaması gözden geçirin

## <a name="calculate-network-latency"></a>Ağ gecikme süresini hesapla

Bu senaryoda, iki uç nokta arasında gerçekleşen bir İletim Denetimi Protokolü (TCP) konuşma ilk gidiş dönüş süresi (RTT) görüntüleme göstereceğiz.

Bir TCP bağlantı kurulduğunda bağlantı gönderilen ilk üç paketler üç yönlü anlaşma adlandırılan deseni izler. Bu bağlantı oluşturulduğunda istemciden bir ilk istek ve yanıt sunucudan bu el sıkışması gönderilen ilk iki paket inceleyerek gecikme hesaplayabiliriz. Bu gecikme süresi, gidiş dönüş süresi (RTT) olarak adlandırılır. TCP protokolü ve üç yönlü anlaşma daha fazla bilgi için aşağıdaki kaynağa bakın. https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip

### <a name="step-1"></a>1\. Adım

WireShark başlatın

### <a name="step-2"></a>2\. Adım

Yük **.cap** dosyasını, paket yakalama. Bu dosya, kaydedildi BLOB bulunabilir bizim nasıl yapılandırdığınıza bağlı olarak sanal makine üzerinde yerel olarak.

### <a name="step-3"></a>3\. Adım

TCP konuşmalardaki ilk gidiş dönüş süresi (RTT) görüntülemek için biz yalnızca ilk iki paketleri TCP el sıkışması dahil, arayacaktır. Biz [SYN] olan üç yönlü anlaşma ilk iki paketlerinde kullanacaklardır [SYN, ACK] paketler. TCP üstbilgisinde ayarlanan bayrakları için adlandırılır. Bu senaryoda, son pakette [ACK] paketini el sıkışması kullanılmaz. [SYN] paketi, istemci tarafından gönderilir. Sunucu [ACK] paketini aldıktan sonra bir bildirim SYN istemciden alma olarak gönderir. Sunucu yanıtı çok az ek yük gerektirdiğini yararlanarak, biz RTT zamanını çıkararak [SYN, ACK] hesaplamak paket alındı [SYN] zamanına göre istemcinin paket istemci tarafından gönderildi.

WireShark kullanarak bu değeri bizim için hesaplanır.

İlk iki paketi TCP üç yönlü anlaşma daha kolay görüntülemek için WireShark tarafından sağlanan filtreleme özelliği yararlanacaktır.

WireShark içinde filtre uygulamak için yakalama [SYN] pakette "İletim Denetimi Protokolü" segmentini genişletin ve TCP üstbilgisinde ayarlanan işaretlere inceleyin.

Tüm [SYN] üzerinde filtre arıyoruz olduğundan ve [SYN ACK] Syn biti 1 olarak ayarlayın ve ardından sağ tıklayarak Syn bit -> Uygula Filtre -> olarak seçili paket bayrakları altında onaylayın.

![Şekil 7][7]

### <a name="step-4"></a>4\. Adım

Yalnızca [SYN] biti ayarlanmış olan paketler görmek için penceresini filtreledi, ilk RTT görüntülemek ilginizi çeken konuşmaları kolayca seçebilirsiniz. RTT WireShark içinde görüntülemek için basit bir yol tıklamanız yeterlidir "SEQ/ACK" analiz işaretlenmiş açılır. Ardından görüntülenen RTT görürsünüz. Bu durumda, RTT 0.0022114 saniye veya 2.211 ms idi.

![Şekil 8][8]

## <a name="unwanted-protocols"></a>İstenmeyen protokolleri

Azure'da dağıtılmış bir sanal makine örneğinde çalışan birçok uygulama olabilir. Bu uygulamaların çoğu ağ üzerinden, belki de açık izniniz olmadan yaşınızı iletişim kurar. Ağ iletişimi depolamak için paket yakalama özelliğini kullanarak, biz nasıl uygulama ağ üzerinde olduğu varsayılır ve herhangi bir sorun için konum araştırabilirsiniz.

Bu örnekte, şimdi bir önceki gözden makinenizde çalışan bir uygulamadan yetkisiz iletişim gösterebilir istenmeyen protokoller için paket yakalaması çalıştı.

### <a name="step-1"></a>1\. Adım

Önceki senaryoda aynı yakalama kullanarak **istatistikleri** > **Protokolü hiyerarşisi**

![protokol hiyerarşi menüsü][2]

Protokol hiyerarşi penceresinde görünür. Bu görünüm, yakalama oturumu ve aktarılan ve protokoller kullanılarak alınan paket sayısı sırasında kullanımda olan tüm protokollerin bir listesini sağlar. Bu görünüm, istenmeyen ağ trafiğini sanal makineler veya ağda bulmak için yararlı olabilir.

![açılan iletişim kuralı hiyerarşisi][3]

Aşağıdaki ekran görüntüsünde görüldüğü gibi eşler arası dosya paylaşımı için kullanılan BitTorrent protokolünü kullanarak trafiği vardı. Yönetici olarak BitTorrent görmeyi beklediğiniz değil Bu belirli sanal makinelerde trafiği. Şimdi, bu trafiğin kullanan bu sanal makinede yüklü eşler arası yazılım kaldırabilir, veya ağ güvenlik grupları veya bir Güvenlik Duvarı'nı kullanarak trafiği engelleme. Buna ek olarak, Protokolü kullanımı üzerindeki sanal makinelerinize düzenli olarak gözden geçirebilmeniz için paket yakalamaları almanızı sağlamanın bir zamanlamaya göre çalıştırabilir katılmak bırakmayı. Azure ağ görevleri otomatikleştirmek nasıl bir örnek ziyaret [azure Otomasyonu ile ağ kaynaklarını izleme](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>En çok kullanılan hedefler ve bağlantı noktalarını bulma

Trafiği, uç noktaları ve bağlantı noktaları üzerinden iletişim kurduğu türlerini anlama bir izleme ya da sorun giderme uygulamaları ve ağınızdaki kaynaklara önemlidir. Bir paket yakalama dosyası yukarıdaki yararlanarak, biz bizim VM ile iletişim kurduğu en çok kullanılan hedefler ve kullanılan bağlantı noktaları hızlıca bilgi edinebilirsiniz.

### <a name="step-1"></a>1\. Adım

Önceki senaryoda aynı yakalama kullanarak **istatistikleri** > **IPv4 istatistikleri** > **hedefler ve bağlantı noktaları**

![Paket yakalama penceresi][4]

### <a name="step-2"></a>2\. Adım

Bir satır belirginleştirmek sonuçları aracılığıyla baktığımızda gibi birden çok bağlantı noktasında 111 vardı. En çok kullanılan bağlantı noktası 3389, Uzak Masaüstü olduğu olduğu ve kalan RPC dinamik bağlantı noktaları değildir.

Bu trafiğe bir şey anlamına gelebilir, ancak birçok bağlantıları için kullanılan ve yöneticinin bilinmeyen bir bağlantı noktası var.

![Şekil 5][5]

### <a name="step-3"></a>3\. Adım

Artık, bir çıkış bağlantı noktasına dayanarak bizim yakalama filtreleyeceğiz yerde bağlantı noktasının belirledik.

Bu senaryoda filtresi şöyle olacaktır:

```
tcp.port == 111
```

Size yukarıda filtre metnini filtre metin kutusuna girin ve ENTER tuşuna basın.

![Şekil 6][6]

Tüm trafik aynı alt ağda yerel bir sanal makineden gelen sonuçlardan görebiliriz. Biz yine de bu trafiği neden oluştuğunu anlamıyorsanız, size daha fazla neden 111 numaralı bağlantı noktasında bu çağrıları yapma belirlemek için paketler inceleyebilirsiniz. Bu bilgileri size uygun eylemi gerçekleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Ağ İzleyicisi'nin diğer tanılama özellikleri hakkında bilgi edinin ederek [Azure ağ izlemeye genel bakış](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













