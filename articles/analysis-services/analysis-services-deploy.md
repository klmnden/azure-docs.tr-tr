---
title: SSDT kullanarak Azure Analysis Services'e dağıtma | Microsoft Docs
description: SSDT kullanarak bir tablo modelini Azure Analysis Services sunucusuna dağıtma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a25066ef8446449148bc0ca95989dc6ca3ca6839
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="deploy-a-model-from-ssdt"></a>SSDT’den model dağıtma
Azure aboneliğinizde bir sunucu oluşturduktan sonra, aboneliğinize bir tablo modeli dağıtmaya hazır olursunuz. Üzerinde çalışmakta olduğunuz bir tablo modeli projesini derleyip dağıtmak için SQL Server Veri Araçları’nı (SSDT) kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Başlamak için gerekli olanlar:

* Azure’da **Analysis Services sunucusu**. Daha fazla bilgi için bkz. [Azure Analysis Services sunucusu oluşturma](analysis-services-create-server.md).
* SSDT’deki **tablosal model projesi** ya da 1200 veya üzeri uyumluluk düzeyine sahip mevcut bir tablosal model. Daha önce hiç oluşturmadınız mı? [Adventure Works İnternet satışı tablosal modelleme öğreticisini](https://msdn.microsoft.com/library/hh231691.aspx) deneyin.
* **Şirket içi ağ geçidi** - Bir veya daha fazla veri kaynağı kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklemeniz gerekir. Ağ geçidi, buluttaki sunucunuzun modeldeki verileri işlemek ve yenilemek üzere şirket içi veri kaynaklarınıza bağlanması için gereklidir.

> [!TIP]
> Dağıtmadan önce tablolarınızdaki verileri işleyebildiğinizden emin olun. SSDT’de **Model** > **İşlem** > **Tümünü İşle**’ye tıklayın. İşleme başarısız olursa dağıtımı başarıyla yapamazsınız.
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a>SSDT’den tablo modeli dağıtmak için

1. Dağıtmadan önce sunucu adını almanız gerekir. **Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
    ![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. SSDT > **Çözüm Gezgini** menüsünde projeye sağ tıklayın > **Özellikler**’e tıklayın. Ardından **Dağıtım** > **Sunucu** alanına sunucu adını yapıştırın.   
   
    ![Sunucu adını dağıtım sunucusu özelliğine yapıştırma](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. **Çözüm Gezgini**’nde **Özellikler**’e sağ tıklayıp **Dağıt**’a tıklayın. Azure'da oturum açmanız istenebilir.
   
    ![Sunucuya dağıtma](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Dağıtım durumu hem Çıktı penceresinde hem de Dağıt menüsünde görünür.
   
    ![Dağıtım durumu](./media/analysis-services-deploy/aas-deploy-status.png)

İşte bu kadar!


## <a name="troubleshooting"></a>Sorun giderme
Meta verileri dağıtırken dağıtım başarısız olursa, bunun nedeni SSDT’nin sunucunuza bağlanamaması olabilir. SSMS kullanarak sunucunuza bağlanabildiğinizden emin olun. Ardından projenin Deployment Server özelliğinin doğru olduğundan emin olun.

Bir tabloda dağıtım başarısız olursa, bunun nedeni sunucunuzun bir veri kaynağına bağlanamaması olabilir. Veri kaynağınız kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Tablo modelinizi sunucunuza dağıttığınıza göre bağlanmak için hazırsınız. [SSMS ile sunucunuza bağlanarak](analysis-services-manage.md) sunucuyu yönetebilirsiniz. Ayrıca, Power BI, Power BI Desktop veya Excel gibi [bir istemci araç kullanarak bağlanabilir](analysis-services-connect.md) ve rapor oluşturmaya başlayabilirsiniz.

