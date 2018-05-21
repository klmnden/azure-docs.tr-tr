---
title: Azure Resource Manager şablonunu dışarı aktarma | Microsoft Belgeleri
description: Mevcut bir kaynak grubundaki şablonu dışarı aktarmak için Azure Resource Manager’ı kullanın.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: 14aa54277cac3369df739a1d84580624f2d3b401
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma
Bu makalede aboneliğinizde var olan kaynaklardan bir Resource Manager şablonunun nasıl dışarı aktarıldığı öğretilir. Şablon söz dizimini daha iyi anlamak için bu oluşturulmuş şablonu kullanabilirsiniz.

Şablonu dışarı aktarmanın iki yolu vardır:

* **Dağıtım için kullanılan gerçek şablonu** dışarı aktarabilirsiniz. Dışarı aktarılan şablonda, tüm parametreler ve değişkenler özgün şablondaki gibidir. Bu yaklaşım kaynakları portal üzerinden dağıttığınızda ve bu kaynakları oluşturmak için kullanılan şablonu görmek istediğinizde yararlıdır. Bu şablon kullanıma hazırdır. 
* **Kaynak grubunun geçerli durumunu temsil eden, oluşturulmuş bir şablonu** dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullandığınız herhangi bir şablonu temel almaz. Bunun yerine, bir "anlık görüntüsü" veya "yedek" kaynak grubunun olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer ve büyük olasılıkla normalde tanımlayacağınızdan daha az sayıda parametre içerir. Kaynaklar aynı kaynak grubuna yeniden dağıtmak için bu seçeneği kullanın. Başka bir kaynak grubu için bu şablonu kullanmak için önemli ölçüde değiştirmeniz gerekebilir.

Bu makalede, portal üzerinden her iki yaklaşımın gösterilmektedir.

## <a name="deploy-resources"></a>Kaynakları dağıtma
Şablon olarak dışarı aktarma için kullanabileceğiniz kaynakları Azure’a dağıtma işlemiyle başlayalım. Aboneliğinizde zaten şablona dışarı aktarmak istediğiniz bir kaynak grubu varsa, bu bölümü atlayabilirsiniz. Web uygulaması ve SQL veritabanı çözümü bu bölümde gösterilen dağıttıktan sonra, bu makalenin kalanında varsayar. Farklı bir çözüm kullanıyorsanız, sizin deneyiminiz biraz farklı olabilir ama şablonu dışarı aktarma adımları aynıdır. 

1. İçinde [Azure portal](https://portal.azure.com)seçin **kaynak oluşturma**.
   
      ![Yeni Seç](./media/resource-manager-export-template/new.png)
2. **Web uygulaması + SQL** için arama yapın ve sağlanan seçeneklerden bunu seçin.
   
      ![Arama web uygulaması ve SQL](./media/resource-manager-export-template/webapp-sql.png)

3. **Oluştur**’u seçin.

      ![Bu seçeneği belirleyin](./media/resource-manager-export-template/create.png)

4. Web uygulaması ve SQL veritabanı için gerekli değerleri sağlayın. **Oluştur**’u seçin.

      ![Web ve SQL değeri sağlayın](./media/resource-manager-export-template/provide-web-values.png)

Dağıtım birkaç dakika sürebilir. Dağıtım tamamlandıktan sonra, aboneliğiniz çözümü içerir.

## <a name="view-template-from-deployment-history"></a>Dağıtım geçmişinden şablonu görüntüleme
1. Yeni kaynak grubunuz için kaynak grubuna gidin. Portal son dağıtımının sonucu gösterdiğine dikkat edin. Bu bağlantıyı seçin.
   
      ![Kaynak grubu](./media/resource-manager-export-template/select-deployment.png)
2. Grup için dağıtım geçmişini görürsünüz. Sizin durumunuzda, portal, büyük olasılıkla yalnızca bir dağıtım listeler. Bu dağıtımı seçin.
   
     ![son dağıtım](./media/resource-manager-export-template/select-history.png)
3. Portal dağıtım özetini görüntüler. Özet, dağıtımın ve işlemlerinin durumunu ve sağladığınız parametreler için değerleri içerir. Dağıtım için kullandığınız şablonu görmek için **Şablonu görüntüle**’yi seçin.
   
     ![Dağıtım özetini görüntüleme](./media/resource-manager-export-template/view-template.png)
4. Resource Manager sizin için aşağıdaki yedi dosyayı alır:
   
   1. **Şablon** - Çözümünüze ait altyapıyı tanımlayan şablon. Portal üzerinden depolama hesabı oluşturduğunuzda, Resource Manager bunu dağıtmak için bir şablon kullandı ve bu şablonu gelecekte başvurmak üzere kaydetti.
   2. **Parametreler**: Dağıtım sırasında değerleri geçirmek için kullanabileceğiniz bir parametre dosyası. İlk dağıtım sırasında sağladığınız değerleri içerir. Şablonu yeniden dağıtırken bu değerlerden herhangi birini değiştirebilirsiniz.
   3. **CLI**: Şablonu dağıtmak için kullanabileceğiniz bir Azure komut satırı arabirimi (CLI) betik dosyası.
   3. **CLI 2.0** - Şablonu dağıtmak için kullanabileceğiniz bir Azure komut satırı arabirimi (CLI) betik dosyası.
   4. **PowerShell**: Şablonu dağıtmak için kullanabileceğiniz bir Azure PowerShell betiği.
   5. **.NET**: Şablonu dağıtmak için kullanabileceğiniz bir .NET sınıfı.
   6. **Ruby** - Şablonu dağıtmak için kullanabileceğiniz bir Ruby sınıfı.
      
      Varsayılan olarak, portal şablonunu görüntüler.
      
       ![Şablonu görüntüle](./media/resource-manager-export-template/see-template.png)
      
Web uygulamanızı ve SQL veritabanınızı oluşturmak için kullanılan gerçek şablon budur. Dağıtım sırasında farklı değerler sağlamanıza olanak tanıyan parametreler içerdiğine dikkat edin. Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).

