---
title: "Bir bulut Hizmeti performansını test etme | Microsoft Docs"
description: "Visual Studio profil oluşturucu kullanılarak bir bulut Hizmeti performansını test etme"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: eafcc2f9d53bcdae63036df070e5adec24cbc252
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="testing-the-performance-of-a-cloud-service"></a>Bir bulut Hizmeti performansını test etme
## <a name="overview"></a>Genel Bakış
Aşağıdaki yollarla bir bulut Hizmeti performansını test edebilirsiniz:

* Azure tanılama istekler ve bağlantılar hakkında bilgi toplamak ve hizmet Müşteri açısından nasıl gerçekleştireceğini Göster site İstatistikler gözden geçirmek için kullanın. İle başlamak için bkz: [tanılama Azure Cloud Services ve sanal makineler için yapılandırma](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* Visual Studio profil oluşturucu nasıl Hizmeti'ni çalıştıran hesaplama yönlerini ayrıntılı analizini almak için kullanın. Bu konuda açıklandığı gibi bir hizmet Azure'da çalışan performansını ölçmek için profil oluşturucu kullanabilirsiniz. Bir hizmet yerel olarak çalışan bir işlem öykünücüsü performansını ölçmek için profil oluşturucu kullanma hakkında daha fazla bilgi için bkz: [Visual Studio profil oluşturucuişlemöykünücüsükullanarakbirAzurebuluthizmetiyerelolarakperformansınıtest](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Bir performans testi yöntemi seçme
### <a name="use-azure-diagnostics-to-collect"></a>Azure tanılama toplamak için kullanın:
* Web sayfaları veya istekler ve bağlantılar gibi Hizmetleri istatistikleri.
* Roller, rol ne sıklıkta yeniden gibi istatistikleri.
* Genel olarak, atık toplayıcı geçen süreyi veya bellek yüzdesi gibi bellek kullanımı hakkında bilgi çalışan rolü ayarlayın.

### <a name="use-the-visual-studio-profiler-to"></a>Visual Studio profil oluşturucu için kullanın:
* Hangi işlevlerin en zaman belirler.
* Her bir pkı'ya yoğun programının parçası tamamlanma süresini ölçün.
* Bir hizmet iki sürümü için ayrıntılı performans raporları karşılaştırın.
* Bellek ayırma tek tek bellek ayırma düzeyini'den daha ayrıntılı analiz edin.
* Birden çok iş parçacıklı kodda eşzamanlılık sorunları çözümleyin.

Profil Oluşturucu kullandığınızda, bir bulut hizmeti yerel olarak veya Azure içinde çalıştığında verileri toplayabilir.

### <a name="collect-profiling-data-locally-to"></a>Profil oluşturma verileri yerel olarak Topla:
* Gerçekçi benzetilmiş yük gerektirmeyen belirli çalışan rolü yürütme gibi bir bulut hizmeti parçası performansını test etme.
* Bir bulut Hizmeti performansını denetimli koşullarda yalıtım test.
* Azure'a dağıtmadan önce bir bulut Hizmeti performansını test etme.
* Bir bulut Hizmeti performansını özel olarak, var olan dağıtımlar etkilemeden sınayın.
* Hizmet performansını test etmek için Azure'da çalışan ücretlerinin yansıtılmasını olmadan.

### <a name="collect-profiling-data-in-azure-to"></a>Azure için profil oluşturma veri toplama:
* Bir bulut hizmeti benzetimli veya gerçek bir yük altında performansını test etme.
* Bu konuda daha sonra açıklandığı gibi profil oluşturma verilerini toplamanın izleme metodunu kullanın.
* Hizmet performansını test etme hizmeti üretimde çalıştığında olarak aynı ortamda.

Genellikle bir yükü normal altında test bulut Hizmetleri veya stres koşullarında benzetimi.

## <a name="profiling-a-cloud-service-in-azure"></a>Azure bulut hizmetinde profil oluşturma
Visual Studio bulut hizmetinizden yayımladığınızda, hizmet profilini ve istediğiniz bilgi vermek profil oluşturma ayarlarını belirtin. Her bir rol örneği için bir profil oluşturma oturumu başlatıldı. Visual Studio'dan hizmetinizi yayımlama hakkında daha fazla bilgi için bkz: [Visual Studio'dan Azure bulut hizmeti için Yayımlama](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Visual Studio'da performans profili oluşturma hakkında daha fazla bilgi için bkz: [performans profili oluşturma Başlangıç Kılavuzu](https://msdn.microsoft.com/library/azure/ms182372.aspx) ve [profil oluşturma araçları kullanarak uygulama performansını analiz etme](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> IntelliTrace veya Bulut hizmetiniz yayımladığınızda, profil etkinleştirebilirsiniz. Her ikisi de etkinleştiremezsiniz.
> 
> 

### <a name="profiler-collection-methods"></a>Profil Oluşturucu koleksiyon yöntemleri
Farklı koleksiyon yöntemleri performans sorunları hakkında temel profil oluşturma için kullanabilirsiniz:

* **CPU örnekleme** -bu yöntem ilk CPU kullanım sorunları analiz için yararlı olan uygulama istatistikleri toplar. CPU örnekleme çoğu performans araştırmalar başlatmak için önerilen yöntemdir. CPU örnekleme verileri toplarken, profil uygulama düşük bir etkisi yoktur.
* **İzleme** -bu yöntem, odaklı analiz ve giriş/çıkış performans sorunlarını çözümlemek için yararlı olan ayrıntılı zamanlama verileri toplar. İzleme yöntemi her giriş, çıkış ve işlev çağrısı işlevlerin bir modüldeki bir çalıştırma profili oluşturma sırasında kaydeder. Bu yöntem, kodunuzu bir bölümünü hakkında ayrıntılı zamanlama bilgi toplama ve girdi ve çıktı işlemleri uygulama performansı üzerindeki etkisini anlamak için kullanışlıdır. Bu yöntem, 32-bit işletim sistemi çalıştıran bir bilgisayar için devre dışı bırakılır. Bu seçenek, yalnızca bulut hizmeti Azure içinde yerel olarak işlem öykünücüsü çalıştırdığınızda kullanılabilir.
* **.NET bellek ayırma** -örnekleme profili oluşturma yöntemi kullanarak bu yöntem .NET Framework bellek ayırma verileri toplar. Toplanan veriler sayısını ve boyutunu tahsis edilen nesnelerin içerir.
* **Eşzamanlılık** -bu yöntem kaynak çakışması veri ve çok kanallı ve birden çok işlem uygulamaları çözümlenmesinde kullanışlıdır işlemi ve iş parçacığı yürütme verileri toplar. Eşzamanlılık yöntemi blokları yürütme gibi bir iş parçacığı zaman bekler, kodunuzu boşaltılacak Uygulama kaynağı erişimi kilitli her olay için verileri toplar. Bu yöntem, çok iş parçacıklı uygulamalar çözümlemek için kullanışlıdır.
* Etkinleştirebilirsiniz **katman etkileşim profil**, zaman uyumlu ADO.NET yürütme sürelerinin hakkında ek bilgi sağlayan bir veya daha fazla veritabanı ile iletişim çok katmanlı uygulamaların işlevlerinde çağırır. Profil oluşturma yöntemlerinden birini ile Katman etkileşim verileri toplayabilir. Katman etkileşimli profil oluşturma hakkında daha fazla bilgi için bkz: [katman etkileşimleri görünümü](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profil oluşturma ayarlarını yapılandırma
Aşağıda Azure uygulamasını Yayımla iletişim kutusundan, profil oluşturma ayarlarının nasıl yapılandırılacağı gösterilmiştir.

![Profil ayarlarını yapılandır](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> Etkinleştirmek için **profil oluşturmayı etkinleştirmek** onay kutusu, profil oluşturucu bulut hizmetinizi yayımlama için kullandığınız yerel bilgisayarda yüklü olması gerekir. Visual Studio yüklediğinizde varsayılan olarak, profil yüklenir.
> 
> 

### <a name="to-configure-profiling-settings"></a>Profil oluşturma ayarlarını yapılandırmak için
1. Çözüm Gezgini'nde, Azure projeniz için kısayol menüsünü açın ve ardından **Yayımla**. Bir bulut hizmeti yayımlama hakkında ayrıntılı adımlar için bkz: [yayımlama Azure araçlarını kullanarak bir bulut hizmeti](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. İçinde **Azure uygulamasını Yayımla** iletişim kutusunda, seçtiğiniz **Gelişmiş ayarları** sekmesi.
3. Profil etkinleştirmek için seçin **profil oluşturmayı etkinleştirmek** onay kutusu.
4. Profil oluşturma ayarlarını yapılandırmak için tercih **ayarları** köprü. Profil ayarları iletişim kutusu görüntülenir.
5. Gelen **hangi profil oluşturma yöntemini kullanmak istediğiniz** seçenek düğmeleri, gerektiğini bildiren profil türünü seçin.
6. Katman etkileşimli profil oluşturma verilerini toplamak için seçin **etkinleştirmek katman etkileşim profil** onay kutusu.
7. Ayarları kaydetmek üzere seçim yapın **Tamam** düğmesi.
   
    Bu uygulama yayımladığınızda, bu ayarlar her rol için profil oluşturma oturumu oluşturmak için kullanılır.

## <a name="viewing-profiling-reports"></a>Profil oluşturma raporları görüntüleme
Bulut hizmetinizde rolünün her örneği için bir profil oluşturma oturumu oluşturulur. Visual Studio'dan her oturumun profil raporlarınızı görüntülemek için Sunucu Gezgini penceresini görüntülemek ve bir rol örneği seçmek için Azure işlem düğümü seçin. Profil oluşturma raporu daha sonra aşağıdaki çizimde gösterildiği gibi da görüntüleyebilirsiniz.

![Azure'dan profil oluşturma raporunu görüntüleyin](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profil oluşturma raporları görüntülemek için
1. Visual Studio'da Sunucu Gezgini penceresini görüntülemek için Sunucu Gezgini görünümünde, menü çubuğunda seçin.
2. Azure işlem düğümü seçin ve ardından Visual Studio'dan yayımlandığında profiline seçili bulut hizmetini Azure dağıtım düğümünü seçin.
3. Bir örneği için profil oluşturma raporları görüntülemek için rol hizmeti seçin, belirli bir örneği için kısayol menüsünü açın ve ardından **profil oluşturma raporunu görüntüle**.
   
    Rapor, bir .vsp dosyası artık Azure'dan indirilir ve Azure etkinlik günlüğü yükleme durumu görüntülenir. Yükleme tamamlandığında, profil oluşturma raporu Düzenleyicisi adlı Visual Studio için bir sekmede görünür <Role name>  *<Instance Number>*  <identifier>.vsp. Rapor için Özet veriler görüntülenir.
4. Geçerli Görünüm listesinde raporun farklı görünümleri görüntülemek için istediğiniz görünüm türünü seçin. Daha fazla bilgi için bkz: [profil oluşturma Araçlar rapor görünümlerini](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Sonraki adımlar
[Hata ayıklama bulut Hizmetleri](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Bir Azure bulut hizmeti Visual Studio'dan yayımlama](https://msdn.microsoft.com/library/azure/ee460772.aspx)

