---
title: Bir web uygulamasını Azure IOT hub'ına gelen algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs
description: Algılayıcıdan toplanan ve IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için bir web uygulaması kullanın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 05/31/2019
ms.author: robinsh
ms.openlocfilehash: 22b15a95e529d4f09560e9b7e59d9f78f70651bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476014"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-in-a-web-application"></a>Bir web uygulamasını Azure IOT hub'ına gelen gerçek zamanlı algılayıcı verilerini Görselleştirme

![Uçtan uca diyagramı](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu öğreticide, yerel bilgisayarınızda çalıştırılan bir node.js web uygulaması ile IOT hub'ınızın aldığı gerçek zamanlı algılayıcı verilerini görselleştirme konusunda bilgi edinin. Web uygulamasını yerel olarak çalıştırdıktan sonra isteğe bağlı olarak Azure App Service'te web uygulaması barındırmak için adımları takip edebilirsiniz. Power BI'ı kullanarak IOT hub'ınızdaki verileri görselleştirmek denemek istiyorsanız bkz [kullanım Power BI'ı, Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Neler

* Web uygulaması, sensör verilerini okumak için kullanacağı IOT hub'ınıza bir tüketici grubu Ekle
* Web uygulaması kodunu Github'dan indirin.
* Web uygulaması kodunu inceleyin
* IOT hub'ı, web uygulamanız tarafından gereken yapıtları tutacak ortam değişkenlerini yapılandırma
* Web uygulaması geliştirme makinenizde çalıştırın
* IOT hub'ınıza gerçek zamanlı sıcaklık ve nem verileri görmek için bir web sayfasını açın
* (İsteğe bağlı) Web uygulamanızı Azure App Service'te barındırmak için Azure CLI kullanma

## <a name="what-you-need"></a>Ne gerekiyor

* Tamamlamak [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya bir cihaz öğreticileri; Örneğin, [node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bu, aşağıdaki gereksinimleri kapsar:

  * Etkin bir Azure aboneliği
  * Aboneliğinizde bir IOT hub
  * IOT hub'ınıza ileti gönderen bir istemci uygulaması

* [Git indir](https://www.git-scm.com/downloads)

* Bu makaledeki adımlarda, bir Windows geliştirme makinesi varsayılır; Ancak, kolayca adımları bir Linux sisteminde tercih edilen kabuğunuzda gerçekleştirebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

## <a name="add-a-consumer-group-to-your-iot-hub"></a>IOT hub'ınıza bir tüketici grubu Ekle

[Tüketici grupları](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) uygulamalar ve bağımsız olarak aynı olay hub'ı uç noktasına ait verilerin kullanmak için Azure hizmetleri sağlayan olay akışının içine bağımsız görünümler sağlar. Bu bölümde, web uygulaması verileri okumak için kullanacağı IOT hub'ınızın yerleşik uç noktasına bir tüketici grubu ekleyin.

IOT hub'ınızın yerleşik uç noktaya bir tüketici grubu eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub consumer-group create --hub-name YourIoTHubName --name YourConsumerGroupName
```

Not seçtiğiniz adın, bu öğreticide daha sonra ihtiyacınız olacak.

## <a name="get-a-service-connection-string-for-your-iot-hub"></a>IOT hub'ınız için bir hizmet bağlantı dizesini alma

IOT hub'ları birkaç varsayılan erişim ilkeleri ile oluşturulur. Bu tür bir ilke **hizmet** okuma ve yazma IOT hub'ın uç noktaları bir hizmet için yeterli izinleri sağlayan bir ilke. Hizmet politikasına uyan IOT hub'ınızın bağlantı dizesini almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIotHub --policy-name service
```

Bağlantı dizesi, aşağıdakine benzer görünmelidir:

```javascript
"HostName={YourIotHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"
```

Not hizmeti bağlantı dizesini, bu öğreticide daha sonra ihtiyacınız olacak.

## <a name="download-the-web-app-from-github"></a>Web uygulamasını Github'dan indirin

Bir komut penceresi açın ve örnek Github'dan indirin ve örnek dizine değiştirmek için aşağıdaki komutları girin:

```cmd
git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
cd web-apps-node-iot-hub-data-visualization
```

## <a name="examine-the-web-app-code"></a>Web uygulaması kodunu inceleyin

Web-apps-node-iot-hub-data-visualization dizinden web uygulaması, sık kullandığınız düzenleyicinizde açın. VS Code'da görüntülenen dosya yapısı gösterilmektedir:

![Web uygulaması dosya yapısı](./media/iot-hub-live-data-visualization-in-web-apps/web-app-files.png)

Aşağıdaki dosyaları incelemek için bir dakikanızı ayırın:

* **Server.js** web yuvasını ve olay hub'ı sarmalayıcı sınıfı başlatan bir hizmet tarafı betiğidir. Bu web yuvasını gelen iletileri yayın sınıfı kullanan olay hub'ı sarmalayıcı sınıf için bir geri çağırma sağlar.

* **Olay hub'ı reader.js** belirtilen bağlantı dizesi ve tüketici grubu kullanarak IOT hub'ın yerleşik uç noktasına bağlanan bir hizmet tarafı betiğidir. Bu cihaz kimliği ve EnqueuedTimeUtc gelen iletiler meta verileri ayıklar ve sonra geri çağırma yöntemi tarafından server.js kayıtlı kullanarak ileti geçirir.

* **Grafik cihaz data.js** web yuvasına dinler, her bir cihaz kimliği izler ve son 50 noktası her cihaz için gelen veri depolayan bir istemci tarafı komut dosyasıdır. Ardından seçilen cihaz verilerini çizelge nesnesine bağlar.

* **Index.HTML** web sayfası için kullanıcı Arabirimi düzenini işler ve istemci tarafı mantığı için gerekli betikler başvurur.

## <a name="configure-environment-variables-for-the-web-app"></a>Web uygulaması için ortam değişkenlerini yapılandırma

IOT hub'ınızdaki verileri okumak için IOT hub'ınızın bağlantı dizesini ve aracılığıyla gözükmelidir tüketici grubunun adını web uygulaması gerekir. Bu dizeler, server.js aşağıdaki satırları işlem ortamı'ndan alır:

```javascript
const iotHubConnectionString = process.env.IotHubConnectionString;
const eventHubConsumerGroup = process.env.EventHubConsumerGroup;
```

Komut pencerenizde aşağıdaki komutları kullanarak ortam değişkenlerini ayarlayın. Yer tutucu değerlerini, IOT hub'ınıza ve daha önce oluşturduğunuz tüketici grubunun adı için hizmet bağlantı dizesiyle değiştirin. Dizeleri teklif yok.

```cmd
set IotHubConnectionString=YourIoTHubConnectionString
set EventHubConsumerGroup=YourConsumerGroupName
```

## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Cihazınız çalışıyor ve veri gönderen emin olun.

2. Komut penceresinde indirin ve başvurulan paketleri yüklemek ve Web sitesini başlatmak için aşağıdaki satırları çalıştırın:

   ```cmd
   npm install
   npm start
   ```

3. Web uygulaması başarıyla IOT hub'ınıza bağlanan ve 3000 bağlantı noktası üzerinde dinleme gösteren konsolunda bir çıktı görmeniz gerekir:

   ![Konsolunda başlatılan web uygulaması](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-start.png)

## <a name="open-a-web-page-to-see-data-from-your-iot-hub"></a>IOT hub'ınızdaki verileri görmek için bir web sayfasını açın

Bir tarayıcıda `http://localhost:3000`.

İçinde **bir cihaz seçin** listesinde, çalışan bir çizim son 50 görmek için cihaz seçin, IOT hub'ına cihaz tarafından gönderilen sıcaklık ve nem veri noktası.

![Gerçek zamanlı sıcaklık ve nem gösteren bir web uygulaması sayfası](./media/iot-hub-live-data-visualization-in-web-apps/web-page-output.png)

Ayrıca, web uygulamanızı yayın iletileri için tarayıcı istemcisi gösteren konsolunda bir çıktı görmeniz gerekir:  

![Web uygulaması, konsol çıktı yayın](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-broadcast.png)

## <a name="host-the-web-app-in-app-service"></a>App Service web uygulaması konağı

[Azure App Service'in Web Apps özelliği](https://docs.microsoft.com/azure/app-service/overview) web uygulamalarını barındırmak için bir platform (PAAS) hizmet olarak sunar. Azure App Service'te barındırılan web uygulamaları gibi ek güvenlik, Yük Dengeleme, güçlü Azure özelliklerinden faydalanabilir ve ölçeklenebilirlikten Azure ve iş ortağı DevOps, sürekli dağıtım, paket yönetimi gibi çözümlerinin yanı sıra ve benzeri. Azure App Service web uygulamaları birçok popüler dilde geliştirilen ve Windows veya Linux altyapısında dağıtılmış destekler.

Bu bölümde, App Service'te web uygulaması sağlama ve Azure CLI komutlarını kullanarak kodunuzu dağıtma. Kullanılan komutlar ayrıntılarını bulabilirsiniz [az webapp](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest) belgeleri. Başlamadan önce adımlarını tamamladığınızdan emin olun [IOT hub'ınıza bir kaynak grubu ekleyin](#add-a-consumer-group-to-your-iot-hub), [IOT hub'ınız için bir hizmet bağlantı dizesini alma](#get-a-service-connection-string-for-your-iot-hub), ve [Github'danwebuygulamasınıindirin](#download-the-web-app-from-github).

1. Bir [App Service planı](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) kümesi çalıştırmak için App Service'te barındırılan bir uygulamanın bilgi işlem kaynaklarını tanımlar. Bu öğreticide, web uygulaması barındırmak üzere Geliştirici/ücretsiz katmanı kullanın. Ücretsiz katmanı ile paylaşılan Windows kaynaklarla diğer müşterilerin uygulamaları dahil olmak üzere diğer App Service uygulamaları, web uygulamanızı çalışır. Azure ayrıca Linux üzerinde web uygulamaları dağıtmak için App Service planları teklifler işlem kaynakları. Kullanmak istediğiniz bir App Service planı zaten varsa bu adımı atlayabilirsiniz.

   Windows ücretsiz katmanı kullanarak bir App Service planı oluşturmak için aşağıdaki komutu çalıştırın. IOT hub'ınızın bulunduğu aynı kaynak grubunu kullanın. Hizmet planının adı büyük ve küçük harf, rakam ve kısa çizgi içerebilir.

   ```azurecli-interactive
   az appservice plan create --name <app service plan name> --resource-group <your resource group name> --sku FREE
   ```

2. Artık, App Service planında bir web uygulaması sağlayın. `--deployment-local-git` Parametresi yüklenir ve yerel makinenizde Git deposundan dağıtılan web uygulaması kodunu sağlar. Web uygulamanızın adına, genel olarak benzersiz olmalıdır ve büyük ve küçük harf, rakam ve kısa çizgi içerebilir.

   ```azurecli-interactive
   az webapp create -n <your web app name> -g <your resource group name> -p <your app service plan name> --deployment-local-git
   ```

3. Şimdi uygulama ayarları, IOT hub bağlantı dizesini ve olay hub'ı tüketici grubu belirtmek için ortam değişkenlerini ekleyin. Tek tek ayarlardır, boşlukla `-settings` parametresi. IOT hub'ınıza ve Bu öğreticide daha önce oluşturduğunuz tüketici grubu için hizmet bağlantı dizesini kullanın. Değer teklifi yok.

   ```azurecli-interactive
   az webapp config appsettings set -n <your web app name> -g <your resource group name> --settings EventHubConsumerGroup=<your consumer group> IotHubConnectionString=<your IoT hub connection string>
   ```

4. Web yuvaları protokolü için web uygulaması ve web uygulamasını HTTPS almak için etkinleştirme istekleri yalnızca (HTTP isteklerini HTTPS için yönlendirilir).

   ```azurecli-interactive
   az webapp config set -n <your web app name> -g <your resource group name> --web-sockets-enabled true
   az webapp update -n <your web app name> -g <your resource group name> --https-only true
   ```

5. Kod App Service'e dağıtmak için kullanacağınız, [kullanıcı düzeyinde dağıtım kimlik bilgilerini](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials). Kullanıcı düzeyinde dağıtım kimlik bilgilerinizi, Azure kimlik bilgilerinizden farklıdır ve yerel Git ve bir web uygulamasına FTP dağıtımları için kullanılır. Bir kez ayarlandıktan sonra App Service uygulamalarınızı Azure hesabınızdaki tüm Aboneliklerdeki tüm geçerli. Daha önce kullanıcı düzeyinde dağıtım kimlik bilgileri oluşturmadıysanız, bunları kullanabilirsiniz.

   Daha önce kullanıcı düzeyinde dağıtım kimlik bilgilerini ayarlamadıysanız veya parolanızı hatırlayamıyorsunuz ise aşağıdaki komutu çalıştırın. Dağıtım kullanıcı adınız Azure içinde benzersiz olmalıdır ve değil içermelidir ' @' yerel Git gönderim için simge. İstendiğinde, girin ve yeni parolanızı onaylayın. Parola en az sekiz karakter uzunluğunda olmalı, şu üç öğeyi sahip olmalıdır: harf, rakam ve semboller.

   ```azurecli-interactive
   az webapp deployment user set --user-name <your deployment user name>
   ```

6. App Service kadar kodunuzu göndermek için kullanılacak Git URL'sini alın.

   ```azurecli-interactive
   az webapp deployment source config-local-git -n <your web app name> -g <your resource group name>
   ```

7. Uzak bir Git deposu için App Service web uygulamasında başvuran, kopya ekleyin. İçin \<Git kopya URL'si\>, önceki adımda döndürülen URL'yi kullanın. Komut pencerenizde aşağıdaki komutu çalıştırın.

   ```cmd
   git remote add webapp <Git clone URL>
   ```

8. Kod App Service'e dağıtmak için komut pencerenizde aşağıdaki komutu girin. Kimlik bilgileri istenirse, 5. adımda oluşturduğunuz kullanıcı düzeyinde dağıtım kimlik bilgilerini girin. App Service uzak ana dala gönderme emin olun.

    ```cmd
    git push webapp master:master
    ```

9. Komut penceresinde dağıtımın ilerleme durumunu güncelleştirir. Başarılı bir dağıtımı aşağıdaki çıktıya benzer satırlarla sona erecek:

    ```cmd
    remote:
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://contoso-web-app-3.scm.azurewebsites.net/contoso-web-app-3.git
    6b132dd..7cbc994  master -> master
    ```

10. Web uygulamanızın durumunu sorgulamak ve çalıştığından emin olmak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az webapp show -n <your web app name> -g <your resource group name> --query state
    ```

11. Bir tarayıcıda `https://<your web app name>.azurewebsites.net` sayfasına gidin. Bir size benzer bir web sayfası, web uygulamasını yerel olarak görüntüler çalıştırdığınızda gördünüz. Aygıtınızı çalıştıran ve veri gönderen varsayılarak, cihaz tarafından gönderilen 50 en son sıcaklık ve nem okumalar, çalışan bir çizim görmeniz gerekir.

## <a name="troubleshooting"></a>Sorun giderme

Bu örnek herhangi bir sorun arasında geliyorsa, aşağıdaki bölümlerde yer alan adımları deneyin. Hala sorunlarla karşılaşırsanız, bu konunun sonundaki görüşlerinizi bize gönderin.

### <a name="client-issues"></a>İstemci sorunları

* Bir cihaz listesinde görünmez ya da hiçbir grafiği çizilen cihaz kodu Cihazınızda çalıştığından emin olun.

* Geliştirici Araçları tarayıcıda açın (birçok tarayıcıda F12 tuşuna açmadan) ve konsol bulun. Uyarıları veya hataları var. yazdırılması için bakın.

* /Js/chat-device-data.js istemci tarafı betikte hata ayıklama yapabilirsiniz.

### <a name="local-website-issues"></a>Yerel Web sitesi sorunları

* Konsol çıktısı için düğüm başlatılan burada penceresinde çıktıyı izleyin.

* Sunucu kodu, özellikle server.js ve /scripts/event-hub-reader.js hata ayıklayın.

### <a name="azure-app-service-issues"></a>Azure App Service sorunları

* Azure portalında web uygulamanıza gidin. Altında **izleme** seçin sol bölmede **App Service günlükleri**. Kapatma **uygulama günlüğü (dosya sistemi)** , ayarlanacak **düzeyi** hata ve ardından **Kaydet**. Açılacağını **günlük akışı** (altında **izleme**).

* Azure portalında web uygulamanızdan altında **geliştirme araçları** seçin **konsol** ve düğüm ve npm sürümlerini ile doğrulama `node -v` ve `npm -v`.

* Bir paket paylaşımın hakkında bir hata görürseniz, düzensiz adımları çalıştırabilirsiniz. Sitenin ne zaman dağıtıldığı (ile `git push`) uygulama hizmeti çalıştığında `npm install`, çalıştığı düğümün geçerli sürüme göre yapılandırılmış vardır. Daha sonra yapılandırmayı değiştirilirse, koda anlamsız değişiklik ve yeniden göndermeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ından gerçek zamanlı sensör verilerini görselleştirmek için web uygulamanızı başarıyla kullandınız.

Azure IOT Hub'ından verileri görselleştirmek başka bir yol için bkz: [kullanım Power BI'ı, IOT hub'ınıza gerçek zamanlı sensör verilerini görselleştirmek için](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
