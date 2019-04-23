---
title: Azure trafik analizi şema | Microsoft Docs
description: Azure ağ güvenlik grubu akış günlüklerini analiz etmek için trafik analizi şemasını anlayın.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: vinigam
ms.openlocfilehash: 246c5256f56fd0b891d4e7d642c421b1e340fc6d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799346"
---
# <a name="schema-and-data-aggregation-in-traffic-analytics"></a>Trafik analizi şema ve veri toplama işlemleri

Trafik analizi, bulut ağlarındaki kullanıcı ve uygulama etkinliğiniz görünürlük sağlayan bir bulut tabanlı bir çözümdür. Trafik analizi, Azure bulut trafik akışını Öngörüler sağlamak için Ağ İzleyicisi ağ güvenlik grubu (NSG) akış günlüklerini analiz eder. Trafik Analizi ile şunları yapabilirsiniz:

- Azure aboneliklerinizde ağ etkinliğini Görselleştirme ve etkin noktaları belirleyin.
- Güvenlik tehditleri belirleyin ve açık bağlantı noktaları, internet erişimi ve sanal ağları standart dışı bağlanmak makineleri (VM) çalışan uygulamalar gibi bilgileri ile ağınızın güvenliğini sağlayın.
- Azure bölgeleri ve performans ve kapasite kendi ağ dağıtımınıza en iyi duruma getirmek için internet üzerinden trafik akış desenlerini anlayın.
- Ağınızda başarısız bağlantılar için önde gelen ağ yanlış yapılandırmalarını saptayın.
- Ağ kullanımı bayt, paketler veya akışlar bildirin.

### <a name="data-aggregation"></a>Veri toplama

1. Tüm "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasında bir NSG akış günlükleri depolama hesabındaki bir dakikalık aralıklarla blobları olarak trafik analizi tarafından işlenmeden önce yakalanır. 
2. Trafik analizi işleme aralığı varsayılan değer 60 dakikadır. Bu, her 60 dakikada trafik analizi için toplama depolama bloblarından seçer anlamına gelir.
3. Akışlar aynı kaynak IP, hedef IP, hedef bağlantı noktası, NSG adı, NSG kuralı, akış yönünü ve Aktarım Protokolü (TCP veya UDP) katman (Not: Kaynak bağlantı noktası için toplama hariç) tek bir akışa göre trafik analizi clubbed
4. Bu tek bir kayıt (bölümde ayrıntıları) düzenlenmiş ve Log analytics'te trafik analizi tarafından alınan.
5. FlowStartTime_t alan işleme aralığı "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasındaki akış günlüğü bu tür bir toplu akış (aynı dört bölütlü) ilk örneğini gösterir. 
6. TA alanındaki herhangi bir kaynağa kullanıcı Arabiriminde belirtilen NSG tarafından görülen toplam akış akışlardır ancak günlük Anlaytics içinde yalnızca tek, azaltılmış kaydı kullanıcı görürsünüz. Tüm akışlar görmek için depolama alanından başvurulabilir blob_id alanı kullanın. Toplam akış sayısı için kayıt blob görülen tek tek akış eşleşir.


### <a name="fields-used-in-traffic-analytics-schema"></a>Trafik analizi şemasında kullanılan alanlar

Trafik Analizi ile donatılmış ve uyarılar aynı veriler üzerinde özel sorgular çalıştırabilmeniz için trafik analizi Log Analytics üzerinde oluşturulmuştur.

Şema ve bunlar geldiğiniz alanlara bazıları aşağıda listelenmiştir

