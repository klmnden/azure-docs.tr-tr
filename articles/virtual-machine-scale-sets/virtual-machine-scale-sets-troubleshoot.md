---
title: Sanal makine ölçek kümeleri ile otomatik ölçeklendirme sorunlarını giderme | Microsoft Docs
description: Sanal makine ölçek kümeleri ile otomatik ölçeklendirme sorunlarını giderin. Karşılaşılan genel sorunları ve bunların nasıl çözüleceğine anlayın.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
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
ms.author: manayar
ms.openlocfilehash: 3308b22606e87853aad7e3d3a3995aab8d1b5401
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803579"
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri ile otomatik ölçeklendirme sorunlarını giderme
**Sorun** – sanal makine ölçek kümeleri'ni kullanarak Azure Resource Manager ile otomatik ölçeklendirme altyapısı oluşturduktan – örneğin de bunun gibi bir şablon dağıtarak: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – ölçeklendirme kurallarınızı tanımlanmış olması ve harika, Hayır dışında çalışır Vm'leri yerleştirdiğiniz ne kadar yük önemli, otomatik ölçeklendirme değil.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Dikkat etmeniz gerekenler şunlardır:

* Her VM Vcpu kaç sahip ve her vCPU yükleniyor?
  Önceki örnek Azure Hızlı Başlangıç şablonu, tek bir vCPU yükleyen bir do_work.php betik vardır. Standard_A1 veya D1 gibi tek vCPU VM boyutundan daha büyük bir VM kullanıyorsanız, bu yükü birden çok kez çalıştırmanız gerekir. Gözden geçirerek Vm'leriniz için ne kadar Vcpu denetleyin [boyutları için Windows azure'da sanal makineler](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Sanal makine ölçek kümesinde kaç tane Vm'niz her biri hakkında daha fazla iş yapıyor?
  
    Ölçeği genişletilmiş olay olduğunda gerçekleşir yalnızca genelinde ortalama CPU **tüm** bir ölçek kümesindeki sanal makineler, otomatik ölçeklendirme kuralları tanımlanan eşik değeri, iç zaman içinde aşıyor.
* Tüm ölçek olayları kaçırdınız mı?
  
    Azure portalında ölçek olayları için denetim günlüklerini denetleyin. Belki de bir ölçek ayarlama ve oluştu bir ölçeği azaltma, kaçırılan. "Ölçek" göre filtreleyebilirsiniz.
  
    ![Denetim Günlükleri][audit]
* Ölçeklendirme ve ölçek genişletme belirlediğiniz eşiklere yeteri kadar farklıdır?
  
    Ortalama CPU ortalama CPU % 50'den az olduğunda, ölçek ve % 50'den beş dakika daha büyük olduğunda ölçeğini genişletmek için bir kural kümesi varsayalım. Bu ayar, CPU kullanımı ile sürekli artması ve kümenin boyutunu küçültme ölçeklendirme eylemi eşiği yakın olduğunda "Çırpma" bir sorun neden olur. Bu ayarı nedeniyle otomatik ölçeklendirme hizmeti "dalgalanma" önlemek, olmaması ölçeklendirmeyle olarak bildirilebilir çalışır. Bu nedenle, ölçeklendirme ve ölçek, belirlediğiniz eşiklere ölçeklendirme arasında bazı alan izin vermek için yeterince farklı olduğundan emin olun.
* Kendi JSON şablonunu yazdınız?
  
    Hata yapma, iş ve küçük artımlı değişiklikler yapmak için yukarıda bir kanıtlanmış gibi bir şablon ile başlatın kolaydır. 
* Veya el ile ölçeklendirebilir miyim?
  
    Sanal makine ölçek "VM sayısını el ile olarak değiştirmek için farklı bir kapasiteye" kaynak kümesi yeniden deneyin. Örnek şablonu aşağıda verilmiştir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – kullanan ölçek kümesi olarak aynı makine boyutu olduğundan emin olmak için şablonu düzenlemeniz gerekebilir. VM sayısını el ile başarılı bir şekilde değiştirebilirsiniz varsa, sorunu otomatik olarak ölçeklendirmek için yalıtılmış ardından bilirsiniz.
* Microsoft.Compute/virtualMachineScaleSet denetleyin ve Microsoft.ınsights kaynaklarında [Azure kaynak Gezgini](https://resources.azure.com/)
  
    Azure kaynak Gezgini, Azure Resource Manager kaynaklarınızı durumunu gösteren vazgeçilmez bir sorun giderme aracıdır. Aboneliğinizde tıklayın ve kaynak grubu, sorun giderme bakın. Oluşturduğunuz ve bir dağıtım durumunu gösterir. örnek görünümünü denetleyin. sanal makine ölçek kümesi işlem kaynak sağlayıcısı altında bakın. Ayrıca, sanal makine ölçek kümesindeki Vm'leri örnek görünümünü denetleyin. Ardından, Microsoft.ınsights kaynak sağlayıcısına gidin ve otomatik ölçeklendirme kuralları gerektiği gibi denetleyin.
* Tanılama uzantısı çalışma ve performans verilerini yayma?
  
    **Güncelleştirme:** Azure otomatik ölçeklendirme, artık bir tanılama uzantısı yüklü olmasını gerektiren bir ana bilgisayar tabanlı ölçümler işlem hattı kullanmak için geliştirilmiştir. Yeni işlem hattı kullanarak otomatik ölçeklendirme uygulaması oluşturursanız, sonraki birkaç paragrafta artık uygulanmaz. Ana işlem hattını kullanma dönüştürülmüştür Azure şablonları bir örneği aşağıda verilmiştir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Otomatik ölçeklendirme için ana bilgisayar tabanlı ölçümler kullanan aşağıdaki nedenlerden dolayı daha iyidir.
  
  * Herhangi bir tanılama uzantısı olarak daha az sayıda hareketli parçadan yüklü olması gerekir.
  * Daha basit şablonları. Insights otomatik ölçeklendirme kuralları için mevcut bir ölçek kümesi şablonunun eklemeniz yeterlidir.
  * Daha güvenilir raporlama ve yeni sanal makinelerin hızlı başlatma.
    
    Bir tanılama uzantısı kullanarak tutmak isteyebileceğiniz tek neden olduğu bellek tanılama raporlama/ölçeklendirme gerekiyorsa. Ana bilgisayar tabanlı ölçümler bellek bildirmez.
    
    Otomatik ölçeklendirme için tanılama uzantılarını kullanıyorsanız, aklınızda yalnızca bu makalenin geri kalanında izleyin.
    
    Azure Resource Manager'daki otomatik ölçeklendirme çalışabilirsiniz (ancak artık gerekir) yoluyla bir sanal makine uzantısı tanılama uzantısını çağrılır. Bu şablonda tanımladığınız bir depolama hesabı için performans verilerini gösterir. Bu veriler daha sonra Azure İzleyici hizmeti tarafından toplanır.
    
    Insights hizmeti veri Vm'lerden okunamıyor, bir e-posta Gönder beklenir. Örneğin, Vm'leri çalışmıyorsa, bir e-posta alırsınız. E-postanıza, Azure hesabınızı oluştururken, belirttiğiniz e-posta adresine denetlediğinizden emin olun.
    
    Ayrıca kendiniz veri bakabilirsiniz. Cloud explorer'ı kullanarak Azure depolama hesabında arayın. Örneğin, kullanarak [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), oturum ve kullandığınız Azure aboneliğini seçin. Daha sonra dağıtım şablonu tanılama uzantısı tanımında başvurulan tanılama depolama hesabı adı bakın.
    
    ![Cloud Explorer][explorer]
    
    Her sanal makine verilerini depolandığı tabloları bir dizi görürsünüz. Linux ve CPU ölçüm örnek olarak alan, en son satırları arayın. Visual Studio cloud explorer'ın bir sorgu dili destekler; dolayısıyla bir sorgu çalıştırabilirsiniz. Örneğin, bir sorgu için çalıştırabileceğiniz "zaman damgası gt datetime'2016-02-02T21:20:00Z'" en son olayların alacağınızdan emin olmak. Saat dilimi UTC değerine karşılık gelir. Var. karşılık gelen ölçek kuralları ayarlamak, gördüğünüz mu? Aşağıdaki örnekte, son beş dakika boyunca % 100'e artırmak için makine 20 CPU başlatıldı.
    
    ![Depolama tabloları][tables]
    
    Veri yoksa, Vm'lerde çalışan tanılama uzantısı sorun olduğu anlamına gelir. Veri yoksa ya da bir sorun var. ölçek kurallarınızı veya Insights hizmeti ile gelir. Denetleme [Azure durumu](https://azure.microsoft.com/status/).
    
    Otomatik ölçeklendirme sorunları yaşamaya devam ediyorsanız bu adımlarda kılavuzluğa odaklanmamı sonra aşağıdaki kaynakları deneyebilirsiniz: 
    * Forumlar okumaya [MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=WAVirtualMachinesforWindows), veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure) 
    * Bir destek çağrısı açın. Şablon ve performans verilerinizi bir görünümü paylaşmak hazırlıklı olun.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
