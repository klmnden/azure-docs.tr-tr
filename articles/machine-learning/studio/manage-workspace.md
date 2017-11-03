---
title: "Machine Learning çalışma alanını yönetme | Microsoft Docs"
description: "Azure Machine Learning çalışma alanları, erişimini yönetmek ve dağıtmak ve ML API web hizmetleri yönetme"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2f90234bdd5c917a502d24cd16256bc11c7fbed0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanını yönetme

> [!NOTE]
> Machine Learning Web Hizmetleri portalında Web hizmetlerini yönetme hakkında daha fazla bilgi için bkz: [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md).
> 
> 

Machine Learning çalışma alanları Azure portalında veya Klasik Azure portalında yönetebilir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Bir çalışma alanı Azure portalında yönetmek için:

1. Oturum [Azure portal](https://portal.azure.com/) Azure aboneliği yönetici hesabı kullanarak.
2. Sayfanın üst kısmındaki arama kutusuna "machine learning çalışma alanları" girin ve ardından **Machine Learning çalışma alanları**.
3. Yönetmek istediğiniz çalışma alanına tıklayın.

Standart kaynak yönetimi bilgileri ve seçenekleri ek olarak, şunları yapabilirsiniz:

- Görünüm **özellikleri** - çalışma ve kaynak bilgileri bu sayfada görüntülenir ve bu çalışma alanı ile bağlantılı abonelik ve kaynak grubunu değiştirebilirsiniz.
- **Depolama anahtarlarını yeniden eşitleme** -çalışma depolama hesabı anahtarları korur. Depolama hesabı anahtarları değişiklikleri sonra tıklayabilirsiniz **anahtarları yeniden eşitleme** anahtarları workspace ile eşitlemek için.

Bu çalışma alanıyla ilişkili web hizmetleri yönetmek için Machine Learning Web Hizmetleri Portalı'nı kullanın. Bkz: [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md) tam bilgi için.

> [!NOTE]
> Dağıtma veya yeni web hizmetleri yönetmek için bir web hizmeti dağıtıldığı abonelik katkıda bulunan veya yönetici rolü atanmalıdır. Machine learning çalışma alanı için başka bir kullanıcı davet, dağıtmak veya web hizmetleri yönetmek için önce bir abonelik katkıda bulunan veya yönetici rolü atamanız gerekir. 
> 
>Erişim izinlerini ayarlama hakkında daha fazla bilgi için bkz: [kullanıcılar ve gruplar Azure portalında - genel Önizleme için erişim atamalarını görüntüle](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-the-azure-classic-portal"></a>Klasik Azure portalını kullanın

Klasik Azure Portalı'nı kullanarak, Machine Learning çalışma alanlarına yönetebilirsiniz:

* Çalışma alanı nasıl kullanıldığını izleyin
* İzin vermek veya erişimi reddetmek için çalışma yapılandırın
* Çalışma alanında oluşturulan Web Hizmetleri yönetme
* Çalışma alanı silme

Ayrıca, Pano sekmesi, çalışma alanı kullanımınızı genel bir bakış ve çalışma bilgilerinizin hızlı bir bakış sağlar.  

> [!TIP]
> Azure Machine Learning Studio'daki üzerinde **WEB Hizmetleri** sekmesinde, ekleyebilir, güncelleştirmek veya bir Machine Learning Web hizmetini silin.
> 
> 

Klasik Azure portalı çalışma yönetmek için:

1. Oturum [Klasik Azure portalı](https://manage.windowsazure.com/) Microsoft Azure kullanarak hesabı - Azure aboneliğiyle ilişkili hesabı kullanın.
2. Microsoft Azure Hizmetleri panelinde tıklatın **MACHINE LEARNING**.
3. Yönetmek istediğiniz çalışma alanına tıklayın.

Çalışma sayfası, üç sekme içerir:

* **PANO** -çalışma alanı kullanımını görüntüleyin ve bilgi izin verir
* **Yapılandırma** -çalışma alanına erişim yönetmenize olanak sağlar
* **WEB Hizmetleri** -bu çalışma alanından yayımlanan Web hizmetlerini yönetmenize olanak sağlar

### <a name="to-monitor-how-the-workspace-is-being-used"></a>Çalışma alanı nasıl kullanıldığını izlemek için
Tıklatın **PANO** sekmesi.

Panodan çalışma alanınızın genel kullanımını görüntüleyin ve hızlı bir bakış çalışma bilgi alın.

* **İşlem** grafik çalışma alanı tarafından kullanılan işlem kaynakları gösterir. Göreli görüntülemek için görünümü veya mutlak değerler değiştirebilirsiniz ve grafikte görüntülenen zaman çerçevesini değiştirebilir.
* **Kullanıma genel bakış** çalışma alanı tarafından kullanılan Azure depolama görüntüler.
* **Hızlı Bakış** çalışma alanı bilgisi ve yararlı bağlantılar özetini sağlar.

> [!NOTE]
> **Oturum açma ML Studio** bağlantısı, Machine Learning Studio, şu anda oturumunuz içine Microsoft Account kullanarak açar. Bir çalışma alanı oluşturmak için Klasik Azure portalında oturum açmak için kullandığınız Microsoft Account otomatik olarak bu çalışma alanını açmak için izni yok. Bir çalışma alanını açmak için çalışma alanı sahibi olarak tanımlandı Microsoft Account oturum açmanız gerekir veya çalışma alanına katılmaya sahibinden davet almak gerekir.
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a>Vermek veya kullanıcıların erişimini askıya almak için
Tıklatın **yapılandırma** sekmesi.

Yapılandırma sekmesinde şunları yapabilirsiniz:

* Machine Learning çalışma alanı erişimini REDDET tıklayarak askıya alma. Kullanıcılar artık Machine Learning Studio'da çalışma alanını açın mümkün olacaktır. Erişimi geri yüklemek için izin VER'i tıklatın.

Machine Learning Studio'da çalışma alanına erişimi olan ek hesapları yönetmek için tıklatın **oturum açma ML Studio** içinde **PANO** sekmesinde (Önceki nota bakın ilgili **oturum açma ML Studio**). Bu çalışma alanı Machine Learning Studio'da açılır. Buradan, tıklatın **ayarları** sekmesini ve ardından **kullanıcılar**. Tıklayabilirsiniz **daha fazla kullanıcı davet** kullanıcılar için çalışma alanına erişim vermek veya bir kullanıcı seçin ve **kaldırmak**.

### <a name="to-manage-web-services-in-this-workspace"></a>Bu çalışma alanında Web hizmetleri yönetmek için
Tıklatın **WEB Hizmetleri** sekmesi.

Bu çalışma alanından yayımlanan web hizmetleri listesini görüntüler.
Bir web hizmetini yönetmek için Web hizmeti sayfasını açmak için listedeki adına tıklayın.

Bir Web hizmeti, tanımlı bir veya daha fazla uç noktaları olabilir.

* Daha fazla uç noktaları "Varsayılan" uç nokta yanı sıra tanımlayabilirsiniz. Uç noktası eklemek için tıklatın **yönetin uç noktaları** Azure Machine Learning Web Hizmetleri Portalı'nı açmak için panonun alt.
* ("Varsayılan" uç nokta silemiyor) bir uç noktasını silmek için uç nokta satırın başındaki onay kutusuna tıklayın ve tıklayın **silmek**. Bu uç nokta Web hizmetinden kaldırır.
  
  > [!NOTE]
  > Uç nokta silindiğinde uygulama web hizmeti uç noktası kullanıyorsa, uygulama bu hizmete erişmeye çalıştığında bir hata alırsınız.
  > 
  > 

Açmak için bir Web Hizmeti uç noktası adına tıklayın. 

Panodan bir süre boyunca, Web hizmetinin genel kullanım görüntüleyebilirsiniz. Sağ üst köşesinde kullanım grafiklerini dönem açılır menüsünden görüntülemek için dönemi seçebilirsiniz. Pano aşağıdaki bilgileri gösterir:

* **Zaman içinde istekleri** seçilen süre boyunca bir adım grafiği isteklerinin sayısını görüntüler. Bu, kullanımında ani karşılaşıyorsanız tanımlamaya yardımcı olabilir.
* **İstek-yanıt istekleri** hizmet seçilen zaman aralığı ve bunlardan kaç tanesinin başarısız üzerinden aldı istek-yanıt çağrıları toplam sayısını gösterir.
* **Ortalama istek-yanıt işlem süresi** alınan isteklerin yürütülebilmesi için gereken süre ortalama görüntüler.
* **Toplu istekleri** Batch hizmeti, seçilen zaman aralığı ve bunlardan kaç tanesinin başarısız üzerinden aldı istek toplam sayısını gösterir.
* **Ortalama iş gecikme süresi** alınan isteklerin yürütülebilmesi için gereken süre ortalama görüntüler.
* **Hataları** web hizmeti çağrıları ortaya çıkan hataları toplam sayısını görüntüler.
* **Hizmetleri maliyetleri** fatura planı hizmetiyle ilişkili ücretleri görüntüler.

Yapılandırma sayfasından aşağıdaki özellikleri güncelleştirebilirsiniz:

* **Açıklama** Web hizmeti için bir açıklama girmenize olanak sağlar. Açıklama gerekli bir alandır.
* **Günlük** etkinleştirmek veya devre dışı noktadaki günlüğü hata olanak tanır. Günlüğe kaydetme hakkında daha fazla bilgi için bkz [Machine Learning web hizmetleri için günlüğe kaydetme](web-services-logging.md).
* **Örnek verileri etkinleştirmek** istek-yanıt hizmeti test etmek için kullanabileceğiniz örnek verileri sağlamanıza izin verir. Machine Learning Studio'da web hizmeti oluşturduysanız, örnek verileri verilerden modelinizi eğitmek için kullanılan, alınır. Hizmet program aracılığıyla oluşturduysanız, verileri JSON paketinin bir parçası sağlanan örnek verileri alınır.

