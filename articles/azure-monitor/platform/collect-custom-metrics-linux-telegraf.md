---
title: Özel ölçümler InfluxData Telegraf Aracısı ile Linux VM için TOPLA
description: Özel ölçümler InfluxData Telegraf Aracısı ile Linux VM için TOPLA
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 14415b88cd6036642442ef9ae23e8dee301bb908
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60741614"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Özel ölçümler InfluxData Telegraf Aracısı ile Linux VM için TOPLA

Azure İzleyicisi'ni kullanarak, özel ölçümler, Azure kaynaklarını veya hatta dışarıdan içeriye izleme sistemleri üzerinde çalışan bir aracının, uygulama telemetrisini aracılığıyla toplayabilirsiniz. Daha sonra bunları doğrudan Azure İzleyici gönderebilirsiniz. Bu makalede nasıl dağıtılacağı konusunda yönergeleri sağlar [InfluxData](https://www.influxdata.com/) azure'da bir Linux sanal makinesi Telegraf aracıda ve Azure İzleyici ölçümleri yayımlamak üzere yapılandırın. 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf aracı 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) projelerimizin 150 farklı kaynaklardan ölçümleri topluluğunun fişi-arada temelli bir aracı. Hangi iş yüklerini sanal makinenizde çalıştırmak bağlı olarak, özel giriş ölçümleri toplamak için eklentilerden aracısının yapılandırabilirsiniz. MySQL, NGINX ve Apache verilebilir. Çıkış eklentilerini kullanarak, aracı, ardından seçtiğiniz hedefe yazabilirsiniz. Telegraf Aracısı, doğrudan Azure İzleyici özel ölçümler ile REST API tümleştirilmiştir. Bu eklenti Azure İzleyici çıktı destekler. Kullanılarak eklentisi, aracı Linux sanal makinenizde iş yüküne özgü ölçümleri toplamak ve bunları Azure İzleyici için özel ölçümleri gönderme. 

 ![Telgraf Aracısı genel bakış](./media/collect-custom-metrics-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Özel ölçümler Gönder 

Bu öğreticide, biz Ubuntu 16.04 LTS işletim sistemi çalıştıran bir Linux VM dağıtın. Telegraf aracı, çoğu Linux işletim sistemleri için desteklenir. Debian hem RPM paketleri paketlenmemiş Linux ikili dosyaları ile birlikte kullanılabilir [InfluxData indirme portalına](https://portal.influxdata.com/downloads). Bkz. Bu [Telegraf Yükleme Kılavuzu](https://docs.influxdata.com/telegraf/v1.8/introduction/installation/) ek yükleme yönergeleri ve seçenekleri için. 

[Azure Portal](https://portal.azure.com) oturum açın.

Yeni bir Linux VM oluşturun: 

1. Seçin **kaynak Oluştur** sol gezinti bölmesinden seçeneği. 
1. Arama **sanal makine**.  
1. Seçin **Ubuntu 16.04 LTS** seçip **Oluştur**. 
1. Bir VM adı gibi sağlamak **MyTelegrafVM**.  
1. Disk türü olarak bırakın **SSD**. Ardından sağlayan bir **kullanıcıadı**, gibi **azureuser**. 
1. İçin **kimlik doğrulama türü**seçin **parola**. Ardından bir parola kullanacaksınız daha sonra bu VM'ye SSH girin. 
1. Tercih **yeni kaynak grubu oluştur**. Ardından gibi bir ad verin **myResourceGroup**. Seçin, **konumu**. Ardından **Tamam**. 

    ![Ubuntu sanal makinesi oluşturma](./media/collect-custom-metrics-linux-telegraf/create-vm.png)

1. VM için bir boyut seçin. Göre filtre uygulayabilirsiniz **işlem türü** veya **Disk türü**, örneğin. 

    ![Sanal makine boyutu Telgraf Aracısı genel bakış](./media/collect-custom-metrics-linux-telegraf/vm-size.png)

1. Üzerinde **ayarları** sayfasını **ağ** > **ağ güvenlik grubu**   >  ** Ortak gelen bağlantı noktası seçin**seçin **HTTP** ve **SSH (22)** . Geri kalan seçin ve varsayılan değerleri bırakın **Tamam**. 

1. Özet sayfasında, seçin **Oluştur** sanal makine dağıtımını başlatın. 

1. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır. 

1. VM bölmesinde gidin **kimlik** sekmesi. Sanal makinenizin ayarlamak bir sistem tarafından atanan kimliği olduğundan emin olun **üzerinde**. 
 
    ![Telegraf VM kimlik önizlemesi](./media/collect-custom-metrics-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>VM’ye bağlanma 

VM ile bir SSH bağlantısı oluşturun. Seçin **Connect** vm'nizin genel bakış sayfasında düğme. 

![Telegraf VM genel bakış sayfası](./media/collect-custom-metrics-linux-telegraf/connect-VM-button2.png)

İçinde **sanal makineye bağlanma** sayfasında, DNS adı ile 22 numaralı bağlantı noktası üzerinden bağlanmak için varsayılan seçenekleri tutun. İçinde **VM yerel hesabı kullanarak oturum açma**, bağlantı komut gösterilir. Komutu kopyalamayı düğmesini seçin. Aşağıdaki örnekte SSH bağlantı komutunun nasıl göründüğü gösterilmiştir: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Azure Cloud Shell veya, Windows üzerinde Ubuntu'da Bash gibi bir kabuk SSH bağlantısı komutu yapıştırın veya bağlantı oluşturmak için tercih ettiğiniz bir SSH İstemcisi'ni kullanın. 

## <a name="install-and-configure-telegraf"></a>Yükleme ve yapılandırma Telegraf 

VM'ye Telegraf Debian paketi yüklemek için SSH oturumunda aşağıdaki komutları çalıştırın: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Yapılandırma dosyası Telegraf'ın Telegraf'ın operations tanımlar. Varsayılan olarak, örnek bir yapılandırma dosyası şu yolda yüklü **/etc/telegraf/telegraf.conf**. Örnek yapılandırma dosyası, tüm olası girdi listeler ve eklentiler çıktı. Ancak, bir özel yapılandırma dosyası oluşturun ve aşağıdaki komutları çalıştırarak kullanın aracıları olan: 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]  
> Yukarıdaki kod eklentileri iki giriş yalnızca sağlar: **cpu** ve **bellek**. Daha fazla giriş eklentileri, makinenizde çalışan iş yüküne bağlı olarak ekleyebilirsiniz. Docker, MySQL ve NGINX verilebilir. Giriş eklentileri tam bir listesi için bkz. **ek yapılandırma** bölümü. 

Son olarak, yeni yapılandırmayı kullanarak aracı başlamasını sağlamak için biz durdurmak ve aşağıdaki komutları çalıştırarak başlatmak için aracı zorlayın: 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Şimdi aracı her belirtilen giriş eklentileri ölçümleri toplamak ve bunları Azure İzleyici gösterin. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Azure portalında Telegraf ölçümlerinizi Çiz 

1. [Azure portalı](https://portal.azure.com) açın. 

1. Yeni Git **İzleyici** sekmesi. Ardından **ölçümleri**.  

     ![İzleyici - ölçümler (Önizleme)](./media/collect-custom-metrics-linux-telegraf/metrics.png)

1. Kaynak Seçici içinde VM'nizi seçin.

     ![Ölçüm grafiği](./media/collect-custom-metrics-linux-telegraf/metric-chart.png)

1. Seçin **Telegraf/CPU** ad alanını seçip **usage_system** ölçümü. Bu ölçüm veya bunları bölme boyutlara göre filtrelemek seçebilirsiniz.  

     ![Ad alanı ve ölçüm seçin](./media/collect-custom-metrics-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Ek yapılandırma 

Önceki izlenecek yol, birkaç temel giriş eklentileri ölçümleri toplamak için Telegraf aracı yapılandırma hakkında bilgi sağlar. Telegraf aracı 150'den fazla giriş eklentileri, destek bazı ek yapılandırma seçenekleri için destek sunar. InfluxData yayımlanan bir [desteklenen eklentiler listesi](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) ve yönergeler [nasıl yapılandırılacakları](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

Ayrıca, bu kılavuzda, Telegraf aracıyı aracı dağıtılan VM ile ilgili ölçümleri yaymak için kullandığınız. Telegraf aracısı ayrıca bir Toplayıcı ve ölçüm iletici olarak diğer kaynaklar için kullanılabilir. Diğer Azure kaynakları için ölçümleri yaymak için aracı yapılandırma konusunda bilgi için bkz: [Telegraf için Azure İzleyici özel ölçüm çıkış](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md).  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Artık ihtiyaç duyulan, kaynak grubunu, sanal makine ve tüm ilgili kaynakları silin. Bunu yapmak için sanal makinenin kaynak grubunu seçin ve seçin **Sil**. Ardından, silmek için kaynak grubunun adını onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).



