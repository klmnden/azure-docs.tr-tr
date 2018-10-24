---
title: Özel ölçümler InfluxData Telegraf Aracısı ile Linux VM için TOPLA
description: Özel ölçümler InfluxData Telegraf Aracısı ile Linux VM için TOPLA
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 651706b269cf24eca85e0601384ef63cb6ed06a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990714"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Özel ölçümler InfluxData Telegraf Aracısı ile Linux VM için TOPLA

Azure İzleyici, uygulama telemetrinizi, Azure kaynaklarını veya hatta dışarıdan içeriye izleme sistemleri üzerinde çalışan bir aracının aracılığıyla özel ölçümleri toplamak ve bunları doğrudan Azure İzleyici gönderme sağlar. Bu makalede nasıl dağıtılacağı konusunda yönergeleri sağlamaya odaklanır [InfluxData](https://www.influxdata.com/) azure'da bir Linux sanal makinesi Telegraf aracıda ve Azure İzleyici ölçümleri yayımlamak üzere yapılandırın. 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf aracı 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) projelerimizin 150 farklı kaynaklardan ölçümleri topluluğunun eklentisi temelli bir aracı. Hangi iş yüklerini (ör. sanal makinenizde çalışan bağlı olarak MySQL, NGINX, Apache, vb.) ölçümleri toplamak için özel giriş eklentileri yararlanmak için aracı yapılandırabilirsiniz. Eklentileri çıkış ardından, seçtiğiniz hedefe yazma Aracı'nı etkinleştirin. Telegraf Aracısı, doğrudan Azure İzleyici özel ölçümler ile REST API ile tümleştirilmiştir ve "Azure İzleyici çıkışı eklentiyi" destekler. Bu, Linux sanal makinenizde iş yükü belirli ölçümleri toplamak ve bunları Azure İzleyici için özel ölçümler olarak göndermek aracı sağlar. 

 ![Telgraf Aracısı genel bakış](./media/metrics-store-custom-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Özel ölçümler Gönder 

Bu öğreticinin amaçları doğrultusunda, biz Ubuntu 16.04 LTS işletim sistemini çalıştıran bir Linux VM dağıtın. Telegraf aracı, çoğu Linux işletim sistemleri için desteklenir. Debian hem RPM paketleri InfluxData indirme portalında paketlenmemiş Linux ikili dosyaları ile birlikte kullanılabilir. Bu ek Telegraf yükleme yönergeleri ve seçenekleri için yükleme kılavuzuna bakın. 

Oturum açma [Azure portalı](https://portal.azure.com)

Yeni bir Linux VM oluşturmak için: 

1. Tıklayın **kaynak Oluştur** sol gezinti bölmesinden seçeneği. 
1. Arama *sanal makine*  
1. Seçin *Ubuntu 16.04 LTS* tıklatıp **oluştur** 
1. Bir VM adı gibi sağlamak *MyTelegrafVM*.  
1. Disk türü olarak bırakın **SSD**, ardından sağlayan bir **kullanıcıadı**, gibi *azureuser*. 
1. İçin *kimlik doğrulama türü*seçin **parola**, kullanacağınız sonrası SSH için bu VM'ye bir parola yazın. 
1. Tercih **yeni kaynak grubu oluştur**, ardından gibi bir ad verin *myResourceGroup*.  İstenen konumunuzu seçin ve ardından **Tamam**. 

     ![Ubuntu sanal makinesi oluşturma](./media/metrics-store-custom-linux-telegraf/create-vm.png)

1. VM için bir boyut seçin. Örneğin işlem türü veya Disk türüne göre filtreleyebilirsiniz. 

     ![Sanal makine boyutu Telgraf Aracısı genel bakış](./media/metrics-store-custom-linux-telegraf/vm-size.png)

1. Üzerinde **Ayarları sayfası**, **ağ** > **ağ güvenlik grubu** > **Ortak gelen bağlantı noktası seçin**seçin *HTTP* ve *SSH (22)*. Geri kalan seçin ve varsayılan değerleri bırakın **Tamam**. 

1. Özet sayfasında, sanal makine dağıtımını başlatın Oluştur'u seçin. 

1. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır. 

1. VM dikey penceresinde, gitmek **kimlik** sekmesini ve sanal makinenize bir sistem tarafından atanan kimliği olduğundan *üzerinde*. 
 
![FILLIN](./media/metrics-store-custom-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>VM’ye bağlanma 

VM ile bir SSH bağlantısı oluşturun. Sanal makinenizin genel bakış sayfasındaki Bağlan düğmesi seçin. 

![FILLIN](./media/metrics-store-custom-linux-telegraf/connect-VM-button2.png)

Bağlan sayfasında sanal makine, DNS adı ile 22 numaralı bağlantı noktası üzerinden bağlanmak için varsayılan seçenekleri tutun. VM kullanarak oturum açma yerel hesap bağlantı komut gösterilmektedir. Düğmeye tıklayarak komutu kopyalayın. Aşağıdaki örnekte SSH bağlantı komutunun nasıl göründüğü gösterilmiştir: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

SSH bağlantısını yapıştırın, Windows üzerinde Ubuntu'da Bash komut Azure Cloud Shell gibi bir kabuk oturum veya bağlantı oluşturmak için seçtiğiniz bir SSH İstemcisi'ni kullanın. 

## <a name="install-and-configure-telegraf"></a>Yükleme ve yapılandırma Telegraf 

VM'ye Telegraf Debian paketi yüklemek için SSH oturumunda aşağıdaki komutları çalıştırın: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Yapılandırma dosyası Telegraf'ın Telegraf'ın operations tanımlar. Varsayılan olarak, örnek bir yapılandırma dosyası şu yolda yüklü "/ etc/telegraf/telegraf.conf". Örnek yapılandırma dosyası listeler tüm olası girdi ve çıktı eklentiler. Ancak, bir özel yapılandırma dosyası oluşturun ve aşağıdaki komutları çalıştırarak kullanmak aracısına sahip kullanacağız 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]
> Yukarıdaki yalnızca iki giriş eklenti "cpu" ve "bellek" etkinleştirir, daha fazla giriş eklentileri (Docker, MySQL, NGINX, vb.), makine üzerinde çalışan iş yüküne bağlı olarak eklemekten çekinmeyin. Giriş eklentileri tam listesini burada bulunabilir. 

Son olarak, yeni yapılandırmayı kullanarak aracı başlatma için biz durdurmak ve başlatmak, aşağıdaki komutları çalıştırarak aracı zorla 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Şimdi aracı her belirtilen giriş eklentileri ölçümleri toplamak ve bunları Azure İzleyici gösterin. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Azure portalında Telegraf ölçümlerinizi Çiz 

1. Açık [Azure portalı](https://portal.azure.com) 

1. Yeni izleme sekmesine gidin ve ardından **ölçümleri**.  
     ![FILLIN](./media/metrics-store-custom-linux-telegraf/metrics.png)

1. Kaynak Seçici içinde VM'nizi seçin

     ![FILLIN](./media/metrics-store-custom-linux-telegraf/metric-chart.png)

1. Seçin *Telegraf/CPU* ad alanını seçip *usage_system* ölçümü. Bu ölçüm boyutlara göre filtre seçin veya bunlar üzerinde bölün.  

     ![FILLIN](./media/metrics-store-custom-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Ek yapılandırma 

Yukarıdaki izlenecek yol, birkaç temel giriş eklentileri ölçümleri toplamak için Telegraf aracı yapılandırma hakkında bilgi sağlar. Telegraf Aracısı'nı destekleyen bazı ek yapılandırma seçenekleri ile 150'den fazla giriş eklentileri için destek sunar. InfluxData yayımlanan bir [desteklenen eklentiler listesi](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) ve yönergeler [nasıl yapılandırılacakları](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

Ayrıca, bu izlenecek yolda Telegraf aracıyı aracı dağıtılan VM ile ilgili ölçümleri yaymak için kullandığınız tanıdı. Telegraf aracısı ayrıca bir Toplayıcı ve ölçüm iletici olarak diğer kaynaklar için kullanılabilir. Diğer Azure kaynakları için ölçümleri yaymak için aracı yapılandırma konusunda bilgi için bkz: [Telegraf için Azure İzleyici özel ölçüm çıkış](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md).  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu seçin, silmeyi seçin ve silmek için kaynak grubunun adını onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).


