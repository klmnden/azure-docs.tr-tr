---
title: "Otomatik ölçeklendirme sanal makine ölçek kümeleri ile ilgili sorunları giderme | Microsoft Docs"
description: "Sanal makine ölçek kümeleri olan otomatik ölçeklendirme sorunlarını giderin. Karşılaşılan tipik sorunları ve bunların nasıl çözüleceği anlayın."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: bd45a0fb99a77851aa7b91d23bd4b830b6f5cc7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri ile sorun giderme otomatik ölçeklendirme
**Sorun** – otomatik ölçeklendirmeyi altyapı VM ölçek kümesi kullanarak Azure Resource Manager ile oluşturduğunuz – bu gibi bir şablon dağıtarak örneğin: https://github.com/Azure/azure-quickstart-templates/tree/master/201- vmss-bottle-otomatik ölçeklendirme – ölçek kurallarınızı tanımlanmış olması ve isteğe bağlı olarak, sanal makinelerin ne kadar yük olursa olsun, otomatik ölçeklendirme olmaz ancak bu harika, çalışır.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Dikkate alınması gereken bazı noktalar şunlardır:

* Her VM kaç çekirdeğe sahip ve her çekirdek yükleniyor?
  Yukarıdaki örnek Azure Hızlı Başlangıç şablonu tek bir çekirdek yükleyen bir do_work.php komut dosyası yok. Tek bir çekirdek VM boyutu Standard_A1 veya D1 gibi daha büyük bir VM kullanıyorsanız, bu yük birden çok kez çalıştırmanız gerekir. Kaç tane Vm'leriniz çekirdek gözden geçirerek denetleyin [boyutları için Windows azure'da sanal makineler](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Kaç VM VM ölçek kümesindeki her bir iş yapmakta olduğunuz?
  
    Olay genişletme yalnızca sürer ne zaman yerleştirin arasında ortalama CPU **tüm** ölçek kümesindeki sanal makineleri otomatik ölçeklendirme kurallarında tanımlanan eşik değeri, iç zamanla aşıyor.
* Herhangi bir ölçek olayı eksik olabilir mi?
  
    Ölçek olayları için Azure portalında denetim günlüklerini denetleyin. Belki de var. ölçek yukarı ve bir ölçekte da aşağı kaçırılan. "Ölçek" tarafından filtreleyebilirsiniz...
  
    ![Denetim Günlükleri][audit]
* Ölçek ve genişletme eşikleri yeterince farklı misiniz?
  
    Ortalama CPU ortalama CPU % 50'den az olduğunda, ölçek ve % 50'den 5 dakika daha büyük olduğunda genişletmek için bir kural kümesi varsayalım. CPU kullanımı ile sürekli artırma ve kümesinin boyutunu azaltmak ölçeklendirme eylemi Bu eşik yakın olduğunda bu "flapping" sorunu neden olur. Bu nedenle otomatik ölçeklendirme hizmeti "dalgalanma" önlemek hangi değil ölçeklendirme olarak bildirilebilir çalışır. Bu nedenle, genişleme ve ölçek bileşenini eşikleri ölçeklendirme Between biraz alan izin vermek için yeterince farklı olduğundan emin olun.
* Kendi JSON şablonunu yazdım?
  
    Hataları olun, böylece iş ve küçük artımlı değişiklikler yapmak için yukarıdaki kanıtlanmış gibi şablonu ile başlayın kolaydır. 
* El ile içeri veya dışarı ölçekleme yapabilirsiniz?
  
    VM ölçek kümesi kaynak VM'lerin sayısını el ile değiştirmek için farklı "kapasitesi" ayarı ile yeniden dağıtmayı deneyin. Bunu yapmak için bir örnek şablonu aşağıda verilmiştir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – ölçek kümesi kullanma gibi aynı makine boyutu olduğundan emin olmak için şablonu düzenlemek gerekebilir. Ardından VM'ler el ile başarılı bir şekilde değiştirirseniz, otomatik ölçeklendirme için yalıtılmış bir sorundur bildiğiniz.
* Microsoft.Compute/virtualMachineScaleSet denetleyin ve Microsoft.ınsights kaynaklarında [Azure kaynak Gezgini](https://resources.azure.com/)
  
    Azure Resource Manager kaynaklarınızı durumunu gösteren vazgeçilmez bir sorun giderme aracı budur. Aboneliğinizi tıklatın ve sorun gidermekte olduğunuz kaynak grubu arayın. İşlem kaynak sağlayıcısı altında VM ölçek oluşturduğunuz kümesi arayın ve bir dağıtım durumunu gösteren örnek görünümünü denetleyin. Ayrıca Vm'lerde VM ölçek kümesi örnek görünümünü denetleyin. Ardından Microsoft.ınsights kaynak sağlayıcısını gidin ve otomatik ölçeklendirme kurallarını iyi görünen denetleyin.
* Tanılama uzantısını çalışma ve performans verilerini yayma?
  
    **Güncelleştirme:** Azure otomatik ölçeklendirme, artık bir tanılama uzantısını yüklü olmasını gerektiren bir ana bilgisayar tabanlı ölçüm ardışık kullanmak için geliştirilmiştir. Bu, yeni işlem hattı kullanarak otomatik ölçeklendirmeyi uygulama oluşturursanız, birkaç paragraf artık geçerli sonraki anlamına gelir. Konak ardışık düzen kullanılacak dönüştürülen Azure şablonları örneğidir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Ana bilgisayar tabanlı ölçümleri için otomatik ölçeklendirme aşağıdaki nedenlerle daha iyi kullanmaktır:
  
  * Daha az hareketli parça tanılama uzantısız olarak yüklenmesi gerekmez.
  * Daha basit şablonları. Var olan bir ölçek kümesi şablonunu Öngörüler otomatik ölçeklendirme kurallarını eklemeniz yeterlidir.
  * Daha güvenilir raporlama ve yeni Vm'leri daha hızlı başlatma.
    
    Tanılama uzantısını kullanarak tutmak isteyebilirsiniz yalnızca nedeniyle Bellek Tanılama raporlama/ölçeklendirme gerekip gerekmediğini olacaktır. Ana bilgisayar tabanlı ölçümleri bellek bildirmez.
    
    Otomatik ölçeklendirmeyi için hala tanılama uzantılarını kullanıyorsanız, aklınızda yalnızca bu makalenin geri kalanını uygulayın.
    
    Otomatik ölçeklendirme Azure Kaynak Yöneticisi'nde çalışabilirsiniz (ancak artık gerekir) yoluyla VM uzantısı tanılama uzantısını çağrılır. Bir depolama hesabı şablonda tanımlamak için performans verilerini gösterir. Bu veriler, ardından Azure izleme hizmeti tarafından toplanır.
    
    Öngörüler hizmeti Vm'lerden veri okunamıyor, onu gerektiği VM'ler aşağı olsaydı bir e-posta – örneğin göndermek, bu nedenle e-posta adresiniz (bir Azure hesabı oluşturulurken belirtilen) denetleyin.
    
    Ayrıca, Git ve veri kendiniz bakın. Cloud explorer'ı kullanarak Azure depolama hesabı arayın. Örneğin kullanarak [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), oturum açma ve çekme kullanmakta olduğunuz Azure aboneliği ve tanılama depolama hesabı adı başvurulan dağıtım şablonunuzu tanılama uzantısını tanımında...
    
    ![Bulut Gezgini][explorer]
    
    Burada, her bir VM verilerden depolandığı tabloları bir dizi görürsünüz. Linux ve bir örnek olarak CPU ölçüm alma, en son satırları arayın. Visual Studio bulut Gezgini, bir sorgu dili destekler, böylece gibi bir sorgu çalıştırabilirsiniz "zaman damgası gt datetime'2016-02-02T21:20:00Z'" (zamanı UTC olarak varsayılmıştır) en son olayların alma emin olmak için. Vardır karşılık ölçeği kuralları ayarlamak, gördüğünüz veri mu? Aşağıdaki örnekte, % 100'e son 5 dakika boyunca artırma makine 20 CPU başlatıldı...
    
    ![Depolama tabloları][tables]
    
    Veri yoksa, daha sonra Vm'lerde çalışan tanılama uzantılı sorun olduğu anlamına gelir. Veri varsa, Ölçek kurallarınızı ile veya Öngörüler hizmeti ile bir sorun var. anlamına gelir. Denetleme [Azure durum](https://azure.microsoft.com/status/).
    
    Otomatik ölçeklendirme sorun yaşıyorsanız bu adımları uygularken size atanmış sonra size Forum çalışabilir [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), veya [yığın taşması](http://stackoverflow.com/questions/tagged/azure), veya bir destek çağrısı oturum. Şablon ve performans verilerini bir görünümünü paylaşmak hazırlıklı olun.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
