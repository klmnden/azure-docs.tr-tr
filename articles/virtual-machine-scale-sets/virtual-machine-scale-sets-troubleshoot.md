---
title: Otomatik ölçeklendirme sanal makine ölçek kümeleri ile ilgili sorunları giderme | Microsoft Docs
description: Sanal makine ölçek kümeleri olan otomatik ölçeklendirme sorunlarını giderin. Karşılaşılan tipik sorunları ve bunların nasıl çözüleceği anlayın.
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: negat
ms.openlocfilehash: ea634ea8bcb4fed1ed63dc8d1e17d215a00758c6
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri ile sorun giderme otomatik ölçeklendirme
**Sorun** – sanal makine ölçekleme kümeleri kullanarak Azure Resource Manager ile bir otomatik ölçeklendirmeyi altyapı oluşturduğunuz – bunun gibi bir şablon dağıtarak örneğin: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – ölçek kurallarınızı tanımlanmış olması ve büyük, Hayır dışında çalışır sanal makinelerin put ne kadar yük önemli, otomatik ölçeklendirme değil.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Dikkate alınması gereken bazı noktalar şunlardır:

* Kaç tane Vcpu'lar her VM sahip ve her vCPU yükleniyor?
  Yukarıdaki örnek Azure Hızlı Başlangıç şablonu tek vCPU yükleyen bir do_work.php komut dosyası yok. Standard_A1 veya D1 gibi tek vCPU VM boyutundan daha büyük bir VM kullanıyorsanız, bu yük birden çok kez çalıştırmanız gerekir. Geçirerek Vm'leriniz için kaç tane Vcpu'lar denetleyin [boyutları için Windows azure'da sanal makineler](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Kaç tane sanal makineleri sanal makine ölçek kümesindeki her bir iş yapmakta olduğunuz?
  
    Genişleme olay olduğunda gerçekleşir yalnızca ortalama CPU arasında **tüm** ölçek kümesindeki sanal makineleri otomatik ölçeklendirme kurallarında tanımlanan eşik değeri, iç zamanla aşıyor.
* Herhangi bir ölçek olayı eksik olabilir mi?
  
    Ölçek olayları için Azure portalında denetim günlüklerini denetleyin. Olabilir oluştu bir ölçek yukarı ve aşağı, Ölçek kaçırılan. "Ölçek" göre filtreleyebilirsiniz.
  
    ![Denetim Günlükleri][audit]
* Ölçek ve genişletme eşikleri yeterince farklı misiniz?
  
    Ortalama CPU ortalama CPU % 50'den az olduğunda, ölçek ve bu beş dakikayı % 50'den büyük olduğunda genişletmek için bir kural kümesi varsayalım. Bu ayar, CPU kullanımı ile sürekli artırma ve kümesinin boyutunu azaltmak ölçeklendirme eylemi eşik yakın olduğunda "flapping" sorunu neden olur. Bu ayarı nedeniyle otomatik ölçeklendirme hizmeti "dalgalanma" önlemek hangi değil ölçeklendirme olarak bildirilebilir çalışır. Bu nedenle, genişleme ve ölçek bileşenini eşikleri ölçeklendirme Between biraz alan izin vermek için yeterince farklı olduğundan emin olun.
* Kendi JSON şablonunu yazdım?
  
    Hataları olun, böylece iş ve küçük artımlı değişiklikler yapmak için yukarıdaki kanıtlanmış gibi şablonu ile başlayın kolaydır. 
* El ile içeri veya dışarı ölçekleme yapabilirsiniz?
  
    Sanal makine ölçek kaynak "VM'ler el ile değiştirmek için ayarı farklı kapasitesi ile" ayarlama yeniden dağıtmayı deneyin. Bir örnek şablonu aşağıda verilmiştir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – ölçek kümesi kullanır gibi aynı makine boyutu olduğundan emin olmak için şablonu düzenlemek gerekebilir. VM sayısını el ile başarılı bir şekilde değiştirirseniz, sonra sorun için otomatik ölçeklendirme yalıtılmış olduğunu bilirsiniz.
* Microsoft.Compute/virtualMachineScaleSet denetleyin ve Microsoft.ınsights kaynaklarında [Azure kaynak Gezgini](https://resources.azure.com/)
  
    Azure kaynak Gezgini, Azure Resource Manager kaynaklarınızı durumunu gösteren vazgeçilmez bir sorun giderme aracıdır. Aboneliğinizi tıklatın ve sorun gidermekte olduğunuz kaynak grubu arayın. İşlem kaynak sağlayıcısı altında oluşturulur ve bir dağıtım durumunu gösteren örnek görünümünü denetleyin sanal makine ölçek kümesi arayın. Ayrıca, sanal makine ölçek kümesindeki sanal makineleri örnek görünümünü kontrol edin. Ardından, Microsoft.ınsights kaynak sağlayıcısını gidin ve otomatik ölçeklendirme kurallarını gerektiği gibi denetleyin.
* Tanılama uzantısını çalışma ve performans verilerini yayma?
  
    **Güncelleştirme:** Azure otomatik ölçeklendirme, artık bir tanılama uzantısını yüklü olmasını gerektiren bir ana bilgisayar tabanlı ölçüm ardışık kullanmak için geliştirilmiştir. Yeni işlem hattı kullanarak otomatik ölçeklendirmeyi uygulama oluşturursanız, sonraki birkaç paragraf artık geçerli. Konak ardışık düzen kullanılacak dönüştürülen Azure şablonları örneği aşağıda verilmiştir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Ana bilgisayar tabanlı ölçümleri için otomatik ölçeklendirme aşağıdaki nedenlerle daha iyi kullanmaktır:
  
  * Daha az hareketli parça tanılama uzantısız olarak yüklenmesi gerekmez.
  * Daha basit şablonları. Var olan bir ölçek kümesi şablonunu Öngörüler otomatik ölçeklendirme kurallarını eklemeniz yeterlidir.
  * Daha güvenilir raporlama ve yeni Vm'leri daha hızlı başlatma.
    
    Tanılama uzantısını kullanarak korumak istediğiniz durumlarda tek neden olduğu bellek tanılama raporlama/ölçeklendirme ihtiyacınız varsa. Ana bilgisayar tabanlı ölçümleri bellek bildirmez.
    
    Otomatik ölçeklendirmeyi için tanılama uzantılarını kullanıyorsanız, aklınızda yalnızca bu makalenin geri kalanında izleyin.
    
    Otomatik ölçeklendirme Azure Kaynak Yöneticisi'nde çalışabilirsiniz (ancak artık gerekir) yoluyla VM uzantısı tanılama uzantısını çağrılır. Bir depolama hesabı şablonda tanımlamak için performans verilerini gösterir. Bu veriler, ardından Azure izleme hizmeti tarafından toplanır.
    
    Öngörüler hizmeti Vm'lerden veri okunamıyor, bir e-posta göndermek için varsayılır. Örneğin, sanal makineleri aşağı varsa, bir e-posta alırsınız. E-posta, Azure hesabınızı oluştururken belirttiğiniz e-posta adresinde kontrol ettiğinizden emin olun.
    
    Ayrıca kendiniz veri bakabilirsiniz. Cloud explorer'ı kullanarak Azure depolama hesabı arayın. Örneğin, kullanarak [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), oturum açma ve kullanmakta olduğunuz Azure aboneliğini seçin. Sonra dağıtım şablonu tanılama uzantısını tanımında başvurulan tanılama depolama hesabı adı bakın.
    
    ![Bulut Gezgini][explorer]
    
   Her VM verilerden depolandığı tabloları bir grup bakın. Linux ve bir örnek olarak CPU ölçüm alma, en son satırları arayın. Visual Studio bulut Gezgini, bir sorgu dili destekler, bu nedenle bir sorgu çalıştırabilirsiniz. Örneğin, bir sorgu için çalıştırabilirsiniz "zaman damgası gt datetime'2016-02-02T21:20:00Z'" en son olayların alma emin olmak için. Saat dilimi UTC'ye karşılık gelir. Vardır karşılık ölçeği kuralları ayarlamak, gördüğünüz veri mu? Aşağıdaki örnekte, % 100'e son beş dakika boyunca artırma makine 20 CPU başlatıldı.
    
    ![Depolama tabloları][tables]
    
    Veri yoksa, Vm'lerde çalışan tanılama uzantılı sorun olduğu anlamına gelir. Veri varsa, Ölçek kurallarınızı ile veya Öngörüler hizmeti ile bir sorun var. anlamına gelir. Denetleme [Azure durum](https://azure.microsoft.com/status/).
    
    Otomatik ölçeklendirme sorunları yaşamaya devam ediyorsanız bu adımları uygularken size atanmış sonra aşağıdaki kaynaklara deneyebilirsiniz: 
    * Forumlar okumaya [MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=WAVirtualMachinesforWindows), veya [yığın taşması](http://stackoverflow.com/questions/tagged/azure) 
    * Destek Telefonu oturum açın. Şablon ve performans verilerinizi bir görünümünü paylaşmak hazırlıklı olun.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
