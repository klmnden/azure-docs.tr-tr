---
title: İşlem öykünücüsü'nde yerel olarak bir bulut hizmetinin profilini oluşturmayı | Microsoft Docs
services: cloud-services
description: Visual Studio Profil Oluşturucu ile bulut hizmetlerinde performans sorunlarını araştırmak
documentationcenter: ''
author: mikejo
manager: douge
editor: ''
tags: ''
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: mikejo
ms.openlocfilehash: 40ba5814bce08037b9e4d0787defbab4d02e58df
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128575"
---
# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Visual Studio Profiler'ı kullanarak Azure işlem öykünücüsü'nde bir bulut hizmetinin performansını yerel olarak test etme
Çeşitli araçları ve teknikleri, bulut Hizmetleri performansını test etmek için kullanılabilir.
Azure'a bir bulut hizmetinde yayımladığınızda, Visual Studio profil oluşturma verilerini toplamak ve daha sonra analiz olabilir bölümünde anlatıldığı gibi yerel olarak [Azure uygulama profili oluşturma][1].
Tanılama performans sayaçları, çeşitli izlemek için açıklandığı gibi kullanabilirsiniz [Azure'da performans sayaçlarını kullanarak][2].
Uygulamanızı buluta dağıtmadan önce işlem öykünücüsü'nde yerel olarak profilini oluşturmak isteyebilirsiniz.

Bu makale, öykünücüde yer olarak uygulanabilen, profil oluşturma için CPU Örnekleme yöntemini kapsar. CPU örnekleme, profil oluşturma yöntemi çok elverişsiz olmadığından ' dir. Belirtilen örnekleme aralıkta, profil oluşturucu çağrı yığını anlık görüntüsünü alır. Veriler, bir zaman dönemi boyunca toplanır ve bir raporda. Burada bir işlem bakımından yoğun uygulamasında en çok CPU çalışmasının yapıldığını belirtmek için bu yöntem, profil oluşturma eğilimi gösterir.  Bu işlem, burada, uygulamanızın en çok zaman harcıyorsa "etkin yolda" fırsatı sağlar.

## <a name="1-configure-visual-studio-for-profiling"></a>1: Profil oluşturma için Visual Studio'yu yapılandırma
İlk olarak, profil oluşturma sırasında yardımcı olabilecek birkaç Visual Studio yapılandırma seçeneği vardır. Profil oluşturma raporları anlamlı için uygulama ve ayrıca sistem kitaplıkları için semboller için simgeleri (.pdb dosyaları) gerekir. Kullanılabilir sembol sunucuları başvuru emin olmak istersiniz. Bunu yapmak için **Araçları** Visual Studio menüsünde **seçenekleri**, ardından **hata ayıklama**, ardından **sembolleri**. Microsoft sembol sunucuları altında listelendiğini doğrulayın **sembol dosyası (.pdb) konumlar**.  Ayrıca başvurabilirsiniz https://referencesource.microsoft.com/symbols, ek sembol dosyaları olabilir.

![Sembol Seçenekleri][4]

İsterseniz, yalnızca kendi kodum ayarlayarak, profil oluşturucu oluşturur raporları basitleştirebilir. Raporlardan çağrıları iç kitaplıkları ve .NET Framework için tamamen gizlidir, yalnızca kendi kodum ile çağrı yığınlarını işlevi basitleştirilmiştir. Üzerinde **Araçları** menüsünde seçin **seçenekleri**. Ardından **performans araçları** düğümünü seçip **genel**. Onay kutusunu seçip **yalnızca benim kodumu etkinleştir profil oluşturucusu raporu için**.

![Yalnızca kendi kodum seçenekleri][17]

Bu yönergeler, var olan bir proje veya yeni bir proje ile kullanabilirsiniz.  Aşağıda açıklanan olan tekniklerle denemek için yeni bir proje oluşturursanız, C# seçin **Azure bulut hizmeti** proje ve seçin bir **Web rolü** ve **çalışan rolü**.

![Azure Cloud Service projesi rolleri][5]

Örneğin amaçları ekleyin bazı kod projenize çok zaman alan ve bazı belirgin bir performans sorununu gösterir. Örneğin, bir çalışan rolü projesi için aşağıdaki kodu ekleyin:

```csharp
public class Concatenator
{
    public static string Concatenate(int number)
    {
        int count;
        string s = "";
        for (count = 0; count < number; count++)
        {
            s += "\n" + count.ToString();
        }
        return s;
    }
}
```

Bu kod çalışan rolün RoleEntryPoint türetilmiş sınıf RunAsync yönteminde çağırmanıza. (Yöntemin zaman uyumlu olarak çalışan ilgili uyarıyı yoksay)

```csharp
private async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following with your own logic.
    while (!cancellationToken.IsCancellationRequested)
    {
        Trace.TraceInformation("Working");
        Concatenator.Concatenate(10000);
    }
}
```

Bulut hizmetinizi yerel olarak oluşturup (Ctrl + F5), hata ayıklama olmadan kümesine çözüm yapılandırması ile çalıştırmak **yayın**. Bu, tüm dosya ve klasörlerin uygulama yerel olarak çalıştırmak için oluşturulur ve tüm öykünücüleri başladığını sağlar sağlar. İşlem öykünücüsü kullanıcı Arabiriminde, çalışan rolünüzü çalıştığını doğrulamak için görev çubuğundan başlatın.

## <a name="2-attach-to-a-process"></a>2: Bir işleme
Visual Studio 2010 IDE'den başlayarak bir uygulama profili oluşturma yerine profil oluşturucuyu çalışan bir işleme eklemeniz gerekir. 

Üzerinde bir işlem için profil oluşturucuyu eklemek **Çözümle** menüsünde seçin **Profiler** ve **Attach/Detach**.

![Profil seçeneği ekleme][6]

Bir çalışan rolü için WaWorkerHost.exe işlemi bulun.

![WaWorkerHost işlemi][7]

Proje klasörünüzdeki bir ağ sürücüsündeyse profil oluşturucu profil oluşturma raporları kaydetmek için başka bir konum girmenizi ister.

 İçin bir web rolü için WaIISHost.exe ekleyerek de ekleyebilirsiniz.
Uygulamanızda birden çok çalışan rolü işlem varsa, bunları ayırt etmek için ProcessId kullanmanız gerekir. İşlem nesnesi erişerek ProcessId programlı olarak sorgulayabilir. Örneğin, bir rol RoleEntryPoint türetilmiş sınıfın yöntemini bu kod ekleme, günlük bağlanmak için hangi işlemin bilmek işlem öykünücüsü kullanıcı arabiriminde göz atabilirsiniz.

```csharp
var process = System.Diagnostics.Process.GetCurrentProcess();
var message = String.Format("Process ID: {0}", process.Id);
Trace.WriteLine(message, "Information");
```

Günlüğü görüntülemek için işlem öykünücüsü kullanıcı arabirimini Başlat.

![İşlem öykünücüsü kullanıcı arabirimini Başlat][8]

Çalışan rolü günlük konsol penceresinde işlem öykünücüsü kullanıcı Arabiriminde konsol penceresinin başlık çubuğunda tıklayarak açın. İşlem kimliği günlüğünde görebilirsiniz.

![Görünüm işlem kimliği][9]

Bir bağlı adımları gerçekleştirin, uygulamanızın kullanıcı Arabiriminde (gerekirse) senaryoyu yeniden oluşturma.

Profil oluşturmayı durdurmak istediğinizde, seçin **profil oluşturmayı Durdur** bağlantı.

![Seçeneği profil oluşturmayı durdur][10]

## <a name="3-view-performance-reports"></a>3: Performans raporları görüntüleme
Uygulamanız için performans raporu gösterilir.

Bu noktada, profil oluşturucu, yürütme durdurur, .vsp dosyaya veri kaydeder ve bu verilerin analizini gösteren bir rapor görüntüler.

![Profiler raporu][11]

Sık erişimli yolunda String.wstrcpy görürseniz, yalnızca kullanıcı kodunun gösterecek şekilde görünümü değiştirmek için kodum üzerinde tıklayın.  String.Concat görürseniz, tüm kod Göster düğmesine basarak deneyin.

Birleştir yöntemi ve yürütme süresi büyük bir kısmı alma String.Concat görmeniz gerekir.

![Raporun analizi][12]

Bu makalede Dize bitiştirme kod eklediyseniz, bu görev listesindeki bir uyarı görmeniz gerekir. Oluşturulan ve elden dizeleri nedeniyle numarasıdır çöp toplama, aşırı miktarda olduğunu belirten bir uyarı da görebilirsiniz.

![Performans uyarıları][14]

## <a name="4-make-changes-and-compare-performance"></a>4: Değişiklik yapmak ve performans karşılaştırın
Önce ve bir kod değişikliğinden sonra performans de karşılaştırabilirsiniz.  Çalışan işlemi durdurur ve StringBuilder kullanımını dize birleştirme işlemi değiştirmek için kodu girin:

```csharp
public static string Concatenate(int number)
{
    int count;
    System.Text.StringBuilder builder = new System.Text.StringBuilder("");
    for (count = 0; count < number; count++)
    {
        builder.Append("\n" + count.ToString());
    }
    return builder.ToString();
}
```

Başka bir performans çalıştırma yapın ve ardından performansını karşılaştırın. Performans Gezgini içinde aynı oturumda çalışır olması durumunda, yalnızca her iki rapor seçebilir, kısayol menüsünü açın ve seçin **Performans raporlarını Karşılaştır**. Başka bir performans oturumu çalıştırmasında karşılaştırmak istiyorsanız, açık **Çözümle** menüsünde ve **Performans raporlarını Karşılaştır**. Görüntülenen iletişim kutusunda her iki dosyayı belirtin.

![Performans raporları seçeneği karşılaştırın][15]

İki çalıştırma arasında farklar raporları vurgulayın.

![Karşılaştırma raporu][16]

Tebrikler! Profil Oluşturucu ile çalışmaya.

## <a name="troubleshooting"></a>Sorun giderme
* Yayın derlemesi profil ve hata ayıklama olmadan Başlat emin olun.
* Profiler menüsünde Attach/Detach seçeneği etkin değilse performans sihirbazını çalıştırın.
* Uygulamanızın durumunu görüntülemek için işlem öykünücüsü kullanıcı arabirimini kullanın. 
* Öykünücüde uygulama başlatma ile ilgili sorunlar varsa, veya profil oluşturucu iliştirme kapatmak için işlem öykünücüsünü aşağı ve yeniden başlatın. Bu sorunu çözmezse, yeniden başlatmayı deneyin. Askıya alma ve çalışan dağıtımları kaldırmak için işlem öykünücüsü kullanıyorsanız, bu sorun oluşabilir.
* Özellikle genel ayarlar, komut satırından profil oluşturma komutlardan herhangi birini kullandıysanız VSPerfClrEnv /globaloff çağrıldıktan ve VsPerfMon.exe kapatıldı emin olun.
* Örnekleme, iletiyi görürseniz "PRF0025: Hiçbir veri toplanamadı,", iliştirilmiş işlem CPU etkinliği olup olmadığını denetleyin. Tüm hesaplama işleri yapmamanın uygulamaları herhangi bir örnekleme veri oluşturamayabilir.  Önce tüm örnekleme yapıldığı işlem çıkıldı mümkündür. Run yöntemi, profil bir role değil sonlandırmak denetleyin.

## <a name="next-steps"></a>Sonraki Adımlar
Visual Studio Profil Oluşturucusu'nda Azure ikili dosyaları öykünücüsünde Alet düzeni desteklenmiyor ancak bellek ayırma test etmek isterseniz, profil oluşturma sırasında bu seçeneği seçebilirsiniz. De eşzamanlılık profil oluşturması, iş parçacıkları için kilit zaman rekabet kaybettikten olup olmadığını belirlemek ya da katman etkileşimli profil oluşturma, bir uygulamanın katmanlar arasında etkileşim kurulurken performans sorunları izlemenize yardımcı olan en yardımcı olan seçebilirsiniz sık sık veri katmanı ve bir çalışan rolü arasında.  Uygulamanızı oluşturan veritabanı sorgularını görüntüleyin ve profil oluşturma verilerinin veritabanı kullanımını iyileştirmek için kullanın. Katman etkileşim profili oluşturma hakkında daha fazla bilgi için blog gönderisine bakın [izlenecek yol: Visual Studio'da Katman etkileşimi Profiler'ı kullanarak Team System 2010][3].

[1]: https://docs.microsoft.com/azure/application-insights/app-insights-profiler
[2]: https://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: https://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
