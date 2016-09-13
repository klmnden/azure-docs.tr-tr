<properties
    pageTitle="Service Bus geçişine genel bakış | Microsoft Azure"
    description="Service Bus geçişine genel bakış."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>


# Service Bus geçişine genel bakış

Service Bus'ın temel bir bileşeni, hem Azure veri merkezinde hem de şirket içi kurumsal ortamınızda çalışan karma uygulamalar oluşturmanıza olanak sağlayan merkezi (ancak yüksek düzeyde yük dengeleme olanağı sunan) *geçiş* hizmetidir.  Service Bus geçişi birçok çeşitli aktarım protokolünü ve web hizmeti standartlarını destekler. Bunlar; SOAP, WS-* ve hatta REST'i bile içerir. Geçiş hizmeti karma uygulamalarınızı kolaylaştırma işlemini, güvenlik duvarı bağlantısını açmanıza veya kurumsal ağ altyapısına müdahale eden değişiklikler yapmanıza gerek kalmadan kurumsal işletme ağında bulunan Windows Communication Foundation (WCF) hizmetlerini genel bulutta kullanıma sunmanıza olanak sağlayarak gerçekleştirir. 

![Geçiş Kavramları](./media/service-bus-relay-overview/sb-relay-01.png)

Geçiş hizmeti, geleneksel tek yönlü mesajlaşmayı, istek/yanıt mesajlaşmasını ve eşdüzey mesajlaşmayı destekler. Ayrıca, artırılmış uçtan uca verimliliğe yönelik çift yönlü yuva iletişimi ile yayımla ve abone ol senaryolarına olanak sağlamak için internet kapsamında olay dağıtımını destekler. 

Geçişli mesajlaşma düzeninde bir şirket içi hizmet, giden bağlantı noktası aracılığıyla geçiş hizmetine bağlanır ve belirli bir randevu adresine bağlanan iletişim için çift yönlü yuva oluşturur. Ardından istemci, randevu adresini hedef alan geçiş hizmetine iletiler göndererek şirket içi hizmet ile iletişim kurabilir. Böylece geçiş hizmeti, zaten kullanımda olan çift yönlü yuva aracılığıyla iletileri şirket içi hizmete "geçirir". İstemcinin, şirket içi hizmete doğrudan bağlantısının olmasına veya hizmetin nerede bulunduğunu bilmesine gerek yoktur. Ayrıca, şirket içi hizmet için güvenlik duvarında gelen bağlantı noktalarının açık olması gerekmez.

WCF "geçiş" bağlamalarının bir paketini kullanarak geçiş hizmeti ile şirket içi hizmetiniz arasındaki bağlantıyı başlatırsınız. Arka planda ise geçiş bağlamaları, bulutta Service Bus ile tümleşen WCF kanalı bileşenlerini oluşturmak üzere tasarlanan yeni aktarım bağlama öğeleriyle eşleşir. 

## Sonraki adımlar

Service Bus geçişi hakkındaki ayrıntılar için aşağıdaki konu başlıklarına bakın.

- [Azure Service Bus Mimarisine Genel Bakış](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus Geçişi hizmetini kullanma](service-bus-dotnet-how-to-use-relay.md)

 


<!--HONumber=sep16_HO1-->


