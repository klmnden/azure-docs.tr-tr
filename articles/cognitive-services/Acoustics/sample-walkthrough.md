---
title: 'Örnek: Project Acoustics'
titlesuffix: Azure Cognitive Services
description: Bu kılavuzda, masaüstü ve VR dağıtımı dahil olmak üzere Project Acoustics için Unity örnek sahnesi açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: sample
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 7d8ba2f25bd53b407ab6860bc57163a79b7d228a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174270"
---
# <a name="unity-sample-walkthrough"></a>Unity örnek kılavuzu
Bu kılavuzda Project Acoustics örneği gösterilmektedir. Project Acoustics hakkında daha fazla bilgi için bkz. [Project Acoustics'e giriş](what-is-acoustics.md). Project Acoustics paketini var olan bir Unity projesine ekleme konusunda yardım için bkz. [Başlangıç kılavuzu](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Örnek projeyi çalıştırmak için gereksinimler
* .NET 4.x betik oluşturma çalışma zamanı sürümünü kullanan Unity 2018.2+
* Windows 64 bit Unity Düzenleyicisi
* Bu örnek, kafaya takılan ekranlar (HMD) dahil olmak üzere Windows masaüstü, UWP ve Android hedeflerini destekler
* Hazırlama işlemi için Azure Batch aboneliği gerekir

## <a name="sample-project-setup"></a>Örnek proje kurulumu
**MicrosoftAcoustics.Sample.unitypackage** dosyasını indirip içeri aktarın. İçeri aktarma sonrasında **Spatializer** ve **Scripting Runtime Version** (Betik Oluşturma Çalışma Zamanı Sürümü) gibi proje ayarları, eklenti gereksinimlerini karşılamak üzere güncelleştirilir. Bu işlem tamamlandığında Unity konsolunda **AcousticsGeometry.cs** dosyasından Scripting Runtime Version ayarını **.NET 4.x Equivalent** olarak değiştirmenizi isteyen bir hata iletisi görüntülenir. Bu ayar değişikliği paketin içeri aktarılması sırasında yapılmıştır ancak geçerli olması için Unity'nin yeniden başlatılması gerekir. Unity'yi şimdi yeniden başlatın.

## <a name="running-the-sample"></a>Örneği çalıştırma
Örnekte **Assets/AcousticsDemo/ProjectAcousticsDemo.unity** adlı tanıtım amaçlı bir sahne bulunur. Bu sahnede üç ses kaynağı vardır. Varsayılan olarak yalnızca bir ses kaynağı yürütülür, diğer ikisi duraklatılmıştır. Bunlar **Hierarchy** (Hiyerarşi) altındaki **Sound Sources** (Ses Kaynakları) bölümünde bulunur. Genel bir gezinti betiği oluşturulmasına yardımcı olmak için Main Camera (Ana Kamera), CameraHolder nesnesinin alt öğesi olarak belirlenmiştir. 

![Hiyerarşi Görünümü](media/SampleHierarchyView.png)

Sahne önceden oluşturulmuştur ancak **Hierarchy** içinde **MicrosoftAcoustics** prefab ile ilişkilendirilmiş bir ACE dosyası vardır. 

Unity düzenleyicisindeki yürütme düğmesine tıklayarak sahnenin sesini dinleyin. Masaüstünde W, A, S, D tuşlarını ve fareyi kullanarak sahnede gezinin. Sahne ve akustik olmadan nasıl sesleri Karşılaştırılacak basın **R** katmana metin kırmızıya döner ve diyor kadar düğmesi "Akustik: Devre dışı bırakıldı." Diğer denetimlerin klavye kısayollarını görmek için **F1** tuşuna basın. Ayrıca sağ tıklayıp bir eylem seçtikten sonra sol tıklama ile o eylemi gerçekleştirerek de tüm denetimleri kullanabilirsiniz.

## <a name="targeting-other-platforms"></a>Diğer platformları hedefleme
Örnekte Windows Masaüstü, UWP, Windows Karma Gerçeklik, Android ve Oculus Go üzerinde çalışma ayarları bulunur. Proje varsayılan olarak Windows Masaüstü için yapılandırılmıştır. Bir VR platformunu hedeflemek için oynatıcı ayarlarına gidin (**Edit > Project Settings > Player** (Düzenle > Proje Ayarları > Oynatıcı)), **XR Settings** (XR Ayarları) bölümün bulun ve **Virtual Reality Supported** (Sanal Gerçeklik Desteği) onay kutusunu işaretleyin.

![VR'yi etkinleştirme](media/VRSupport.png)  

Bilgisayarınıza VR ekipmanı bağlayın. **File > Build Settings** (Dosya > Derleme Ayarları) sayfasına gidip **Build and Run** (Derle ve Çalıştır) öğesine tıklayarak örneği VR ekipmanınıza dağıtın. Sahnede gezinmek için ekipmanınızın hareket denetleyicilerini kullanın veya klavyedeki W, A, S, D tuşlarını kullanmayı deneyin.    
Android ve Oculus Go platformlarını hedeflemek için **Build Settings** (Derleme Ayarları) menüsünden Android'i seçin. **Switch Target** (Hedef Değiştir) ve ardından **Build and Run** (Derle ve Çalıştır) öğesine tıklayın. Bunu yaptığınızda örnek sahne, bağlı Android cihazınıza dağıtılır. Android için Unity ile uygulama geliştirme hakkında daha fazla bilgi için bkz. [Unity belgeleri](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Hedef: Android](media/TargetAndroid.png)  

## <a name="next-steps"></a>Sonraki adımlar
* Kendiniz içerik hazırlamak için [bir Azure hesabı oluşturun](create-azure-account.md)
* [Tasarım sürecini](design-process.md) keşfedin

