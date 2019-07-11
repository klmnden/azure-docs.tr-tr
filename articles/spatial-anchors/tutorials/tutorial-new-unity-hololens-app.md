---
title: Öğretici - Azure uzamsal bağlayıcılarını kullanarak yeni bir HoloLens Unity uygulaması oluşturmak için adım adım yönergeler | Microsoft Docs
description: Bu öğreticide, Azure uzamsal bağlayıcılarını kullanarak yeni bir HoloLens Unity uygulamasının nasıl oluşturulacağını öğrenin.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 07/05/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 57244dd9f3365b3899bcc1dde6382cc3b51719d9
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722944"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-hololens-unity-app-using-azure-spatial-anchors"></a>Öğretici: Azure uzamsal bağlayıcılarını kullanarak yeni bir HoloLens Unity uygulaması oluşturmak için adım adım yönergeler

Bu öğreticide Azure uzamsal Çıpasıyla yeni HoloLens Unity uygulaması oluşturulacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

1. Bir Windows makineyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> ile yüklenen **Evrensel Windows platformu geliştirme** iş yükü ve **Windows 10 SDK (10.0.17763.0 ya da daha yeni)** bileşeni ve <a href="https://git-scm.com/download/win" target="_blank">Git için Windows</a>.
2. [ C++WinRT Visual Studio Uzantısı (VSIX)](https://aka.ms/cppwinrt/vsix) for Visual Studio yüklü [Visual Studio Market](https://marketplace.visualstudio.com/).
3. HoloLens cihazla [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin. Bu makale gerekir HoloLens cihazla [Windows 10 Ekim 2018 güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (RS5 olarak da bilinir). HoloLens üzerinde en son sürümüne güncelleştirmek için **ayarları** uygulama, Git **güncelleştirme ve güvenlik**, ardından **Güncelleştirmeleri denetle** düğmesi.

## <a name="getting-started"></a>Başlarken

Biz öncelikle bizim proje ve Unity Sahne ayarlama:
1. Unity başlatın.
2. **Yeni**'yi seçin.
4. Olun **3B** seçilir.
5. Projenizi adlandırın ve kaydetmeyi girin **konumu**.
6. Tıklayın **proje oluştur**.
7. Boş bir varsayılan Sahne yeni kullanarak bir dosyaya kaydet: **Dosya** > **Kaydet**.
8. Yeni bir Sahne ad **ana** basın **Kaydet** düğmesi.

**Proje ayarlarını belirleme**

Bize yardımcı olacak bazı Unity proje ayarları artık hedef geliştirme için Windows Holographic SDK ayarlarsınız. 

Öncelikle, uygulamamız için kalite ayarlar sağlar. 
1. Seçin **Düzenle** > **proje ayarları** > **kalite**
2. Sütununda altında **Windows Store** logosu, oka tıklayarak **varsayılan** seçin ve satır **çok düşük**. Ayarı uygulandığında doğru zaman anlarsınız kutuya **Windows Store** sütun ve **çok düşük** satır yeşildir.

Unity vermeye çalıştığınız uygulama 2B bir görünüm yerine kapsamlı bir görünümünü oluşturması gerektiğini bilmeniz sağlamak ihtiyacımız var. Windows 10 SDK'sını hedefleyen Unity desteğini sanal gerçeklik etkinleştirerek kapsamlı bir görünüm oluşturacağız.

1. Git **Düzenle** > **proje ayarları** > **Player**.
2. İçinde **Inspector paneli** için **Player ayarları**seçin **Windows Store** simgesi.
3. Genişletin **XR ayarları** grubu.
4. İçinde **işleme** bölümünde onay **sanal gerçeklik desteklenen** yeni bir checkbox **sanal gerçeklik SDK'ın** listesi.
5. Doğrulayın **Windows karma gerçeklik** listesinde görünür. Aksi takdirde, seçin **+** seçin ve düğme listesinin en altında **Windows karma gerçeklik**.
 
> [!NOTE]
> Windows Store simgesini görmüyorsanız, Windows Store .NET komut arka yüklemeden seçili olduğundan emin olun çift. Aksi durumda, Unity doğru Windows yükleme işlemine yeniden yüklemeniz gerekebilir.

**.NET yapılandırmasını doğrula**
1. Git **Düzenle** > **proje ayarları** > **Player** (yine de olabilir **Player** önceki açık Adım).
2. İçinde **Inspector paneli** için **Player ayarları**seçin **Windows Store** simgesi.
3. İçinde **diğer ayarlar** yapılandırma bölümünde, emin **betik arka uç** ayarlanır **.NET**.

**kümesi özellikleri**
1. Git **Düzenle** > **proje ayarları** > **Player** (yine de olabilir **Player** önceki açık Adım).
2. İçinde **Inspector paneli** için **Player ayarları**seçin **Windows Store** simgesi.
3. İçinde **yayımlama ayarları** yapılandırma bölümü, onay **InternetClientServer** ve **SpatialPerception**.

**Ana sanal kamera ayarlayın**
1. İçinde **hiyerarşi paneli**seçin **ana kamera**.
2. İçinde **denetçisi**, dönüştürme konumunu ayarlayın **0,0,0**.
3. Bulma **Temizle bayrakları** özelliği, açılan listeden değiştirip **Skybox** için **düz renk**.
4. Tıklayarak **arka plan** renk seçiciyi açmak için alan.
5. Ayarlama **R, G, B ve A** için **0**.
6. Seçin **Bileşen Ekle** ve aramak ve eklemek **uzamsal eşleme Collider**.

**betiğimizi oluşturma**
1. İçinde **proje** bölmesinde, yeni bir klasör oluşturun **betikleri**altında **varlıklar** klasör. 
2. Klasörü sağ tıklatın ve ardından **Oluştur >** ,  **C# betik**. Bu başlık **AzureSpatialAnchorsScript**. 
3. Git **GameObject** -> **boş oluşturma**. 
4. Onu seçin ve **denetçisi** buradan Yeniden Adlandır **GameObject** için **MixedRealityCloud**. Seçin **Bileşen Ekle** ve aramak ve eklemek **AzureSpatialAnchorsScript**.

**Küre prefab oluşturma**
1. Git **GameObject** -> **3B nesne** -> **küre**.
2. İçinde **denetçisi**, kendi ölçek kümesine **0.25, 0.25, 0,25**.
3. Bulma **küre** nesnesine **hiyerarşi** bölmesi. Üzerinde tıklayın ve sürükleyin **varlıklar** klasöründe **proje** bölmesi.
4. Sağ tıklayın ve **Sil** özgün sphere öğesinde görüntülenen oluşturduğunuz **hiyerarşi** bölmesi.

Şimdi de prefab küre olmalıdır, **proje** bölmesi.

## <a name="trying-it-out"></a>Denediğiniz
Çıkış her şeyin çalıştığını test etmek için uygulamanızı yapı içinde **Unity** ve buradan dağıtmak **Visual Studio**. Bölüm 6'dan izleyin [ **100 MR temelleri: Unity ile çalışmaya başlama** kurs](https://docs.microsoft.com/windows/mixed-reality/holograms-100#chapter-6---build-and-deploy-to-device-from-visual-studio) Bunu yapmak için. Başlangıç ekranı ve NET görünen Unity görmeniz gerekir.

## <a name="place-an-object-in-the-real-world"></a>Gerçek dünyada bir nesne getirin
Şimdi oluşturma ve uygulamanızı kullanarak bir nesne getirin. Ne zaman oluşturduğumuz Visual Studio çözümünü açmak biz [uygulamamızı dağıtılan](#trying-it-out). 

İlk olarak, içine aşağıdaki içeri aktarmaları ekleyin, `Assembly-CSharp (Universal Windows)\Scripts\AzureSpatialAnchorsScript.cs`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=19-24)]

Ardından, aşağıdaki üyeleri değişkenleri ekleyin, `AzureSpatialAnchorsScript` sınıfı: 

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=26-42,48-52,60-79)]

