---
title: 'Öğretici: Azure’da okuma erişimli yedekli depolamaya erişimde hata benzetimi gerçekleştirme | Microsoft Docs'
description: Okuma erişimli coğrafi olarak yedekli depolamaya erişimde hata benzetimi gerçekleştirme
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.openlocfilehash: 0cbb4d2bc6449dc1cf12a374085b429743224995
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60391880"
---
# <a name="tutorial-simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Öğretici: Okuma erişimli yedekli depolamaya erişimde hata benzetimi gerçekleştirme

Bu öğretici, bir dizinin ikinci bölümüdür. İçinde avantajları hakkında bilgi edinin bir [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) tarafından bir hatanın benzetimi.

Bir hata benzetimi yapmak için kullanabilirsiniz [Fiddler](#simulate-a-failure-with-fiddler) veya [statik yönlendirme](#simulate-a-failure-with-an-invalid-static-route). Birincil uç noktasına istekler için hata benzetimi yapmak her iki yöntem sağlayacak, [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabı, uygulamanın sonlandırılmasına neden okuma ikincil uç noktadan yerine.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamayı çalıştırma ve duraklatma
> * [Fiddler](#simulate-a-failure-with-fiddler) veya [geçersiz bir statik rota](#simulate-a-failure-with-an-invalid-static-route) ile hata benzetimi yapma 
> * Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce önceki öğreticide tamamlayın: [Uygulama verilerinizi Azure depolama ile yüksek oranda kullanılabilir yap][previous-tutorial].

Fiddler kullanarak hata benzetimi yapmak için: 

* [Fiddler](https://www.telerik.com/download/fiddler)’ı indirip yükleyin

## <a name="simulate-a-failure-with-fiddler"></a>Fiddler ile hata benzetimi yapma

Fiddler ile hata benzetimi yapmak için RA-GRS depolama hesabınızın birincil uç noktasına istekler için başarısız bir yanıt eklersiniz.

Aşağıdaki bölümlerde, hata ve fiddler ile birincil uç noktayı geri yükleme benzetimi yapmak nasıl kullanılırlar.

### <a name="launch-fiddler"></a>Fiddler'ı açma

Fiddler’ı açıp **Kurallar**’ı ve **Kuralları Özelleştir**’i seçin.

![Fiddler kurallarını özelleştirme](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Fiddler Macro'yu çalıştırır ve görüntüler **SampleRules.js** dosya. Bu dosya, Fiddler’ı özelleştirmek için kullanılır.

Aşağıdaki kod örneğini `OnBeforeResponse` işlevine yapıştırın. Yeni kod, kendisi tarafından oluşturulan mantığın hemen uygulanmamasını sağlamak amacıyla açıklama satırı yapılır.

İşlem tamamlandıktan sonra seçin **dosya** ve **Kaydet** yaptığınız değişiklikleri kaydedin.

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

### <a name="interrupting-the-application"></a>Uygulama kesme

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python ve Java v7](#tab/dotnet-python-java-v7)

Uygulamayı, IDE veya Kabuğu'nu çalıştırın.

Uygulama birincil uç noktadan okumaya başladığında uygulamayı duraklatmak için konsol penceresinde **herhangi bir tuşa** basın.

![Senaryo uygulaması](media/storage-simulate-failure-ragrs-account-app/scenario.png)

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Uygulamayı, IDE veya Kabuğu'nu çalıştırın.

Örnek denetim olduğundan, bir hata benzetimi yapmak için kesme gerekmez. Dosya örneği çalıştırmadan ve girerek depolama hesabınıza yüklendikten emin olmanız yeterlidir **P**.

![Senaryo uygulaması](media/storage-simulate-failure-ragrs-account-app/Java-put-list-output.png)

---

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış durumdayken, Fiddler'da kaydettiğiniz özel kuralı açıklama durumundan çıkarın.

Kod örneği RA-GRS depolama hesabına istekleri arar ve yol dosyasının adını içeriyorsa `HelloWorld`, bir yanıt kodunu döndürür `503 - Service Unavailable`.

Fiddler’a gidip **Kurallar** -> **Kuralları Özelleştir...** seçeneğini belirleyin.

Aşağıdaki satırları açıklama durumundan çıkarın değiştirin `STORAGEACCOUNTNAME` depolama hesabınızın adıyla. Değişikliklerinizi kaydetmek için **Dosya** -> **Kaydet**’e tıklayın. 

> [!NOTE]
> Örnek uygulamayı Linux üzerinde çalıştırıyorsanız, **CustomRule.js** dosyasını her düzenlediğinizde Fiddler’ın özel mantığı yüklemesi için Fiddler'ı yeniden başlatmanız gerekir.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python ve Java v7](#tab/dotnet-python-java-v7)

Uygulamayı sürdürmek için **herhangi bir tuşa** basın.

Uygulama yeniden çalışmaya başladığında birincil uç nokta istekleri başarısız olmaya başlar. Uygulama birincil uç noktaya 5 kez yeniden bağlanmayı dener. Beş denemelik hata eşiğine ulaştıktan sonra görüntüyü ikincil, salt okunur uç noktadan ister. Uygulama görüntüsü başarıyla ikincil uç noktadan 20 kez alır, birincil uç noktaya bağlanmayı dener. Birincil uç nokta hala ulaşılamaz durumdaysa uygulama ikincil uç noktadan okumayı sürdürür.

Bu düzen, önceki öğreticide açıklanan [Devre Kesici](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) düzenidir.

![Özelleştirilmiş kuralı yapıştırma](media/storage-simulate-failure-ragrs-account-app/figure3.png)

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Başarısız kullanıma sunduk, girin **G** başarısız test etmek için.

Bunu birincil ardışık düzen yerine ikincil işlem hattı kullandığını bilgilendirecektir.

---

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python ve Java v7](#tab/dotnet-python-java-v7)

Önceki adımda yer alan Fiddler özel kural kümesi ile birincil uç noktaya yapılan istekler başarısız olur.

Birincil uç noktanın yeniden çalışmaya başlamasının benzetimini yapmak için `503` hatasını ekleme mantığını kaldırırsınız.

Uygulamayı duraklatmak için **herhangi bir tuşa** basın.

Fiddler’a gidip **Kurallar**’ı ve **Kuralları Özelleştir...** seçeneğini belirleyin. 

`OnBeforeResponse` işlevindeki özel mantığı açıklama satırı yapın veya kaldırın ve varsayılan işlevi olduğu gibi bırakın.

**Dosya**’yı ve **Kaydet**’i seçerek değişiklikleri kaydedin.

![Özelleştirilmiş kuralı kaldırma](media/storage-simulate-failure-ragrs-account-app/figure5.png)

İşlem tamamlandığında, uygulamayı sürdürmek için **herhangi bir tuşa** basın. Uygulama, 999 okuma sayısına ulaşana kadar birincil uç noktadan okumaya devam eder.

![Uygulamayı sürdürme](media/storage-simulate-failure-ragrs-account-app/figure4.png)

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Önceki adımda yer alan Fiddler özel kural kümesi ile birincil uç noktaya yapılan istekler başarısız olur.

Birincil uç noktanın yeniden çalışmaya başlamasının benzetimini yapmak için `503` hatasını ekleme mantığını kaldırırsınız.

Fiddler’a gidip **Kurallar**’ı ve **Kuralları Özelleştir...** seçeneğini belirleyin.  `OnBeforeResponse` işlevindeki özel mantığı açıklama satırı yapın veya kaldırın ve varsayılan işlevi olduğu gibi bırakın.

**Dosya**’yı ve **Kaydet**’i seçerek değişiklikleri kaydedin.

İşlem tamamlandığında girin **G** indirmeyi test etmek için. Uygulama, artık birincil işlem hattını yeniden kullanıyor olduğunu bildirir.

---

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Geçersiz bir statik rota ile hata benzetimi yapma

[Okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabınızın birincil uç noktasına yönelik tüm istekler için geçersiz bir statik rota oluşturabilirsiniz. Bu öğreticide, isteklerin depolama hesabına yönlendirilmesi için ağ geçidi olarak yerel ana bilgisayar kullanılır. Yerel ana bilgisayarın ağ geçidi olarak kullanılması, depolama hesabınızın birincil uç noktasına yönelik tüm isteklerin ana bilgisayara dönecek şekilde bir döngüye girmesine ve sonuçta başarısız olmasına yol açar. Geçersiz bir statik rota ile bir hata ve birincil uç noktayı geri yükleme benzetimi yapmak için aşağıdaki adımları izleyin. 

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python ve Java v7](#tab/dotnet-python-java-v7)

Uygulamayı, IDE veya Kabuğu'nu çalıştırın. Uygulama birincil uç noktadan okumaya başladığında uygulamayı duraklatmak için konsol penceresinde **herhangi bir tuşa** basın.

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Örnek denetim bu yana başarısız test etmek için kesme gerekmez.

Dosya depolama hesabınıza örnek çalıştıran ve girerek yüklendiğini doğrulayın **P**.

---

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış durumdayken Windows’da yönetici olarak komut istemini başlatın ya da Linux’ta root olarak terminali çalıştırın.

Bir komut istemine veya terminale aşağıdaki komutu girerek depolama hesabı birincil uç nokta etki alanı hakkında bilgi alın.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 `STORAGEACCOUNTNAME` değerini depolama hesabınızın adıyla değiştirin. Depolama hesabınızın IP adresini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

Yerel ana bilgisayarın IP adresini almak için Windows komut isteminde `ipconfig` veya Linux terminalinde `ifconfig` yazın. 

Hedef ana bilgisayar için statik rota eklemek üzere bir Windows komut isteminde veya Linux terminalinde aşağıdaki komutu yazın. 

#### <a name="linux"></a>Linux

`route add <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route add <destination_ip> <gateway_ip>`

`<destination_ip>` öğesini depolama hesabınızın IP adresiyle, `<gateway_ip>` öğesini ise yerel ana bilgisayarınızın IP adresiyle değiştirin.

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python ve Java v7](#tab/dotnet-python-java-v7)

Uygulamayı sürdürmek için **herhangi bir tuşa** basın.

Uygulama yeniden çalışmaya başladığında birincil uç nokta istekleri başarısız olmaya başlar. Uygulama birincil uç noktaya beş kez yeniden dener. Beş denemelik hata eşiğine ulaştıktan sonra görüntüyü ikincil, salt okunur uç noktadan ister. Uygulama görüntüyü ikincil uç noktadan 20 kez başarıyla aldıktan sonra birincil uç noktaya bağlanmaya çalışır. Birincil uç nokta hala ulaşılamaz durumdaysa uygulama ikincil uç noktadan okumayı sürdürür. Bu düzen, önceki öğreticide açıklanan [Devre Kesici](/azure/architecture/patterns/circuit-breaker) düzenidir.

# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Başarısız kullanıma sunduk, girin **G** başarısız test etmek için. Bunu birincil ardışık düzen yerine ikincil işlem hattı kullandığını bilgilendirecektir.

---

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Birincil uç noktanın yeniden çalışır duruma gelmesinin benzetimini yapmak için birincil uç noktanın statik rotasını yönlendirme tablosundan silin. Bu, birincil uç noktaya yönelik tüm isteklerin varsayılan ağ geçidi üzerinden yönlendirilmesini sağlar.

Hedef ana bilgisayarın (depolama hesabı) statik rotasını silmek için bir Windows komut isteminde veya Linux terminalinde aşağıdaki komutu yazın.

#### <a name="linux"></a>Linux

`route del <destination_ip> gw <gateway_ip>`

#### <a name="windows"></a>Windows

`route delete <destination_ip>`

# <a name="net-python-and-java-v7tabdotnet-python-java-v7"></a>[.NET, Python ve Java v7](#tab/dotnet-python-java-v7)

Uygulamayı sürdürmek için **herhangi bir tuşa** basın. Uygulama, 999 okuma sayısına ulaşana kadar birincil uç noktadan okumaya devam eder.

![Uygulamayı sürdürme](media/storage-simulate-failure-ragrs-account-app/figure4.png)


# <a name="java-v10tabjava-v10"></a>[Java v10](#tab/Java-v10)

Girin **G** indirmeyi test etmek için. Uygulama, artık birincil işlem hattını yeniden kullanıyor olduğunu bildirir.

![Uygulamayı sürdürme](media/storage-simulate-failure-ragrs-account-app/java-get-pipeline-example-v10.png)

---

## <a name="next-steps"></a>Sonraki adımlar

Serinin iki okuma erişimli coğrafi olarak yedekli depolamayı test etmek için hata benzetimi hakkında bilgi edindiniz.

Daha fazla RA-GRS depolama, ilişkili riskleri yanı sıra, işleyişi hakkında bilgi edinmek için şu makaleyi okuyun:

> [!div class="nextstepaction"]
> [RA-GRS ile HA uygulamaları tasarlama](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md
