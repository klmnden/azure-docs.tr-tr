---
title: Azure standart yük dengeleyici tanılama | Microsoft Docs
description: Kullanılabilir Ölçümler ve sistem durumu bilgileri için tanılama Azure standart yük dengeleyici için kullanın.
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
ms.openlocfilehash: 9d5d596254f673b86650e8d9754dacdb70be0666
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32179803"
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Standart yük dengeleyici için ölçümleri ve sistem durumu tanılama

Standart Azure yük dengeleyici kaynaklarınızı aşağıdaki tanılama yetenekleri sağlar:
* **Çok boyutlu ölçümleri**: yeni çok boyutlu tanılama özellikleri için hem genel hem de iç yük dengeleyici yapılandırmalarının sağlar. İzlemek, yönetmek ve yük dengeleyici kaynaklarınızı giderebilirsiniz.

* **Kaynak durumu**: Azure portalında yük dengeleyici sayfası ve kaynak durumu sayfası (altında monitör) kullanıma standart yük dengeleyicinin genel yük dengeleyici yapılandırması için kaynak durumu bölümü.

Bu makalede bu yetenekleri hızlı turuna sağlar ve standart yük dengeleyici için kullanılacak yol sunar.

## <a name = "MultiDimensionalMetrics"></a>Çok boyutlu ölçümleri

Azure yük dengeleyici Azure Portal'daki yeni Azure ölçümleri (Önizleme) aracılığıyla yeni çok boyutlu ölçümleri sağlar ve gerçek zamanlı tanılama Öngörüler yük dengeleyici kaynakları almanıza yardımcı olur. 

Çeşitli standart yük dengeleyici yapılandırmalarının aşağıdaki ölçümleri sağlar:

| Ölçüm | Kaynak türü | Açıklama | Önerilen toplama |
| --- | --- | --- | --- |
| VIP kullanılabilirlik (veri yolu kullanılabilirlik) | Genel yük dengeleyiciye | Standart yük dengeleyici sürekli veri yolu bir bölgedeki yük dengeleyici ön uç, bu süreç boyunca tüm VM destekleyen SDN yığınına uygular. Sağlıklı örnekleri kaldığı sürece ölçüm uygulamanızın yükü dengelenmiş trafiğinin aynı yol izler. Müşterilerinize kullanmak veri yolu ayrıca doğrulanır. Ölçüm uygulamanıza görünmez olur ve diğer işlemlerle engellemez.| Ortalama |
| DIP kullanılabilirlik (sistem durumu araştırma durumu) |  Genel ve iç yük dengeleyici | Standart yük dengeleyicisi yapılandırması ayarlarınıza göre uygulama uç noktanın sistem durumunu izler dağıtılmış sistem durumu yoklama hizmet kullanır. Bu ölçüm, bir toplama veya uç nokta başına sağlar her örneğinin uç nokta yük dengeleyici havuzunda görünümünü filtrelenir. Yük Dengeleyici, uygulamanızın durumunu nasıl görünümleri, sistem durumu araştırma yapılandırması tarafından belirtilen şekilde görebilirsiniz. |  Ortalama |
| Eşitlemeye (eşitleme) paketleri |  Genel yük dengeleyiciye | Standart yük dengeleyici bırakmaz İletim Denetimi Protokolü (TCP) bağlantılarını sona veya TCP veya UDP paket akışları ile etkileşim. Akışlar ve bunların el sıkışmaları her zaman kaynak ve VM örneği arasında olur. Daha iyi TCP protokolü senaryolarınızı gidermek için Eşitlemeye kullanmak yapabileceğiniz kaç TCP bağlantısı anlamak için paketler sayaçları çalışır hale getirilir. Ölçüm alınan TCP Eşitlemeye paketlerin sayısını raporlar.| Ortalama |
| SNAT bağlantıları |  Genel yük dengeleyiciye |Standart yük dengeleyici genel IP adresi ön ucu verdiğinizi giden akış sayısı bildirir. Kaynak ağ adresi çevirisi (SNAT) bağlantı noktalarını exhaustible bir kaynaktır. Bu ölçüm nasıl yoğun bir şekilde uygulamanızın üzerinde SNAT giden kaynaklı akışlar için bağlı bir gösterge verebilirsiniz. Başarılı ve başarısız giden SNAT akışlar için sayaçları bildirilir ve sorun giderme ve giden trafik akışları durumunu anlamak için kullanılabilir.| Ortalama |
| Bayt sayaçları |  Genel ve iç yük dengeleyici | Standart yük dengeleyici ön uç işlenen veri bildirir.| Ortalama |
| Paket sayaçları |  Genel ve iç yük dengeleyici | Standart yük dengeleyici ön uç işlenen paketleri bildirir.| Ortalama |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Azure Portalı'nda, yük dengeleyici ölçümlerinizi görüntüleyin

