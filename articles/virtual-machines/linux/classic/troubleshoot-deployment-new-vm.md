---
title: "Linux VM dağıtımı-Klasik sorunlarını giderme | Microsoft Docs"
description: "Azure'da yeni bir Linux sanal makine oluşturduğunuzda, Klasik dağıtım sorunlarını giderme"
services: virtual-machines-linux
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: c8a963fa-6b2a-4c7a-a1f4-7793adb02b19
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/06/2016
ms.author: cjiang
ms.openlocfilehash: 4f97f28118e0fc6e92373f04c45450537723fce5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Azure'da yeni bir Linux sanal makine oluşturma ile klasik dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümü için bkz: [burada](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Toplama denetim günlüklerini
Sorun giderme başlatmak için sorunu ile ilişkili hata tanımlamak için denetim günlüklerini toplayın.

Azure portalında tıklatın **Gözat** > **sanal makineleri** > *Windows sanal makinenizi* > **ayarları** > **denetim günlüklerini**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** genelleştirilmiş, Linux işletim sistemi olan ve bunu karşıya ve/veya genelleştirilmiş ayarıyla yakalanan durumunda hataları olmayacak. Benzer şekilde, işletim sistemi ise Linux özelleştirilmiş ve bunu karşıya ve/veya özel ayarlarla yakalanan ardından hataları olmayacak.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** genelleştirilmiş Linux işletim sistemi ise ve olarak karşıya VM sağlama aşamada takıldı için özelleştirilmiş, sağlama zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** OS Linux özelleştirilmiş ve genelleştirilmiş gibi karşıya varsa, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama bir hata alırsınız.

**Çözüm:**

Her iki bu hataları gidermek için özgün VHD, mevcut şirket içi, işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile karşıya yükleyin. Genelleştirilmiş olarak karşıya yükleme, çalıştırmak - unutmayın ilk sağlamayı sonlandırın. Bkz: [oluşturun ve Linux işletim sistemini içeren bir Sanal Sabit Disk yüklemek](create-upload-vhd.md) daha fazla bilgi için.

**Hata yakalama:**

**N<sup>3</sup>:** genelleştirilmiş Linux işletim sistemi ise ve olarak yakalanan orijinal VM kullanılabilir olmadığından genelleştirilmiş olarak işaretlenen gibi özelleştirilmiş, sağlama zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** Linux özelleştirilmiş ve genelleştirilmiş gibi yakalanan işletim sistemi ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama bir hata alırsınız. İşaretlendiğinden Ayrıca, orijinal VM kullanılamaz olarak özelleştirilmiş.

**Çözüm:**

Her iki bu hataları gidermek için geçerli görüntü Portalı'ndan silmek ve [geçerli VHD'leri yeniden yakalar](capture-image.md) ayarıyla aynı olarak işletim sistemi (genelleştirilmiş/özel) için.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Sorun: Özel / Galeri / Market görüntüsü; ayırma hatası
Bu hata, isteği gerçekleştirmek için kullanılabilir boş alan yok veya istenen VM boyutu destekleyemez bir kümeye yeni VM istek gönderildiğinde durumlarda ortaya çıkar. Farklı dizi sanal makineleri aynı bulut hizmetindeki karışık mümkün değildir. Bulut hizmetinizi desteği ne değerinden farklı bir boyutta yeni bir VM oluşturmak istiyorsanız, bu nedenle işlem isteği başarısız olur.

Yeni bir VM oluşturmak için kullandığınız bulut hizmeti kısıtlamaları bağlı olarak, iki durumlardan biri nedeniyle hatayla karşılaşabilirsiniz.

**1. neden:** bulut hizmeti için belirli bir kümeyi sabitlenmiş veya bunu bir benzeşim grubuna bağlı ve bu nedenle belirli bir kümeye tasarım gereği sabitlenir. Benzeşim grubu çalıştı varolan kaynakları barındırıldığı kümede, böylece yeni işlem kaynağı ister. Ancak, aynı küme değil istenen VM boyutu desteklemek veya oluşan bir ayırma hata yeterli kullanılabilir alan vardır. Yeni kaynaklar var olan bir bulut hizmetini veya yeni bir bulut hizmeti aracılığıyla oluşturulan bu geçerlidir.

**Çözüm 1:**

* Yeni bir bulut hizmeti oluşturun ve bir bölge veya bölge tabanlı bir sanal ağ ile ilişkilendirin.
* Yeni bir VM yeni bulut hizmeti oluşturun.
  Yeni bir bulut hizmeti oluşturmaya çalışırken bir hata alırsanız, daha sonra yeniden deneyin veya Bulut hizmeti için bölge değiştirin.

> [!IMPORTANT]
> Var olan bir bulut hizmetinde yeni bir VM oluşturmaya çalıştığınız ancak uygulanamadı ve yeni VM için yeni bir bulut hizmeti oluşturması gerekiyordu, aynı bulut hizmetindeki tüm Vm'leriniz birleştirmek seçebilirsiniz. Bunu yapmak için var olan bulut hizmetindeki sanal makineleri silin ve bunların disklerden yeni bulut hizmeti onları yeniden yakalar. Ancak, bunlar şu anda mevcut bulut hizmeti için bu bilgileri kullanan tüm bağımlılıkları için güncelleştirmeniz gerekir böylece yeni bulut hizmeti yeni bir ad ve VIP, olacaktır unutmamak önemlidir.
> 
> 

**Neden 2:** bulut hizmeti için belirli bir kümeyi tasarım gereği sabitlendi böylece bir benzeşim grubuna bağlı sanal ağ ile ilişkilidir. Benzeşim grubu bu nedenle çalıştı varolan kaynakları barındırıldığı kümesinde, tüm yeni işlem kaynak ister. Ancak, aynı küme değil istenen VM boyutu desteklemek veya oluşan bir ayırma hata yeterli kullanılabilir alan vardır. Yeni kaynaklar var olan bir bulut hizmetini veya yeni bir bulut hizmeti aracılığıyla oluşturulan bu geçerlidir.

**Çözüm 2:**

* Yeni bir bölgesel sanal ağ oluşturun.
* Yeni VM yeni sanal ağı oluşturun.
* [Varolan sanal ağınıza bağlamak](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) yeni sanal ağ. Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Alternatif olarak, [benzeşim grubu tabanlı sanal ağınızı bölgesel bir sanal ağa geçirmeniz](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)ve ardından yeni bir VM oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
Durdurulmuş bir Linux VM başlattığınızda veya varolan bir Linux VM azure'da yeniden boyutlandırma sorunlarıyla karşılaşırsanız bkz [yeniden başlatmayı veya varolan bir Linux sanal makinesini Azure yeniden boyutlandırma Klasik dağıtım sorunlarını giderme](restart-resize-error-troubleshooting.md).

