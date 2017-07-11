---
title: "SSDT kullanarak Azure Analysis Services&quot;e dağıtma | Microsoft Docs"
description: "SSDT kullanarak bir tablo modelini Azure Analysis Services sunucusuna dağıtma hakkında bilgi edinin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 0b15399cade0a9dc21b2274a64172d65f2f4e877
ms.contentlocale: tr-tr
ms.lasthandoff: 06/03/2017


---
<a id="deploy-a-model-from-ssdt" class="xliff"></a>

# SSDT’den model dağıtma
Azure aboneliğinizde bir sunucu oluşturduktan sonra, aboneliğinize bir tablo modeli dağıtmaya hazır olursunuz. Üzerinde çalışmakta olduğunuz bir tablo modeli projesini derleyip dağıtmak için SQL Server Veri Araçları’nı (SSDT) kullanabilirsiniz. 

<a id="before-you-begin" class="xliff"></a>

## Başlamadan önce
Başlamak için gerekli olanlar:

* Azure’da **Analysis Services sunucusu**. Daha fazla bilgi için bkz. [Azure Analysis Services sunucusu oluşturma](analysis-services-create-server.md).
* SSDT’deki **tablo modeli projesi** ya da 1200 veya üzeri uyumluluk düzeyine sahip mevcut bir tablo modeli. Daha önce hiç oluşturmadınız mı? [Adventure Works Öğreticisi](https://msdn.microsoft.com/library/hh231691.aspx)’ni deneyin.
* **Şirket içi ağ geçidi** - Bir veya daha fazla veri kaynağı kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklemeniz gerekir. Ağ geçidi, buluttaki sunucunuzun modeldeki verileri işlemek ve yenilemek üzere şirket içi veri kaynaklarınıza bağlanması için gereklidir.

> [!TIP]
> Dağıtmadan önce tablolarınızdaki verileri işleyebildiğinizden emin olun. SSDT’de **Model** > **İşlem** > **Tümünü İşle**’ye tıklayın. İşleme başarısız olursa dağıtımı başarıyla yapamazsınız.
> 
> 

<a id="to-deploy-a-tabular-model-from-ssdt" class="xliff"></a>

## SSDT’den tablo modeli dağıtmak için

1. Dağıtmadan önce sunucu adını almanız gerekir. **Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
    ![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. SSDT > **Çözüm Gezgini** menüsünde projeye sağ tıklayın > **Özellikler**’e tıklayın. Ardından **Dağıtım** > **Sunucu** alanına sunucu adını yapıştırın.   
   
    ![Sunucu adını dağıtım sunucusu özelliğine yapıştırma](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. **Çözüm Gezgini**’nde **Özellikler**’e sağ tıklayıp **Dağıt**’a tıklayın. Azure'da oturum açmanız istenebilir.
   
    ![Sunucuya dağıtma](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Dağıtım durumu hem Çıktı penceresinde hem de Dağıt menüsünde görünür.
   
    ![Dağıtım durumu](./media/analysis-services-deploy/aas-deploy-status.png)

İşte bu kadar!


<a id="but-something-went-wrong" class="xliff"></a>

## Ancak, bir sorun oluştu
Meta verileri dağıtırken dağıtım başarısız olursa, bunun nedeni SSDT’nin sunucunuza bağlanamaması olabilir. SSMS kullanarak sunucunuza bağlanabildiğinizden emin olun. Ardından projenin Deployment Server özelliğinin doğru olduğundan emin olun.

Bir tabloda dağıtım başarısız olursa, bunun nedeni sunucunuzun bir veri kaynağına bağlanamaması olabilir. Veri kaynağınız kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklediğinizden emin olun.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
Tablo modelinizi sunucunuza dağıttığınıza göre bağlanmak için hazırsınız. [SSMS ile sunucunuza bağlanarak](analysis-services-manage.md) sunucuyu yönetebilirsiniz. Ayrıca, Power BI, Power BI Desktop veya Excel gibi [bir istemci araç kullanarak bağlanabilir](analysis-services-connect.md) ve rapor oluşturmaya başlayabilirsiniz.


