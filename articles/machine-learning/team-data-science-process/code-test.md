---
title: Azure üzerinde UCI yetişkin geliri tahmin ile dataset - takım veri bilimi işlemi ve Visual Studio Team Services sınama veri bilimi kodu
description: Veri bilimi kod UCI yetişkin geliri tahmin verileri ile test etme
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
ms.openlocfilehash: de1ed0b85957413a254503fc72375866dfd1bea1
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837166"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Veri bilimi kod UCI yetişkin geliri tahmin veri kümesi ile test etme
Bu makalede, bir veri bilimi akışı kodu test etmek için ön yönergeleri sağlar. Bu tür sınama veri bilimcilerine kalitesini ve kendi kod beklenen sonucu denetlemek için sistematik ve verimli bir yol sağlar. Bir takım veri bilimi işlem (TDSP) kullanıyoruz [UCI yetişkin gelir veri kümesi kullanan proje](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) biz kodu test etme nasıl yapılabilir göstermek için daha önce yayımlanan. 

## <a name="introduction-on-code-testing"></a>Kodu test etme hakkında giriş
"Birim testi" yazılım geliştirme için döngümüzün bir uygulamadır. Ancak veri bilimi için hangi Temizle olmayan genellikle anlamına gelir ve nasıl, kodu farklı bir veri bilimi yaşam döngüsünün aşamaları gibi test etmeniz gerekir:

* Veri hazırlama
* Veri Kalitesi İnceleme
* Modelleme
* Model dağıtımı 

Bu makalede "Birim"kod testi ile."test" terimi değiştirir. "Beklendiği." sonuçları veri bilimi yaşam döngüsünün belirli adım için kod üretme olmadığını değerlendirmek için yardımcı işlevleri test etme için başvurur Test "beklendiği gibi" nedir tanımlar--işlevi sonuca bağlı olarak örneğin yazıyor kişiye, veri kalitesi denetimi veya model.

Bu makalede, kullanışlı kaynaklar olarak başvuru sağlar.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Test framework için Visual Studio Team Services
Bu makalede, Visual Studio Team Services (VSTS) kullanarak testleri otomatikleştirmek ve gerçekleştirmek açıklar. Alternatif araçlarını kullanmaya karar verebilirsiniz. Biz de VSTS kullanarak bir otomatik bir yapı ayarlayın ve yapı aracılarını gösterilmektedir. Derleme aracıları için Azure veri bilimi sanal makineler (DSVMs) kullanın.

## <a name="flow-of-code-testing"></a>Kodu test etme akışı
Veri bilimi projesindeki sınama kodu genel iş akışı şu şekildedir: 

