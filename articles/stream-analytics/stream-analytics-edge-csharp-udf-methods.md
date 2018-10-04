---
title: .NET Standard kullanıcı tanımlı işlevler (Önizleme) Azure Stream Analytics Edge işleri için geliştirin
description: C# için Stream Analytics Edge işleri, kullanıcı tanımlı işlevleri yazmayı öğrenin.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f0da25410fe81a93501df940ffbb0e115456a9e8
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247816"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-edge-jobs-preview"></a>.NET Standard kullanıcı tanımlı işlevler (Önizleme) Azure Stream Analytics Edge işleri için geliştirin

Azure Stream Analytics, olay veri akışları üzerinde dönüşümler ve hesaplamalar gerçekleştirmek için bir SQL benzeri sorgu dili sağlar. Birçok yerleşik işlevleri vardır, ancak bazı karmaşık senaryolar daha fazla esneklik gerektirir. .NET Standard kullanıcı tanımlı işlevlerle (UDF), tüm .NET standart dilinde (C#, F #, vb.) Stream Analytics sorgu dili genişletmek için kendi işlevleri çağırabilir. UDF karmaşık matematik hesaplamaları gerçekleştirme, özel ML modelleri ML.NET kullanarak içeri aktarmak izin ve özel imputation mantığının eksik verileri için kullanın. Stream Analytics Edge işleri UDF özelliği şu anda Önizleme aşamasındadır ve üretim iş yükleri için kullanılmamalıdır.

## <a name="overview"></a>Genel Bakış
Visual Studio Araçları Azure Stream Analytics, UDF'ler, yazmanızı kolaylaştırmak için işlerinizi yerel olarak test etme (çevrimdışıyken bile) ve Stream Analytics işinizi Azure'da yayımlayın. Azure'a yayımlandıktan sonra işinizi IOT hub'ı kullanarak IOT cihazlarına dağıtabilirsiniz.

UDF uygulamak için üç yolu vardır:

* CodeBehind bir ASA projesindeki dosyalar
* Yerel bir projeden UDF
* Azure depolama hesabınız var olan bir paketten

## <a name="package-path"></a>Paket yolu

Herhangi bir UDF paket biçimi yoluna sahip `/UserCustomCode/CLR/*`. Dinamik bağlantı kitaplıklarını (DLL'ler) ve kaynakları altında kopyalanır `/UserCustomCode/CLR/*` yardımcı olan, kullanıcı DLL'leri sisteminden ve Azure Stream Analytics DLL'leri yalıtmak klasörü. Bu paket yolu bunları kullanmak istemiyorsunuz kullanılan yöntem ne olursa olsun tüm işlevler için kullanılır.

## <a name="supported-types-and-mapping"></a>Desteklenen türler ve eşleme

|**UDF türü (C#)**  |**Azure Stream Analytics yazın**  |
|---------|---------|
|boylam  |  bigint   |
|double  |  double   |
|dize  |  nvarchar(max)   |
|Tarih/saat  |  Tarih/saat   |
|Yapı  |  Irecord   |
|object  |  Irecord   |
|Dizi<object>  |  IArray   |
|Sözlük < string, object >  |  Irecord   |

## <a name="codebehind"></a>CodeBehind
Kullanıcı tanımlı işlevler yazabilirsiniz **Script.asql** CodeBehind. Visual Studio Araçları, otomatik olarak dosyası bir derleme dosyasına derlenir. Derlemeleri zip dosyası olarak paketlenir ve Azure iş gönderdiğinizde, depolama hesabına yüklediniz. Bir C# CodeBehind izleyerek kullanarak UDF yazılacak öğrenebilirsiniz [Stream Analytics Edge işleri için C# UDF](stream-analytics-edge-csharp-udf.md) öğretici. 

## <a name="local-project"></a>Yerel proje
Kullanıcı tanımlı işlevleri, daha sonra bir Azure Stream Analytics sorguda başvurulan bir derlemede yazılabilir. .NET Standard dilinin ifade dili, yordam mantığı ya da özyineleme gibi dışında tüm gücünden gerektiren karmaşık işlevler için önerilen seçenek budur. İşlevi mantığı birden çok Azure Stream Analytics sorguları arasında paylaşmak istediğinizde UDF'ler yerel bir projeden de kullanılabilir. UDF yerel projenize ekleme işlevlerinizi Visual Studio'dan yerel olarak test ve hata ayıklama olanağı sağlar.

Yerel bir proje başvurusu için:

1. Çözümünüzde bir yeni sınıf kitaplığı oluşturun.
2. Kod Sınıfınız içinde yazın. Sınıflar olarak tanımlanması gerekir unutmayın *genel* ve nesneleri olarak tanımlanması gerektiğini *statik genel*. 
3. Projenizi derleyin. Araçları tüm yapılar bin klasörü bir zip dosyasına paketleyin ve zip dosyasını depolama hesabına yükleyin. Dış başvurular için bütünleştirilmiş kod başvurusu yerine NuGet paketini kullanın.
4. Azure Stream Analytics projenizde yeni sınıf başvurusu.
5. Yeni bir işlev, Azure Stream Analytics projenize ekleyin.
6. Proje yapılandırma dosyasına bütünleştirilmiş kod yolu yapılandırma `JobConfig.json`. Derleme yolunu ayarlamak **yerel proje başvurusu ya da CodeBehind**.
7. İşlev projesi hem Azure Stream Analytics projeyi yeniden derleyin.  

### <a name="example"></a>Örnek

Bu örnekte, **UDFTest** bir C# sınıf kitaplığı projesi ve **ASAEdgeUDFDemo** Bakacağınız Azure Stream Analytics Edge proje **UDFTest**.

![Visual Studio'da Azure Stream Analytics IOT Edge projesi](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. Azure Stream Analytics sorgusu, C# UDF için başvuru eklemenize olanak tanıyan, C# projesini oluşturun.
    
   ![Visual Studio'da bir Azure Stream Analytics IOT Edge projesi oluşturmak](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. C# projesinin başvuru ASA Edge projeye ekleyin. Başvurular düğümüne sağ tıklayın ve Başvuru Ekle öğesini seçin.

   ![Visual Studio'da C# projesine bir başvuru ekleyin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. C# proje adı listeden seçin. 
    
   ![C# projenizin adına başvuru listeden seçin.](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. Görmelisiniz **UDFTest** altında listelenen **başvuruları** içinde **Çözüm Gezgini**.

   ![Görünümü Kullanıcı işlev başvurusu, Azure Stream Analytics Edge Çözüm Gezgini içinde tanımlanan](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. Sağ tıklayın **işlevleri** klasörü seçin **yeni öğe**.

   ![Visual Studio'da Azure Stream Analytics Edge çözümünüzdeki işlevleri için yeni bir öğe ekleyin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Bir C# işlev eklemek **SquareFunction.json** Azure Stream Analytics projenize.

   ![Visual Studio için Stream Analytics Edge öğelerinde listesinden C Sharp işlevini seçin](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. İşlev çift **Çözüm Gezgini** yapılandırma iletişim kutusunu açın.

   ![Visual Studio'da C sharp işlevi yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. C# işlevi yapılandırması, seçin **ASA proje başvurusu yükün** ve aşağı açılan listeden ilgili derleme, sınıf ve yöntem adları. Yöntemleri, türleri ve işlevleri Stream Analytics Edge sorgusunda başvurmak için sınıflar olarak tanımlanması gerekir *genel* ve nesneleri olarak tanımlanması gerekir *statik genel*.

   ![Stream Analytics C sharp işlevi yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Var olan paketler

Tercih ettiğiniz herhangi bir IDE'de .NET standart UDF'ler yazabilir ve bunları Azure Stream Analytics sorgunuz çağırır. Önce kod ve DLL'leri paket derleyin. Paket biçimi yoluna sahip `/UserCustomCode/CLR/*`. Ardından, karşıya yükleme `UserCustomCode.zip` Azure depolama hesabınızdaki kapsayıcı köküne.

Azure depolama hesabınıza derleme ZIP paketlerine karşıya yüklendikten sonra Azure Stream Analytics sorguları işlevleri kullanabilirsiniz. Tek yapmak için ihtiyacınız olan Stream Analytics Edge işi yapılandırmasında depolama bilgilerini içerir. Visual Studio Araçları, paketi indirmez çünkü işlevi yerel olarak bu seçenek ile test edilemez. Paket yolu doğrudan hizmete ayrıştırılır. 

Proje yapılandırma dosyasına bütünleştirilmiş kod yolu yapılandırmak için `JobConfig.json`:

Genişletin **kullanıcı tanımlı kod yapılandırma** bölümünde ve yapılandırmanın önerilen aşağıdaki değerlerle doldurun:

 |**Ayar**  |**Önerilen değer**  |
 |---------|---------|
 |Bütünleştirilmiş Kod Kaynağı  | Bulut mevcut derleme paketlerden    |
 |Kaynak  |  Geçerli hesaptaki veri seçin   |
 |Abonelik  |  Aboneliğinizi seçin.   |
 |Depolama Hesabı  |  Depolama hesabınızı seçin.   |
 |Kapsayıcı  |  Depolama hesabınızda oluşturduğunuz kapsayıcıya seçin.   |

![Visual Studio'da Azure Stream Analytics Edge işi yapılandırması](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-job-config.png)

## <a name="limitations"></a>Sınırlamalar
UDF önizlemesi şu anda aşağıdaki sınırlamalara sahiptir:

* Standart .NET dilleri, yalnızca, IOT Edge üzerinde Azure Stream Analytics için de kullanılabilir. Bulut işleri için JavaScript kullanıcı tanımlı işlevler yazabilirsiniz. Daha fazla bilgi için ziyaret [Azure Stream Analytics JavaScript UDF](stream-analytics-javascript-user-defined-functions.md) öğretici.

* .NET standard UDF'ler yalnızca Visual Studio'da yazılan ve Azure'da yayımladınız. .NET Standard UDF'ler salt okunur sürümlerini altında görüntülenebilir **işlevleri** Azure portalında. .NET Standard işlevlerini yazma Azure Portalı'nda desteklenmiyor.

* Azure portal sorgu Düzenleyicisi, .NET standart UDF portalında kullanırken bir hata gösterir. 

* Özel kod Azure Stream Analytics altyapısıyla bağlam paylaştığından, özel kod Azure Stream Analytics kod ile çakışan bir ad alanı/dll_name sahip herhangi bir şey başvuramaz. Örneğin, başvuramaz *Newtonsoft Json*.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: bir C# kullanıcı tanımlı işlev (Önizleme) Azure Stream Analytics Edge işi için yazın](stream-analytics-edge-csharp-udf.md)
* [Öğretici: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevler](stream-analytics-javascript-user-defined-functions.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)
