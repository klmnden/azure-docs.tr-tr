---
title: Veri bilimi kodu Azure'da UCI yetişkinlere yönelik gelir tahmin kümesiyle - Team Data Science Process ve Visual Studio Team Services test etme
description: Veri bilimi kodu UCI yetişkinlere yönelik gelir tahmin verileriyle test etme
services: machine-learning, team-data-science-process
documentationcenter: ''
author: weig
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2018
ms.author: weig
ms.openlocfilehash: 46d156ce09b1ebcdcceb27ede6e7fa1595d30da6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439506"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Veri bilimi kodu UCI yetişkinlere yönelik gelir tahmin veri kümesi ile test etme
Bu makalede, kodu test etmek için bir veri bilimi iş akışında başlangıç yönergeleri sağlar. Veri bilimcileri, bu tür bir testi beklenen sonuç kodlarını ve kalite kontrol etmek için sistematik ve etkili bir yol sağlar. Team Data Science işlem (TDSP) kullanıyoruz [UCI yetişkinlere yönelik gelir veri kümesini kullanan proje](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) biz nasıl kodu test yapılabilir göstermek için daha önce yayımlanmış. 

## <a name="introduction-on-code-testing"></a>Sınama kodu giriş
"Birim testi", yazılım geliştirme çalışmalarını uzun süredir davam bir uygulamadır. Ancak veri bilimi için hangi, NET değildir anlamına gelir ve nasıl kod bir veri bilimi yaşam döngüsünün farklı aşamalarında gibi test etmelisiniz:

* Veri hazırlama
* Veri Kalitesi İnceleme
* Modelleme
* Model dağıtımı 

Bu makale, "" kod testiyle."birim testi" terimi yerine geçer Belirli bir adımında bir veri bilimi yaşam döngüsü için kod, "bekleniyor." olarak sonuçları üreten, değerlendirmek için yardımcı işlevleri test başvuruyor Test "beklendiği gibi" nedir tanımlar bağlı olarak işlevi--sonucunu örneğin yazıyor kişiye, veri kalite denetimi veya model.

