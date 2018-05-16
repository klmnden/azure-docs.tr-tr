---
title: Azure işlevlerini Azure SignalR Hizmeti ile tümleştirme öğreticisi | Microsoft Docs
description: Bu öğreticide, Azure İşlevleri’ni Azure SignalR Hizmeti ile birlikte kullanmayı öğreneceksiniz
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/24/2018
ms.author: wesmc
ms.openlocfilehash: b1bb6b3fe545d5a42fa985ab85bd7a914128e56b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-integrate-azure-functions-with-azure-signalr-service"></a>Öğetici: Azure İşlevleri’ni Azure SignalR Hizmeti ile tümleştirme

Bu öğretici, önceki öğreticilere oluşturulan sohbet odası uygulamasını temel alır. [SignalR Hizmeti ile sohbet odası oluşturma](signalr-quickstart-dotnet-core.md) ve [Azure SignalR Hizmeti kimlik doğrulaması](./signalr-authenticate-oauth.md) egzersizlerini tamamlamadıysanız önce onları tamamlayın. 

Gerçek zamanlı uygulamalara ilişkin yaygın bir senaryo, içerik güncelleştirmelerinin web istemcilerinde yayımlanması için kaynağının bir sunucu olmasıdır. [Azure İşlevleri](../azure-functions/functions-overview.md), bu içerik güncelleştirmelerini oluşturmak için mükemmel bir adaydır. Azure işlevlerini kullanmanın başlıca avantajlarından biri, tüm uygulamanın mimarisi ya da çalıştıracak altyapı hakkında endişelenmeden kodunuzu isteğe bağlı olarak çalıştırabilmenizdir. Ayrıca, yalnızca kodunuzun gerçekten çalıştığı zaman için ödeme yaparsınız.  

Normalde, SignalR içerik güncelleştirmelerini göndermek için istemci ile sunucu arasında bir bağlantı sürdürmeye çalıştığından, SignalR kullanmaya çalışırken bu senaryo bir sorun oluşturur. Kod yalnızca isteğe bağlı olarak çalıştığından bir bağlantı sürdürülemez. Ancak, Azure SignalR Hizmeti çalışma zamanında bağlantıları yönettiği için bu senaryoyu destekleyebilir.

Bu öğreticide, Azure İşlevleri’ni kullanarak her dakikanın başında bir zamanlayıcı işlevi ile iletiler oluşturacaksınız. İşlev, iletileri önceki öğreticilerde oluşturulan sohbet odasının tüm istemcilerine yayımlar. Zamanlayıcı işlevleri hakkında daha fazla bilgi için bkz. [Zamanlayıcı İşlevi](../azure-functions/functions-create-scheduled-function.md).

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

Bu öğreticinin kodu [AzureSignalR-samples GitHub deposundan](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer) indirilebilir.

