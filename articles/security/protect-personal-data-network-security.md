---
title: "Azure ağı güvenlik özellikleri ile kişisel verileri koruma | Microsoft Docs"
description: "Azure ağı güvenlik özellikleri kullanarak kişisel verileri koruyun. Bu genel veri koruma düzenleme (GDPR) ile uyum sağlamak için kullanılabilir"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 54aeb22a501e248105931df341d23e524448155a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Ağ güvenlik özellikleri ile kişisel verileri koruma: Azure uygulama ağ geçidi ve ağ güvenlik grupları

Bu makalede bilgiler sağlar ve yardımcı olacak yordamlar kişisel verilerinizi korumak için Azure uygulama ağ geçidi ve ağ güvenlik grupları kullanın. Bu bilgiler genel veri koruma düzenleme (GDPR) ile uyumlu çabalarına yardımcı olabilir.

Kişisel verilerin gizliliği korumak için çok katmanlı güvenlik stratejisinin önemli bir öğedeki SQL ekleme veya siteler arası komut dosyası gibi ortak güvenlik açıklarına karşı savunma ' dir. İstenmeyen ağ trafiğini tutulması dışında Azure sanal ağ hassas verileri olası riski belirtebilen karşı korur ve Microsoft Azure, verilerinizi saldırganlara karşı korumaya yardımcı olan araçlar sağlar.

## <a name="scenario"></a>Senaryo

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, masraflarını Akdeniz, Adriatic ve Baltık seas yanı sıra İngiliz Adaları arasında içinde sunmaya işlemlerini genişletmektedir. İçinde furtherance Bu çalışmalarınızı İtalya, Almanya, Danimarka ve İngiltere dayanarak birkaç küçük seyahat satırları aldı

Şirket Microsoft Azure bulutta Kurumsal verileri depolayabildiği ve sanal makineler üzerinde işleyen ve bu veri erişimi uygulamaları çalıştırmak için kullanır. Bu veriler, adlar, adresler, telefon numaraları gibi kişisel olarak tanımlanabilir bilgileri ve genel müşteri tabanı kredi kartı bilgilerini içerir. Ayrıca tüm konumlarda adresleri, telefon numaralarını, vergi kimlik numaraları ve şirket çalışanlar hakkında diğer bilgiler gibi geleneksel İnsan Kaynakları bilgileri içerir. Seyahat satır de geçerli ve geçmiş müşterilerle ilişkilerini izlemek için kişisel bilgi içeren büyük bir veritabanı ödül ve bağlılık programı üyelerinin tutar.

Şirket çalışanları şirketin şubelere ağ erişim ve seyahat tüm dünyada bulunan aracıları şirket kaynaklarına erişimi ve Azure Vm'lerde barındırılan web tabanlı uygulamalar ile etkileşim için kullanabilirsiniz.

## <a name="problem-statement"></a>Sorun bildirimi

Şirket müşterilerin gizliliğini korumak gerekir ve kişisel veriler maruz bırakabileceğinden kötü amaçlı kod çalıştırabilir yazılım güvenlik açıklarından yararlanma saldırganlar çalışanların kişisel verileri depolanan veya şirketin bulut tabanlı uygulamalar tarafından kullanılan.

## <a name="company-goal"></a>Şirket hedefi

Yetkisiz kişilerin şirket Azure sanal ağlar ve uygulama ve ortak güvenlik açıkları yararlanarak var. bulunan veri erişememesini sağlamak için şirketin hedefi. 

## <a name="solutions"></a>Çözümler

Microsoft Azure istenmeyen trafiği Azure sanal ağlar geçmesini önlemeye yardımcı olmak amacıyla güvenlik mekanizmaları sağlar. Gelen ve giden trafik, geleneksel güvenlik duvarları tarafından gerçekleştirilir. Azure içinde basit dağıtılmış güvenlik duvarı olarak hareket Web uygulaması güvenlik duvarı ve ağ güvenlik grupları (NSG) ile uygulama ağ geçidi kullanabilirsiniz. Bu araçlar, istenmeyen ağ trafiğin algılanmasına ve engellenmesine olanak tanır.

