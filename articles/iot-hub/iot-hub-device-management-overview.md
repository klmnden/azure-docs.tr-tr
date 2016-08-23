<properties
 pageTitle="Cihaz yönetimine genel bakış | Microsoft Azure"
 description="Azure IoT Hub cihaz yönetimine genel bakış: cihaz çiftleri, cihaz sorguları, cihaz işleri"
 services="iot-hub"
 documentationCenter=""
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Azure IoT Hub cihaz yönetimine genel bakış (önizleme)

Azure IoT Hub cihaz yönetimi standartlara dayalı IoT cihaz yönetimini, cihazlarınızı uzaktan yönetmeniz, yapılandırmanız ve güncelleştirmeniz için etkinleştirir.

Azure IoT’de cihaz yönetimine yönelik üç temel kavram vardır:

1.  **Cihaz çifti:** IoT Hub’daki fiziksel cihazın gösterimi.

2.  **Cihaz sorguları**: Cihaz çiftlerini bulmanıza imkan tanır ve birden fazla cihaz çifti için toplu bir anlayış oluşturur. Örneğin, üretici yazılımı sürümü 1.0 olan tüm cihaz çiftlerini bulmak üzere bir sorgu çalıştırabilirsiniz.

3.  **Cihaz işleri**: bir veya daha fazla fiziksel cihaz üzerinde gerçekleştirilecek üretici yazılımı güncelleştirmesi, yeniden başlatma ve fabrika sıfırlaması gibi bir eylem.

## Cihaz çifti

Cihaz çifti Azure IoT’taki bir fiziksel cihazın gösterimidir. Cihaz çiftini göstermek için **Microsoft.Azure.Devices.Device** nesnesi kullanılır.

![][img-twin]

Cihaz çifti aşağıdaki bileşenlere sahiptir:

1.  **Cihaz Alanları:** Cihaz alanları hem IoT Hub mesajlaşması hem de cihaz yönetimi için kullanılan önceden tanımlanmış özelliklerdir. Bunlar IoT Hub'ın fiziksel cihazları tanımlamasına ve bunlara bağlanmasına yardımcı olur. Cihaz alanları cihaz ile eşitlenmez ve özel olarak cihaz çiftine depolanır. Cihaz alanları, cihaz kimliği ile kimlik doğrulama bilgilerini içerir.

2.  **Cihaz Özellikleri:** Cihaz özellikleri fiziksel cihazı açıklayan önceden tanımlanmış bir özellik sözlüğüdür. Fiziksel cihaz her bir cihaz özelliğinin ana öğesi ve karşılık gelen her bir değerin yetkili deposudur. Bu özelliklerin tutarlı bir son gösterimi bulutta cihaz çiftine depolanır. Tutarlılık ve yenilik, [Öğretici: cihaz çiftini kullanma][lnk-tutorial-twin] içinde açıklanan eşitleme ayarlarına bağlıdır. Üretici yazılımı sürümü, pil düzeyi ve üretici adı, cihaz özelliklerinin bazı örnekleridir.

3.  **Hizmet Özellikleri:** Hizmet özellikleri geliştiricinin hizmet özellikleri sözlüğüne eklediği **&lt;anahtar,değer&gt;** çiftleridir. Bu özellikler cihaz çiftinin veri modelini genişleterek cihazınızı daha iyi nitelemenizi sağlar. Hizmet özellikleri cihaz ile eşitlenmez ve özel olarak bulutta cihaz çiftine depolanır. Hizmet özelliğinin bir örneği, cihazları sonraki servis tarihine göre bulmak için kullanılabilen **&lt;NextServiceDate, 11/12/2017&gt;**’dir.

4.  **Etiketler:** Etiketler, sözlük özellikleri yerine rastgele dizeleri olan bir hizmet özellikleri alt kümesidir. Cihaz çiftlerine açıklama eklemek veya cihazları gruplar halinde düzenlemek için kullanılabilir. Etiketler cihaz ile eşitlenmez ve özel olarak cihaz çiftine depolanır. Örneğin, cihaz çiftiniz bir kamyonu temsil ediyorsa kamyondaki her bir kargo türü için bir etiket ekleyebilirsiniz: **elmalar**, **portakallar** ve **muzlar**.

## Cihaz Sorguları

Önceki bölümde, cihaz çiftinin farklı bileşenleri hakkında bilgi aldınız. Şimdi ise IoT Hub cihaz kayıt defterindeki cihaz çiftlerini cihaz özelliklerine, hizmet özelliklerine veya etiketlere göre bulma işlemi açıklanacaktır. Güncelleştirilmesi gereken cihazları bulmak üzere sorgu kullanabilirsiniz. Belirli bir üretici yazılımı sürümüne sahip tüm cihazları sorgulayabilir ve sonuçları belirli bir eylemde toplayabilirsiniz (IoT Hub’da cihaz işi olarak bilinir ve sonraki bölümde anlatılmıştır).

Etiketleri ve özellikleri kullanarak sorgulama yapabilirsiniz:

-   Etiketleri kullanarak cihaz çiftlerini sorgulamak için bir dize dizisi geçirirsiniz ve sorgu bu dizelerin tümüyle etiketlenmiş cihaz kümesini döndürür.