## <a name="export-the-template-from-resource-group"></a>Şablonu kaynak grubundan dışarı aktarma
El ile kaynaklarınızı değiştirdiyseniz ya da birden çok dağıtımlarda kaynakları eklenen, bir şablonu dağıtım geçmişinden alma kaynak grubunun geçerli durumunu yansıtmaz. Bu bölümde kaynak grubunun geçerli durumunu yansıtan bir şablonun nasıl dışarı aktarıldığı gösterilir. Aynı kaynak grubuna dağıtmak için kullanabileceğiniz kaynak grubunun anlık görüntü olarak tasarlanmıştır. Diğer çözümleri için dışarı aktarılan şablonu kullanmak için önemli ölçüde değiştirmeniz gerekir.

> [!NOTE]
> 200'den fazla kaynaklara sahip bir kaynak grubu için bir şablonu dışarı aktaramazsınız.
> 
> 

1. Bir kaynak grubu için şablonu görüntülemek üzere **Otomasyon betiği**’ni seçin.
   
      ![kaynak grubunu dışarı aktarma](./media/resource-manager-export-template/select-automation.png)
   
     Resource Manager, kaynak grubundaki kaynakları değerlendirir ve söz konusu kaynaklar için bir şablon oluşturur. Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Dışarı aktarma işleminde sorun olduğunu belirten bir hata görebilirsiniz. Bu sorunların nasıl ele alınacağını [Dışarı aktarma sorunlarını düzeltme](#fix-export-issues) bölümünden öğrenebilirsiniz.
2. Çözümü yeniden dağıtmak için kullanabileceğiniz altı dosyayı yeniden görürsünüz. Öte yandan, bu kez şablon biraz farklıdır. Oluşturulan şablonun, önceki bölümdeki şablondan daha az parametre içerdiğine dikkat edin. Ayrıca, bu şablonda birçok değer (konum ve SKU değerleri gibi) parametre değeri kabul etmek yerine sabit kodlanmıştır. Bu şablonu yeniden kullanmadan önce, şablonu düzenleyerek parametrelerin daha iyi kullanılmasını sağlamak isteyebilirsiniz. 
   
3. Bu şablonla çalışmaya devam etmek için kullanabileceğiniz iki seçenek vardır. Şablonu indirebilir ve JSON düzenleyicisiyle üzerinde yerel olarak çalışabilirsiniz. Alternatif olarak, şablonu kitaplığınıza kaydedip portal aracılığıyla üzerinde çalışabilirsiniz.
   
     [VS Code](https://code.visualstudio.com/) veya [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) gibi bir JSON düzenleyicisini kullanabiliyorsanız, şablonu yerel olarak indirip bu düzenleyiciyi kullanmayı seçebilirsiniz. Yerel olarak çalışmak için **İndir**’i seçin.
   
      ![Şablon indirme](./media/resource-manager-export-template/download-template.png)
   
     Bir JSON düzenleyicisiyle ayarlamazsanız, portal üzerinden şablon düzenlemeyi tercih edebilirsiniz. Bu makalenin geri kalanında kitaplığınıza portalında şablon kaydettiğiniz varsayar. Bununla birlikte, ister JSON düzenleyicisiyle yerel olarak çalışın ister portal aracılığıyla çalışın şablon üzerinde aynı söz dizimi değişikliklerini yaparsınız. Portal üzerinden çalışmak için **Kitaplığa ekle**’yi seçin.
   
      ![Kitaplığa ekle](./media/resource-manager-export-template/add-to-library.png)
   
     Bir şablon kitaplığa eklerken, bir ad ve açıklama şablonuna verin. Ardından **Kaydet**’i seçin.
   
     ![kümesi şablon değerleri](./media/resource-manager-export-template/save-library-template.png)
4. Kitaplığınıza kaydedilen bir şablonu görüntülemek için **Diğer hizmetler**’i seçin, sonuçları filtrelemek için **Şablonlar** yazın ve **Şablonlar**’ı seçin.
   
      ![Şablonları bulunamadı](./media/resource-manager-export-template/find-templates.png)
5. Kaydettiğiniz ada sahip şablonu seçin.
   
      ![Şablonu seçin](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-the-template"></a>Şablonu özelleştirme
Dışarı aktarılan şablon, her dağıtım için aynı web uygulamasını ve SQL veritabanını oluşturmak isterseniz düzgün çalışır. Bununla birlikte Resource Manager, şablonları çok daha fazla esneklikle dağıtabileceğiniz seçenekler sunar. Bu makalede veritabanı yöneticisi adı ve parolası için parametrelerin nasıl ekleneceği gösterilir. Şablondaki diğer değerlere daha fazla esneklik getirmek için de bu yaklaşımı kullanabilirsiniz.

1. Şablonu özelleştirmek için, **Düzenle**’yi seçin.
   
     ![Şablonu göster](./media/resource-manager-export-template/select-edit.png)
2. Şablonu seçin.
   
     ![Şablonu düzenle](./media/resource-manager-export-template/select-added-template.png)
3. Dağıtım sırasında belirtmek isteyebileceğiniz değerleri geçirebilmek için, şablondaki **parameters** bölümüne aşağıdaki iki parametreyi ekleyin:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. Yeni parametreleri kullanmak için, **resources** bölümündeki SQL Server tanımını değiştirin. Şimdi **administratorLogin** ve **administratorLoginPassword** için parametre değerlerinin kullanıldığına dikkat edin.

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. Şablonu düzenlemeyi tamamladığınızda **Tamam**’ı seçin.
7. Şablonda yapılan değişiklikleri kaydetmek için **Kaydet**’i seçin.
   
     ![Şablonu kaydetme](./media/resource-manager-export-template/save-template.png)
8. Güncelleştirilmiş şablonu yeniden dağıtmak için **Dağıt**’ı seçin.
   
     ![Şablon dağıtma](./media/resource-manager-export-template/redeploy-template.png)
9. Parametre değerlerini sağlayın ve kaynakların dağıtılacağı kaynak grubunu seçin.


## <a name="fix-export-issues"></a>Dışarı aktarma sorunlarını düzeltme
Şablonu dışarı aktarma işlevini tüm kaynak türleri desteklemez. Bu sorunu çözümlemek için, eksik kaynakları şablonunuza el ile tekrar ekleyin. Hata iletisi, dışarı aktarılamayan kaynak türlerini içerir. Bu kaynak türünü [Şablon başvurusunda](/azure/templates/) bulun. Örneğin, bir sanal ağ geçidini el ile eklemek için [Microsoft.Network/virtualNetworkGateways şablon başvurusuna](/azure/templates/microsoft.network/virtualnetworkgateways) bakın.

> [!NOTE]
> Yalnızca, dağıtım geçmişiniz yerine bir kaynak grubundan dışarı aktarma yaparken dışarı aktarma sorunlarıyla karşılaşırsınız. Son dağıtımınız kaynak grubunun geçerli durumunu doğru şekilde temsil ediyorsa, şablonu kaynak grubu yerine dağıtım geçmişinden dışarı aktarmanız gerekir. Yalnızca tek bir şablonda tanımlanmamış kaynak grubunda değişiklikler yaptığınızda kaynak grubundan dışarı aktarma yapın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

* Bir şablonu [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) veya [REST API](resource-group-template-deploy-rest.md) aracılığıyla dağıtabilirsiniz.
* Bir şablonu PowerShell aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz: [PowerShell ile dışarı aktarma Azure Resource Manager şablonları](resource-manager-export-template-powershell.md).
* Bir şablonu Azure CLI aracılığıyla nasıl dışarı aktaracağınızı görmek için bkz: [verme Azure Resource Manager şablonları Azure CLI ile](resource-manager-export-template-cli.md).

