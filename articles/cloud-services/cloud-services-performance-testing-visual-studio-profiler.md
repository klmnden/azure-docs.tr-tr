---
title: "Bir bulut hizmeti yerel olarak işlem öykünücüsü'nde profil oluşturma | Microsoft Docs"
services: cloud-services
description: "Visual Studio Profil Oluşturucu ile bulut Hizmetleri performans sorunları araştırmak"
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
tags: 
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 51c8192d8312dc5cf546b4c357aeecf6f19d56b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Bir bulut Hizmeti performansını Visual Studio profil oluşturucu kullanılarak Azure işlem öykünücüsü yerel olarak test etme
Çeşitli araçları ve teknikleri, bulut Hizmetleri performansını test etmek için kullanılabilir.
Azure için bulut hizmeti yayımladığınızda, Visual Studio profil oluşturma verileri toplamak ve ardından analiz olabilir açıklandığı gibi yerel olarak [bir Azure uygulama profili oluşturma][1].
Ayrıca Tanılama performans sayaçları, çeşitli izlemek için açıklandığı gibi kullanabileceğiniz [Azure'da performans sayaçlarını kullanarak][2].
Buluta dağıtmadan önce uygulamanızda yerel olarak işlem öykünücüsü profil isteyebilirsiniz.

Bu makale, öykünücüde yer olarak uygulanabilen, profil oluşturma için CPU Örnekleme yöntemini kapsar. CPU örnekleme, profil oluşturma yöntemi çok zorlayıcı değildir. Belirlenen örnekleme aralığı sırasında profil oluşturucu çağrı yığınının bir anlık görüntüsünü alır. Veriler, bir süre boyunca toplanan ve bir raporda. Profil oluşturma yöntemi, burada bir pkı'ya yoğun uygulamasında CPU işlerin çoğunu yapılır belirtmek eğilimindedir.  Bu, uygulamanızın en uzun süre burada harcama "etkin yolunuzda" odak fırsatı verir.

## <a name="1-configure-visual-studio-for-profiling"></a>1: profil oluşturma için Visual Studio'yu yapılandırma
İlk olarak, profil oluşturma zaman yararlı olabilecek birkaç Visual Studio yapılandırma seçenekleri mevcuttur. Profil oluşturma raporları anlamlı için uygulama ve ayrıca sistem kitaplıkları simgelerini simge (.pdb dosyaları) gerekir. Kullanılabilir simge sunucuları başvuru emin olmak istersiniz. Bunu yapmak için **Araçları** Visual Studio'da menüsünü seçin **seçenekleri**, ardından **hata ayıklama**, ardından **simgeleri**. Microsoft simge sunucuları altında listelendiğini doğrulayın **simge (.pdb) dosya konumları**.  Ayrıca ek simge dosyaları olabilir http://referencesource.microsoft.com/symbols başvuruda bulunabilir.

![Sembol Seçenekleri][4]

İsterseniz, sadece kendi kodumu ayarlayarak profil oluşturucu oluşturur raporları basitleştirebilirsiniz. Böylece çağrıları kitaplıkları ve .NET Framework tamamen iç raporlarını gizli yalnızca My etkin kodla işlevi çağrı yığınları basitleştirilmiştir. Üzerinde **Araçları** menüsünde seçin **seçenekleri**. Ardından **performans araçları** düğümü seçin **genel**. Onay kutusunu seçip **sadece kendi kodumu etkinleştir profil oluşturucu raporlar için**.

![Yalnızca kendi kodum seçenekleri][17]

Bu yönergeler, var olan bir proje veya yeni bir proje ile kullanabilirsiniz.  Aşağıda açıklanan teknikleri denemek için yeni bir proje oluşturursanız, C# seçin **Azure bulut hizmeti** proje ve seçin bir **Web rolü** ve **çalışan rolü**.

![Azure bulut hizmeti projesi rolleri][5]

Örneğin amaçları ekleyin biraz kod projenize çok zaman alır ve bazı bariz performans sorununu gösterir. Örneğin, bir çalışan rolü projesi için aşağıdaki kodu ekleyin:

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

Bu kod çalışan rolün RoleEntryPoint türetilmiş sınıf RunAsync yönteminde çağırmanıza. (Zaman uyumlu olarak çalışan yöntemi hakkında uyarıyı yok sayın.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Derleme ve bulut hizmetinizi yerel olarak (Ctrl + F5) hata ayıklama olmadan kümesine çözüm yapılandırması ile çalıştırma **sürüm**. Bu, tüm dosya ve klasörlerin uygulama yerel olarak çalıştırmak için oluşturulur ve tüm Öykünücüler başlatıldığını sağlar sağlar. İşlem öykünücüsü kullanıcı Arabiriminde çalışan rolünüzün çalıştığını doğrulamak için görev çubuğundan başlatın.

## <a name="2-attach-to-a-process"></a>2: bir işlem ekleme
Visual Studio 2010 IDE'den başlatarak uygulama profil yerine, bir çalışan işlemi için profil oluşturucu ekleme gerekir. 

Üzerinde bir işlemi için profil oluşturucu ekleme için **Çözümle** menüsünde seçin **profil oluşturucu** ve **Attach/Detach**.

![Profil seçeneği ekleme][6]

Çalışan rolü için WaWorkerHost.exe işlem bulunamadı.

![WaWorkerHost işlemi][7]

Proje klasörünüzdeki bir ağ sürücüsündeyse profil oluşturucu profil oluşturma raporları kaydetmek için başka bir konum sağlamak üzere istenir.

 Bir web rolü için WaIISHost.exe ekleyerek de ekleyebilirsiniz.
Uygulamanızda birden çok rol işçi varsa, ProcessId bunları ayırt etmek için kullanmanız gerekebilir. İşlem nesnesi erişerek ProcessId program aracılığıyla sorgulayabilirsiniz. Örneğin, bir roldeki RoleEntryPoint türetilmiş sınıf Run yöntemi için bu kodu eklerseniz, bağlanmak için hangi işlemin bilmeniz işlem öykünücüsü UI günlüğüne bakabilirsiniz.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Günlüğünü görüntülemek için işlem öykünücüsü kullanıcı arabirimini Başlat.

![İşlem öykünücüsü kullanıcı arabirimini Başlat][8]

Çalışan rolü günlük konsol penceresi işlem öykünücüsü kullanıcı Arabiriminde, konsol penceresinin başlık çubuğunda tıklatarak açın. İşlem kimliği günlüğünde görebilirsiniz.

![Görünüm işlem kimliği][9]

Bir bağlı adımları gerçekleştirir, uygulamanın kullanıcı Arabiriminde (gerekirse) senaryoyu yeniden oluşturma.

Profil oluşturmayı durdurmak istediğinizde, belirleyin **durdurmak profil** bağlantı.

![Seçeneği profil oluşturmayı durdurmak][10]

## <a name="3-view-performance-reports"></a>3: performans raporları görüntüleyin
Uygulamanız için performans raporu görüntülenir.

Bu noktada, profil oluşturucu yürütülürken durdurur, veri .vsp dosyası olarak kaydeder ve bu verilerin analizini gösteren bir rapor görüntüler.

![Profil Oluşturucu raporu][11]

Sık kullanılan yolundaki String.wstrcpy görürseniz, sadece kendi kullanıcı kodu yalnızca göstermek için görünümü değiştirmek için kodumu üzerinde'ı tıklatın.  String.Concat görürseniz, tüm kod Göster düğmesine basarak deneyin.

Birleştir yöntemi ve yürütme süresi büyük bir kısmı alma String.Concat görmeniz gerekir.

![Analiz raporu][12]

Bu makalede dize birleştirme kod eklediyseniz, bunun için bir uyarı görev listesinde görmeniz gerekir. Oluşturulan ve atıldı dizeleri sayısı nedeniyle çöp toplama aşırı miktarda olduğunu belirten bir uyarı da görebilirsiniz.

![Performans uyarıları][14]

## <a name="4-make-changes-and-compare-performance"></a>4: değişiklikleri yapın ve performans karşılaştırma
Önce ve sonra bir kod değişikliği performans de karşılaştırabilirsiniz.  Çalışan işlemi durdurmak ve dize birleştirme işlemi StringBuilder kullanımı ile değiştirmek için kod düzenleyin:

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

Başka bir performans çalışma yapın ve ardından performans karşılaştırın. Performans Explorer'ın çalıştığı aynı oturumda varsa, yalnızca her iki raporu seçebilir, kısayol menüsünü açın ve seçin **karşılaştırmak performans raporları**. Başka bir performans oturumda çalıştırılan karşılaştırma yapmak isterseniz, açmak **Çözümle** menüsünde ve **karşılaştırmak performans raporları**. Görüntülenen iletişim kutusunda her iki dosya belirtin.

![Performans raporları seçeneği Karşılaştır][15]

Raporları iki çalışması arasındaki farklar vurgulayın.

![Karşılaştırma raporu][16]

Tebrikler! Profil Oluşturucu ile başladıktan.

## <a name="troubleshooting"></a>Sorun giderme
* Yayın derlemesi profil ve hata ayıklama olmadan Başlat emin olun.
* Profil Oluşturucu menüsünde Attach/Detach seçeneği etkin değilse, performans Sihirbazı'nı çalıştırın.
* Uygulamanızın durumunu görüntülemek için işlem öykünücüsü kullanıcı arabirimini kullanın. 
* Öykünücüde uygulamaları başlatma ile ilgili sorunlar varsa veya profil oluşturucu ekleme Kapat işlem öykünücüsü aşağı ve yeniden başlatın. Bu sorunu çözmezse, yeniden başlatmayı deneyin. Askıya alma ve çalışan dağıtımları kaldırmak için işlem öykünücüsü kullanıyorsanız bu sorun oluşabilir.
* Özellikle genel ayarları, komut satırından profil oluşturma komutlardan herhangi birini kullandıysanız, VSPerfClrEnv /globaloff çağrıldı ve VsPerfMon.exe kapatıldı emin olun.
* Örnekleme yaparken iletisini görürseniz, "PRF0025: Toplanan hiçbir veriler,", ekli işlem CPU etkinliği olup olmadığını denetleyin. Herhangi bir hesaplama iş yapmamanın uygulamaları herhangi bir örnekleme veri oluşturamayabilir.  Tüm örnekleme yapıldığı önce işlem çıktı mümkündür. Run yöntemi profil bir rol için değil sonlandırmak denetleyin.

## <a name="next-steps"></a>Sonraki Adımlar
Azure ikili öykünücü dosyalarda düzenleme Visual Studio Profil Oluşturucusu'nda desteklenmez, ancak bellek ayırma test etmek isterseniz, profil olduğunda bu seçeneği seçebilirsiniz. Ayrıca eşzamanlılık profili oluşturma, iş parçacıkları performans sorunlarının en sık veri katmanı ve çalışan rolü arasındaki bir uygulama katmanları arasında kullanılırken izlemenize yardımcı olan kilitleri veya katman etkileşim profil, rekabet zaman olup olmadığını belirlemenize yardımcı olan tercih edebilirsiniz.  Uygulamanızı oluşturur veritabanı sorgularını görüntüleyin ve veritabanı kullanımınızı iyileştirmek için profil oluşturma verileri kullanın. Katman etkileşimli profil oluşturma hakkında daha fazla bilgi için blog gönderisine bakın [izlenecek yol: Visual Studio Team System 2010'katman etkileşim Profiler kullanarak][3].

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
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
