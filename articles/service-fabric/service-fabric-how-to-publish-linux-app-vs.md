---
title: Oluşturma ve.Net Core yayımlama hakkında bilgi edinin. uzak bir Azure Service Fabric Linux kümesi uygulamaları | Microsoft Docs
description: Oluşturma ve yayımlama.Net Core Visual Studio'dan uzak bir Linux kümesi hedefleyen uygulamalar
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: 46d76edbe8cede12e8c7811f43c28a65c1ebaed0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078670"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Oluşturma ve yayımlama.Net Core için Visual Studio'yu kullanın uzak Linux Service Fabric kümesi hedefleyen uygulamalar
Visual Studio Araçları, geliştirebilir ve Service Fabric.Net Core yayımlama Linux Service Fabric kümesi hedefleyen uygulamalar. SDK sürümü 3.4 olmalıdır veya bir.Net Core dağıtmak için yukarıdaki uygulama Linux Service Fabric hedefleme kümelerinde Visual Studio'dan.

> [!Note]
> Visual Studio hata ayıklama hedef Linux Service Fabric uygulamaları desteklemez.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>.Net Core hedefleyen bir Service Fabric uygulaması oluşturma
1. Visual Studio'yu **yönetici** olarak başlatın.
2. Bir proje oluşturma **Dosya -> Yeni Proje ->** .
3. İçinde **yeni proje** iletişim kutusunda seçin **bulut -> Service Fabric uygulaması**.
![uygulama oluşturma]
4. Uygulamaya bir ad ve tıklayın **Tamam**.
5. Üzerinde **yeni Service Fabric hizmeti** sayfasında, hizmetin altında oluşturmak istediğiniz türü seçin **.Net Core bölüm**.
![hizmet oluşturma]

## <a name="deploy-to-a-remote-linux-cluster"></a>İçin uzak bir Linux kümesi dağıtma
1. Çözüm Gezgini'nde sağ tıklatın ve uygulama tıklayın **yapı**.
![Uygulama derleme]
2. Uygulama için derleme işlemi tamamlandıktan sonra hizmete sağ tıklayın ve Düzenle'yi **csproj dosyasını**.
![edit-csproj]
3. True UpdateServiceFabricManifestEnabled özelliğinden Düzenle **False** hizmeti ise bir **aktör projesi türü**. Aktör hizmeti uygulamanız yoksa, 4. adıma geçin.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> UpdateServiceFabricManifestEnabled false olarak ayarlamak, ServiceManifest.xml güncelleştirmeleri bir yapı sırasında devre dışı bırakır. Herhangi bir değişiklik gibi eklemek, kaldırmak veya yeniden adlandırmak için hizmet olarak ServiceManifest.xml yansıtılmaz. Herhangi bir değişiklik yapılırsa ServiceManifest UpdateServiceFabricManifestEnabled true ve ServiceManifest.xml güncelleştirin ve sonra geri hizmet oluşturmak için el ile veya geçici olarak ayarlayın ya da güncelleştirme false olarak yedeklemeniz gerekir.
>

4. Win7 x64 gelen RuntimeIndetifier hizmet projesi hedef platform için güncelleştirin.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. ServiceManifest içinde .exe kaldırmak için giriş noktası programın güncelleştirin. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. Çözüm Gezgini'nde sağ tıklatın ve uygulama **Yayımla**. **Yayımla** iletişim kutusu görüntülenir.
7. İçinde **bağlantı uç noktası**, hedef olarak istersiniz uzak bir Service Fabric Linux kümesi için uç nokta seçin.
![uygulamayı Yayımla]

<!--Image references-->
[uygulama oluşturma]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[hizmet oluşturma]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[Uygulama derleme]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[edit-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[uygulamayı Yayımla]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [.Net Core ile Service Fabric ile çalışmaya başlama](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/)
