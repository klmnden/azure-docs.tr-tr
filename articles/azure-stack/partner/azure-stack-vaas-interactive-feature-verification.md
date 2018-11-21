---
title: Hizmet olarak Azure Stack doğrulama test etkileşimli özellik doğrulama | Microsoft Docs
description: Doğrulama bir hizmet olarak Azure Stack için etkileşimli özellik doğrulama testleri oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: bc2fbf7aadf99a2f765def2d352819dfa6cd5fa4
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52265904"
---
# <a name="interactive-feature-verification-testing"></a>Etkileşimli özellik doğrulama testi  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sisteminiz için testleri istemek için etkileşimli özellik doğrulama test çerçevesi'ni kullanabilirsiniz. Bir test istediğinde Microsoft framework etkileşimli el ile yapılacak adımlar gerekli testleri hazırlamak için kullanır. Microsoft, birden fazla tek başına otomatik testleri bir araya zincirleyebilirsiniz çerçevesini kullanabilirsiniz.

Bu makalede, basit bir el ile senaryosu açıklanır. Azure stack'teki disk değiştirme test denetler. Framework, her bir adımı tanılama günlüklerini toplar. Bulduğunuz gibi sorunları hata ayıklaması yapabilirsiniz. Framework ayrıca diğer araçları veya işlemler tarafından üretilen günlükleri paylaşıma izin veren ve senaryoya geri bildirim sağlamak sağlar.

## <a name="overview-of-a-test-pass"></a>Test geçişi genel bakış

Disk değiştirme için bir test sık karşılaşılan bir senaryodur. Bu örnekte, test yedi adım vardır:

1.  Yeni bir **Test geçiş** iş akışı.
2.  Seçin **Disk kimliği Test**.
3.  Testi başlatın.
4.  Etkileşimli doğrulama senaryoda eylemleri seçin.
5.  Senaryo sonucunu denetleyin.
6.  Test sonucu Microsoft'a gönderin.
7.  VaaS portalında durumunu kontrol edin.

## <a name="create-a-new-test-pass"></a>Yeni bir test geçişi oluşturma

1.  Gidin [Azure Stack doğrulama portalı](https://www.azurestackvalidation.com) ve oturum açın.

2.  Yeni bir çözüm oluşturun veya var olanı seçin.

3.  Seçin **Başlat** üzerinde **Test geçiş** Döşe.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image1.png)

4.  İçin bir ad girin **Test geçiş** iş akışı.

5.  Ortak test parametreleri ve ortamı'ndaki yönergeleri takip ederek girin [iş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama](azure-stack-vaas-parameters.md) makalesi.

6.  Tanılama bağlantı dizesinde belirtilen biçimi izlemelidir [Test parametreleri](azure-stack-vaas-parameters.md#test-parameters) konusundaki [iş akışı ortak parametrelerinin](azure-stack-vaas-parameters.md) makalesi.

7.  Test için gereken parametreler girin.

8.  Aracıyı etkileşimli testi çalıştırmak için seçin.

> [!Note]  
> Disk kimliği etkileşimli özellik doğrulama testi için etki alanı yönetici kullanıcı adı ve parola belirtilmelidir.

![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image2.png)

## <a name="select-the-test"></a>Test seçin

1.  Seçin **Disk kimliği Test\<sürüm >**.

    > [!Note]  
    > Test malzemeleri geliştirmeleri yapılmış gibi test sürümünü artırır. Aksi halde Microsoft belirtmiyorsa en yüksek sürümü her zaman kullanılmalıdır.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image4.png)

2.  Etki alanı yönetici kullanıcı adı ve parola girmek **Düzenle**.

3.  Uygun test yürütme aracısı / test başlatmaya DVM seçin.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image5.png)

4.  Seçin **Gönder** testi başlatmak için.

![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image6.png)

## <a name="start-the-test"></a>Testi Başlat

Disk kimliği Test istemleri VaaS Aracısı'nı çalıştıran bilgisayarda gösterir. Genellikle bu DVM veya Sıçrama kutusu Azure Stack için örneğidir.

![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image8.png)

## <a name="choose-the-actions"></a>Eylemleri seçin

1.  İzleyin **belgeleri** ve **doğrulama** bağlantıları bu senaryoyu gerçekleştirmek nasıl Microsoft gelen yönergeleri gözden geçirin.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image9.png)

2.  **İleri**’yi seçin.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image10.png)

3.  Precheck betiği çalıştırmak için yönergeleri izleyin.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image11.png)

4.  Precheck betik başarıyla tamamlandıktan sonra el ile senaryoyu (Disk değiştirme) olarak başına yürütme **belgeleri** ve **doğrulama** gelen bağlantılar **bilgi**sekmesi.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image12.png)

5.  El ile senaryo gerçekleştirirken iletişim kutusunu kapatmayın.

6.  El ile senaryo gerçekleştirme tamamladığınızda onay sonrası betiği çalıştırmak için yönergeleri izleyin.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image13.png)

7.  Başarıyla tamamlandığında el ile senaryosunun (Disk değiştirme), seçin **sonraki**.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image14.png)

> [!Important]  
> Pencereyi kapatırsanız yapıldığını önce test durur.

## <a name="check-the-status"></a>Onay durumu

1.  Test tamamlandığında, geri bildirim sağlamanız istenir.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image15.png)

2.  Bu soruları senaryo başarı oranı ve yayın kalitesini değerlendirmek Microsoft yardımcı olur.

## <a name="send-the-test-result"></a>Test sonucu Gönder

1.  Microsoft'a göndermek istediğiniz herhangi bir günlük dosyasının ekleyin.

    ![Alternatif metin](media\azure-stack-vaas-interactive-feature-verification\image16.png)

2.  Geri bildirim gönderimine EULA'yı kabul edin.

3.  Seçin **Gönder** sonuçlarını Microsoft'a gönderilecek.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)
