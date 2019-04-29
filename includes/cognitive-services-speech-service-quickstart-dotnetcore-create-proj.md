---
author: erhopf
ms.service: cognitive-services
ms.topic: include
origin.date: 12/12/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: 6e49db90aa9e9f933a190425afbafd15e0057fca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621163"
---
1. Visual Studio 2017'yi başlatın.

1. **.NET platformlar arası geliştirme** iş yükünün bulunduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Araçları ve Özellikleri Al**'ı seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın.

   ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-net-core-workload.png)

   Etkin değilse **.NET Core platformlar arası geliştirme**'nin yanındaki kutuyu işaretleyin ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenme birkaç saniye sürer.

1. Yeni bir Visual C# .NET Core Konsol Uygulaması oluşturun. **Yeni Proje** iletişim kutusunda, sol panelden **Yüklü** > **Visual C#** > **.NET Core**'u genişletin. Ardından **Console App (.NET Core)** seçeneğine tıklayın. Proje adı olarak *helloworld* girin.

   ![Yeni Project iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C# Konsol Uygulaması (.NET Core) Oluştur")

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'i seçin.

   ![Çözüm Gezgini'nin, Çözüm için NuGet Paketlerini Yönet vurgulanmış olarak ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönet")

1. Sağ üst köşede, **Paket Kaynağı** alanında **nuget.org**'u seçin. `Microsoft.CognitiveServices.Speech` paketini arayın ve **helloworld** projesine yükleyin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet paketini yükle")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

   ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Lisansı kabul et")

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.
