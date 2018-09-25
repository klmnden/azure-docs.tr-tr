---
title: Öğretici - Azure Service Fabric Mesh uygulama yükseltme | Microsoft Docs
description: Visual Studio kullanarak Service Fabric uygulaması yükseltme hakkında bilgi edinin
services: service-fabric-mesh
documentationcenter: .net
author: tylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: c23646bca6109d27e57b2f928363e65c83c634eb
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031161"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Öğretici: Visual Studio kullanarak Service Fabric uygulaması yükseltme hakkında bilgi edinin

Bu öğretici bir serinin dördüncü bölümüdür ve, doğrudan Visual Studio'dan Azure Service Fabric Mesh uygulama yükseltme işlemini göstermektedir. Yükseltmesi hem kod güncelleştirmesi hem de yapılandırma güncelleştirme içerir. Yükseltme ve Visual Studio içinden yayımlamakta adımları aynı olduğunu görürsünüz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak bir Service Fabric Mesh hizmet yükseltme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Bir Service Fabric kafes oluşturma Visual Studio'da uygulama](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Yerel geliştirme kümenizde çalışan bir Service Fabric Mesh uygulaması hata ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Bir Service Fabric kafes dağıtma uygulama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Bir Service Fabric kafes yükseltme uygulama
> * [Service Fabric Mesh kaynakları temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* To-do uygulaması dağıtmadıysanız bölümündeki yönergeleri [bir Service Fabric Mesh web uygulaması yayımlamak](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Visual Studio kullanarak bir Service Fabric Mesh hizmet yükseltme

Bu makalede, bir mikro hizmet uygulamasından bağımsız olarak yükseltebilirsiniz gösterilmektedir.  Bu örnekte biz değiştirecek `WebFrontEnd` görev Kategorisi'ne görüntülemek için hizmet. Daha sonra size dağıtılmış hizmet yükseltmeniz.

## <a name="modify-the-config"></a>Yapılandırma değiştirme

Yükseltme, kod değişiklikleri, yapılandırma değişikliği veya her ikisi de nedeniyle olabilir.  Bir yapılandırma değişikliği tanıtmak için açık `WebFrontEnd` projenin `service.yaml` dosyası (altında olduğu **hizmet kaynakları** düğümü).

İçinde `resources:` bölümünde `cpu:` 0,5 olarak 1.0, web ön ucu yoğun olarak kullanılacak olasılığına öğesinden. Bunu artık şöyle görünmelidir:

```xml
              ...
              resources:
                requests:
                  cpu: 1.0
                  memoryInGB: 1
              ...
```

## <a name="modify-the-model"></a>Modeli Değiştir

Bir kod değişikliği tanıtmak için ekleme bir `Category` özelliğini `ToDoItem` sınıfını `ToDoItem.cs` dosya.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

Ardından update `Load()` kategori varsayılan dize olarak ayarlamak için aynı dosyada yöntemi:

```csharp
public static ToDoItem Load(string description, int index, bool completed)
{
    ToDoItem newItem = new ToDoItem(description)
    {
        Index = index,
        Completed = completed, 
        Category = "none"    // <-- add this line
    };

    return newItem;
}
```

## <a name="modify-the-service"></a>Hizmet değiştirme

`WebFrontEnd` Bir ASP.NET Core uygulaması Yapılacaklar listesi öğelerini gösteren bir web sayfasıyla projesidir. İçinde `WebFrontEnd` projesini açarsanız `Index.cshtml` ve görev kategorisi görüntülemek için aşağıda belirtilen şu iki satırı ekleyin:

```HTML
<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Category</th>           @*add this line*@
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Category</td>           @*add this line*@
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Derleme ve görevleri listeler web sayfasında yeni bir kategori sütunu gördüğünüzü doğrulamak için uygulamayı çalıştırın.

## <a name="upgrade-the-app-from-visual-studio"></a>Uygulamayı Visual Studio'dan yükseltme

Bir kod yükseltme veya bir yapılandırma yükseltme yapmakta olan bağımsız olarak (Bu durumda biz ikisini de uygulamak), Service Fabric Mesh uygulamanızı Azure sağ yükseltmek **todolistapp** Visual Studio seçip **Yayımla ...**

Ardından **Service Fabric Uygulamasını Yayımla** iletişim kutusunu göreceksiniz.

![Visual Studio Service Fabric Mesh yayımla iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. Emin **konumu** Yapılacaklar uygulamayı Azure'a ilk yayımlandığında kullandığınız konumuna ayarlayın. Bu makalede kullanılan **Doğu ABD**.

Emin **kaynak grubu** Yapılacaklar uygulamayı Azure'a ilk yayımlandığında, kullandığınız kaynak grubunu ayarlanır.

Emin **Azure Container Registry** Yapılacaklar uygulamayı Azure'a ilk yayımlandığında, oluşturduğunuz azure kapsayıcı kayıt defteri adı için ayarlanır.

Yayımla iletişim kutusunda basın **Yayımla** azure'da Yapılacaklar uygulama yükseltme düğmesi.

Seçerek yükseltmesinin ilerleme durumunu izleyebilirsiniz **Service Fabric Araçları** bölmesinde Visual Studio **çıkış** penceresi. Yükseltme tamamlandıktan sonra **Service Fabric Araçları** çıkış IP adresini ve bağlantı noktası, uygulamanızın bir URL biçiminde görüntülenir.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Bir web tarayıcısı açıp URL'ye giderek Azure'da çalışan web sitenizi görebilirsiniz. Artık bir kategori sütunu içeren bir web sayfasını görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şunları öğrendiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak bir Service Fabric karmaşa uygulama yükseltme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh kaynakları temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)