Devam ediyoruz küre prefab spherePrefab üye değişkeni üzerinde oluşturduğumuz ayarlamanız gerekir. Geri Git **Unity**.
1. İçinde **Unity**seçin **MixedRealityCloud** nesnesine **hiyerarşi** bölmesi.
2. Tıklayarak **küre** kaydettiğiniz prefab **proje** bölmesi. Sürükleme **küre** içine tıkladığınız **küre Prefab** alanında **Azure uzamsal bağlayıcılarını betik (betik)** içinde **denetçisi** bölmesi .

Artık **küre** betiğinizle prefab olarak ayarlayın. Yapıdan **Unity** ve ortaya çıkan açın **Visual Studio** çözümü yeniden gibi yalnızca yaptığınız [denediğiniz](#trying-it-out). 

İçinde **Visual Studio**, açın `AzureSpatialAnchorsScript.cs` yeniden. Aşağıdaki kodu ekleyin, `Start()` yöntemi. Bu kod kanca `GestureRecognizer`, hangi algılayacak olduğunda bir havadan dokunma ve çağrı `HandleTap`.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-90,93&highlight=4-10)]

Artık aşağıdaki eklemek sahibiz `HandleTap()` yöntemi aşağıdaki `Update()`. Bu ray atama yapmak ve bir küre yerleştirileceği isabet bir noktada alın. 

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-277,299-300,304-312)]