Azure portalında yük dengeleyici ölçümleri hem yük dengeleyici kaynak sayfasında belirli bir kaynak için ve Azure İzleyici sayfasında kullanılabilir ölçümleri (Önizleme) sayfası yoluyla kullanıma sunar. 

Standart yük dengeleyici kaynaklarınız için ölçümleri görüntülemek için:
1. Ölçümleri (Önizleme) sayfasına gidin ve aşağıdakilerden birini yapın:
   * Yük Dengeleyici kaynak sayfasında ölçüm türü aşağı açılan listeden seçin.
   * Azure İzleyici sayfasında, yük dengeleyici kaynak seçin.
2. Uygun toplama türünü ayarlayın.
3. İsteğe bağlı olarak, gerekli filtreleme ve gruplandırma yapılandırın.

![Standart yük dengeleyici için ölçümleri Önizleme](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Şekil: DIP kullanılabilirlik ve standart yük dengeleyici için sistem durumu araştırma durumu ölçümü*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>API'ler aracılığıyla programlı olarak çok boyutlu ölçümleri alma

Çok boyutlu ölçüm tanımlarını ve değerlerini almak için API yönergeler için bkz [Azure izleme REST API izlenecek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Genel tanılama senaryoları ve önerilen görünümleri

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Yukarı ve kullanılabilir veri yolu my yük dengeleyici VIP için mi?

VIP kullanılabilirlik ölçümü bölgedeki Vm'leriniz bulunduğu işlem barındırmak için veri yolu durumunu açıklar. Azure altyapısının sistem yansıması ölçümüdür. Ölçüm için kullanabilirsiniz:
- Hizmetinizi dış kullanılabilirliğini izleyin
- Derinliklerine ve hizmetinizi dağıtıldığı platform sağlıklı olup ya da konuk işletim sistemi veya uygulama örneği sağlıklı olup anlayın.
- Bir olay, hizmet veya temel alınan veri düzlemi ilgili olup olmadığını belirleyin. Bu ölçüm araştırma durumu ("DIP kullanılabilirlik") ile karıştırmayın.

VIP kullanılabilirlik için standart yük dengeleyici kaynaklarınızı almak için:
1. Doğru yük dengeleyici kaynak seçili olduğundan emin olun. 
2. İçinde **ölçüm** aşağı açılan listesinden, **VIP kullanılabilirlik**. 
3. İçinde **toplama** aşağı açılan listesinden, **Avg**. 
4. Ayrıca, bir filtre VIP adresi veya VIP bağlantı noktası gerekli ön uç IP adresi ile boyutun olarak veya ön uç bağlantı noktası ekleyin ve ardından seçili boyuta göre gruplandırın.

![VIP yoklama](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Şekil: Yük dengeleyici VIP ayrıntıları yoklama*

Ölçüm etkin, bant dışı bir ölçüye göre oluşturulur. Bölge içindeki bir yoklama hizmeti trafiği için ölçümü kaynaklanır. Hizmet, ortak bir ön uç ile bir dağıtımını oluşturun ve ön uç kaldırana kadar devam eder hemen etkinleştirildi. 

>[!NOTE]
>İç ön uçlar şu anda desteklenmiyor. 

Dağıtımınızın ön uç ve kural eşleşen bir paketi düzenli olarak oluşturulur. Bu bölge kaynaktan konağa arka uç havuzundaki bir VM bulunduğu erişir. Diğer tüm trafik için yaptığı gibi yük dengeleyici altyapı Yük Dengeleme ve çeviri işlemlerin aynısını gerçekleştirir. Bu araştırma bant dışı yük dengeli uç noktasındaki. Arka uç havuzundaki sağlıklı bir VM bulunduğu, işlem konakta araştırma ulaştıktan sonra işlem konak Araştırma Hizmeti için bir yanıt oluşturur. VM, bu trafiği görmez.

VIP kullanılabilirlik aşağıdaki nedenlerden dolayı başarısız olur:
- Dağıtımınız arka uç havuzunda kalan sağlıklı VM sahiptir. 
- Altyapı kesinti oluştu.

Tanılama amacıyla kullanabileceğiniz [VIP kullanılabilirlik ölçümü durumu araştırma birlikte](#vipavailabilityandhealthprobes).

Kullanım **ortalama** çoğu senaryo için toplama olarak.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>My VIP için arka uç örnekler için araştırmalar yanıt?

Sistem durumu araştırma durumu ölçümü, yük dengeleyici durum araştırması yapılandırdığınızda, sizin tarafınızdan yapılandırılan uygulama dağıtımınızın durumunu açıklar. Yük Dengeleyici durum araştırması durumunu yeni akışları gönderileceği yeri belirlemek için kullanır. Sistem durumu araştırmalarının bir Azure altyapı adresinden kaynaklanan ve VM konuk işletim sistemi içinde görünür.

Standart yük dengeleyici kaynaklarınız için DIP kullanılabilirlik almak için:
1. Seçin **DIP kullanılabilirlik** ile ölçüm **Avg** toplama türü. 
2. Bir filtre gerekli VIP IP adres veya bağlantı noktası (veya her ikisi de) uygulayın.

![DIP kullanılabilirliği](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Şekil: Yük dengeleyici VIP kullanılabilirliği*

Sistem durumu araştırmalarının aşağıdaki nedenlerden dolayı başarısız olur:
- Bir sistem durumu araştırma dinlemiyor veya yanıt vermiyor bir bağlantı noktasına yapılandırmanız veya yanlış protokolünü kullanarak. Hizmetinizi doğrudan sunucu dönüşünü (DSR veya kayan IP) kullanıyorsa, kurallar, NIC'ın IP yapılandırması IP adresinde dinleyen bir hizmet emin olun ve yalnızca ön uç IP ile yapılandırılmış geri döngü adres.
- Araştırma ağ güvenlik grubu, sanal makinenin konuk işletim sistemi güvenlik duvarı ya da uygulama katman filtreleri izin verilmez.

Kullanım **ortalama** çoğu senaryo için toplama olarak.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>My giden bağlantı istatistikleri nasıl kontrol edilsin mi? 

Başarılı ve başarısız bağlantılarında hacmi SNAT bağlantıları ölçüm açıklar [giden trafik akışları](https://aka.ms/lboutbound).

Başarısız bağlantı hacmi sıfırdan büyük SNAT bağlantı noktası Tükenme gösterir. Bu hatalarına neden belirlemek için daha fazla araştırmak gerekir. SNAT bağlantı noktası Tükenme bildirimleri kurmak için bir hata olarak bir [giden akış](https://aka.ms/lboutbound). Senaryolar ve iş yerindeki mekanizmaları anlamak için nasıl azaltılacağını öğrenmek ve SNAT bağlantı noktası Tükenme önlemek için tasarım giden bağlantılar hakkında makalesini gözden geçirin. 

SNAT bağlantı istatistikleri almak için:
1. seçin **SNAT bağlantıları** ölçüm türü ve **toplam** toplama olarak. 
2. Group by **bağlantı durumu** farklı çizgilerle gösterilir başarılı ve başarısız SNAT bağlantı sayıları. 

![SNAT bağlantı](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Şekil: Yük dengeleyici SNAT bağlantısı sayısı*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>İçin Hizmetimi nasıl gelen/giden bağlantı denemeleri denetleyin?

Birimin gelmedi veya gönderilen TCP Eşitlemeye paketlerin Eşitlemeye paketleri ölçüm açıklar (için [giden trafik akışları](https://aka.ms/lboutbound)), belirli bir ön uç ile ilişkili. Bu ölçüm, hizmete TCP bağlantı denemeleri anlamak için kullanabilirsiniz.

Kullanım **toplam** çoğu senaryo için toplama olarak.

![Eşitlemeye bağlantı](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Şekil: Yük dengeleyici Eşitlemeye sayısı*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>My ağ bant genişliği kullanımını nasıl kontrol edilsin mi? 

Bayt ve paket sayaçları ölçüm birimin bayt ve gönderilen veya başına ön-uç temelinde, hizmeti tarafından alınan paketlerin açıklar.

Kullanım **toplam** çoğu senaryo için toplama olarak.

Bayt veya paket sayısı istatistikleri almak için:
1. Seçin **bayt sayısını** ve/veya **paket sayısı** ölçüm türü ile **Avg** toplama olarak. 
2. Aşağıdakilerden birini yapın:
   * Filtre, belirli ön uç IP, ön uç bağlantı noktası, arka uç IP veya arka uç bağlantı noktası üzerinde uygulayın.
   * Hiçbir filtreleme olmadan yük dengeleyici kaynak genel istatistiklerini alın.

![Bayt sayısı](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Şekil: Yük dengeleyici bayt sayısı*

#### <a name = "vipavailabilityandhealthprobes"></a>My yük dengeleyici dağıtımı nasıl tanılamak?

Tek bir grafikte VIP kullanılabilirlik ve sistem durumu araştırma ölçümleri bir bileşimini kullanarak sorun için bakın ve sorunu çözmek nereye tanımlayabilirsiniz. Azure düzgün çalışmasını güvence edinebilir ve conclusively yapılandırma veya uygulama kök neden olduğunu belirlemek için bu bilgileri kullanın.

Sistem durumu araştırma ölçümleri, nasıl Azure dağıtımınızın sağladığınız yapılandırma göredir sistem görünümleri anlamak için kullanabilirsiniz. Sistem durumu araştırmalarının arayan her zaman izleme veya bir nedeni belirleme bir harika ilk adımdır.

Başka bir adım yapmanıza ve VIP kullanılabilirlik ölçümleri nasıl Azure belirli bir dağıtım için sorumlu olduğu temel alınan veri düzlemi sistem görünümleri içine daha iyi kavramak için kullanın. Her iki ölçümleri birleştirdiğinizde, bu örnekte gösterildiği gibi burada hataya olabilir, ayırabilirsiniz:

![VIP tanılama](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Şekil: Birleştirme DIP ve VIP kullanılabilirlik ölçümleri*

Grafiği aşağıdaki bilgileri görüntüler:
- Altyapı sağlıklı Vm'leriniz barındırma altyapısını ulaşılabilir ve birden fazla VM arka uç yerleştirilir. Bu bilgileri yüzde 100'e VIP kullanılabilirlik mavi izleme tarafından belirtilir. 
- Ancak, araştırma durumu (DIP kullanılabilirlik) grafik başındaki yüzde 0 turuncu izleme tarafından belirtildiği şekilde altındadır. Yeşil vurgular (DIP kullanılabilirlik) durumu sağlıklı, ve Müşteri'nin dağıtım noktada, burada hale geldi daire içinde alanında yeni akışları kabul edemiyor.

Grafik tahmin veya diğer sorunlar ortaya çıkan desteği isteyin zorunda kalmadan kendi başlarına dağıtım sorunlarını giderme olanak tanır. Sistem durumu araştırmalarının yanlış yapılandırılması veya başarısız bir uygulamayı nedeniyle başarısız çünkü hizmet kullanılamaz.

### <a name = "Limitations"></a>Sınırlamaları

VIP kullanılabilirlik yalnızca genel ön uçları için şu anda kullanılabilir değil.

## <a name = "ResourceHealth"></a>Kaynak sistem durumu

Standart yük dengeleyici kaynaklar için sistem durumu varolan aracılığıyla gösterilir **kaynak durumu** altında **İzleyici > Hizmet durumu**.

>[!NOTE]
>Yük Dengeleyici için kaynak sistem durumu şu anda standart yük dengeleyici yapılandırması yalnızca ortak için kullanılabilir. İç yük dengeleyici kaynakları veya temel SKU'ları, yük dengeleyici kaynakları kaynak durumu gösterme.

Ortak standart yük dengeleyici kaynaklarınızın sistem durumunu görüntülemek için:
1. Seçin **İzleyici** > **hizmet sistem durumu**.

   ![İzleme sayfası](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Şekil: Hizmet durumu bağlantıyı Azure İzleyicisi*

2. Seçin **kaynak durumu**ve ardından olduğundan emin olun **abonelik kimliği** ve **kaynak türü yük dengeleyici =** seçilir.

   ![Kaynak sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Şekil: sistem durumu görünümü için kaynağı seçin*

3. Listede, geçmiş sistem durumunu görüntülemek için yük dengeleyici kaynak seçin.

    ![Yük Dengeleyici sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Şekil: Yük dengeleyici kaynak sistem durumu görünümü*
 
Çeşitli kaynak sistem durumları ve açıklamaları aşağıdaki tabloda listelenmiştir: 

| Kaynak sistem durumu | Açıklama |
| --- | --- |
| Kullanılabilir | Genel standart yük dengeleyici kaynak sağlıklı kullanılabilir. |
| Kullanılamaz | Genel standart yük dengeleyici kaynak iyi durumda değil. Seçerek sağlığını tanılamanız **Azure İzleyici** > **ölçümleri**.<br>(*Kullanılamıyor* durum anlamına da kaynak ile genel standart yük dengeleyiciye bağlı değil.) |
| Bilinmiyor | Kaynak sistem durumu, genel standart yük dengeleyici kaynağınız için henüz güncelleştirilmemiş.<br>(*Bilinmeyen* durum anlamına da kaynak ile genel standart yük dengeleyiciye bağlı değil.)  |

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- Daha fazla bilgi edinmek, [yük dengeleyici giden bağlantı](https://aka.ms/lboutbound).