### <a name="application-gatewayweb-application-firewall"></a>Uygulama ağ geçidi/Web uygulaması güvenlik duvarı

[Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF) bileşeninin [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) giderek ortak bilinen açıklarından kötü amaçlı saldırıları hedefleri olan web uygulamaları korur güvenlik açıkları. Merkezi WAF hem web saldırılarına karşı korur ve uygulama değişiklikleri gerek kalmadan güvenlik yönetimini basitleştirir.

Azure WAF SQL ekleme, siteler arası komut dosyası, HTTP protokolü ihlali ve anormallikleri, bot, gezginleri, tarayıcılar, ortak uygulama yapılandırma hataları, HTTP hizmet reddi ve diğer ortak saldırıları gibi dahil olmak üzere çeşitli saldırı kategorilerini adresleri komut ekleme işlemi, HTTP Kaçakçılığı, HTTP yanıt bölme ve uzak dosya ekleme saldırıları isteyin. 

Bir uygulama ağ geçidi ile WAF oluşturun veya var olan bir uygulama ağ geçidi WAF ekleyin. Her iki durumda da, kendi alt Azure uygulama ağ geçidi gerektirir.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>WAF ile nasıl bir uygulama ağ geçidi oluşturulsun mu? 

Etkin WAF ile yeni bir uygulama ağ geçidi oluşturmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Seçin **kaynak oluşturma** > **ağ** > **uygulama ağ geçidi**.
5. İçinde **Temelleri** görünür, dikey penceresinde, aşağıdaki alanlar için değerleri girin: ad, katmanı (standart veya WAF), SKU boyutunu (küçük, Orta veya büyük) örnek sayısını (2), yüksek kullanılabilirlik için abonelik, kaynak grubunu ve konumu.
6. İçinde **ayarları** altında görüntülenen dikey **sanal ağ**, tıklatın **sanal ağ seçin**. Bu adımı açılır Seç sanal ağ dikey girin.

7. Tıklatın **Yeni Oluştur** açmak için **sanal ağ oluştur** dikey.

8. Aşağıdaki değerleri girin: ad, adres alanı, alt ağ adı, alt ağ adres aralığı. **Tamam**’a tıklayın.

9. Üzerinde **ayarları** altında dikey **ön uç IP yapılandırmasını**, IP adres türünü seçin.

10. Tıklatın **genel bir IP adresi seçin** sonra **yeni oluştur.**

11. Varsayılan değerini kabul edin ve tıklatın **Tamam.**

12. Üzerinde **ayarları** altında dikey **dinleyici Yapılandırması**, HTTP veya HTTPS altında kullanmak için seçin **Protokolü**. HTTPS kullanmak üzere bir sertifika gereklidir.

13. WAF belirli ayarlarını yapılandırabilirsiniz: **güvenlik duvarı durumu** (**etkin**) ve **güvenlik duvarı modu** (**önleme**). Seçerseniz **algılama** modu olarak trafiği yalnızca günlüğe kaydedilir.

14. Gözden geçirme **Özet** sayfasında ve tıklayın **Tamam**. Artık uygulama ağ geçidi sıraya ve oluşturuldu.

Uygulama ağ geçidi oluşturulduktan sonra portalda gitmek ve uygulama ağ geçidi yapılandırmasını devam edebilirsiniz.