| Alan | Biçimlendir | Yorumlar | 
|:---   |:---    |:---  |
| TableName | AzureNetworkAnalytics_CL | Trafik Anlaytics veri tablosu
| SubType_s | FlowLog | Akış günlükleri için alt tür |
| FASchemaVersion_s |   1   | Scehma sürümü. NSG akış günlüğü sürümü güncel değil |
| TimeProcessed_t   | Tarih ve saat (UTC)  | Trafik analizi depolama hesabından ham akış günlüklerini işlenme süresi |
| FlowIntervalStartTime_t | Tarih ve saat (UTC) |  Akış günlüğü işleme aralığı başlangıç zamanı. Bu akış aralığının ölçüldüğü süredir |
| FlowIntervalEndTime_t | Tarih ve saat (UTC) | Bitiş saati akış günlük işleme aralığı |
| FlowStartTime_t | Tarih ve saat (UTC) |  Akış günlüğü işleme aralığı "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasında ilk geçtiği (hangi toplanan) akış. Bu akış toplama mantığına göre toplanmış |
| FlowEndTime_t | Tarih ve saat (UTC) | "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasındaki akış günlük işleme aralıkta son a geçişi (hangi toplanan) akış. Akış günlüğü v2 açısından, bu alan aynı dört bölütlü son akışı ("B" ham akış kaydında işaretli) başlatıldığı zaman içerir. |
| FlowType_s |  * IntraVNet <br> * InterVNet <br> * S2S <br> * P2S <br> * AzurePublic <br> * ExternalPublic <br> * MaliciousFlow <br> * Bilinmeyen özel <br> * Bilinmiyor | Aşağıdaki tabloda notları tanımında |
| SrcIP_s | Kaynak IP adresi | AzurePublic durumunda boş olur ve ExternalPublic akışlar |
| DestIP_s | Hedef IP adresi | AzurePublic durumunda boş olur ve ExternalPublic akışlar |
| VMIP_s | VM'nin IP'si | AzurePublic ve ExternalPublic akışlar için kullanılan |
| PublicIP_S | Genel IP adresleri | AzurePublic ve ExternalPublic akışlar için kullanılan |
| DestPort_d | Hedef Bağlantı Noktası | Trafiği gelen olan bağlantı noktası | 
| L4Protocol_s  | * T <br> * U  | Aktarım Protokolü. T = TCP <br> U UDP = | 
| L7Protocol_s  | Protokol adı | Hedef bağlantı noktasından türetilmiş |
| FlowDirection_s | * Miyim = gelen<br> * O giden = | Akış günlüğü göre NSG/akış yönünü | 
| FlowStatus_s  | * NSG kuralı tarafından izin verilen a = <br> * D = NSG kuralı tarafından reddedildi  | Akış izin/nblocked tarafından NSG akış günlüğü göre durumu |
| NSGList_s | \<SUBSCRIPTIONID>\/<RESOURCEGROUP_NAME>\/<NSG_NAME> | Ağ güvenlik grubu (NSG) akışı ile ilişkilendirilmiş |
| NSGRules_s | \<Dizin değeri 0) >< NSG_RULENAME >\<akış yönü >\<akış durumu >\<FlowCount ProcessedByRule > |  Bu akış izin verileceğini veya NSG kuralı |
| NSGRuleType_s | * Kullanıcı tanımlı * varsayılan |   NSG akış tarafından kullanılan kural türü |
| MACAddress_s | MAC Adresi | Akış yakalandığı NIC MAC adresi |
| Subscription_s | Abonelik Azure sanal ağ / ağ arabirimi / sanal makine, bu alanda doldurulur | Yalnızca FlowType uygulanabilir S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow ve UnknownPrivate akış türleri (akış türleri yalnızca bir tarafına azure olduğu) = |
| Subscription1_s | Abonelik Kimliği | Abonelik kimliği, sanal ağ / ağ arabirimi / sanal makine kaynak IP akış ait olduğu |
| Subscription2_s | Abonelik Kimliği | Abonelik kimliği, sanal ağ / ağ arabirimi / hedef IP akış ait olduğu sanal makine |
| Region_s | Sanal ağın Azure bölgesi / ağ arabirimi / sanal IP akış ait olduğu makine | Yalnızca FlowType uygulanabilir S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow ve UnknownPrivate akış türleri (akış türleri yalnızca bir tarafına azure olduğu) = |
| Region1_s | Azure Bölgesi | Sanal ağın Azure bölgesi / ağ arabirimi / sanal makine kaynak IP akış ait olduğu |
| Region2_s | Azure Bölgesi | Hedef IP akış ait sanal ağın Azure bölgesi |
| NIC_s | \<resourcegroup_Name>\/\<NetworkInterfaceName> |  NIC gönderme veya alma trafiği VM ile ilişkili |
| NIC1_s | <resourcegroup_Name>/\<NetworkInterfaceName> | Akıştaki kaynak IP ile ilişkili NIC |
| NIC2_s | <resourcegroup_Name>/\<NetworkInterfaceName> | Akış hedef IP ile ilişkili NIC |
| VM_s | <resourcegroup_Name>\/\<NetworkInterfaceName> | Sanal makine NIC_s ağ arabirimi ile ilişkilendirilmiş |
| VM1_s | < resourcegroup_Name > /\<Sourceserver > | Akıştaki kaynak IP ile ilişkili sanal makine |
| VM2_s | < resourcegroup_Name > /\<Sourceserver > | Akış hedef IP ile ilişkili sanal makine |
| Subnet_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | NIC_s ile ilişkili alt ağ |
| Subnet1_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Akıştaki kaynak IP ile ilişkili alt ağ |
| Subnet2_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName>    | Akış hedef IP ile ilişkili alt ağ |
| ApplicationGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<ApplicationGatewayName> | Akıştaki kaynak IP ile ilişkili uygulama ağ geçidi | 
| ApplicationGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<ApplicationGatewayName> | Akış hedef IP ile ilişkili uygulama ağ geçidi |
| LoadBalancer1_s | \<Subscriptionıd > /\<ResourceGroupName > /\<LoadBalancerName > | Yük Dengeleyici kaynak IP akış ile ilişkili |
| LoadBalancer2_s | \<Subscriptionıd > /\<ResourceGroupName > /\<LoadBalancerName > | Yük Dengeleyici akışındaki hedef IP ile ilişkili |
| LocalNetworkGateway1_s | \<Subscriptionıd > /\<ResourceGroupName > /\<LocalNetworkGatewayName > | Yerel ağ geçidi akıştaki kaynak IP ile ilişkili |
| LocalNetworkGateway2_s | \<Subscriptionıd > /\<ResourceGroupName > /\<LocalNetworkGatewayName > | Yerel ağ geçidi akışındaki hedef IP ile ilişkili |
| ConnectionType_s | Olası değerler şunlardır: VNetPeering VpnGateway ve ExpressRoute |    Bağlantı Türü |
| ConnectionName_s | \<SubscriptionID>/\<ResourceGroupName>/\<ConnectionName> | Bağlantı Adı |
| ConnectingVNets_s | Sanal ağ adlarının boşluklarla ayrılmış listesi | Hub ve bağlı bileşen topolojisi olması durumunda, hub sanal ağları burada doldurulur |
| Country_s | İki harfli ülke kodu (ISO 3166 1 alfa-2) | Akış türü için ExternalPublic doldurulur. Tüm IP adresleri PublicIPs_s alanında aynı ülke kodunda paylaşır |
| AzureRegion_s | Azure bölgesi Konumlar | Akış türü için AzurePublic doldurulur. Tüm IP adresleri PublicIPs_s alanında Azure bölgesi paylaşır. |
| AllowedInFlows_d | | İzni olan gelen akışlar sayısı. Bu aynı dört bölütlü paylaşılan akış sayısını temsil eder, akış yakalanan netweork arabirimine gelen | 
| DeniedInFlows_d |  | Reddedildi gelen akışlar sayısı. (Akış yakalandığı ağ arabirimine gelen) |
| AllowedOutFlows_d | | Sayısı (giden akış yakalandığı ağ arabirimine) izni olan giden akışlar |
| DeniedOutFlows_d  | | Sayısı (giden akış yakalandığı ağ arabirimine) reddedildi giden akışlar |
| FlowCount_d | Kullanım dışı. Aynı dört bölütlü eşleşen toplam akış. Akış türleri olması durumunda ExternalPublic ve AzurePublic sayısı akışları çeşitli Publicıp adresleri de dahil edilir.
| InboundPackets_d | NSG kuralı uygulandığı ağ arabiriminin yakalanan olarak alınan paket sayısı | Bu yalnızca, sürüm 2 NSG akış günlüğü şeması doldurulur |
| OutboundPackets_d  | NSG kuralı uygulandığı ağ arabiriminin yakalanan olarak gönderilen paketleri | Bu yalnızca, sürüm 2 NSG akış günlüğü şeması doldurulur |
| InboundBytes_d |  NSG kuralı uygulandığı ağ arabiriminin yakalanan olarak alınan bayt sayısı | Bu yalnızca, sürüm 2 NSG akış günlüğü şeması doldurulur |
| OutboundBytes_d | NSG kuralı uygulandığı ağ arabiriminin yakalanan olarak gönderilen bayt sayısı | Bu yalnızca, sürüm 2 NSG akış günlüğü şeması doldurulur |
| CompletedFlows_d  |  | Bu yalnızca, sürüm 2 NSG akış günlüğü şeması için sıfır değeri ile doldurulur |
| PublicIPs_s | &LT; PUBLIC_IP &GT;\|\<FLOW_STARTED_COUNT &GT;\|\<FLOW_ENDED_COUNT &GT;\|\<OUTBOUND_PACKETS &GT;\|\<INBOUND_PACKETS &GT;\| \<OUTBOUND_BYTES &GT;\|\<INBOUND_BYTES &GT; | Giriş dikey çubuklar |
    