![Sunucu iletileri ile sohbet uygulaması](./media/signalr-integrate-functions/signalr-functions-complete.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure İşlevleri ile yeni bir Zamanlayıcı işlevi oluşturun.
> * Yerel Git deposu dağıtımı için zamanlayıcı işlevini yapılandırın.
> * Her dakika güncelleştirme göndermek için zamanlayıcıyı SignalR Hizmetinize bağlayın

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olmanız gerekir:

* [Git](https://git-scm.com/)
* [.NET Core SDK](https://www.microsoft.com/net/download/windows) 
* [Yapılandırılmış Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)
* [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples) github deposunu indirin veya kopyalayın.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinize yönelik yürütme ortamını tanımlamak için bir işlev uygulaması oluşturmanız gerekir. İşlev uygulaması ayrıca daha kolay yönetim, dağıtım ve kaynak paylaşımı için mantıksal bir birim olarak birden fazla işlevi gruplandırmanızı sağlar. Daha fazla bilgi için bkz. [Azure CLI kullanarak ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function-azure-cli.md).

Bu bölümde Azure Cloud Shell kullanarak bir yerel Git deposundan dağıtım için yapılandırılmış yeni bir Azure İşlevi oluşturacaksınız. 

İşlev uygulaması kaynaklarını oluştururken, önceki öğreticilerde kullandığınız aynı kaynak grubunda oluşturun. Bu yaklaşım tüm öğretici kaynakları yönetmeyi daha kolay hale getirir.

Aşağıdaki betiği metin düzenleyicinize kopyalayın ve değişken parametrelerinin değerlerini kaynaklarınızın değerleriyle değiştirin. Güncelleştirilmiş betiği Azure Cloud Shell’e kopyalayıp yapıştırın ve **Enter** tuşuna basarak bir depolama hesabı ve işlev uygulaması oluşturun.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your values from previous    === 
#=== tutorials.                                                   ===
#====================================================================
ResourceGroupName=SignalRTestResources
location=eastus

#====================================================================
#=== Update these variables for the new function app and storage  ===
#=== account.                                                     ===
#====================================================================
functionappName=mysignalfunctionapp
storageAccountName=mystorageaccount

# Create a storage account to hold function app code and settings
az storage account create --resource-group $ResourceGroupName \
--name $storageAccountName \
--location $location --sku Standard_LRS

# Create the function app
az functionapp create --resource-group $ResourceGroupName \
--name $functionappName \
--consumption-plan-location $location \
--storage-account $storageAccountName

```

| Parametre | Açıklama |
| -------------------- | --------------- |
| ResourceGroupName | Bu kaynak grubu adı, önceki öğreticilerde önerilmiştir. Tüm öğretici kaynaklarını aynı grup altında tutmak iyi bir fikirdir. Önceki öğreticilerde kullandığınız kaynak grubunun aynısını kullanın. | 
| location | Bu değişkeni, önceki öğreticilerde kaynak grubunu oluşturmak için kullandığınız konumla güncelleştirin. | 
| functionappName | Bu değişkeni, yeni işlev uygulamanızın benzersiz adıyla güncelleştirin. Örneğin, signalrfunctionapp22665120. | 
| storageAccountName | İşlev uygulaması kodunu ve ayarlarını tutacak yeni depolama hesabı için bir ad girin. | 



## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

Bu bölümde, işlev uygulamanızı Azure SignalR Hizmeti kaynağınızın bağlantı dizesini içeren bir uygulama ayarıyla yapılandıracaksınız. İşlev kodunuz, iletileri sohbet odanıza bağlamak ve yayımlamak için bu ayarı kullanacak. Ayrıca, bir yerel Git deposundan dağıtım için işlev uygulamasını yapılandıracaksınız.

Aşağıdaki betiği kopyalayın ve parametrelerinin değerlerini değiştirin. Güncelleştirilmiş betiği Azure Cloud Shell’e yapıştırın ve **Enter** tuşuna basın.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your resources.              === 
#====================================================================
ResourceGroupName=SignalRTestResources
functionappName=mysignalfunctionapp

#========================================================================
#=== Replace this value with the connection string for your           ===
#=== SignalR Service resource.                                        ===
#========================================================================
connstring="Endpoint=<service_endpoint>;AccessKey=<access_key>;"

# Add the SignalR Service connection string app setting
az functionapp config appsettings set --resource-group $ResourceGroupName \
    --name $functionappName \
    --setting "Azure:SignalR:ConnectionString=$connstring"

# configure for deployment from a local Git repository
az functionapp deployment source config-local-git --name $functionappName \
    --resource-group $ResourceGroupName

```

| Parametre | Açıklama |
| -------------------- | --------------- |
| ResourceGroupName | Bu kaynak grubu adı, önceki öğreticilerde önerilmiştir. Tüm öğretici kaynaklarını aynı grup altında tutmak iyi bir fikirdir. Önceki öğreticilerde kullandığınız kaynak grubunun aynısını kullanın. | 
| functionappName | Bu değişkeni, yeni işlev uygulamanızın benzersiz adıyla güncelleştirin. Örneğin, signalrfunctionapp22665120. | 
| connstring | SignalR Hizmeti kaynağınızın bağlantı dizesini girin. Bu bağlantı dizesini Azure portalındaki SignalR Service kaynak sayfanızdan **AYARLAR** altındaki **Anahtarlar**’a tıklayarak alabilirsiniz. | 



Son komuttan döndürülen Git dağıtım URL'sini not edin. Bu URL’yi, işlev kodunu dağıtmak için kullanacaksınız.


## <a name="the-timer-function"></a>Zamanlayıcı işlevi

Zamanlayıcı işlevi örneğini */samples/Timer* dizininden veya [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer) github deposunun kopyasından indirebilirsiniz. Zamanlayıcı işlev kodu, *TimerFunction.cs* dosyasında bulunur. Bu kod, varsayılan yapılandırma anahtarı (*Azure:SignalR:ConnectionString*) ile depolanmış bağlantı dizesini kullanarak hub’da bir ara sunucu oluşturur. İşlev kodu sunucu tarafında çalıştığından, normal bir istemci olarak kimlik doğrulaması yapmak için gerekli olmaz. Bağlantı dizesini kullanmak için koda güvenilir. İşlev kodu bu hub ara sunucusunu kullanarak hub’ınız üzerinde tanımladığınız yöntemlerden herhangi birini çağırabilir. Kod, tetikleyici başlatıldığında geçerli saati içeren bir ileti yayımlamak üzere `BroadcastMessage` yöntemini çağırır.

İşlev kodunun tetikleyicisi, *TimerFunction/function.json* içindeki bağlamalarda tanımlanan bir *timerTrigger*’dır. Zamanlayıcı tetikleyicisini her dakikanın başında başlatacak şekilde ayarlamak için bir [CRON ifadesi](https://wikipedia.org/wiki/Cron#CRON_expression) içerir.

```json
{
  "bindings": [
    {
      "type": "timerTrigger",
      "schedule": "0 * * * * *",
      "useMonitor": true,
      "runOnStartup": false,
      "name": "myTimer"
    }
  ],
  "disabled": false,
  "scriptFile": "../Timer.dll",
  "entryPoint": "Timer.TimerFunction.Run"
}
```


## <a name="building-the-timer-function"></a>Zamanlayıcı işlevini oluşturma

İşlevi oluşturmak ve dağıtıma hazırlamak için aşağıdaki adımlarda [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanın:

1. İndirme işlemini gerçekleştirdiğiniz */samples/Timer* alt dizinine veya [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples) github deposunun kopyasına gidin.

2. Aşağıdaki komutu kullanarak NuGet paketlerini geri yükleyin:

        dotnet restore

3. Aşağıdaki komutu kullanarak *Zamanlayıcı* işlev uygulamasını derleyin:

        msbuild /p:Configuration=Release


## <a name="create-and-deploy-the-local-git-repo"></a>Yerel Git deposunu oluşturma ve dağıtma

1. Bir Git kabuğunda */samples/Timer/bin/Release/net461* derleme alt dizinine gidin.

        cd ./AzureSignalR-samples/samples/Timer/bin/Release/net461

2. Aşağıdaki komutu kullanarak dizini yeni bir Git deposu olarak başlatın:

        git init

3. Derleme dizinindeki tüm dosyalar için yeni bir işleme ekleyin.

        git add -A
        git commit -v -a -m "Initial Timer function commit"        

4. İşlev uygulamanızın yapılandırması sırasında not aldığınız Git dağıtım URL’si için bir uzak uç nokta ekleyin:

        git remote add Azure <enter your Git deployment URL>

5. İşlev uygulamasını dağıtma

        git push Azure master

   Kod dağıtıldıktan hemen sonra zamanlayıcı, kodunuzu yürütmek için her dakika tetiklenmeye başlar.

## <a name="test-the-chat-app"></a>Sohbet uygulamasını test etme

Sohbet uygulamasına gidin. Yeni oluşturduğunuz Zamanlayıcı işlevi, her dakikanın başında saati bildirecektir.

![Sunucu iletileri ile sohbet uygulaması](./media/signalr-integrate-functions/signalr-functions-complete.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulamayı test etmeye devam etmek istiyorsanız, oluşturduğunuz kaynakları kullanmaya devam edebilirsiniz.

Aksi takdirde, örnek uygulamayı tamamladıysanız ücret yansıtılmaması için Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
> 
> 

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu makaledeki yönergelerde *SignalRTestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

   
![Sil](./media/signalr-integrate-functions/signalr-delete-resource-group.png)


Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.
   
Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure İşlevi tetikleyicilerini temel alarak istemcilere güncelleştirmeler göndermek için Azure İşlevi ile tümleştirme yapma hakkında bilgi edindiniz. Azure SignalR Sunucusu’nu kullanma hakkında daha fazla bilgi almak için SignalR Hizmetine yönelik Azure CLI örnekleri bölümüne devam edin.

> [!div class="nextstepaction"]
> [Azure SignalR CLI Örnekleri](./signalr-cli-samples.md)



