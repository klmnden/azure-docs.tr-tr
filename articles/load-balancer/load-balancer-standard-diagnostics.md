---
title: Azure standart yük dengeleyici - tanılama | Microsoft Docs
description: Azure standart yük dengeleyici için tanılama için kullanılabilir ölçümleri ve sistem durumu bilgilerinin kullanma
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: Kumud
ms.openlocfilehash: 7d60925381abe617f6e2fac51176b8e30517c3ba
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Standart yük dengeleyici için ölçümleri ve sistem durumu tanılama

Standart Azure yük dengeleyici kaynaklarınız için aşağıdaki tanılama yetenekleri sağlar:
* Çok boyutlu ölçümler: Standart yük dengeleyici hem genel hem de iç yük dengeleyici yapılandırmalarının yeni çok boyutlu tanılama özelliklerini sağlar. Bu, izlemek, yönetmek ve yük dengeleyici kaynaklarınızı sorunlarını gidermenizi sağlar.

* Kaynak durumu: Kaynak durumu standart yük dengeleyicinin genel yük dengeleyici yapılandırması için Azure portalında yük dengeleyici sayfası ve kaynak durumu sayfası (altında monitör) kullanıma sunar.

Bu makalede Hızlı turuna bu özellikleri ve bunlar standart yük dengeleyici için kullanmanın yolları sağlar.

## <a name = "MultiDimensionalMetrics"></a>Çok boyutlu ölçümleri

Azure yük dengeleyici Portal'daki yeni Azure ölçümleri (Önizleme) aracılığıyla yeni çok boyutlu ölçümleri sağlar ve yük dengeleyici kaynaklarınızı tanılama gerçek zamanlı Öngörüler almanıza yardımcı olur. 

Aşağıdaki ölçümleri bugün farklı standart yük dengeleyici (LB) yapılandırmalar için kullanılabilir:

| Ölçüm | Kaynak türü | Açıklama | Önerilen toplama |
| --- | --- | --- | --- |
| VIP kullanılabilirlik (veri yolu kullanılabilirlik) | Ortak LB | Standart yük dengeleyici veri yolundan bir bölgedeki tüm VM destekleyen SDN yığınına yük dengeleyici ön uç için sürekli olarak uygular. Sağlıklı örnekleri kaldığı sürece ölçüm uygulamanızın yükü dengelenmiş trafiğinin aynı yol izler. Müşteriler tarafından kullanılan veri yolu ayrıca doğrulanır. Ölçüm uygulamanıza görünmez olur ve diğer işlemlerle engellemez.| Ortalama |
| DIP kullanılabilirlik (sistem durumu araştırma durumu) |  Ortak & iç LB | Standart yük dengeleyicisi yapılandırması ayarlarınıza göre uygulama uç noktanın sistem durumunu izler hizmeti yoklama dağıtılmış bir sistem durumu kullanır. Bu ölçüm bitiş noktası filtre görünümünde yük dengeleyici her bağımsız örnek uç başına havuzu veya bir toplama sağlar.  Yük Dengeleyici sistem durumu araştırma yapılandırmanızı tarafından belirtildiği gibi uygulamanızın nasıl görünümleri görebilirsiniz. |  Ortalama |
| Eşitlemeye paketleri |  Ortak LB | Standart yük dengeleyici bırakmaz TCP bağlantılarını sonlandırmak veya TCP veya UDP paket akışları ile etkileşim. Akışlar ve bunların el sıkışmaları her zaman kaynak ve VM örneği arasında olur. Daha iyi TCP protokolü senaryolarınızı gidermek için Eşitlemeye kullanmak yapabileceğiniz kaç TCP bağlantısı anlamak için paketler sayaçları çalışır hale getirilir. Ölçüm alınan TCP Eşitlemeye paketlerin sayısını raporlar.| Ortalama |
| SNAT bağlantıları |  Ortak LB |Standart yük dengeleyici genel IP adresi ön verdiğinizi giden akış sayısı bildirir. SNAT bağlantı noktalarını exhaustible bir kaynaktır. Bu ölçüm nasıl yoğun bir şekilde uygulamanızın üzerinde SNAT giden kaynaklı akışlar için bağlı bir gösterge verebilirsiniz.  Başarılı ve başarısız giden SNAT akışlar için sayaçları bildirilir ve sorun giderme ve giden trafik akışları durumunu anlamak için kullanılabilir.| Ortalama |
| Bayt sayaçları |  Ortak & iç LB | Standart yük dengeleyici ön uç işlenen veri bildirir.| Ortalama |
| Paket sayaçları |  Ortak & iç LB | Standart yük dengeleyici ön uç işlenen paketleri bildirir.| Ortalama |