Bu makalede, başvuru faydalı kaynaklar sağlar.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Test çerçevesi için Visual Studio Team Services
Bu makalede, Visual Studio Team Services (VSTS) kullanarak testi otomatikleştirme ve gerçekleştirmek açıklar. Diğer araçları kullanmaya karar verebilirsiniz. VSTS kullanarak otomatik bir derlemeyi Ayarla ve yapı aracılarını nasıl da gösteriyoruz. Derleme aracıları için Azure veri bilimi sanal makineleri (Dsvm'leri) kullanıyoruz.

## <a name="flow-of-code-testing"></a>Kodu test akışı
Genel iş akışını bir veri bilimi proje test kod şöyle görünür: 

![Kodu test akış çizelgesi](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Ayrıntılı adımlar

Ayarlanmış ve bir yapı aracısı ve VSTS kullanarak kodu test etme ve otomatik bir yapı çalıştırmak için aşağıdaki adımları kullanın:

1. Visual Studio masaüstü uygulaması, bir proje oluşturun:

    ![Visual Studio'da "Yeni Proje oluştur" Ekran](./media/code-test/create_project.PNG)

   Projenizi oluşturduktan sonra sağ bölmeden Çözüm Gezgini'nde bulabilirsiniz:
    
    ![Bir proje oluşturma adımları](./media/code-test/create_python_project_in_vs.PNG)

    ![Çözüm Gezgini](./media/code-test/solution_explorer_in_vs.PNG)

1. Proje kodunuzu VSTS proje kod deposuna akış: 

    ![Proje kod deposu](./media/code-test/create_repo.PNG)

1. Veri alımı, özellik Mühendisliği ve etiket sütun oluşturma gibi bazı veri hazırlama çalışmalarınız yaptığınızı varsayalım. Kodunuzu beklediğiniz sonuçlar üretiyor emin olmanız gerekir. Veri işleme kodunu doğru şekilde çalışıp çalışmadığını sınamak için kullanabileceğiniz bazı kodlar aşağıda verilmiştir:

    * Sütun adlarının doğru olduğunu kontrol edin:
    
      ![Sütun adlarını eşleştirmek için kod](./media/code-test/check_column_names.PNG)

    * Yanıt düzeyleri doğru olduğundan emin olun:

      ![Düzeylerini eşleştirmek için kod](./media/code-test/check_response_levels.PNG)

    * Yanıt yüzdesi makul olup olmadığını denetleyin:

      ![Kod yanıt yüzdesi](./media/code-test/check_response_percentage.PNG)

    * Her bir sütunun veri eksik oranı denetleyin:
    
      ![Kod için eksik oranı](./media/code-test/check_missing_rate.PNG)


1. Veri işleme ve mühendislik özellik çalışmalarına yaptıktan ve iyi bir modeli eğittiğimize sonra eğitilen modeli yeni veri kümelerini doğru Puanlama emin emin olun. Aşağıdaki iki sınama için öngörü düzeyleri ve etiket değerlerini dağıtımını kontrol etmek için kullanabilirsiniz:

    * Öngörü düzeyleri denetleyin:
    
      ![Kod öngörü düzeyleri](./media/code-test/check_prediction_levels.PNG)

    * Dağıtım tahmin değerleri kontrol edin:

      ![Kod tahmin değerleri](./media/code-test/check_prediction_values.PNG)

1. Tüm test işlevleri birlikte adlı bir Python komut dosyasına put **test_funcs.py**:

    ![Test işlevleri için Python betiği](./media/code-test/create_file_test_func.PNG)


1. Test kodları hazırladıktan sonra Visual Studio test ortamında ayarlayabilirsiniz.

   Adlı bir Python dosyası oluşturun **test1.py**. Bu dosyada, yapmak istediğiniz tüm testleri içeren bir sınıf oluşturun. Aşağıdaki örnek, altı testleri hazırlanmış gösterir:
    
    ![Bir sınıftaki testleri listesiyle soubor Pythonu](./media/code-test/create_file_test1_class.PNG)

1. Koyarsanız bu testler otomatik olarak bulunabileceğini **codetest.testCase** sonra sınıf adı. Sağ bölmede Test Gezgini'ni açın ve seçin **tümünü Çalıştır**. Tüm testler sırayla çalışır ve testin başarılı olup olmadığını size bildirir.

    ![Testleri çalıştırma](./media/code-test/run_tests.PNG)

1. Proje deposu için Git komutlarını kullanarak iade edin. En son işi, kısa bir süre sonra VSTS'de yansıtılır.

    ![Kodu iade etme Git komutları](./media/code-test/git_check_in.PNG)

    ![VSTS en son iş](./media/code-test/git_check_in_most_recent_work.PNG)

1. Otomatik yapı ayarlayın ve VSTS'de test:

    a. Proje deposu seçin **derleme ve yayın**ve ardından **+ yeni** yeni bir yapı işlemi oluşturmak için.

       ![Selections for starting a new build process](./media/code-test/create_new_build.PNG)

    b. Kaynak kodu konumu, proje adı, depo ve dal bilgilerini seçmek için yönergeleri izleyin.
    
       ![Source, name, repository, and branch information](./media/code-test/fill_in_build_info.PNG)

    c. Bir şablon seçin. Hiçbir Python proje şablonu olduğundan seçerek başlayın **boş işlem**. 

       ![List of templates and "Empty process" button](./media/code-test/start_empty_process_template.PNG)

    d. Derleme adı ve Aracı'nı seçin. Yapı işlemini tamamlamak için bir DSVM kullanmak istiyorsanız, varsayılan buradan seçebilirsiniz. Ayar aracıları hakkında daha fazla bilgi için bkz. [derleme ve yayın aracıları](https://docs.microsoft.com/vsts/build-release/concepts/agents/agents?view=vsts).
    
       ![Build and agent selections](./media/code-test/select_agent.PNG)

    e. Seçin **+** bu derleme aşaması için bir görev eklemek için sol bölmedeki. Python betiğini çalıştırılacak yapacağız çünkü **test1.py** tüm denetimleri tamamlamak için bu görev bir PowerShell komutu Python kodu çalıştırmak için kullanıyor.
    
       !["Add tasks" pane with PowerShell selected](./media/code-test/add_task_powershell.PNG)

    f. PowerShell ayrıntılarında adı ve PowerShell sürümü gibi gerekli bilgileri doldurun. Seçin **satır içi betik** türü. 
    
       In the box under **Inline Script**, you can type **python test1.py**. Make sure the environment variable is set up correctly for Python. If you need a different version or kernel of Python, you can explicitly specify the path as shown in the figure: 
    
       ![PowerShell details](./media/code-test/powershell_scripts.PNG)

    g. Seçin **Kaydet ve kuyruğa** derleme tanımı işlemini tamamlamak için.

       !["Save & queue" button](./media/code-test/save_and_queue_build_definition.PNG)

Artık her seferinde yeni bir işleme kodu depoya gönderildiğinde, yapı işlemi otomatik olarak başlatılacak. (Ana depo burada kullanıyoruz, ancak herhangi bir dala tanımlayabilirsiniz.) İşlem sürerken **test1.py** kod içinde tanımlanan her şeyin düzgün çalıştığından emin olmak için aracı makinede dosyasında. 

Uyarılar doğru şekilde ayarlanmışsa, derleme tamamlandığında e-posta ile bildirilir. Ayrıca vsts'de yapı durumunu kontrol edebilirsiniz. Başarısız olursa, derleme ayrıntılarını kontrol edin ve hangi parçanın bozuk olduğunu öğrenin.

![Yapı Başarı e-posta bildirimi](./media/code-test/email_build_succeed.PNG)

![VSTS bildirim derleme başarısı](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [UCI gelir tahmin depo](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) veri bilimi senaryoları için birim testleri somut örnekleri için.
* Önceki anahat ve kendi veri bilimi projeleri UCI gelir tahmin senaryoda örneklerden izleyin.

## <a name="references"></a>Başvurular
* [Team Data Science Process](https://aka.ms/tdsp)
* [Visual Studio Test Araçları](https://www.visualstudio.com/vs/features/testing-tools/)
* [VSTS test kaynakları](https://www.visualstudio.com/team-services/)
* [Veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)