![oluşturulan uygulama ağ geçidi](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-to-an-existing-application"></a>Varolan bir uygulamaya nasıl WAF eklensin mi?

WAF önleme modunda desteklemek için varolan uygulama ağ geçidi güncelleştirmek için aşağıdakileri yapın:

1. Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’a tıklayın.

2. Mevcut uygulama ağ geçidi tıklatın **tüm kaynakları** dikey. 
>[!NOTE]
Not: zaten seçili abonelik çeşitli kaynaklar varsa, ad filtrede adıyla girebileceğiniz... girebilirsiniz.
3. Tıklatın **Web uygulaması güvenlik duvarı** ve uygulama ağ geçidi ayarlarını güncelleştirin: **yükseltme WAF katmanına** (işaretli) **güvenlik duvarı durumu** (etkin)  **Güvenlik Duvarı modu** (önleme). Kural kümesini yapılandırmak ve devre dışı kurallarını yapılandırmak gerekir.

Yeni bir uygulama ağ geçidi WAF ve mevcut bir uygulama ağ geçidi için WAF ekleme ile nasıl oluşturulacağı hakkında daha ayrıntılı bilgi için bkz: [portalını kullanarak bir uygulama ağ geçidi ile web uygulaması güvenlik duvarı oluşturun.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

A [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) izin veren veya reddeden ağ trafiği için bağlı kaynaklar için güvenlik kuralları listesini içeren [Azure sanal ağlar](https://docs.microsoft.com/azure/virtual-network/) (VNet). Nsg'ler alt ağları veya tek tek sanal makineleri için ilişkili olabilir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Bir NSG’nin bir VM veya ağ arabirimi ile ilişkilendirilmesi yoluyla da trafik kısıtlanabilir.

Nsg'ler dört özellikleri içerir: ad, bölge, kaynak grubu ve kuralları.
>[!Note]
Bir NSG bir kaynak grubunda mevcut olsa da, kaynağın NSG'nin ait olduğu Azure bölgesinin bir parçası olması koşuluyla, NSG herhangi bir kaynak grubuyla ilişkilendirilebilir.

NSG kuralları dokuz özellikler içeriyor: adı, Protokolü (TCP, UDP veya \*, ICMP yanı sıra UDP ve TCP içerir), kaynak bağlantı noktası aralığı, hedef bağlantı noktası aralığı, kaynak adres öneki, hedef adres ön eki, yönünü (gelen veya giden) öncelik ( 100 ile 4096 arasında) ve erişim türüne (izin verme veya reddetme). Tüm Nsg'ler silinmiş veya oluşturduğunuz kurallar tarafından geçersiz kılınmış varsayılan kurallar kümesini içerir.

#### <a name="how-do-i-implement-nsgs"></a>Nsg'ler nasıl uygulansın mı?

Nsg'leri uygulamadan planlama gerektirir ve dikkate almanız gereken birkaç tasarım konuları vardır. Abonelik ve başına NSG kuralları başına Nsg'ler sayısını sınırlandırır bunlar; VNet ve alt ağ tasarımı, özel kurallar, ICMP trafiğine, alt ağları, yük Dengeleyiciler ve daha fazlasını katmanlarıyla yalıtım.

Planlama ve uygulama Nsg'ler ve bir örnek dağıtım senaryosu, daha fazla yönergeler için bkz [filtre ağ güvenlik grupları ile ağ trafiği.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Bir NSG'yi nasıl kurallar oluşturulsun mu?

Gelen kuralları var olan bir NSG'de oluşturmak için aşağıdakileri yapın:

1. Tıklatın **tüm hizmetleri**ve ardından **ağ güvenlik grupları**.

2. Nsg'ler listesinde tıklatın **NSG ön uç**ve ardından **gelen güvenlik kuralları.**

3. Gelen güvenlik kuralları listesinde tıklatın **Ekle.**

4. Aşağıdaki alanlara değerleri girin: ad, öncelik, kaynak, protokol, kaynak aralığı, hedef, hedef bağlantı noktası aralığı ve eylem.

Yeni Kural NSG'de birkaç saniye sonra görünür.

![Ağ güvenlik kuralları](media/protect-netsec/inbound-security.png)

Nsg'ler alt ağlarında oluşturma hakkında daha fazla yönerge kurallar oluşturabilir ve bir NSG bir ön uç ve arka uç alt ağı ile ilişkilendirmek için bkz: [Azure Portalı'nı kullanarak ağ güvenlik grupları oluşturun.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)

## <a name="next-steps"></a>Sonraki adımlar

[Azure ağ güvenliği](https://azure.microsoft.com/blog/azure-network-security/)

[Azure Ağ Güvenliği İçin En İyi Uygulamalar](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[Ağ güvenlik grubu hakkında bilgi edinin](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