-   Cihaz çiftlerini hizmet özellikleri veya cihaz özellikleri kullanarak sorgulamak için bir JSON sorgu ifadesi kullanırsınız. Aşağıdaki örnekte **FirmwareVersion** anahtarına ve **1.0** değerine sahip tüm cihazları nasıl sorgulayabileceğiniz gösterilmektedir. Özellik **türünün** **cihaz** olduğunu, diğer bir deyişle sorgunun hizmet özelliklerine göre değil, cihaz özelliklerine göre yapıldığını görebilirsiniz:

  ```
  {                           
      "filter": {                  
        "property": {                
          "name": "FirmwareVersion",   
          "type": "device"             
        },                           
        "value": "1.0",              
        "comparisonOperator": "eq",  
        "type": "comparison"         
      },                           
      "project": null,             
      "aggregate": null,           
      "sort": null                 
  }
  ```

## Cihaz İşleri

Cihaz yönetiminde sonraki kavram, birden fazla cihaz üzerinde çok adımlı düzenlemelerin eşgüdümünü sağlayan cihaz işleridir.

Şu anda Azure IoT Hub cihaz yönetimi tarafından sağlanan altı tür cihaz işi bulunmaktadır (müşteriler ihtiyaç duydukça başka işler eklenecektir):

- **Üretici yazılımı güncelleştirme**: Fiziksel cihazdaki üretici yazılımını (veya işletim sistemi görüntüsünü) güncelleştirir.
- **Yeniden başlatma**: Fiziksel cihazı yeniden başlatır.
- **Fabrika sıfırlaması**: Fiziksel cihazın üretici yazılımını (veya işletim sistemi görüntüsünü), cihaza depolanmış olup fabrikada sağlanan bir yedek görüntüye geri döndürür.
- **Yapılandırma güncelleştirmesi**: Fiziksel cihazda çalışan IoT Hub istemci aracısını yapılandırır.
- **Cihaz özelliği okuma**: Fiziksel cihaz üzerindeki bir cihaz özelliğinin en son değerini alır.
- **Cihaz özelliği yazma:** Fiziksel cihazdaki bir cihaz özelliğini değiştirir.

Bu işlerin her birinin nasıl kullanılacağına ilişkin ayrıntılar için lütfen bkz. [C\# ve node. js için API belgeleri][lnk-apidocs].

Bir iş birden fazla cihaz üzerinde çalışabilir. Bir işi başlattığınızda bu cihazların her biri için ilişkili bir alt iş oluşturulur. Alt iş tek bir cihaz üzerinde çalışır. Her bir alt iş üst işine yönelik bir işaretçiye sahiptir. Üst iş yalnızca alt işlerin kapsayıcısıdır, cihaz türlerini birbirinden ayırt etmek için herhangi bir mantık uygulamaz (bir Intel Edison’u güncelleştirme ile Raspberry Pi’yi güncelleştirme ayrımı gibi). Aşağıdaki diyagramda bir üst iş, alt öğeleri ve ilişkili fiziksel cihazlar arasındaki ilişki gösterilmektedir.

![][img-jobs]

Başlattığınız işlerin durumunu anlamak için iş geçmişini sorgulayabilirsiniz. Bazı örnek sorgular için bkz. [sorgu kitaplığı][lnk-query-samples].

## Cihaz Uygulaması

Hizmet tarafı kavramlarını ele aldıktan sonra yönetilen bir fiziksel cihazı oluşturma konusuna geçelim. Azure IoT Hub DM istemci kitaplığı IoT cihazlarınızı Azure IoT Hub ile yönetmenize imkan tanır. “Yönetme”; yeniden başlatma, fabrika sıfırlaması ve üretici yazılımının güncelleştirilmesi gibi eylemleri içerir.  Bugün platformdan bağımsız bir C kitaplığı sunulmaktadır, ancak yakın zamanda diğer diller için de destek eklenecektir.  

DM istemci kitaplığı, cihaz yönetiminde iki ana sorumluluğa sahiptir:

- Fiziksel cihaz üzerindeki özellikleri IoT Hub’da karşılık gelen cihaz çifti ile eşitleyin
- IoT Hub tarafından cihaza gönderilen koreograf cihaz işleri

Bu sorumluluklar ve fiziksel cihaz üzerindeki uygulama hakkında daha fazla bilgi almak için bkz. [C için Azure IoT Hub cihaz yönetimi istemci kitaplığına giriş][lnk-library-c].

## Sonraki adımlar

Çok çeşitli cihaz donanım platformları ve işletim sistemlerinde istemci uygulamalarını uygulamak için IoT cihaz SDK'larını kullanabilirsiniz. IoT cihaz SDK'ları, bir IoT hub'ına telemetri göndermeyi ve bulut-cihaz komutlarını almayı gerçekleştiren kitaplıkları içerir. SDK'ları kullandığınızda IoT Hub ile iletişim kurmak için birçok ağ protokolünden seçim yapabilirsiniz. Daha fazla bilgi için bkz. [Cihaz SDK'ları hakkında bilgi][lnk-device-sdks].

Azure IoT Hub cihaz yönetimi özellikleri hakkında daha fazla bilgi almak için [Azure IoT Hub cihaz yönetimini kullanmaya başlama][lnk-get-started] öğreticisine bakın.

<!-- Images and links -->
[img-twin]: media/iot-hub-device-management-overview/image1.png
[img-jobs]: media/iot-hub-device-management-overview/image2.png
[img-client]: media/iot-hub-device-management-overview/image3.png

[lnk-lwm2m]: http://technical.openmobilealliance.org/Technical/technical-information/release-program/current-releases/oma-lightweightm2m-v1-0
[lnk-library-c]: iot-hub-device-management-library.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-apidocs]: http://azure.github.io/azure-iot-sdks/
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks



<!--HONumber=Aug16_HO1-->


