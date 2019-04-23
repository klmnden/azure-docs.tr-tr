---
title: Cloud service dağıtım sorunlarını giderme | Microsoft Docs
description: Bir bulut hizmeti, Azure'a dağıtırken çalışabilir bazı yaygın sorunlar vardır. Bu makalede, bunlardan bazıları için çözümler sağlar.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: cc2a0177525013736445db5fd1befa478dc9b9b8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798579"
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Cloud service dağıtım sorunlarını giderme
Azure'a bir bulut hizmeti uygulama paketini dağıttığınızda, dağıtımdan hakkında bilgi edinebilirsiniz **özellikleri** bölmesinde Azure portalında. Bulut hizmeti sorunları gidermenize yardımcı olması için bu bölmede ayrıntılarını kullanabilirsiniz ve, bu bilgiler Azure desteği için yeni bir destek isteği açma sağlayabilirsiniz.

Bulabilirsiniz **özellikleri** bölmesinde gösterildiği gibi:

* Azure Portalı'nda bulut hizmetinizin dağıtımını tıklatın, **tüm ayarlar**ve ardından **özellikleri**.

> [!NOTE]
> İçeriğini kopyaladığınız **özellikleri** bölmesinin sağ üst köşedeki simgeye tıklayarak panoya bölmesi.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Sorun: Web sitemi erişemiyorum ancak dağıtımım başlatılır ve tüm rol örneklerinin hazır
Portalda gösterilen Web sitesi URL'si bağlantı bağlantı noktası içermez. Web siteleri için varsayılan bağlantı noktası 80'dir. Uygulamanız içinde farklı bir bağlantı noktası çalıştırmak için yapılandırılmışsa, doğru bağlantı noktasını URL'ye Web sitesi erişirken eklemelisiniz.

1. Azure Portalı'nda bulut hizmetinizin dağıtım'ı tıklatın.
2. İçinde **özellikleri** bölmesinde Azure Portalı'nın rol örnekleri için bağlantı noktalarını denetleyin (altında **giriş uç noktaları**).
3. Bağlantı noktası 80 değilse, doğru bağlantı noktası değeri uygulamaya eriştiğinde URL'sini ekleyin. Varsayılan olmayan bağlantı noktasını belirlemek için boşluk olmayan bir bağlantı noktası numarası ardından bir iki nokta üst üste (:), ardından URL'yi yazın.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Sorun: Rol Örneklerim bana hiçbir şey geri dönüştürüldü
Azure sorun düğümleri algılar ve bu nedenle rol örnekleri için yeni düğümler taşır iyileştirme hizmeti otomatik olarak gerçekleşir. Böyle bir durumda otomatik olarak geri dönüşüm rolü örneklerinizi görebilirsiniz. Hizmet onarma oluşup olmadığını öğrenmek için:

1. Azure Portalı'nda bulut hizmetinizin dağıtım'ı tıklatın.
2. İçinde **özellikleri** Azure portal'ın bölmesinde bilgileri gözden geçirin ve hizmet onarımı olanaklarından yararlanarak rollerinin geri dönüşüme uğramasının gözlemlenen süre boyunca oluşup oluşmadığını belirleyin.

Roller ayrıca kabaca ayda bir kez ana bilgisayar işletim sistemi ve konuk işletim sistemi güncelleştirmeleri sırasında geri dönüşüm.  
Daha fazla bilgi için bkz. blog gönderisine [rol örneği yeniden nedeniyle işletim sistemi yükseltmeleri](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Sorun: Ben bir VIP takası yapmak ve bir hata alıyorum
Dağıtım güncelleştirme devam ediyor durumunda bir VIP takası izin verilmez. Dağıtım güncelleştirmeleri otomatik olarak gerçekleşebilir olduğunda:

* Yeni konuk işletim sistemi kullanılabilir ve otomatik güncelleştirmeler için yapılandırılır.
* Hizmet onarma gerçekleşir.

Bir otomatik güncelleştirme yapmasını bir VIP takası engellemediğini öğrenmek için:

1. Azure Portalı'nda bulut hizmetinizin dağıtım'ı tıklatın.
2. İçinde **özellikleri** bölmesinde değerinde Azure Portal'da, Ara **durumu**. İse **hazır**, ardından **son işlem** biri son meydana geldiği varsa görmek için VIP takas engel olabilir.
3. Üretim dağıtımı için 1 ve 2 numaralı adımları tekrarlayın.
4. Bir otomatik güncelleştirme işlemi varsa, VIP takası yapmak denemeden önce kurulumun tamamlanmasını bekleyin.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Sorun: Bir rol örneği başlatıldı, başlatılıyor, meşgul ve durduruldu arasında döngü
Bu durum, uygulama kodunuz, paketiniz veya yapılandırma dosyanızla ilgili bir sorundan kaynaklanıyor olabilir. Bu durumda, birkaç dakikada değiştirme durum görmeye olmalı ve Azure portalı gibi gözükebilir **geri dönüştürme**, **meşgul**, veya **başlatılıyor**. Bu olduğunu bir şey yanlış bir rol örneği çalışmasını engelliyor uygulamayla gösterir.

Bu sorunu gidermeye ilişkin daha fazla bilgi için blog gönderisine bakın [Azure PaaS işlem Tanılama verileri](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) ve [rollerin geri dönüştürülmesine neden olan yaygın sorunlar](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Sorun: Uygulamam çalışmıyor
1. Azure portalında rol örneğine tıklayın.
2. İçinde **özellikleri** bölmesinde Azure portal'ın, sorununuzu çözmek için aşağıdaki koşulları göz önünde bulundurun:
   * Rol örneği yakın zamanda durursa (değerini kontrol edebilirsiniz **durdurma sayısı**), dağıtım güncelleştiriliyor. Rol örneği kendi üzerinde çalışmayı sürdürür, görmek için bekleyin.
   * Rol örneği ise **meşgul**, uygulama kodunuz için denetleyin [StatusCheck](/previous-versions/azure/reference/ee758135(v=azure.100)) olayı işlenir. Ekleme veya bu olayı işleyen kod düzeltme gerekebilir.
   * Tanılama verilerine gidin ve sorun giderme senaryoları blog gönderisinde [Azure PaaS işlem Tanılama verileri](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Bulut hizmetinize Geri Dönüşüm, özelliklerini dağıtımı için etkili bir şekilde özgün sorun bilgilerini silme sıfırlayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Daha fazlasını görüntüle [sorun giderme makaleleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-allocation-failures) bulut Hizmetleri için.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme konusunda bilgi almak için bkz: [Kevin Williamson'ın blog dizisini](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
