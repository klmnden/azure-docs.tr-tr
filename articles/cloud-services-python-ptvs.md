<properties
    pageTitle="Python web ve çalışan rolleri içeren Visual Studio için Python Araçları 2.2 | Microsoft Azure"
    description="Web rolleri ve çalışan rolleri dahil olmak üzere Azure Cloud Services oluşturmak üzere Visual Studio için Python Araçları’nı kullanma hakkında genel bilgi edinin."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/30/2015"
    ms.author="adegeo"/>




# Python web ve çalışan rolleri içeren Visual Studio için Python Araçları 2.2

Bu makalede, [Visual Studio için Python Araçları][] ile Python web ve çalışan rollerini kullanmaya genel bir bakış sunulmuştur.

## Ön koşullar

 - Visual Studio 2013 veya 2015
 - [Visual Studio için Python Araçları 2.2][] (PTVS)
 - [VS 2013 için Azure SDK Araçları][] veya [VS 2015 için Azure SDK Araçları][]
 - [Python 2.7 32 bit][] veya [Python 3.4 32 bit][]

[AZURE.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## Python web ve çalışan rolleri nelerdir?

Azure uygulamaları çalıştırmak üzere üç işlem modeli sunar: [Azure App Service için Web Apps][execution model-web sites], [Azure Virtual Machines][execution model-vms], ve [Azure Cloud Services][execution model-cloud services]. Python bu üç modeli de destekler. Web ve çalışan rolleri içeren Cloud Services *Hizmet Olarak Platform (PaaS)* sunar. Web rolü, bir bulut hizmetinde ön uç web uygulamalarını barındırmak için özel Internet Information Services (IIS) web sunucusu sağlar. Çalışan rolü ise kullanıcı etkileşimi ve girişinden bağımsız zaman uyumsuz, uzun çalışan ve kalıcı görevleri çalıştırabilir.

Daha fazla bilgi için bkz. [Bulut Hizmeti nedir?].

> [AZURE.NOTE] *Basit bir web sitesi tasarlamak mı istiyorsunuz?*
Senaryonuz yalnızca basit bir web sitesi ön ucu içeriyorsa, Azure App Service’teki basit Web Apps özelliğini kullanmayı düşünün. Web siteniz büyüdükçe ve gereksinimleriniz değiştikçe kolayca Bulut Hizmetleri’ne yükseltebilirsiniz. Azure App Service’teki Web Apps özelliğini geliştirme hakkındaki makaleler için <a href="/develop/python/">Python Geliştirici Merkezi</a>’ne bakın.
<br />


## Proje oluşturma

Visual Studio’da, **Python** altındaki **Yeni Proje** iletişim kutusunda **Azure Bulut Hizmeti**’ni seçebilirsiniz.

![Yeni Proje İletişim Kutusu](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Azure Bulut Hizmeti sihirbazında, yeni web ve çalışan rolleri oluşturabilirsiniz.

![Azure Bulut Hizmeti İletişim Kutusu](./media/cloud-services-python-ptvs/new-service-wizard.png)

Çalışan rolü şablonu, Azure Storage hesabına veya Azure Service Bus’a bağlamak üzere demirbaş kod ile birlikte gelir.

![Bulut Hizmeti Çözümü](./media/cloud-services-python-ptvs/worker.png)

Herhangi bir zamanda mevcut bulut hizmetine web veya çalışan rolleri ekleyebilirsiniz.  Çözümünüze ister mevcut projeleri ekleyin, ister yeni projeler oluşturun.

![Rol Komutu Ekleme](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Bulut hizmetiniz farklı dillerde uygulanan roller içerebilir.  Örneğin, Python veya C# çalışan rolleri ile Django kullanılarak uygulanan bir Python web rolünüz olabilir.  Service Bus kuyruklarını veya depolama kuyruklarını kullanarak rolleriniz arasında kolaya iletişim kurabilirsiniz.

## Yerel olarak çalıştırma

Bulut hizmeti projenizi başlangıç projesi olarak ayarlar ve F5 tuşuna basarsanız, bulut hizmeti yerel Azure öykünücüsünde çalışacaktır.

PTVS öykünücüde başlatmayı desteklese de, hata ayıklama (örneğin, kesme noktaları) çalışmaz.

Web ve çalışan rollerinizin hatalarını ayıklamak için rol projesini başlangıç projesi olarak ayarlayabilir ve bunun hatalarını ayıklayabilirsiniz.  Ayrıca, birden fazla başlangıç projesi ayarlayabilirsiniz.  Çözüme sağ tıklayın ve ardından **Başlangıç Projelerini Ayarla**’yı seçin.

![Çözüm Başlangıç Projesi Özellikleri](./media/cloud-services-python-ptvs/startup.png)

## Azure’da Yayımlama

Yayımlamak için, çözümdeki bulut hizmeti projesine sağ tıklayın ve ardından **Yayımla**’yı seçin.

![Microsoft Azure Yayımlama Oturumu Açma](./media/cloud-services-python-ptvs/publish-sign-in.png)

Ayarlar sayfasında, yayımlamak istediğiniz bulut hizmeti seçin.

![Microsoft Azure Yayımlama Ayarları](./media/cloud-services-python-ptvs/publish-settings.png)

Bulut hizmetiniz yoksa yeni bir tane oluşturabilirsiniz.

![Bulut Hizmeti Oluşturma İletişim Kutusu](./media/cloud-services-python-ptvs/publish-create-cloud-service.png)

Ayrıca, hata ayıklama ile ilgili hatalar için uzak masaüstü - makine bağlantılarını etkinleştirebilirsiniz.

![Uzak Masaüstü Yapılandırması İletişim Kutusu](./media/cloud-services-python-ptvs/publish-remote-desktop-configuration.png)

Yapılandırma ayarları bittiğinde **Yayımla**’ya tıklayın.

Çıktı penceresinde devam eden bazı işlemler görünür, ardından Microsoft Azure Etkinlik Günlüğü penceresini görürsünüz.

![Microsoft Azure Etkinlik Günlüğü Penceresi](./media/cloud-services-python-ptvs/publish-activity-log.png)

Dağıtımın tamamlanması birkaç dakika sürer, ardından web ve/veya çalışan rolleri Azure üzerinde çalışır!

## Sonraki adımlar

Visual Studio için Python Araçları’ndaki web ve çalışan rolleri ile çalışma hakkında daha ayrıntılı bilgi için PTVS belgelerine bakın:

- [Bulut Hizmeti Projeleri][]

Web ve çalışan rollerinizden Azure Storage veya Service Bus gibi Azure hizmetlerini kullanma hakkında daha ayrıntılı bilgi için aşağıdaki makalelere göz atın.

- [Blob Hizmeti][]
- [Tablo Hizmeti][]
- [Kuyruk Hizmeti][]
- [Service Bus Kuyrukları][]
- [Service Bus Konuları][]


<!--Link references-->

[Bulut Hizmeti nedir?]: ./cloud-services/cloud-services-choose-me.md
[execution model-web sites]: ./app-service-web/app-service-web-overview.md
[execution model-vms]: ./virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: ./cloud-services/cloud-services-choose-me.md
[Python Geliştirici Merkezi]: /develop/python/

[Blob Hizmeti]: ./storage/storage-python-how-to-use-blob-storage.md
[Kuyruk Hizmeti]: ./storage/storage-python-how-to-use-queue-storage.md
[Tablo Hizmeti]: ./storage/storage-python-how-to-use-table-storage.md
[Service Bus Kuyrukları]: ./service-bus/service-bus-python-how-to-use-queues.md
[Service Bus Konuları]: ./service-bus/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Visual Studio için Python Araçları]: http://aka.ms/ptvs
[Visual Studio için Python Araçları Belgeleri]: http://aka.ms/ptvsdocs
[Bulut Hizmeti Projeleri]: http://go.microsoft.com/fwlink/?LinkId=624028
[Visual Studio için Python Araçları 2.2]: http://go.microsoft.com/fwlink/?LinkID=624025
[VS 2013 için Azure SDK Araçları]: http://go.microsoft.com/fwlink/?LinkId=323510
[VS 2015 için Azure SDK Araçları]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 bit]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32 bit]: http://go.microsoft.com/fwlink/?LinkId=517191



<!--HONumber=Jun16_HO2-->


