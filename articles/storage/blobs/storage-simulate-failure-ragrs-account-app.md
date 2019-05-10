---
title: 'Öğretici: Azure’da okuma erişimli yedekli depolamaya erişimde hata benzetimi gerçekleştirme | Microsoft Docs'
description: Okuma erişimli coğrafi olarak yedekli depolamaya erişimde hata benzetimi gerçekleştirme
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.reviewer: artek
ms.openlocfilehash: 1f5c404e410ded2714be761e35060f3c07379bd3
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508089"
---
# <a name="tutorial-simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Öğretici: Okuma erişimli yedekli depolamaya erişimde hata benzetimi gerçekleştirme

Bu öğretici, bir dizinin ikinci bölümüdür. İçinde avantajları hakkında bilgi edinin bir [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) tarafından bir hatanın benzetimi.

Bir hata benzetimi yapmak için kullanabilirsiniz [statik yönlendirme](#simulate-a-failure-with-an-invalid-static-route) veya [Fiddler](#simulate-a-failure-with-fiddler). Birincil uç noktasına istekler için hata benzetimi yapmak iki yöntem de sağlayacak, [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabı, uygulamanın sonlandırılmasına neden okuma ikincil uç noktadan yerine.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamayı çalıştırma ve duraklatma
> * İle hata simülasyonu [geçersiz bir statik rota](#simulate-a-failure-with-an-invalid-static-route) veya [Fiddler](#simulate-a-failure-with-fiddler)
> * Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce önceki öğreticide tamamlayın: [Uygulama verilerinizi Azure depolama ile yüksek oranda kullanılabilir yap][previous-tutorial].

Statik yönlendirme ile hata simülasyonu için yükseltilmiş bir komut istemi kullanın.

Fiddler kullanarak hata benzetimi yapmak için indirme ve [fiddler'ı yükleme](https://www.telerik.com/download/fiddler)

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Geçersiz bir statik rota ile hata benzetimi yapma

[Okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) depolama hesabınızın birincil uç noktasına yönelik tüm istekler için geçersiz bir statik rota oluşturabilirsiniz. Bu öğreticide, isteklerin depolama hesabına yönlendirilmesi için ağ geçidi olarak yerel ana bilgisayar kullanılır. Yerel ana bilgisayarın ağ geçidi olarak kullanılması, depolama hesabınızın birincil uç noktasına yönelik tüm isteklerin ana bilgisayara dönecek şekilde bir döngüye girmesine ve sonuçta başarısız olmasına yol açar. Geçersiz bir statik rota ile bir hata ve birincil uç noktayı geri yükleme benzetimi yapmak için aşağıdaki adımları izleyin.

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

Yönergeleri kullanın [önceki öğreticide] [ previous-tutorial] örneği başlatın ve birincil depolama alanından gelir onaylama, test dosyasını indirin. Hedef platforma bağlı olarak, daha sonra el ile örnek duraklatma veya bir isteminde bekleyin.

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış durumdayken Windows Yönetici olarak bir komut istemi açın ya da Linux'ta root olarak Terminali çalıştırın.

Bir komut istemi veya terminal değiştirerek aşağıdaki komutu girerek depolama hesabı birincil uç nokta etki alanı hakkında bilgi almak `STORAGEACCOUNTNAME` depolama hesabınızın adıyla.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
```

Depolama hesabınızın IP adresini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

Yerel ana bilgisayarın IP adresini almak için Windows komut isteminde `ipconfig` veya Linux terminalinde `ifconfig` yazın.

Bir hedef konak için bir statik rota eklemek için aşağıdaki komutu bir Windows komut isteminde veya Linux terminalinde, değiştirme türü `<destination_ip>` , depolama hesabının IP adresine sahip ve `<gateway_ip>` , yerel ana bilgisayar IP adresine sahip.

#### <a name="linux"></a>Linux

```
route add <destination_ip> gw <gateway_ip>
```

#### <a name="windows"></a>Windows

```
route add <destination_ip> <gateway_ip>
```

Çalışan örnek penceresinde, uygulamayı sürdürmek veya örnek dosya indirin ve ikincil depolama alanından geldiğini doğrulamak için uygun tuşuna basın. Ardından, yeniden örnek duraklatma veya isteminde bekleyin.

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Birincil uç nokta yeniden çalışır hale benzetimini yapmak için geçersiz statik rotasını yönlendirme tablosundan silin. Bu, birincil uç noktaya yönelik tüm isteklerin varsayılan ağ geçidi üzerinden yönlendirilmesini sağlar. Bir Windows komut isteminde veya Linux terminalinde aşağıdaki komutu yazın.

#### <a name="linux"></a>Linux

```
route del <destination_ip> gw <gateway_ip>
```

#### <a name="windows"></a>Windows

```
route delete <destination_ip>
```

Ardından uygulama veya bunu yeniden birincil depolama alanından gelir onaylayan bu zaman örneği indirmek için uygun anahtar yeniden dosya press devam edebilir.

## <a name="simulate-a-failure-with-fiddler"></a>Fiddler ile hata benzetimi yapma

Fiddler ile hata benzetimi yapmak için RA-GRS depolama hesabınızın birincil uç noktasına istekler için başarısız bir yanıt eklersiniz.

Aşağıdaki bölümlerde, hata ve fiddler ile birincil uç noktayı geri yükleme benzetimi yapmak nasıl kullanılırlar.

### <a name="launch-fiddler"></a>Fiddler'ı açma

Fiddler’ı açıp **Kurallar**’ı ve **Kuralları Özelleştir**’i seçin.

![Fiddler kurallarını özelleştirme](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Fiddler Macro'yu çalıştırır ve görüntüler **SampleRules.js** dosya. Bu dosya, Fiddler’ı özelleştirmek için kullanılır.

Aşağıdaki kod örneğinde yapıştırın `OnBeforeResponse` değiştirerek, işlev `STORAGEACCOUNTNAME` depolama hesabınızın adıyla. Örnek bağlı olarak, aynı zamanda değiştirmeniz gerekebilir `HelloWorld` test dosyası adını (veya bir önek gibi `sampleFile`) indiriliyor. Yeni kod hemen çalıştırmaz emin olmak için dışı bırakılmıştır.

İşlem tamamlandıktan sonra seçin **dosya** ve **Kaydet** yaptığınız değişiklikleri kaydedin. Macro'yu penceresi, aşağıdaki adımlarda kullanmak için açık bırakın.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error.
        // When you're ready to stop sending back errors, comment these lines of script out again
        //     and save the changes.

        if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;
        }
    */
```

![Özelleştirilmiş kuralı yapıştırma](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

Yönergeleri kullanın [önceki öğreticide] [ previous-tutorial] örneği başlatın ve birincil depolama alanından gelir onaylama, test dosyasını indirin. Hedef platforma bağlı olarak, daha sonra el ile örnek duraklatma veya bir isteminde bekleyin.

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış durumdayken, geçiş geri fiddler'ı ve, kaydettiğiniz özel kuralı açıklama durumundan `OnBeforeResponse` işlevi. Seçtiğinizden emin olun **dosya** ve **Kaydet** kural etkili şekilde yaptığınız değişiklikleri kaydedin. Bu kod RA-GRS depolama hesabına istekleri arar ve yol örnek dosya adını içeren bir yanıt kodunu döndürür `503 - Service Unavailable`.

Çalışan örnek penceresinde, uygulamayı sürdürmek veya örnek dosya indirin ve ikincil depolama alanından geldiğini doğrulamak için uygun tuşuna basın. Ardından, yeniden örnek duraklatma veya isteminde bekleyin.

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Fiddler, kaldırın veya yeniden özel kuralı açıklama satırı yapın. Seçin **dosya** ve **Kaydet** kural artık olacaktır yürürlükte emin olmak için.

Çalışan örnek penceresinde, uygulamayı sürdürmek veya örnek dosya indirin ve onu yeniden birincil depolama alanından gelir onaylamak için uygun tuşuna basın. Ardından örnek çıkabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Serinin iki okuma erişimli coğrafi olarak yedekli depolamayı test etmek için hata benzetimi hakkında bilgi edindiniz.

Daha fazla RA-GRS depolama, ilişkili riskleri yanı sıra, işleyişi hakkında bilgi edinmek için şu makaleyi okuyun:

> [!div class="nextstepaction"]
> [RA-GRS ile HA uygulamaları tasarlama](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md