![Kodu test etme, akış çizelgesi](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Ayrıntılı adımlar

Ayarlama ve kodu test etme ve otomatik derleme bir yapı aracısı ve VSTS kullanarak çalıştırmak için aşağıdaki adımları kullanın:

1. Visual Studio masaüstü uygulaması, bir proje oluşturun:

    ![Visual Studio ekranında "Yeni proje oluşturma"](./media/code-test/create_project.PNG)

   Projenizi oluşturduktan sonra sağ bölmede Çözüm Gezgini'nde bulabilirsiniz:
    
    ![Proje oluşturma adımları](./media/code-test/create_python_project_in_vs.PNG)

    ![Çözüm Gezgini](./media/code-test/solution_explorer_in_vs.PNG)

3. Proje kodunuza VSTS proje kodunu depoya akış: 

    ![Proje kodunu deposu](./media/code-test/create_repo.PNG)

4. Veri alımı, özellik Mühendisliği ve etiket sütunlar oluşturma gibi bazı veri hazırlık çalışması yaptığınızı varsayalım. Kodunuzu beklediğiniz sonuç üretiyor emin olmak istersiniz. Veri işleme kod düzgün çalışıp çalışmadığını sınamak için kullanabileceğiniz bazı kod aşağıdadır:

    * Sütun adlarının doğru olduğunu denetleyin:
    
      ![Sütun adlarını eşleştirmek için kod](./media/code-test/check_column_names.PNG)

    * Yanıt düzeyleri doğru olduğundan emin olun:

      ![Düzeyleri eşleştirmek için kod](./media/code-test/check_response_levels.PNG)

    * Yanıt yüzdesi makul olup olmadığını denetleyin:

      ![Kod yanıt yüzdesi](./media/code-test/check_response_percentage.PNG)

    * Her sütunun veri eksik oranını denetleyin:
    
      ![Kod eksik hızı](./media/code-test/check_missing_rate.PNG)


5. Veri işleme ve özellik Mühendisliği iş yaptıktan ve iyi bir modeli eğittiğimize sonra eğitilmiş model yeni veri kümeleri doğru puan olduğundan emin olun. Tahmin düzeyleri ve etiket değerlerini dağıtımını denetlemek için aşağıdaki iki sınama kullanabilirsiniz:

    * Tahmin düzeyleri denetleyin:
    
      ![Tahmin düzeyleri denetleme kodu](./media/code-test/check_prediction_levels.PNG)

    * Tahmin değerleri dağıtımını kontrol edin:

      ![Tahmin değerleri denetleme kodu](./media/code-test/check_prediction_values.PNG)

6. Tüm test işlevleri birlikte adlı bir Python komut dosyasına put **test_funcs.py**:

    ![Test işlevleri için Python komut dosyası](./media/code-test/create_file_test_func.PNG)


7. Test kodları hazırladıktan sonra Visual Studio test ortamında ayarlayabilirsiniz.

   Adlı bir Python dosyası oluşturun **test1.py**. Bu dosyada yapmak istediğiniz tüm testleri içeren bir sınıf oluşturun. Aşağıdaki örnek, hazırlanan altı testleri gösterir:
    
    ![Bir sınıf testlerinde listesini içeren Python dosyası](./media/code-test/create_file_test1_class.PNG)

8. Bu testler, yerleştirirseniz otomatik olarak bulunabileceğini **codetest.testCase** sonra sınıf adı. Sağ bölmede Test Explorer'ı açın ve seçin **tümünü Çalıştır**. Tüm testleri sıralı olarak çalışır ve test başarılı olup olmadığını bildirir.

    ![Testleri çalıştırma](./media/code-test/run_tests.PNG)

9. Proje deponuza kodunuzda Git komutlarını kullanarak denetleyin. En son iş kısa süre içinde VSTS yansıtılır.

    ![Kodda denetleme Git komutları](./media/code-test/git_check_in.PNG)

    ![VSTS en son iş](./media/code-test/git_check_in_most_recent_work.PNG)

10. Otomatik bir yapı ayarlayın ve VSTS içinde test edin:

    a. Proje deposu seçmeye **derleme ve sürüm**ve ardından **+ yeni** yeni bir derleme işlemi oluşturmak için.

       ![Yeni bir derleme işlemi başlatmak için seçimleri](./media/code-test/create_new_build.PNG)

    b. Kaynak kodu konumu, proje adı, depo ve şube bilgilerini seçmek için istemleri izleyin.
    
       ![Kaynak, ad, depo ve dal bilgisi](./media/code-test/fill_in_build_info.PNG)

    c. Bir şablon seçin. Hiçbir Python proje şablonu olduğundan seçerek başlayın **boş işlem**. 

       ![Şablonlar ve "işlemi boş" düğmesi listesi](./media/code-test/start_empty_process_template.PNG)

    d. Derleme adı ve Aracısı'nı seçin. Derleme işlemi sonlandırmak için bir DSVM kullanmak istiyorsanız varsayılan burada seçebilirsiniz. Ayar aracıları hakkında daha fazla bilgi için bkz: [derleme ve sürüm aracıları](https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/agents?view=vsts).
    
       ![Derleme ve aracı seçimleri](./media/code-test/select_agent.PNG)

    e. Seçin **+** bu yapı aşama için bir görev eklemek için sol bölmede. Python komut dosyasını çalıştırmak için yapacağız çünkü **test1.py** tüm denetimler bitirmek için bu görev bir PowerShell komut Python kodu çalıştırmak için kullanıyor.
    
       !["Görevleri Ekle" bölmesinde seçili PowerShell ile](./media/code-test/add_task_powershell.PNG)

    f. PowerShell sürümü ve adı gibi gerekli bilgileri PowerShell ayrıntıları doldurun. Seçin **satır içi betiği** türü. 
    
       Altındaki kutuya **satır içi betiği**, yazabilirsiniz **python test1.py**. Ortam değişkeni Python için doğru ayarlandığından emin olun. Farklı bir sürüm veya Python çekirdek gerekiyorsa, aşağıdaki şekilde gösterildiği gibi açıkça yolu belirtebilirsiniz: 
    
       ![PowerShell ayrıntıları](./media/code-test/powershell_scripts.PNG)

    g. Seçin **sıraya & Kaydet** derleme tanımı işlemini bitirmek için.

       !["& Sıraya Kaydet" düğmesi](./media/code-test/save_and_queue_build_definition.PNG)

Şimdi yeni bir yürütme kodu depoya gönderilen her zaman, yapılandırma işlemi otomatik olarak başlatılacak. (Burada biz asıl deposu olarak kullanabilirsiniz, ancak herhangi bir dal tanımlayabilirsiniz.) İşlemi çalıştırılan **test1.py** kod içinde tanımlanan her şeyin doğru şekilde çalıştığından emin olmak için aracı makine dosyasında. 

Uyarıları doğru şekilde ayarlanmışsa, yapı tamamlandığında e-posta ile bildirilir. Ayrıca VSTS yapı durumu kontrol edebilirsiniz. Başarısız olursa, yapı ayrıntılarını kontrol edin ve hangi parça bozuk olduğunu öğrenin.

![Yapı Başarı e-posta bildirimi](./media/code-test/email_build_succeed.PNG)

![Yapı Başarısı VSTS bildirimi](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [UCI geliri tahmin depo](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) veri bilimi senaryoları için birim testleri somut örnekleri için.
* Önceki anahat ve kendi veri bilimi projeleri UCI geliri tahmin senaryoda örneklerinden izleyin.

## <a name="references"></a>Başvurular
* [Team Data Science Process](https://aka.ms/tdsp)
* [Visual Studio Test Araçları](https://www.visualstudio.com/vs/features/testing-tools/)
* [VSTS test kaynakları](https://www.visualstudio.com/team-services/)
* [Veri bilimi sanal makineler](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)