Biz artık küre oluşturmanız gerekir. Küre başlangıçta beyaz olacaktır, ancak bu değer daha sonra ayarlanır. Aşağıdaki `CreateAndSaveSphere()` yöntemi:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-325,390)]

Uygulamanızdan çalıştırma **Visual Studio** bir kez daha doğrulamak için. Bu kez, ekranın oluşturmak ve tercih ettiğiniz yüzeyi üzerinde beyaz, küre yerleştirmek için dokunun.

## <a name="set-up-the-dispatcher-pattern"></a>Dağıtıcı deseni oluşturan ayarlayın

Unity ile çalışırken, tüm Unity API'leri, örneğin kullanıcı Arabirimi güncelleştirmeleri yapmak için kullandığınız API'leri ana iş parçacığında gerçekleştirilmesi gerekir. Ancak biz yazacaksınız kodda, diğer iş parçacıkları üzerinde geri çağırmaları aldığımız. Ana iş parçacığı üzerine yan akıştan gitmek için bir yol yüzden bu geri aramalarda UI'yi güncellemeye istiyoruz. Bir iş parçacığından yan ana iş parçacığında kod yürütmek için dağıtıcı deseni kullanacağız. 

Bir kuyruk eylemleri olan bir üye değişkeni dispatchQueue, ekleyelim. İptal eder kuyruğuna eylemleri gönderin ve ardından sıradan çıkarma ve eylemleri ana iş parçacığı üzerinde çalıştırın. 

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=38-51&highlight=6-9)]

Ardından, kuyruğa bir eylem eklemek için bir yol ekleyelim. Ekleme `QueueOnUpdate()` hemen sonra `Update()` :

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=107-117)]

Şimdi artık kullanım Update() döngünün bir eylem varsa denetlemek için kuyruğa alındı. Bu durumda, biz eylemi sıradan çıkarma ve çalıştırın.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=95-105&highlight=4-10)]

## <a name="get-the-azure-spatial-anchors-sdk"></a>Azure uzamsal bağlayıcılarını SDK'sı Al

Biz, artık Azure uzamsal bağlayıcılarını SDK indirirsiniz. Git [Azure uzamsal bağlayıcıları GitHub sürümleri sayfasından](https://github.com/Azure/azure-spatial-anchors-samples/releases). Varlıkları altında **AzureSpatialAnchors.unitypackage** dosya. 

Unity içinde Git **varlıklar**, tıklayın **paketini içeri aktar** > **özel paket...** . Paketi Seç gidip **açık**.

Yeni **Unity paketini içeri aktar** açılarak Seç penceresi **hiçbiri** sol altta. Altında **AzureSpatialAnchorsPlugin** > **eklentileri**seçin **ortak**, **Düzenleyicisi**, ve  **HoloLens**. Tıklayın **alma** sağ alt köşedeki.

Artık Azure uzamsal bağlayıcılarını SDK'sı almak için Nuget paketlerini geri yüklemek ihtiyacımız var. Yapıdan **Unity** açın ve ardından elde edilen derleme **Visual Studio** çözümü yeniden ayrıntılarıyla açıklandığı gibi [denediğiniz](#trying-it-out). 

İçinde **Visual Studio** çözümün içine aşağıdaki import deyimini ekleyin, `<ProjectName>\Assets\Scripts\AzureSpatialAnchorsScript.cs`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=23-26&highlight=1)]

Ardından, içine aşağıdaki üye değişkenlerini ekleyin, `AzureSpatialAnchorsScript` sınıfı:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=48-63&highlight=6-11)]

## <a name="attach-a-local-azure-spatial-anchor-to-the-local-anchor"></a>Yerel bir Azure uzamsal bağlantı için yerel bağlantı ekleme

Azure uzamsal bağlantı'nın CloudSpatialAnchorSession ' ayarlayalım. Aşağıdakileri ekleyerek başlayacağız `InitializeSession()` yöntemi içinde `AzureSpatialAnchorsScript` sınıfı. Çağrıldığında, bir Azure uzamsal bağlayıcılarını oturumu oluşturulur ve uygulama başlatma sırasında düzgün başlatılmadı sağlayacaktır.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=174-202,205-209)]

Artık temsilci çağrıları işlemek üzere kod yazmak ihtiyacımız var. Biz devam ettikçe daha kendisine ekleyeceğiz.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=211-226)]

Şimdi, şimdi kanca, `initializeSession()` yönteme, `Start()` yöntemi.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-93&highlight=12)]

Son olarak, aşağıdaki kodu ekleyin, `CreateAndSaveSphere()` yöntemi. Gerçek dünyada biz yerleştirme Küre, yerel bir Azure uzamsal bağlantı bağlanacağı.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-338,390&highlight=14-25)]

