---
title: "Azure Güvenlik Merkezi ve Windows sanal makineleri azure'da | Microsoft Docs"
description: "Azure Güvenlik Merkezi ile Azure Windows sanal makineniz için güvenlik hakkında bilgi edinin."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adb00e28b0b204858a763f83836ee2ac96f8f9e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Azure Güvenlik Merkezi'ni kullanarak sanal makine güvenlik izleme

Azure Güvenlik Merkezi güvenlik uygulamaları, Azure kaynak görünürlük elde size yardımcı olabilir. Güvenlik Merkezi, tümleşik güvenlik izleme sunar. Kaçabilecek tehditleri algılayabilir. Bu öğreticide, Azure Güvenlik Merkezi hakkında bilgi edinmek ve nasıl yapılır:
 
> [!div class="checklist"]
> * Veri toplamayı ayarlama
> * Güvenlik ilkeleri Ayarla
> * Görüntüleme ve yapılandırma sistem durumu sorunları giderin
> * Algılanan tehditler gözden geçirin  

## <a name="security-center-overview"></a>Güvenlik Merkezi'ne genel bakış

Güvenlik Merkezi olası sanal makine (VM) yapılandırma sorunları tanımlar ve güvenlik tehditlerini hedeflenen. Bu ağ güvenlik grupları, şifrelenmemiş diskleri ve kaba kuvvet Uzak Masaüstü Protokolü (RDP) saldırıları eksik olan VM'ler içerebilir. Bilgiler, kolay okunur grafiklerde Güvenlik Merkezi panosunda görüntülenir.

Güvenlik Merkezi panosunda menüsünde Azure portalına erişmek için seçin **Güvenlik Merkezi**. Panoda Azure ortamınıza güvenlik durumunu görmek, geçerli önerileri sayısını bulun ve tehdit uyarıları geçerli durumunu görüntülemek. Daha fazla ayrıntı için her bir üst düzey grafik genişletebilirsiniz.

![Güvenlik Merkezi Panosu](./media/tutorial-azure-security/asc-dash.png)

Güvenlik Merkezi algıladığı sorunlar için önerilerle sağlamak için veri bulma ötesine gider. Örneğin, bir VM bir bağlı ağ güvenlik grubu dağıttıysanız, Güvenlik Merkezi ile yapabileceğiniz düzeltme adımları bir öneri görüntüler. Güvenlik Merkezi bağlamında ayrılmadan otomatik düzeltme alın.  