### <a name="notes"></a>Notlar
    
1. AzurePublic ve ExternalPublic akışlar olması durumunda, müşteri PublicIPs_s alanında genel IP adresleri dolduruluyor sırada Azure VM IP VMIP_s alanında doldurulur ait. Bu iki akış türleri için biz SrcIP_s ve DestIP_s alanları yerine VMIP_s ve PublicIPs_s kullanmalıdır. Müşteri log analytics çalışma alanı için alınan kayıt sayısı çok az olmasını AzurePublic ve ExternalPublicIP adresleri için size daha iyi, toplayın. (Bu alan yakında kullanımdan kaldırılacak ve biz SrcIP_ ve DestIP_s azure VM kaynak veya hedef akışın olmasına bağlı olarak kullanıyor olması gerekir) 
1. Akış türleri için Ayrıntılar: Akışta kullanılan IP adreslerine bağlı olarak, biz akışlarında aşağıdaki akış türleri için kategorilere ayırın: 
1. IntraVNet – akıştaki her iki IP adreslerini aynı Azure sanal ağında bulunur. 
1. Sanal ağlar arası - IP adresleri akış iki farklı Azure sanal ağ içinde yer alır. 
1. Bir IP adresi, Azure sanal ağ VPN ağ geçidinden veya Expressroute bağlı müşteri ağ (Site) ait olduğu sırada S2S – IP adreslerinin (siteden siteye) bir Azure sanal ağına ait. 
1. Bir IP adresi, Azure sanal ağ VPN ağ geçidi üzerinden bağlı müşteri ağ (Site) ait olduğu sırada IP adreslerinden birini Azure sanal ağına ait P2S - (noktadan siteye).
1. Bir IP adresi Microsoft'a ait Azure iç genel IP adreslerine ait olduğu sırada AzurePublic - IP adreslerinden birini Azure sanal ağına ait. Müşterinin sahip olduğu genel IP adresleri, bu akış türü bir parçası olmayacaktır. VM trafik gönderen bir Azure hizmetine (depolama uç noktası), bu akış türü altında kategorilere örneği için tüm müşterilere ait. 
1. ExternalPublic - IP adreslerinden birini ait Azure sanal ağ için bir IP adresi Azure'da değil bir genel IP olsa da, kötü amaçlı trafik analizi işleme aralığını için kullandığı ASC akışlardaki olarak bildirilmedi " FlowIntervalStartTime_t"ve"FlowIntervalEndTime_t". 
1. MaliciousFlow - IP adreslerinden birini ait azure sanal ağına Azure'da değil ve kötü amaçlı trafik analizi işleme aralığını için kullandığı ASC akışlardaki olarak bildirilen bir genel IP olsa da bir IP adresi" FlowIntervalStartTime_t"ve"FlowIntervalEndTime_t". 
1. UnknownPrivate - IP adreslerinden birini ait Azure sanal ağ için bir IP adresi RFC 1918 ' tanımlanan özel IP aralığına ait ve site veya Azure sanal ağına ait bir müşteriye tarafından trafik analizi eşleştirilemedi.
1. Bilinmiyor – IP birini eşleme yapılamıyor azure'da müşteri topolojisi ile akışlarında adresleri yanı sıra şirket içinde (site).

### <a name="next-steps"></a>Sonraki Adımlar
Sık sorulan sorulara yanıtlar almak için bkz. [trafik Analizi ile ilgili SSS](traffic-analytics-faq.md) işlevleri hakkında daha fazla ayrıntı için bkz: [trafik analizi belgeleri](traffic-analytics.md)
    


    


