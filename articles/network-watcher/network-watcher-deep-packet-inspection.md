---
title: Azure Ağ İzleyicisi ile paket incelemesi | Microsoft Docs
description: Bu makalede, bir VM'den toplanan derin paket incelemesi gerçekleştirmek için Ağ İzleyicisi'ni kullanmayı açıklar
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 1ad6ca4abe73336ce9ce3539fdaf2a9d7dd23fa6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23863975"
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Azure Ağ İzleyicisi ile paket incelemesi

Ağ İzleyicisi'nin paket yakalama özelliği kullanarak, Azure vm'lerinde portalı, PowerShell CLI ve SDK ve REST API aracılığıyla programlı olarak yakalamaları oturumlarını yönetmesine ve başlatın. Paket yakalama, paket düzeyinde veri kullanılabilir bir biçimde bilgileri sağlayarak gerektiren adresi senaryolar olanak tanır. Verileri incelemek için ücretsiz Araçlar yararlanarak, Vm'leriniz gelen ve giden iletişimler inceleyin ve ağ trafiğinizi alın. Paket yakalama veri bazı örnek kullanımları şunlardır: ağ veya uygulama sorunları incelemeye, ağ kötüye ve yetkisiz erişim girişimlerini algılama veya Mevzuat uyumluluğu bakımını yapma. Bu makalede, Ağ İzleyicisi tarafından sağlanan bir paket yakalama dosyasını açmak nasıl bir popüler açık kaynak aracını kullanarak gösteriyoruz. Biz de nasıl bir bağlantı gecikmesi hesaplamak, anormal trafiği tanımlamak ve ağ istatistikleri inceleyin gösteren örnekler sağlanır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede önceden çalıştırılmış olan bir paket yakalama önceden yapılandırılmış bazı senaryolar üzerinden gider. Bu senaryolar, bir paket yakalama gözden geçirerek erişilebilir özellikleri gösterilmektedir. Bu senaryo kullanır [WireShark](https://www.wireshark.org/) paket yakalama incelemek için.