![Öneriler](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Veri toplamayı ayarlama

VM Güvenlik yapılandırmaları görünürlük sağlayabilmek için önce Güvenlik Merkezi veri toplama ayarlamanız gerekir. Bu, veri koleksiyonu açık kapatarak ve toplanan verileri tutmak için bir Azure depolama hesabı oluşturmayı içerir. 

1. Güvenlik Merkezi panosunda tıklatın **Güvenlik İlkesi**, aboneliğinizi seçin. 
2. İçin **veri toplama**seçin **üzerinde**.
3. Bir depolama hesabı oluşturmak için seçin **depolama hesabı seç**. Ardından, seçin **Tamam**.
4. Üzerinde **Güvenlik İlkesi** dikey penceresinde, select **kaydetmek**. 

Güvenlik Merkezi veri toplama Aracısı sonra tüm Vm'lere yüklü ve veri toplama başlar. 

## <a name="set-up-a-security-policy"></a>Bir güvenlik ilkesi ayarlama

Güvenlik ilkeleri, kendisi için Güvenlik Merkezi veri toplar ve öneriler yapar öğeleri tanımlamak için kullanılır. Farklı Azure kaynakları için farklı güvenlik ilkeleri uygulayabilirsiniz. Varsayılan olarak Azure kaynaklarına karşı tüm ilke öğeleri değerlendirilir rağmen tüm Azure kaynakları için veya bir kaynak grubu için tek tek ilke öğelerini devre dışı bırakabilirsiniz. Güvenlik Merkezi güvenlik ilkeleri hakkında ayrıntılı bilgi için bkz: [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](../../security-center/security-center-policies.md). 

Tüm Azure kaynakları için bir güvenlik ilkesi ayarlamak için:

1. Güvenlik Merkezi panosunda seçin **Güvenlik İlkesi**, aboneliğinizi seçin.
2. Seçin **önleme İlkesi**.
3. Açma veya tüm Azure kaynaklarına uygulamak istediğiniz ilke öğeleri kapatın.
4. Ayarlarınızı seçerek tamamladığınızda seçin **Tamam**.
5. Üzerinde **Güvenlik İlkesi** dikey penceresinde, select **kaydetmek**. 

Belirli bir kaynak grubu için bir ilke ayarlamak için:

1. Güvenlik Merkezi panosunda seçin **Güvenlik İlkesi**, bir kaynak grubunu seçin.
2. Seçin **önleme İlkesi**.
3. Açma veya kaynak grubuna uygulamak istediğiniz ilke öğeleri kapatın.
4. Altında **DEVRALMA**seçin **benzersiz**.
5. Ayarlarınızı seçerek tamamladığınızda seçin **Tamam**.
6. Üzerinde **Güvenlik İlkesi** dikey penceresinde, select **kaydetmek**.  

Bu sayfada belirli bir kaynak grubu için verilerin toplanmasını kapatabilirsiniz.

Aşağıdaki örnekte, bir kaynak grubu için benzersiz bir ilke oluşturuldu *myResoureGroup*. Bu ilkede disk şifreleme ve web uygulaması güvenlik duvarı önerileri devre dışı bırakılmıştır.

![Benzersiz bir ilke](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>VM yapılandırma durumunu görüntüle

Veri toplama açık ve bir güvenlik ilkesi ayarladıktan sonra uyarıları ve öneriler sağlamak üzere Güvenlik Merkezi başlar. Sanal makineleri dağıtılan gibi veri toplama Aracısı yüklenir. Güvenlik Merkezi, ardından yeni VM'ler için verilerle doldurulur. VM yapılandırma sistem durumu hakkında ayrıntılı bilgi için bkz: [Güvenlik Merkezi'nde, Vm'leri koruma](../../security-center/security-center-virtual-machine-recommendations.md). 

Toplanan veriler gibi her bir VM ve ilgili Azure kaynağı için kaynak durumu toplanır. Bilgiler bir kolay okunur grafiğinde gösterilir. 

Kaynak durumu görüntülemek için:

1.  Güvenlik Merkezi panosunda, altında **kaynak güvenlik durumu**seçin **işlem**. 
2.  Üzerinde **işlem** dikey penceresinde, select **sanal makineleri**. Bu görünüm tüm VM'ler için yapılandırma durumu özetini sağlar.

![İşlem durumu](./media/tutorial-azure-security/compute-health.png)

Bir VM için tüm önerilerini görmek için VM seçin. Öneriler ve düzeltme bu öğreticinin sonraki bölümünde daha ayrıntılı olarak ele alınmaktadır.

## <a name="remediate-configuration-issues"></a>Yapılandırma sorunlarını düzeltmek

Güvenlik Merkezi ile yapılandırma verilerini doldurmak başladıktan sonra öneriler ayarladığınız güvenlik ilkesine göre yapılır. Örneğin, bir VM ilişkili ağ güvenlik grubu olmadan ayarlarsanız, oluşturmak için bir öneri yapılır. 

Tüm öneriler listesini görmek için: 

1. Güvenlik Merkezi panosunda seçin **önerileri**.
2. Belirli bir öneri seçin. Kendisi için tüm kaynakların öneri geçerli bir listesi görüntülenir.
3. Bir öneri uygulamak için belirli bir kaynak seçin. 
4. Düzeltme adımları için yönergeleri izleyin. 

Çoğu durumda, Güvenlik Merkezi Güvenlik Merkezi ayrılmadan bir öneri adres için uygulayabileceğiniz tıklatılabilir adımları sağlar. Aşağıdaki örnekte, Güvenlik Merkezi sınırsız bir gelen kuralı sahip bir ağ güvenlik grubu algılar. Öneri sayfasında seçtiğiniz **gelen kuralları Düzenle** düğmesi. Kural değiştirmek için gerekli kullanıcı Arabirimi görüntülenir. 

![Öneriler](./media/tutorial-azure-security/remediation.png)

Öneriler çözümlendi olarak çözümlendi olarak işaretlenir. 

## <a name="view-detected-threats"></a>Algılanan tehditler görüntüleyin

Kaynak yapılandırma önerilerine ek olarak Güvenlik Merkezi tehdit algılama uyarıları görüntüler. Güvenlik Uyarıları özelliği her VM, Azure ağ günlükleri ve bağlı iş ortağı çözümlerinden güvenlik tehditlerine karşı Azure kaynaklarını algılamak için toplanan verileri toplar. Güvenlik Merkezi tehdit algılaması özellikleri hakkında ayrıntılı bilgi için bkz: [Azure Güvenlik Merkezi algılama özellikleri](../../security-center/security-center-detection-capabilities.md).

Güvenlik Uyarıları özellik gelen artırılacak güvenlik fiyatlandırma katmanı merkezi gerektirir *serbest* için *standart*. 30 günlük **ücretsiz deneme sürümü** bu daha yüksek fiyatlandırma katmanı taşıdığınızda kullanılabilir. 

Fiyatlandırma katmanını değiştirmek için:  

1. Güvenlik Merkezi panosunda tıklatın **Güvenlik İlkesi**, aboneliğinizi seçin.
2. Seçin **fiyatlandırma katmanı**.
3. Yeni katman seçin ve ardından **seçin**.
4. Üzerinde **Güvenlik İlkesi** dikey penceresinde, select **kaydetmek**. 

Fiyatlandırma katmanı değiştirdikten sonra güvenlik uyarıları grafik tehditler algılanır güvenlik doldurmak başlar.

![Güvenlik uyarıları](./media/tutorial-azure-security/security-alerts.png)

Bilgileri görüntülemek için bir uyarı seçin. Örneğin, tehdit, algılama zamanı, tüm tehdit girişimleri ve önerilen düzeltme açıklamasını görebilirsiniz. Aşağıdaki örnekte, bir RDP yanılma saldırısı, ile 294 başarısız RDP deneme algılandı. Önerilen çözüm sağlanır.

![RDP saldırısı](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, Azure Güvenlik Merkezi ayarlayın ve ardından VM'ler Güvenlik Merkezi'nde gözden. Size nasıl öğrenilen için:

> [!div class="checklist"]
> * Veri toplamayı ayarlama
> * Güvenlik ilkeleri Ayarla
> * Görüntüleme ve yapılandırma sistem durumu sorunları giderin
> * Algılanan tehditler gözden geçirin

Visual Studio Team Services ve IIS çalıştıran bir Windows VM ile CI/CD işlem hattı oluşturma konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Visual Studio Team Services CI/CD ardışık düzen](./tutorial-vsts-iis-cicd.md)
