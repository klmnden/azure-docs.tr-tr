<properties 
    pageTitle="Service Bus mimarisi | Microsoft Azure"
    description="Azure Service Bus hizmetinin ileti işleme mimarisini açıklar."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="04/19/2016"
    ms.author="sethm" />

# Service Bus mimarisi

Bu makale, Azure Service Bus hizmetinin ileti işleme mimarisini açıklar.

## Service Bus ölçek birimleri

Service Bus, *ölçek birimleri* tarafından düzenlenir. Ölçek birimi, bir dağıtım birimidir ve hizmeti çalıştırmak için gerekli tüm bileşenleri içerir. Her bölge, bir veya daha fazla Service Bus ölçek birimi dağıtır.

Service Bus ad alanı, bir ölçek birimi ile eşleştirilir. Ölçek birimi, Service Bus varlıklarının her türünü işler: Geçişler ve aracılı mesajlaşma varlıkları (kuyruklar, konu başlıkları, abonelikler). Service Bus ölçek birimi, şu bileşenlerden oluşur:

- **Ağ geçidi düğümleri kümesi.** Ağ geçidi düğümleri, gelen isteklerin kimliklerini doğrular ve geçiş isteklerini işler. Her ağ geçidi düğümünün genel bir IP adresi vardır.

- **Mesajlaşma aracısı düğümleri kümesi.** Mesajlaşma aracısı düğümleri, mesajlaşma varlıkları ile ilgili istekleri işler.

- **Bir ağ geçidi deposu.** Ağ geçidi deposu, bu ölçek biriminde tanımlanan her varlık için veriler tutar. Ağ geçidi deposu, SQL Azure veritabanının üst kısmında uygulanır.

- **Birden çok mesajlaşma deposu.** Mesajlaşma depoları, bu ölçek biriminde tanımlanan tüm kuyrukların, konu başlıklarının ve aboneliklerin iletilerini içerir. Ayrıca, tüm abonelik verilerini de içerir. [Bölümlenmiş mesajlaşma varlıkları](service-bus-partitioning.md) etkinleştirilmediği sürece kuyruk veya konu başlığı, bir mesajlaşma deposuna eşlenir. Abonelikler, üst konu başlıklarıyla aynı mesajlaşma deposunda depolanır. Service Bus [Premium Mesajlaşma](service-bus-premium-messaging.md) dışındaki mesajlaşma depoları, SQL Azure veritabanının üst kısmında uygulanır.

## Kapsayıcılar

Her mesajlaşma varlığı, belirli bir kapsayıcıya atanır. Kapsayıcı, kendisiyle ilgili tüm verileri depolamak için tam olarak bir mesajlaşma deposu kullanan mantıksal bir yapıdır. Her kapsayıcı, bir mesajlaşma aracısı düğümüne atanır. Genellikle, mesajlaşma aracısı düğümlerinden daha fazla sayıda kapsayıcı vardır. Bu nedenle, her mesajlaşma aracısı düğümü birden çok kapsayıcı yükler. Kapsayıcıların bir mesajlaşma aracısı düğümüne dağıtımı, tüm mesajlaşma aracısı düğümlerine eşit olarak yüklenecek şekilde düzenlenmiştir. Yük düzeni değişirse (örneğin, kapsayıcılardan biri çok meşgul olursa) veya bir mesajlaşma aracısı düğümü geçici olarak kullanılamazsa kapsayıcılar mesajlaşma aracısı düğümleri arasında yeniden dağıtılır.

## Gelen mesajlaşma isteklerinin işlenmesi

Bir istemci, Service Bus hizmetine istek gönderdiğinde Azure yük dengeleyici bu isteği ağ geçidi düğümlerinden herhangi birine yönlendirir. Ağ geçidi düğümü, isteği yetkilendirir. İstek, mesajlaşma varlığıyla (kuyruk, konu başlığı, abonelik) ilgiliyse ağ geçidi düğümü, ağ geçidi deposunda varlığı arar ve varlığın hangi mesajlaşma deposunda bulunduğunu belirler. Daha sonra, şu anda bu kapsayıcıya hizmet veren mesajlaşma aracısı düğümünü arar ve o mesajlaşma aracısı düğümüne istek gönderir. Mesajlaşma aracısı düğümü, isteği işler ve kapsayıcı deposundaki varlık durumunu güncelleştirir. Ardından mesajlaşma aracısı düğümü, orijinal talebi sağlayan istemciye uygun yanıtı gönderen ağ geçidi düğümüne isteği geri gönderir.

![Gelen Mesajlaşma İsteklerinin İşlenmesi](./media/service-bus-architecture/IC690644.png)

## Gelen geçiş isteklerinin işlenmesi

Bir istemci, Service Bus hizmetine istek gönderdiğinde Azure yük dengeleyici bu isteği ağ geçidi düğümlerinden herhangi birine yönlendirir. İstek, bir dinleme isteğiyse ağ geçidi düğümü yeni bir geçiş oluşturur. İstek, belirli bir geçiş için bağlantı isteğiyse ağ geçidi düğümü, geçişe sahip olan ağ geçidi düğümüne bağlantı isteğini iletir. Geçişe sahip ağ geçidi dinleyen istemciye bir randevu isteği göndererek dinleyiciden bağlantı isteğini alan ağ geçidi düğümüne geçici bir kanal kurmasını ister.

Geçiş bağlantısı kurulduğunda istemciler, randevu için kullanılan ağ geçidi düğümü aracılığıyla iletileri değiştirebilir.

![Gelen Geçiş İsteklerinin İşlenmesi](./media/service-bus-architecture/IC690645.png)

## Sonraki adımlar

Service Bus hizmetinin nasıl çalıştığına ilişkin genel bakışı okuduğunuza göre, aşağıdaki bağlantıları ziyaret edebilirsiniz:

- [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
- [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus kuyruklarını kullanan kuyruğa alınan mesajlaşma çözümü](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)



<!----HONumber=Jun16_HO2-->