### <a name="view-load-balancer-metrics-in-the-portal"></a>Yük Dengeleyici ölçümleri portalında görüntüleyin

Azure portal aracılığıyla belirli bir yük dengeleyici kaynak için yük dengeleyici kaynak sayfası hem de Azure İzleyicisi sayfası kullanılabilir ölçümleri (Önizleme) sayfasında, yük dengeleyici ölçümleri kullanıma sunar. 

Ölçüm ölçümleri (yük dengeleyici altında Kaynak İzleyicisi sayfasında), standart yük dengeleyici kaynaklarınızı bu konumlardan herhangi birinde ölçümleri (Önizleme) tarayıcı seçin görüntülemek için açılan listeden, yazın uygun toplama türünü ayarlamak ve isteğe bağlı olarak, gerekli filtreleme ve gruplandırma yapılandırın ve sağlanan koşullarda ölçümleri Geçmiş görünümünü kullanabileceksiniz.

![Standart yük dengeleyici için ölçümleri Önizleme](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Şekil - DIP kullanılabilirlik / sistem durumu ölçümü yük dengeleyici için araştırma*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>API'ler aracılığıyla programlı olarak çok boyutlu ölçümleri alma

Çok boyutlu ölçüm tanımlarını ve değerlerini almak için API Kılavuzu şu adreste [izleme REST API Walkthrouh > çok boyutlu API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api)

### <a name = "DiagnosticScenarios"></a>Genel tanılama senaryoları ve önerilen görünümleri

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Veri yolu my yük dengeleyici VIP için açık ve kullanılabilir mi?

VIP kullanılabilirlik ölçümü bölgedeki Vm'leriniz bulunduğu işlem barındırmak için veri yolu durumunu açıklar. Bu, Azure altyapı yansıma durumunu gösterir. Bu ölçüm kullanabilirsiniz:
- Hizmetinizi dış kullanılabilirliğini izleyin
- Derinliklerine ve hizmetinizi dağıtıldığı platform sağlıklı olup olmadığını veya olup olmadığını anlamak için konuk işletim sistemi veya uygulama örneği iyi durumda
- Bir olay, hizmet veya temel alınan veri düzlemi ilgili olup olmadığını belirleyin. Bu sistem durumu araştırma durum ("DIP kullanılabilirlik") ile karıştırmayın.

VIP kullanılabilirlik için standart yük dengeleyici kaynaklarınızı almak için:
- Doğru yük dengeleyici kaynak seçili olduğundan emin olun. 
- Seçin **VIP kullanılabilirlik** gelen **ölçüm** açılır. 
- Seçin **Avg** için **toplama**. 
- Ayrıca, filtre VIP adresi veya VIP bağlantı noktası gerekli ön uç IP adresi ile boyutun olarak veya ön uç bağlantı noktası ve grup tarafından seçilen boyut ekleyin.

![VIP yoklama](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*-Ayrıntılar yoklama yük dengeleyici VIP Şekil*

Ölçüm etkin, bant dışı bir ölçüye göre oluşturulur. Bölge içindeki bir yoklama hizmeti bu ölçüm için trafiğin kaynaklandığı ve ortak bir ön uç ile bir dağıtımı oluşturur oluşturmaz etkinleştirilir ve ön uç kaldırana kadar devam eder. 

>[!NOTE]
>İç ön uçlar şu anda desteklenmiyor. 

Dağıtımınızın ön uç ve kural eşleşen bir paketi düzenli olarak oluşturulur. Bu bölge kaynaktan konağa arka uç havuzundaki bir VM bulunduğu erişir. Diğer tüm trafik için yaptığı gibi yük dengeleyici altyapı Yük Dengeleme ve çeviri işlemlerin aynısını gerçekleştirir. Bu araştırma bant dışı yük dengeli uç nokta. Arka uç havuzundaki sağlıklı bir VM bulunduğu işlem konakta araştırma geldikten sonra işlem konak Araştırma Hizmeti için bir yanıt oluşturur. VM, bu trafiği görmez.
VIP kullanılabilirlik aşağıdaki nedenlerden dolayı başarısız olur:
- Dağıtımınız arka uç havuzunda kalan sağlıklı VM sahiptir. 
- Altyapı kesinti VIP kullanılabilirlik başarısız olmasına neden olan oluştu.

Kullanabileceğiniz [VIP kullanılabilirlik ölçümü tanılama durumu araştırma birlikte](#vipavailabilityandhealthprobes).

Kullanım **ortalama** çoğu senaryo için toplama olarak.

#### <a name="are-the-backend-instances-for-my-vip-responding-to-probes"></a>My VIP için arka uç örnekler için araştırmalar yanıt?

Sistem durumu araştırma durumu ölçümü, yük dengeleyici durum araştırması yapılandırdığınızda, sizin tarafınızdan yapılandırılan uygulama dağıtımınızın durumunu açıklar. Yük Dengeleyici durum araştırması durumunu yeni akışları gönderileceği yeri belirlemek için kullanır. Sistem durumu araştırmalarının bir Azure altyapı adresinden kaynaklanan ve VM konuk işletim sistemi içinde görünür.

Standart yük dengeleyici kaynaklarınız için DIP kullanılabilirlik elde etmek için
- Seçin **DIP kullanılabilirlik** ile ölçüm **Avg** toplama türü. 
- Bir filtre gerekli VIP IP adres veya bağlantı noktası (veya her ikisi de) uygulayın.

![DIP kullanılabilirliği](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Şekil - yük dengeleyici VIP kullanılabilirliği*

Sistem durumu araştırmalarının aşağıdaki nedenlerden dolayı başarısız olur:
- bir sistem durumu araştırması dinlemiyor veya yanıt vermiyor bir bağlantı noktasına veya yanlış protokolü ile yapılandırırsanız. Hizmetinizi DSR (kayan IP) kuralları kullanıyorsanız, hizmetiniz NIC'ın IP yapılandırması IP adresinde dinlemeye devam eder ve yalnızca üzerinde geri döngü ile ön uç IP adresi yapılandırılmamış olduğundan emin olmanız gerekir.
- Araştırma ağ güvenlik grubu tarafından izin verilmiyorsa, sanal makinenin konuk işletim sistemi güvenlik duvarı ya da uygulama katmanı filtreler.

Kullanım **ortalama** çoğu senaryo için toplama olarak.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>My giden bağlantı istatistikleri nasıl kontrol edilsin mi? 

SNAT bağlantıları ölçüm başarılı ve başarısız hacmi açıklar (için [giden trafik akışları](https://aka.ms/lboutbound)).

Sıfırdan büyük bir bağlantının başarısız birim SNAT bağlantı noktası Tükenme gösterir. Daha fazla araştırmak ve belirlemek bu hatalarına neden. SNAT bağlantı noktası Tükenme bildirimleri kurmak için hataları olarak bir [giden trafik akışları](https://aka.ms/lboutbound). İş ve nasıl en aza / SNAT bağlantı noktası Tükenme önlemek için tasarım mekanizmaları senaryoları anlamak için giden bağlantılar makalesini gözden geçirin. 

SNAT bağlantı istatistikleri almak için
- seçin **SNAT bağlantıları** ölçüm türü ve **toplam** toplama olarak. 
- Gruplandırma ölçütü **bağlantı durumu** başarılı ve başarısız SNAT bağlantı için farklı çizgilerle gösterilen sayar. 

![SNAT bağlantı](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Şekil - yük dengeleyici için SNAT bağlantısı sayısı*


#### <a name="how-do-i-check-inbound--outbound-connection-attempts-for-my-service"></a>İçin Hizmetimi nasıl gelen / giden bağlantı denemeleri denetleyin?

Eşitlemeye paketleri ölçüm birimin gelmedi veya gönderilen TCP Eşitlemeye paketlerin açıklar (için [giden trafik akışları](https://aka.ms/lboutbound)) belirli bir ön uç ile ilişkili. Bu ölçüm, TCP bağlantı denemeleri hizmetinize anlamak için kullanılabilir.
Toplam toplama çoğu senaryosu için kullanın.

![SYN Connection](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Şekil - yük dengeleyici için Eşitlemeye sayısı*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>My ağ bant genişliği kullanımını nasıl kontrol edilsin mi? 

Bayt / paket sayaçları ölçüm birimin bayt ve gönderilen veya başına ön uç temelinde, hizmeti tarafından alınan paketlerin açıklar.
Kullanım **toplam** çoğu senaryo için toplama olarak.

Bayt veya paket sayısı istatistikleri almak için
- Seçin **bayt sayısını** ve / veya **paket sayısı** ölçüm türüyle **Avg** toplama olarak. 
- Bir özel ön uç IP, ön uç bağlantı noktası veya arka uç IP veya söz konusu bağlantı noktası filtre uygulayın. 
- veya hiçbir filtreleme olmadan yük dengeleyici kaynak için genel istatistiklerini alın.


Ölçümleri farklı yapılandırmalarıyla için bazı örnek görünümleri-

![Bayt sayısı](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Şekil - yük dengeleyici için bayt sayısı*

#### <a name = "vipavailabilityandhealthprobes"></a>My yük dengeleyici dağıtımı nasıl tanılamak?

VIP kullanılabilirlik & sistem durumu Araştırmalarının ölçümleri tek bir grafik birleşimi sorun için bakın ve sorunu çözmek nereye tanımlamanıza izin verebilirsiniz. Azure düzgün çalışmasını güvence kazanmak ve conclusively yapılandırma belirlemek için bunu kullanın veya kök nedeni uygulamasıdır.

Sistem durumu araştırma ölçümleri, nasıl Azure dağıtımınızın sağladığınız yapılandırma göredir sistem görünümleri anlamak için kullanabilirsiniz. Sistem durumu araştırmalarının arayan her zaman izleme veya bir nedeni belirleme bir harika ilk adımdır.

Daha fazla adımı ve VIP kullanılabilirlik ölçümleri nasıl Azure belirli bir dağıtım için sorumlu temel alınan veri düzlemi sistem görünümleri içine kavramak için kullanın. Her iki ölçümleri birleştirdiğinizde, bu örnekte gösterildiği gibi hata nerede olabilir ayırabilirsiniz:

![VIP tanılama](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Şekil - birleştirme DIP ve VIP kullanılabilirlik ölçümleri*

Grafik aşağıdaki bilgileri gösterir:
- Altyapı sağlıklı Vm'leriniz barındırma altyapısını ulaşılabilir ve birden fazla VM arka yerleştirilir. Bu, % 100 oranında gösterir VIP kullanılabilirlik mavi izleme tarafından belirtilir. 
- Ancak, araştırma durumu (DIP kullanılabilirlik) %0 grafik başındaki turuncu izleme tarafından belirtildiği şekilde altındadır. Burada (DIP kullanılabilirlik) sistem durumu araştırmalarının kırmızı vurgular daire içinde alanında sağlıklı hale geldi ve bu noktada müşterinin dağıtım yeni akışları kabul edemiyor.

Grafik tahmin veya gerçekleşen diğer sorunlar gerçekleştirildiğini desteği isteyin zorunda kalmadan kendi başlarına dağıtım sorunlarını giderme müşteriye izin. Sistem durumu araştırmalarının yanlış yapılandırılması veya başarısız bir uygulamayı başarısız çünkü Müşteri'nin hizmet kullanılamaz.

### <a name = "Limitations"></a>Sınırlamaları

- VIP kullanılabilirlik yalnızca genel ön uçlar için şu anda kullanılabilir değil.

## <a name = "ResourceHealth"></a>Kaynak sistem durumu

Standart yük dengeleyici kaynaklar için sistem durumu varolan aracılığıyla gösterilir **kaynak durumu** altında **İzleyici > Hizmet durumu**.

>[!NOTE]
>Yük Dengeleyici için kaynak durumu şu anda yalnızca standart yük dengeleyicinin genel yapılandırması için kullanılabilir. İç yük dengeleyici kaynakları veya temel SKU, yük dengeleyici kaynakları kaynak durumu gösterme.

Ortak standart yük dengeleyici kaynaklarınızın sistem durumunu görüntülemek için
 - Gözat **İzleyici > Hizmet sistem durumu**.

  ![İzleme sayfası](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Şekil - Azure monitörde hizmet durumu*

 - Seçin **kaynak durumu** doğru olduğundan emin olun **abonelik kimliği** ve **kaynak türü yük dengeleyici =** seçilir.

  ![Kaynak sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Şekil - sistem durumu görünümü için kaynağı seçin*

 - Yük Dengeleyici kaynak geçmiş sistem durumlarını görüntülemek için listeden tıklayın.

    ![Yük Dengeleyici sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Şekil - yük dengeleyici kaynak sistem durumu görünümü*
 
Aşağıdaki tabloda, çeşitli kaynak sistem durumu ve açıklamalarını listeler. 

| Kaynak sistem durumu | Açıklama |
| --- | --- |
| Kullanılabilir | Ortak standart yük dengeleyici kaynak sağlıklı ve kullanılabilir durumda |
| Kullanılamaz | Ortak standart yük dengeleyici kaynak iyi durumda değil. Azure İzleyicisi aracılığıyla sağlığını tanılamanız > ölçümleri. (*Bu kaynak genel standart yük dengeleyici değil gelebilir*) |
| Bilinmiyor | Kaynak durumu, ortak bir standart yük dengeleyici için henüz güncelleştirilmemiş. (*Bu kaynak genel standart yük dengeleyici değil gelebilir*)  |

## <a name="next-steps"></a>Sonraki Adımlar

- Daha fazla bilgi edinmek [standart yük dengeleyici](load-balancer-standard-overview.md)
- Daha fazla bilgi edinmek [yük dengeleyici giden bağlantı](https://aka.ms/lboutbound)


