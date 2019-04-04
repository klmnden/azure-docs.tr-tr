---
title: Visual Studio (SSDT) kullanarak Azure Analysis Services'e dağıtma | Microsoft Docs
description: SSDT kullanarak bir tablo modelini Azure Analysis Services sunucusuna dağıtma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b9740d74a25964286ea92b4238684db81a64c9e2
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893614"
---
# <a name="deploy-a-model-from-visual-studio"></a>Visual Studio üzerinden model dağıtma

Azure aboneliğinizde bir sunucu oluşturduktan sonra, aboneliğinize bir tablo modeli dağıtmaya hazır olursunuz. Üzerinde çalışmakta olduğunuz bir tablo modeli projesini derleyip dağıtmak için SQL Server Veri Araçları’nı (SSDT) kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Başlamak için gerekli olanlar:

* Azure’da **Analysis Services sunucusu**. Daha fazla bilgi için bkz. [Azure Analysis Services sunucusu oluşturma](analysis-services-create-server.md).
* SSDT’deki **tablosal model projesi** ya da 1200 veya üzeri uyumluluk düzeyine sahip mevcut bir tablosal model. Daha önce hiç oluşturmadınız mı? [Adventure Works İnternet satışı tablosal modelleme öğreticisini](/sql/analysis-services/tabular-modeling-adventure-works-tutorial) deneyin.
* **Şirket içi ağ geçidi** - Bir veya daha fazla veri kaynağı kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklemeniz gerekir. Ağ geçidi, buluttaki sunucunuzun modeldeki verileri işlemek ve yenilemek üzere şirket içi veri kaynaklarınıza bağlanması için gereklidir.

> [!TIP]
> Dağıtmadan önce tablolarınızdaki verileri işleyebildiğinizden emin olun. SSDT’de **Model** > **İşlem** > **Tümünü İşle**’ye tıklayın. İşleme başarısız olursa dağıtımı başarıyla yapamazsınız.
> 
> 

## <a name="get-the-server-name"></a>Sunucu adını alma

**Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="to-deploy-from-ssdt"></a>SSDT'den dağıtmak için

1. SSDT > **Çözüm Gezgini** menüsünde projeye sağ tıklayın > **Özellikler**’e tıklayın. Ardından **Dağıtım** > **Sunucu** alanına sunucu adını yapıştırın.   
   
    ![Sunucu adını dağıtım sunucusu özelliğine yapıştırma](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
2. **Çözüm Gezgini**’nde **Özellikler**’e sağ tıklayıp **Dağıt**’a tıklayın. Azure'da oturum açmanız istenebilir.
   
    ![Sunucuya dağıtma](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Dağıtım durumu hem Çıktı penceresinde hem de Dağıt menüsünde görünür.
   
    ![Dağıtım durumu](./media/analysis-services-deploy/aas-deploy-status.png)

İşte bu kadar!


## <a name="troubleshooting"></a>Sorun giderme

Meta verileri dağıtırken dağıtım başarısız olursa, bunun nedeni SSDT’nin sunucunuza bağlanamaması olabilir. SSMS kullanarak sunucunuza bağlanabildiğinizden emin olun. Ardından projenin Deployment Server özelliğinin doğru olduğundan emin olun.

Bir tabloda dağıtım başarısız olursa, bunun nedeni sunucunuzun bir veri kaynağına bağlanamaması olabilir. Veri kaynağınız kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Tablo modelinizi sunucunuza dağıttığınıza göre bağlanmak için hazırsınız. [SSMS ile sunucunuza bağlanarak](analysis-services-manage.md) sunucuyu yönetebilirsiniz. Ayrıca, Power BI, Power BI Desktop veya Excel gibi [bir istemci araç kullanarak bağlanabilir](analysis-services-connect.md) ve rapor oluşturmaya başlayabilirsiniz.

