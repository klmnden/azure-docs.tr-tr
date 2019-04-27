---
title: Öğretici - Azure Service Fabric Mesh uygulama yükseltme | Microsoft Docs
description: Visual Studio kullanarak Service Fabric uygulaması yükseltme hakkında bilgi edinin
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/29/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 23809abd06d626eb87e5d5d15d265f1769b97b66
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809095"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Öğretici: Visual Studio kullanarak Service Fabric uygulaması yükseltme hakkında bilgi edinin

Bu öğretici bir serinin dördüncü bölümüdür ve, doğrudan Visual Studio'dan Azure Service Fabric Mesh uygulama yükseltme işlemini göstermektedir. Yükseltmesi hem kod güncelleştirmesi hem de yapılandırma güncelleştirme içerir. Yükseltme ve Visual Studio içinden yayımlamakta adımları aynı olduğunu görürsünüz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak bir Service Fabric Mesh hizmet yükseltme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Visual Studio’da Service Fabric Mesh uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Yerel geliştirme kümenizde çalışan bir Service Fabric Mesh uygulamasının hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Service Fabric Mesh uygulaması dağıtma](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Bir Service Fabric kafes yükseltme uygulama
> * [Service Fabric Mesh kaynaklarını temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* To-Do uygulamasını dağıtmadıysanız, [Service Fabric Mesh web uygulamasını yayımlama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md) başlığı altında verilen yönergeleri izleyin.

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Visual Studio kullanarak bir Service Fabric Mesh hizmet yükseltme

Bu makalede bir mikro hizmet uygulama içinde sürümüne yükseltme yapmayı gösterir. Bu örnekte biz değiştireceksiniz `WebFrontEnd` görev Kategorisi'ne görüntülemek ve bunu verildiğinde CPU miktarını artırmak için hizmet. Daha sonra size dağıtılmış hizmet yükseltmeniz.

## <a name="modify-the-config"></a>Yapılandırma değiştirme

Bir Service Fabric Mesh uygulaması oluşturduğunuzda, Visual studio ekler bir **parameters.yaml** her dağıtım ortamı (Bulut ve yerel) dosyası. Bu dosyalarda, parametreler ve ardından service.yaml veya network.yaml gibi kafes *.yaml dosyanızdan başvurulan değerleri tanımlayabilirsiniz.  Visual Studio bazı değişkenler, hizmeti ne kadar CPU gibi sağlar.

Güncelleştireceğiz `WebFrontEnd_cpu` cpu kaynakları güncelleştirmek için parametre `1.5` olasılığına, **WebFrontEnd** hizmet daha yoğun bir şekilde kullanılır.

1. İçinde **todolistapp** altında proje **ortamları** > **bulut**açın **parameters.yaml** dosya. Değiştirme `WebFrontEnd_cpu`, değerini `1.5`. Parametre adı, hizmet adıyla başında `WebFrontEnd_` aynı ada sahip farklı hizmetler için geçerli parametreler ayırmak için en iyi uygulama olarak.

    ```xml
    WebFrontEnd_cpu: 1.5
    ```

2. Açık **WebFrontEnd** projenin **service.yaml** altında dosya **WebFrontEnd** > **hizmet kaynakları**.

    Unutmayın, `resources:` bölümünde `cpu:` ayarlanır `"[parameters('WebFrontEnd_cpu')]"`. Bulut değeri için proje oluşturulduğunda, `'WebFrontEnd_cpu` alınır **ortamları** > **bulut** > **parameters.yaml** dosya ve olacak `1.5`. Projeyi yerel olarak çalıştırmak için oluşturuluyorsa, değer alınır **ortamları** > **yerel** > **parameters.yaml** dosyası ve '0,5' olacaktır.

> [!Tip]
> Varsayılan olarak, bir eş profile.yaml dosyanın parametre dosyasını profile.yaml dosyanın değerlerini sağlamak için kullanılır.
> Örneğin, ortamları > bulut > parameters.yaml ortamları için parametre değerlerini sağlar > bulut > profile.yaml.
>
> Bu aşağıdaki profile.yaml dosyasına ekleyerek geçersiz kılabilirsiniz:`parametersFilePath=”relative or full path to the parameters file”` Örneğin, `parametersFilePath=”C:\MeshParms\CustomParameters.yaml”` veya `parametersFilePath=”..\CommonParameters.yaml”`

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

Olup, yapmadan bir kod yükseltme veya bir yapılandırma yükseltme (Bu durumda düşünüyorsunuz her ikisi de), Service Fabric Mesh uygulamanızı azure'da sağ tıklayarak yükseltin **todolistapp** Visual Studio ve ardından **Yayımla...**

Ardından **Service Fabric Uygulamasını Yayımla** iletişim kutusunu göreceksiniz.

Kullanma **hedef profil** bu dağıtım için kullanılacak profile.yaml dosyasını seçmek için açılır. U seçiyoruz bulutta uygulama Yükseltmekte olduğunuz **cloud.yaml** açılır menüde, kullanacağınız `WebFrontEnd_cpu` bu dosyada tanımlanan 1.0 değeri.

![Visual Studio Service Fabric Mesh yayımla iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. Ayarlama **konumu** Yapılacaklar uygulamayı Azure'a ilk yayımlandığında, kullandığınız konuma. Bu makalede kullanılan **Doğu ABD**.

Ayarlama **kaynak grubu** Yapılacaklar uygulamayı Azure'a ilk yayımlandığında, kullandığınız kaynak grubu.

Ayarlama **Azure Container Registry** Yapılacaklar uygulamayı Azure'a ilk yayımlandığında, oluşturduğunuz azure kapsayıcı kayıt defteri adı için.

Yayımla iletişim kutusunda basın **Yayımla** azure'da Yapılacaklar uygulama yükseltme düğmesi.

Seçerek yükseltmesinin ilerleme durumunu izlemek **Service Fabric Araçları** bölmesinde Visual Studio **çıkış** penceresi. 

Görüntü oluşturulur ve Azure Container Registry'ye gönderildi sonra bir **durumu** bağlantı Azure portalında dağıtımı izlemek için tıklayabileceği çıktıda görünür.

Yükseltme tamamlandıktan sonra **Service Fabric Araçları** çıkış IP adresini ve bağlantı noktası, uygulamanızın bir URL biçiminde görüntülenir.

```json
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Bir web tarayıcısı açıp URL'ye giderek Azure'da çalışan web sitenizi görebilirsiniz. Artık bir kategori sütunu içeren bir web sayfasını görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şunları öğrendiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak bir Service Fabric Mesh uygulaması yükseltme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh kaynaklarını temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)