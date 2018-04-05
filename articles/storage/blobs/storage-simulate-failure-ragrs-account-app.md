---
title: Azure’da okuma erişimli yedekli depolamaya erişimde hata benzetimi gerçekleştirme | Microsoft Docs
description: Okuma erişimli coğrafi olarak yedekli depolamaya erişimde hata benzetimi gerçekleştirme
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: tutorial
ms.date: 12/23/2017
ms.author: tamram
ms.openlocfilehash: 0e7ab68075fbce729d3905375acce0dace22c483
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Okuma erişimli yedekli depolamaya erişimde hata benzetimi gerçekleştirme

Bu öğretici, bir dizinin ikinci bölümüdür.  Bu öğreticide, [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabınızın birincil uç noktasına yönelik istekler için hata benzetimi yapmak amacıyla [Fiddler](#simulate-a-failure-with-fiddler)’ı veya [Statik Yönlendirme](#simulate-a-failure-with-an-invalid-static-route)’yi kullanabilir ve uygulamanın ikincil uç noktadan okumasını sağlayabilirsiniz.

![Senaryo uygulaması](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Bu öğreticiyi tamamlamak için önceki depolama öğreticisini tamamlamış olmanız gerekir: [Azure depolama ile uygulama verilerinizi yüksek oranda kullanılabilir hale getirme][previous-tutorial].

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamayı çalıştırma ve duraklatma
> * [Fiddler](#simulate-a-failure-with-fiddler) veya [geçersiz bir statik rota](#simulate-a-failure-with-an-invalid-static-route) ile hata benzetimi yapma 
> * Birincil uç noktayı geri yükleme benzetimi gerçekleştirme


## <a name="prerequisites"></a>Ön koşullar

Fiddler kullanarak hata benzetimi yapmak için: 

* [Fiddler](https://www.telerik.com/download/fiddler)’ı indirip yükleyin

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="simulate-a-failure-with-fiddler"></a>Fiddler ile hata benzetimi yapma

Fiddler ile hata benzetimi yapmak için RA-GRS hesabınızın birincil uç noktasına istekler için başarısız bir yanıt eklersiniz.

Fiddler ile bir hata ve birincil uç noktayı geri yükleme benzetimi yapmak için aşağıdaki adımları izleyin.

### <a name="launch-fiddler"></a>Fiddler'ı açma

Fiddler’ı açıp **Kurallar**’ı ve **Kuralları Özelleştir**’i seçin.

![Fiddler kurallarını özelleştirme](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Fiddler ScriptEditor açılır ve **SampleRules.js** dosyasını gösterir. Bu dosya, Fiddler’ı özelleştirmek için kullanılır. Aşağıdaki kod örneğini `OnBeforeResponse` işlevine yapıştırın. Yeni kod, kendisi tarafından oluşturulan mantığın hemen uygulanmamasını sağlamak amacıyla açıklama satırı yapılır. İşlem tamamlandığında değişikliklerinizi kaydetmek için **Dosya**’yı ve **Kaydet**’i seçin.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Özelleştirilmiş kuralı yapıştırma](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

Uygulamayı IDE’nizde veya metin düzenleyicinizde çalıştırın. Uygulama birincil uç noktadan okumaya başladığında uygulamayı duraklatmak için konsol penceresinde **herhangi bir tuşa** basın.

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatıldığı için artık önceki adımlardan birinde kaydettiğiniz özel kuralı açıklama durumundan çıkarabilirsiniz. Kod örneği RA-GRS depolama hesabına yönelik istekleri arar ve yol görüntünün adını (`HelloWorld`) içeriyorsa `503 - Service Unavailable` yanıt kodunu döndürür.

Fiddler’a gidip **Kurallar** -> **Kuralları Özelleştir...** seçeneğini belirleyin.  Aşağıdaki satırları açıklama durumundan çıkarın, `STORAGEACCOUNTNAME` yerine depolama hesabınızın adını yazın. Değişikliklerinizi kaydetmek için **Dosya** -> **Kaydet**’e tıklayın. 

> [!NOTE]
> Örnek uygulamayı Linux üzerinde çalıştırıyorsanız, **CustomRule.js** dosyasını her düzenlediğinizde Fiddler’ın özel mantığı yüklemesi için Fiddler'ı yeniden başlatmanız gerekir. 
> 
> 


```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Uygulamayı sürdürmek için **herhangi bir tuşa** basın.

Uygulama yeniden çalışmaya başladığında birincil uç nokta istekleri başarısız olmaya başlar. Uygulama birincil uç noktaya 5 kez yeniden bağlanmayı dener. Beş denemelik hata eşiğine ulaştıktan sonra görüntüyü ikincil, salt okunur uç noktadan ister. Uygulama görüntüyü ikincil uç noktadan 20 kez başarıyla aldıktan sonra birincil uç noktaya bağlanmaya çalışır. Birincil uç nokta hala ulaşılamaz durumdaysa uygulama ikincil uç noktadan okumayı sürdürür. Bu düzen, önceki öğreticide açıklanan [Devre Kesici](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) düzenidir.

![Özelleştirilmiş kuralı yapıştırma](media/storage-simulate-failure-ragrs-account-app/figure3.png)

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Önceki adımda yer alan Fiddler özel kural kümesi ile birincil uç noktaya yapılan istekler başarısız olur. Birincil uç noktanın yeniden çalışmaya başlamasının benzetimini yapmak için `503` hatasını ekleme mantığını kaldırırsınız.

Uygulamayı duraklatmak için **herhangi bir tuşa** basın.

Fiddler’a gidip **Kurallar**’ı ve **Kuralları Özelleştir...** seçeneğini belirleyin.  `OnBeforeResponse` işlevindeki özel mantığı açıklama satırı yapın veya kaldırın ve varsayılan işlevi olduğu gibi bırakın. **Dosya**’yı ve **Kaydet**’i seçerek değişiklikleri kaydedin.

![Özelleştirilmiş kuralı kaldırma](media/storage-simulate-failure-ragrs-account-app/figure5.png)

İşlem tamamlandığında, uygulamayı sürdürmek için **herhangi bir tuşa** basın. Uygulama, 999 okuma sayısına ulaşana kadar birincil uç noktadan okumaya devam eder.

![Uygulamayı sürdürme](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Geçersiz bir statik rota ile hata benzetimi yapma 
[Okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabınızın birincil uç noktasına yönelik tüm istekler için geçersiz bir statik rota oluşturabilirsiniz. Bu öğreticide, isteklerin depolama hesabına yönlendirilmesi için ağ geçidi olarak yerel ana bilgisayar kullanılır. Yerel ana bilgisayarın ağ geçidi olarak kullanılması, depolama hesabınızın birincil uç noktasına yönelik tüm isteklerin ana bilgisayara dönecek şekilde bir döngüye girmesine ve sonuçta başarısız olmasına yol açar. Geçersiz bir statik rota ile bir hata ve birincil uç noktayı geri yükleme benzetimi yapmak için aşağıdaki adımları izleyin. 

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

Uygulamayı IDE’nizde veya metin düzenleyicinizde çalıştırın. Uygulama birincil uç noktadan okumaya başladığında uygulamayı duraklatmak için konsol penceresinde **herhangi bir tuşa** basın. 

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış durumdayken Windows’da yönetici olarak komut istemini başlatın ya da Linux’ta root olarak terminali çalıştırın. Depolama hesabı birincil uç nokta etki alanı hakkında bilgi edinmek için bir komut istemine veya terminale aşağıdaki komutu girin.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 `STORAGEACCOUNTNAME` değerini depolama hesabınızın adıyla değiştirin. Depolama hesabınızın IP adresini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın. Yerel ana bilgisayarın IP adresini almak için Windows komut isteminde `ipconfig` veya Linux terminalinde `ifconfig` yazın. 

Hedef ana bilgisayar için statik rota eklemek üzere bir Windows komut isteminde veya Linux terminalinde aşağıdaki komutu yazın. 


# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  route add <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  route add <destination_ip> <gateway_ip>

---
 
`<destination_ip>` öğesini depolama hesabınızın IP adresiyle, `<gateway_ip>` öğesini ise yerel ana bilgisayarınızın IP adresiyle değiştirin. Uygulamayı sürdürmek için **herhangi bir tuşa** basın.

Uygulama yeniden çalışmaya başladığında birincil uç nokta istekleri başarısız olmaya başlar. Uygulama birincil uç noktaya 5 kez yeniden bağlanmayı dener. Beş denemelik hata eşiğine ulaştıktan sonra görüntüyü ikincil, salt okunur uç noktadan ister. Uygulama görüntüyü ikincil uç noktadan 20 kez başarıyla aldıktan sonra birincil uç noktaya bağlanmaya çalışır. Birincil uç nokta hala ulaşılamaz durumdaysa uygulama ikincil uç noktadan okumayı sürdürür. Bu düzen, önceki öğreticide açıklanan [Devre Kesici](/azure/architecture/patterns/circuit-breaker.md) düzenidir.

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Birincil uç noktanın yeniden çalışır duruma gelmesinin benzetimini yapmak için birincil uç noktanın statik rotasını yönlendirme tablosundan silin. Bu, birincil uç noktaya yönelik tüm isteklerin varsayılan ağ geçidi üzerinden yönlendirilmesini sağlar. 

Hedef ana bilgisayarın (depolama hesabı) statik rotasını silmek için bir Windows komut isteminde veya Linux terminalinde aşağıdaki komutu yazın. 
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)

route del <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

route delete <destination_ip>

---

Uygulamayı sürdürmek için **herhangi bir tuşa** basın. Uygulama, 999 okuma sayısına ulaşana kadar birincil uç noktadan okumaya devam eder.

![Uygulamayı sürdürme](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="next-steps"></a>Sonraki adımlar

Serinin ikinci bölümünde, okuma erişimli coğrafi yedekli depolamayı test etmek ve aşağıdaki gibi işlemler gerçekleştirmek için hata benzetimi yapmayı öğrendiniz:

> [!div class="checklist"]
> * Uygulamayı çalıştırma ve duraklatma
> * [Fiddler](#simulate-a-failure-with-fiddler) veya [geçersiz bir statik rota](#simulate-a-failure-with-an-invalid-static-route) ile hata benzetimi yapma 
> * Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Önceden oluşturulmuş depolama örneklerini görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure depolama betiği örnekleri](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md