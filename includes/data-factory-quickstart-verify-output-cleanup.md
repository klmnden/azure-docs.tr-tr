---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 831a72fff0931d116a669060b160f51fde6e1d3e
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572865"
---
## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı adftutorial blob kapsayıcısında çıktı klasörünü otomatik olarak oluşturur. Ardından, giriş klasöründeki emp.txt dosyasını çıktı klasörüne kopyalar. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı sayfasında **Yenile**’ye tıklayın. 
    
    ![Yenile](media/data-factory-quickstart-verify-output-cleanup/output-refresh.png)
2. Klasör listesinde **output** öğesine tıklayın. 
2. **emp.txt** dosyasının output klasörüne kopyalandığını onaylayın. 

    ![Yenile](media/data-factory-quickstart-verify-output-cleanup/output-file.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki şekilde temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../articles/azure-resource-manager/resource-group-overview.md) silebilirsiniz. Diğer kaynakları korumak istiyorsanız yalnızca bu öğreticide oluşturduğunuz veri kaynağını silin.

Bir kaynak grubunun silinmesi, içindeki veri fabrikaları dahil olmak üzere tüm kaynakları siler. Kaynak grubunun tamamını silmek için şu komutu çalıştırın: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Not: Bir kaynak grubunun bırakılması biraz zaman alabilir. Lütfen işlemin tamamlanmasını bekleyin

Tüm kaynak grubu yerine yalnızca veri fabrikasını silmek istiyorsanız aşağıdaki aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```