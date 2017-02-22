---
title: "Azure Geçiş nedir ve neden kullanılır? - Genel bakış | Microsoft Docs"
description: "Azure Geçiş’e Genel Bakış"
services: service-bus-relay
documentationcenter: .net
author: banisadr
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/14/2017
ms.author: babanisa;sethm
translationtype: Human Translation
ms.sourcegitcommit: ca66a344ea855f561ead082091c6941540b1839d
ms.openlocfilehash: 1b9423c22ff6e003a6236e01118b600a2c746db4


---
# <a name="what-is-azure-relay"></a>Azure Geçiş nedir?
Azure Geçiş hizmeti, güvenlik duvarı bağlantısını açmanıza veya kurumsal ağ altyapısına müdahale eden değişiklikler yapmanıza gerek kalmadan kurumsal işletme ağında bulunan hizmetleri genel bulutta kullanıma sunmanıza olanak sağlayarak karma uygulamaları kolaylaştırır. Geçiş, birçok çeşitli aktarım protokolünü ve web hizmeti standartlarını destekler.

Geçiş hizmeti, geleneksel tek yönlü trafiği, istek/yanıt trafiğini ve eşler arası trafiği destekler. Ayrıca, artırılmış uçtan uca verimliliğe yönelik çift yönlü yuva iletişimi ile yayımla ve abone ol senaryolarına olanak sağlamak için internet kapsamında olay dağıtımını destekler. 

Geçişli veri aktarımı deseninde bir şirket içi hizmet, giden bağlantı noktası aracılığıyla geçiş hizmetine bağlanır ve belirli bir randevu adresine bağlanan iletişim için çift yönlü yuva oluşturur. Ardından istemci, randevu adresini hedef alan geçiş hizmetine trafik göndererek şirket içi hizmet ile iletişim kurabilir. Geçiş hizmeti daha sonra her bir istemciye ayrılmış çift yönlü bir yuva aracılığıyla verileri şirket içi hizmete "geçirir". İstemcinin, şirket içi hizmete doğrudan bağlantısının olmasına veya hizmetin nerede bulunduğunu bilmesine gerek yoktur. Ayrıca, şirket içi hizmet için güvenlik duvarında gelen bağlantı noktalarının açık olması gerekmez.

Geçiş tarafından sağlanan önemli özellik öğeleri, TCP gibi azaltma, uç nokta bulma, bağlantı durumu ve kaplanmış uç nokta güvenliği ile ağ sınırları arasında çift yönlü, ara belleksiz iletişimdir. Geçiş özellikleri, VPN gibi ağ düzeyinde tümleştirme teknolojilerinden farklıdır; geçiş tek bir makinedeki tek bir uygulama uç noktasını kapsayabilirken, VPN teknolojisi ağ ortamını değiştirmeye bağlı olduğundan çok daha kapsayıcıdır.

Azure Geçiş iki özelliğe sahiptir:

1. [Karma Bağlantılar](#hybrid-connections) - Çok platformlu senaryoları etkinleştiren açık standart web yuvalarını kullanır.
2. [WCF Geçişleri](#wcf-relays) - Windows Communication Foundation’ı (WCF) kullanarak uzak yordam çağrılarını etkinleştirir. WCF Geçişi, birçok müşterinin WCF programlama modelleriyle zaten kullanıyor olabileceği eski Geçiş teklifidir.

Hem Karma Bağlantılar hem de WCF Geçişleri bir kurumsal kuruluş ağı içinde bulunan varlıklara güvenli bağlantı olanağı sağlar. Hangisinin diğerine tercih edileceği, aşağıdaki tabloda açıklandığı gibi özel gereksinimlerinize bağlıdır:

|  | WCF Geçişi | Karma Bağlantılar |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS*** | |x |
| **Java*** | |x |
| **Standart Tabanlı Açık Protokol** | |x |
| **Birden Çok RPC Programlama Modeli** | |x |

*Genel Erişilebilirlik tarihinde

## <a name="hybrid-connections"></a>Karma Bağlantılar
[Azure Geçiş Karma Bağlantılar](relay-hybrid-connections-protocol.md) özelliği, yaygın olarak kullanılan web tarayıcılarındaki WebSocket API’yi açıkça içeren temel bir WebSocket özelliğine sahip tüm platform ve dillerde uygulanabilen mevcut Geçiş özelliklerinin güvenli, açık yordam kullanılarak evrim geçirmiş bir halidir. Karma Bağlantılar HTTP ve WebSocket’ları temel alır.

## <a name="wcf-relays"></a>WCF Geçişleri
WCF Geçişi tam .NET Framework (NETFX) ve WCF için çalışır. Geçiş hizmeti ile şirket içi hizmetiniz arasındaki bağlantıyı bir WCF "geçiş" bağlamaları paketi kullanarak başlatırsınız. Arka planda ise geçiş bağlamaları, bulutta Service Bus ile tümleşen WCF kanalı bileşenlerini oluşturmak üzere tasarlanan yeni aktarım bağlama öğeleriyle eşleşir.

## <a name="service-history"></a>Hizmet geçmişi
Karma Bağlantılar, Azure Service Bus WCF Geçişi üzerinde oluşturulan eski ve benzer ada sahip "BizTalk Services" özelliğini tamamlar. Yeni Karma Bağlantılar özelliği mevcut WCF Geçişi özelliğini tamamlar ve bu iki hizmet özelliği şimdilik Geçiş hizmetinde beraber bulunur. Ortak bir ağ geçidine sahip bu iki özellik, diğer açılardan farklı olan uygulamalardır.

## <a name="next-steps"></a>Sonraki adımlar:
* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Jan17_HO4-->