Bu senaryo, bir sanal makinede bir paket yakalama zaten başlattıysanız varsayar. Bir paket yakalama ziyaret oluşturmayı öğrenmek için [Yönet paket yakalar portal ile](network-watcher-packet-capture-manage-portal.md) veya REST şu adresi ziyaret ederek ile [REST API ile paket yakalar yönetme](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Senaryo

Bu senaryoda:

* Paket yakalama gözden geçirin

## <a name="calculate-network-latency"></a>Ağ gecikmesi Hesapla

Bu senaryoda, iki uç noktaları arasında gerçekleşen bir İletim Denetimi Protokolü (TCP) konuşma ilk gidiş dönüş süresi (RTT) görüntülemek nasıl gösterir.

Bir TCP bağlantı kurulduğunda bağlantı gönderilen ilk üç paketleri genellikle üç yönlü el sıkışması olarak adlandırılan bir yol izler. Bu bağlantı kuruldu, istemci bir ilk istek ve yanıt sunucudan bu el sıkışma gönderilen ilk iki paketleri inceleyerek biz gecikme hesaplayabilirsiniz. Bu gecikme gidiş dönüş süresi (RTT) olarak adlandırılır. TCP protokolü ve üç yönlü el sıkışma hakkında daha fazla bilgi için aşağıdaki kaynağa bakın. https://support.microsoft.com/en-US/Help/172983/Explanation-of-the-three-Way-Handshake-via-TCP-ip

### <a name="step-1"></a>1. Adım

WireShark başlatma

### <a name="step-2"></a>2. Adım

Yük **.cap** paket yakalama dosyasından. Bu dosya içinde kaydedildi blob bulunabilir bizim nasıl yapılandırdığınıza bağlı olarak sanal makinenin yerel olarak.

### <a name="step-3"></a>3. Adım

TCP görüşmeleri ilk gidiş dönüş süresi (RTT) görüntülemek için biz yalnızca TCP anlaşma söz konusu ilk iki paket adresindeki arayacaktır. Biz [Eşitlemeye] olan üç yönlü el sıkışma ilk iki paketlerinde kullanacağınız [Eşitlemeye, ACK] paketler. TCP üstbilgisinde ayarlanan bayraklarının adlandırılır. Son paket [ACK] paket el sıkışma içinde bu senaryoda kullanılmaz. İstemci tarafından gönderilen [Eşitlemeye] paket. Alındıktan sonra sunucu [ACK] paket Eşitlemeye istemciden alma onay olarak gönderir. Sunucu yanıtı çok az ek yük gerektirdiğini yararlanarak, biz RTT zamanını çıkararak [Eşitlemeye, ACK] hesaplamak paket alındı [Eşitlemeye] zamana göre istemci tarafından paket istemci tarafından gönderildi.

WireShark kullanarak bu değeri bize hesaplanır.

Daha kolay TCP üç yönlü anlaşma ilk iki paketlerini görüntülemek için biz WireShark tarafından sağlanan filtreleme özellik yararlanacak olan.

WireShark filtre uygulamak için yakalama [Eşitlemeye] pakette "İletim Denetimi Protokolü" kesimi genişletin ve TCP üstbilgisinde ayarlanan bayraklar inceleyin.

Tüm [Eşitlemeye] üzerinde filtrelemek için arıyoruz beri ve [Eşitlemeye, ACK] bayrakları altında paketleri cofirm eşitlemeye biti 1 olarak ayarlayın, sonra sağ tıklayarak eşitlemeye bit Filtre -> olarak seçili Uygula ->.

![Şekil 7][7]

### <a name="step-4"></a>4. Adım

Yalnızca [Eşitlemeye] bit kümesiyle paketleri görmek için penceresini filtre, ilk RTT görüntülemek ilgilendiğiniz görüşmeleri kolayca seçebilirsiniz. RTT WireShark içinde görüntülemek için basit bir yol tıklamanız yeterlidir "SEQ/ACK" analiz işaretlenmiş açılır. Ardından görüntülenen RTT görürsünüz. Bu durumda, RTT 0.0022114 saniye ya da 2.211 ms idi.

![Şekil 8][8]

## <a name="unwanted-protocols"></a>İstenmeyen protokolleri

Azure üzerinde dağıtılan sanal makine örneği üzerinde çalışan birçok uygulamalar olabilir. Bu uygulamalar çoğunu ağ üzerinden belki de açık izniniz olmadan iletişim kurar. Paket yakalama ağ iletişimi depolamak için kullanarak, biz nasıl uygulama ağ üzerinde varsayılır ve herhangi bir sorun için Ara araştırabilirsiniz.

Bu örnekte, sizi bir önceki gözden makinenizde çalışan bir uygulama yetkisiz iletişimi gösterebilir istenmeyen protokoller için paket yakalama çalıştı.

### <a name="step-1"></a>1. Adım

Önceki senaryoda aynı yakalama kullanarak **istatistikleri** > **Protokolü hiyerarşisi**

![protokol hiyerarşi menüsü][2]

Protokol hiyerarşisi penceresi görüntülenir. Bu görünüm yakalama oturumunu ve aktarılan ve protokoller kullanılarak alınan paket sayısı sırasında kullanımda olan tüm protokollerin bir listesini sağlar. Bu görünüm, sanal makine ya da ağ üzerindeki istenmeyen ağ trafiğini bulmak için yararlı olabilir.

![açılan iletişim kuralı hiyerarşisini][3]

Aşağıdaki ekran görüntüsünde gördüğünüz gibi eşler arası dosya paylaşımı için kullanılan BitTorrent protokolünü kullanarak trafiği vardı. Yönetici olarak BitTorrent görmeyi beklediğiniz değil Bu belirli sanal makinelerde trafiği. Şimdi, bu trafiğin kullanan, bu sanal makinede yüklü eşler arası yazılım kaldırabilir, veya ağ güvenlik grupları veya bir Güvenlik Duvarı'nı kullanarak trafiği engelle. Ayrıca, Protokolü kullanımı, sanal makinelere düzenli olarak gözden geçirebilirsiniz şekilde paket yakalamaları bir zamanlamaya göre çalışmasını tercih edebilirsiniz. Azure ağ görevleri otomatik hale getirme konusunda bir örnek ziyaret [izlemek azure automation ile ağ kaynakları](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>En çok kullanılan hedefler ve bağlantı noktalarını bulma

Üzerinden iletilen bağlantı noktalarını, uç noktalarını ve trafik türlerini anlama bir izleme veya sorun giderme uygulamaları ve ağınızdaki kaynaklara önemlidir. Paket yakalama dosyası üstten yararlanarak, biz bizim VM ile iletişim kurduğu üst hedefler ve kullanılan bağlantı noktaları hızla bilgi edinebilirsiniz.

### <a name="step-1"></a>1. Adım

Önceki senaryoda aynı yakalama kullanarak **istatistikleri** > **IPv4 istatistikleri** > **hedefleri ve bağlantı noktaları**

![Paket yakalama penceresi][4]

### <a name="step-2"></a>2. Adım

Bir satır öne çıkması sonuçları göz biz gibi birden çok bağlantı noktası bağlantı 111 vardı. En fazla kullanılan bağlantı noktası 3389, Uzak Masaüstü olduğu idi ve kalan RPC dinamik bağlantı noktalarının.

Bu trafik bir şey olabilir, ancak birçok bağlantıları için kullanılan ve yönetici için bilinmeyen bir bağlantı noktasıdır.

![Şekil 5][5]

### <a name="step-3"></a>3. Adım

Yerleştir bağlantı noktası dışı belirledik şimdi biz bağlantı noktasını temel alarak bizim yakalama filtreleyebilirsiniz.

Bu senaryoda filtresi şöyle olacaktır:

```
tcp.port == 111
```

Biz üstten filtre metni filtre metin kutusuna girin ve ENTER tuşuna basın.

![Şekil 6][6]

Tüm trafiğin aynı alt ağda yerel bir sanal makineden gelen sonuçlarından görebiliriz. Biz bu trafiği neden oluştuğunu hala anlamadığınız, size daha fazla neden bu bağlantı noktası 111 çağrıları yapma belirlemek için paket inceleyebilirsiniz. Bu bilgileri size uygun eylemi gerçekleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Ağ İzleyicisi, diğer tanılama özellikleri hakkında bilgi edinin ziyaret ederek [Azure ağ izlemeye genel bakış](network-watcher-monitoring-overview.md)

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













