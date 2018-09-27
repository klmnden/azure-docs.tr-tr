---
title: İzlenecek yol - Bilişsel hizmetler proje akustik örneği
description: Bu izlenecek yolda, masaüstü ve VR dağıtımına dahil olmak üzere proje akustik için Unity örnek Sahne açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: e0c28645de8c45aaf89afb6b5116aa9a3cb04768
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227512"
---
# <a name="unity-sample-walkthrough"></a>Unity için izlenecek örnek yol
Proje akustik örnek bir kılavuz budur. Hangi proje akustik hakkında daha fazla bilgi için kullanıma [proje akustik giriş](what-is-acoustics.md). Önceden varolan bir Unity proje için proje akustik paket ekleme daha fazla yardım almak için kullanın [Başlarken kılavuzunda](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Örnek projeyi çalıştırmak için gereksinimler
* Unity 2018.2 .NET 4.x komut dosyası çalışma zamanı sürümü kullanılarak +,
* Windows 64 bit Unity Düzenleyicisi
* Windows Masaüstü, UWP ve baş monte görüntüler (HMDs) dahil olmak üzere Android hedefleri örneği destekler.
* Azure Batch aboneliği Hazırlama işlemi için gereklidir

## <a name="sample-project-setup"></a>Örnek Proje ayarları
İndirme ve içeri aktarma **MicrosoftAcoustics.Sample.unitypackage**. İçeri aktarma işlemi sırasında proje ayarları dahil olmak üzere **Spatializer** ve **Scripting çalışma zamanı sürümü** eklenti kişinin gereksinimlerini karşılamak için güncelleştirildi. Bu tamamlandığında Unity konsolundan bir hata göreceğiniz **AcousticsGeometry.cs** Scripting çalışma zamanı sürümüne değiştirme hakkında **.NET 4.x eşdeğer**. Bu ayarları değiştirme paketini içeri aktarma işleminin bir parçası olarak gerçekleştirilir, ancak etkili olması için Unity yeniden başlatılması gerekiyor. Unity şimdi yeniden başlatın.

## <a name="running-the-sample"></a>Örneği çalıştırma
Örnek Tanıtım sahnesi içerir **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Bu Sahne kayan bir küp yürütmeyi bir tek spatialized ses kaynağı yok (adlı **AudioHolder** içinde **hiyerarşi**). Genel gezinti betik yardımcı olmak için CameraHolder nesne alt ana kamera öğesidir. 

![Hiyerarşi görünümü](media/SampleHierarchyView.png)

Sahneye önceden desteklenmiş ve bir ACE dosyası ile ilişkili **MicrosoftAcoustics** içinde prefab **hiyerarşi**. 

Unity editor denetimindeki yürütme düğmesine tıklayarak Sahne nasıl ses için dinleyin. Kullanım W, A, S, D ve fareyi hareket etmek için. Sahne ve akustik olmadan nasıl sesleri karşılaştırmak için farenin sol düğmesine veya birincil denetleyici düğmesine tıklayın. Çeşitli ses kaynakları arasında geçiş yapmak için sağ fare düğmesine veya denetleyicinizde geri düğmesine tıklayın.

## <a name="targeting-other-platforms"></a>Diğer platformları hedefleme
Örnek, Windows Masaüstü, UWP, Windows karma gerçeklik, Android ve Oculus Git üzerinde çalıştırmak için ayarları içerir. Varsayılan olarak, projeyi Windows Masaüstü için yapılandırılır. VR platformunu hedeflemek için player ayarlarına gidin (**Düzenle > Proje Ayarları > Player**), bulma **XR ayarları**ve denetleyin **sanal gerçeklik desteklenen** onay kutusu.

![VR etkinleştir](media/VRSupport.png)  

VR kulaklık bilgisayarınıza bağlayın. Git **Dosya > Yapı ayarları**, tıklatıp **derleme ve çalıştırma** VR kendi kulaklık örnek dağıtmak için. İçin kendi kulaklık hareket denetleyicileri kullanarak Sahne gezinmek ya da kullanmayı deneyin W, A, S, klavyedeki D.    
Android ve Oculus Git hedeflemek için Android seçin **Build Settings** menüsü. Tıklayın **geçiş hedef**, ardından **derleme ve çalıştırma**. Bu örnek Sahne bağlı Android cihazınıza dağıtır. Android için Unity geliştirme hakkında daha fazla bilgi için bkz: [Unity belgeleri](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Hedef Android](media/TargetAndroid.png)  

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure hesabı oluşturun](create-azure-account.md) kendi bakes için
* Keşfedin [tasarım işlemi](design-process.md)