Başka bir işlem yapmadan önce bir Azure uzamsal yer işaretleri oluşturmanız gerekecektir bunları zaten yoksa, hesap tanımlayıcı ve anahtar. Bunları almak için aşağıdaki bölümü izleyin.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Yerel, yer işareti buluta yükleyin

Uzamsal bağlayıcılarını Azure hesabınızı tanımlayıcı ve anahtar oluşturduktan sonra gidin ve Yapıştır `Account Id` içine `SpatialAnchorsAccountId` ve `Account Key` içine `SpatialAnchorsAccountKey`.

Son olarak, şimdi her şeyi birbirine bağlayın. İçinde `SpawnNewAnchoredObject()` yöntemine aşağıdaki kodu ekleyin. Çağıracaktır `CreateAnchorAsync()` , küre oluşturulduktan hemen sonra yöntemi. Bir kez yöntemi döndüğünde, aşağıdaki kod, küre rengini Mavi olarak değiştirilmesi, son bir güncelleştirme gerçekleştirir.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-391&highlight=26-77)]

Uygulamanızdan çalıştırma **Visual Studio** bir kez daha. Geçici bir çözüm birikimine taşıyın ve ardından, küre yerleştirmek için havadan. Biz yeterli çerçeveler oluşturduktan sonra kürenin sarı kapatır ve bulut karşıya yükleme başlar. Karşıya yükleme tamamlandığında, küre mavi kapatır. İsteğe bağlı olarak, çıkış penceresine da kullanabileceğinizi **Visual Studio** uygulamanızı gönderdiği iletileri izlemek için. Oluşturma ilerleme durumu, yanı sıra bulut karşıya yükleme tamamlandıktan sonra döndüren bağlantı tanımlayıcısı önerilen izleme mümkün olacaktır.

> [!NOTE]
> Alırsanız "DllNotFoundException: DLL 'AzureSpatialAnchors' yüklenemiyor: Belirtilen modül bulunamadı. ", aşağıdakileri yapmalısınız **temiz** ve **derleme** çözümünüzü yeniden.

## <a name="locate-your-cloud-spatial-anchor"></a>Bulut uzamsal yer işareti bulun

Buluta yüklenen bir, bağlantı, tekrar bulma girişiminde hazırız. Aşağıdaki kodu ekleyelim, `HandleTap()` yöntemi. Bu kod size:

* Çağrı `ResetSession()`, hangi durdurur `CloudSpatialAnchorSession` ve mevcut bizim mavi küre ekranından kaldırın.
* Başlatma `CloudSpatialAnchorSession` yeniden. Şimdi yerel yer işaretini oluşturduk olan yerine buluttan bulunacak yapacağız bağlantı geldiğinden emin ediyoruz bunu yapabilirsiniz.
* Oluşturma bir **İzleyicisi** Azure uzamsal yer işaretleri dosyamızı bağlantı arar.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-305&highlight=13-31,35-36)]

Artık ekleyelim bizim `ResetSession()` ve `CleanupObjects()` yöntemleri. Bunları aşağıdaki koyabilirsiniz `QueueOnUpdate()`

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=119-172)]

Artık için sorguladığınız bağlantı bulunduğunda, çağrılan kodu yeteneklerinizi ihtiyacımız var. İçine `InitializeSession()`, aşağıdaki geri çağırmalar ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=200-206&highlight=4-5)]

 
Artık, oluşturma ve yeşil bir küre CloudSpatialAnchor bulunduktan sonra yerleştirin kodu ekleyin olanak tanır. Tüm senaryo bir kez daha yineleyin için onu da ekranı yeniden dokunarak etkinleştirir: başka bir yerel bağlantı oluşturmak, karşıya yükleyin ve tekrar bulun.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=228-265)]

İşte bu kadar! Uygulamanızdan çalıştırma **Visual Studio** tam senaryoyu uçtan uca denemek için son kez. Cihazınızı taşıyın ve beyaz, küre yerleştirin. Ardından, birikimine küre sarıya kadar ortam verilerini yakalamak için taşıma tutun. Yerel, yer işareti yüklenevek ve, küre mavi kapatır. Son olarak, ekranınıza bir kez daha, yerel bağlantı kaldırılır ve ardından şu bulut çözümlemesiyle sorgulayacaksınız dokunun. Bulut uzamsal yer işareti bulunana kadar Cihazınızı taşınması devam edin. Yeşil bir küre doğru konumda görüntülenmelidir ve, parlatıcı & tam senaryoyu yeniden tekrarlayın.

[!INCLUDE [AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md)]