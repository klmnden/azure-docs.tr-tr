---
title: Programlamayla Azure panoları oluşturma | Microsoft Docs
description: Bu makalede, programlamayla Azure panoları oluşturma açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: adamabmsft
manager: dougeby
editor: tysonn
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/01/2017
ms.author: kfollis
ms.openlocfilehash: b24a0397a1365479907fedc6348caa54508dbbb0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60552237"
---
# <a name="programmatically-create-azure-dashboards"></a>Programlamayla Azure panoları oluşturma

Bu belge, program aracılığıyla oluşturma ve Azure panoları yayımlama sürecinde yardımcı olur. Aşağıda gösterilen Pano, belge boyunca başvuruluyor.

![Örnek Pano](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Genel Bakış

Azure olan panoları paylaşılan [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) olduğu gibi sanal makineler ve depolama hesapları.  Bu nedenle, program aracılığıyla aracılığıyla yönetilebilir [Azure Resource Manager REST API'leri](/rest/api/), [Azure CLI](https://docs.microsoft.com/cli/azure), [Azure PowerShell komutlarını](https://docs.microsoft.com/powershell/azure/get-started-azureps)ve birçok [ Azure portalında](https://portal.azure.com) kaynak yönetimini kolaylaştırmak için bu API'leri temel özellikler oluşturun.  

Bu API'leri ve araçları, listesi oluşturmak için yol almak, değiştirebilir ve kaynakları silmek sunar.  Panolar kaynaklar olduğundan, en sevdiğiniz API'nizi seçin / aracı.

Hangi aracı kullanın, tüm kaynak oluşturma API'si çağrı yapmadan önce bir JSON temsilini Pano nesnenizin oluşturmak gerekir. Bu nesne (diğer adıyla) bölümleri hakkında bilgi içerir Panoda kutucuğu). Bu, boyutları ve konumları, için bağlı kaynaklar ve herhangi bir kullanıcı özelleştirmelerine içerir.

Bu JSON belgeyi oluşturmak için en kullanışlı yolu [portalı](https://portal.azure.com/) etkileşimli olarak ekleme ve kutucukları getirin. Ardından JSON dışarı aktarın. Son olarak, sonuç betikleri, programlar ve dağıtım araçları daha sonra kullanmak için bir şablon oluşturun.

## <a name="create-a-dashboard"></a>Bir pano oluşturma

Yeni bir Pano oluşturmak için portalın ana ekranında yeni pano komutunu kullanın.

![Yeni Pano komutu](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

Kutucuk Galerisi ardından bulmak ve kutucukları eklemek için de kullanabilirsiniz. Kutucuklar, sürükleyip bırakarak eklenir. Bazı kutucuklar, diğerleri kendi bağlam menüsünde görülebilir boyutları destek düzeltmeler bir sürükleme tutamacı aracılığıyla yeniden boyutlandırılmasını destekleyecek.

### <a name="drag-handle"></a>Sürükleme tutamacı
![Sürükleme tutamacı](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

### <a name="fixed-sizes-via-context-menu"></a>Sabit boyutlar bağlam menüsü aracılığıyla
![boyutları bağlam menüsü](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Panoyu Paylaş

Pano (Paylaş komutunu kullanarak) panoya yayımlayın ve ardından JSON getirmek için kaynak Gezgini'ni kullanma sonraki adımlarla istediğiniz gibi yapılandırdıktan sonra.

![Paylaş komutu](./media/azure-portal-dashboards-create-programmatically/share-command.png)

Paylaşım komutuna tıklayarak yayımlamak için abonelik ve kaynak grubunu seçtikten isteyen bir iletişim kutusu gösterir. Aklınızda [yazma erişiminiz olmalıdır](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) seçtiğiniz abonelik ve kaynak grubu.

![Paylaşım ve erişim](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>Pano JSON temsili getirilemedi

Yayımlama yalnızca birkaç saniye sürer.  İşlem tamamlandığında, sonraki adıma adresine gitmektir [kaynak Gezgini](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) JSON getirilemedi.

![Kaynak Gezgini Gözat](./media/azure-portal-dashboards-create-programmatically/browse-resource-explorer.png)

Kaynak Gezgini'nde seçtiğiniz abonelik ve kaynak grubuna gidin. Ardından, JSON açığa çıkarmak için yeni yayımlanan Pano kaynakta tıklayın.

![Kaynak Gezgini json](./media/azure-portal-dashboards-create-programmatically/resource-explorer-json.png)

## <a name="create-a-template-from-the-json"></a>JSON'dan bir şablon oluşturma

Sonraki adım, programlama yoluyla uygun kaynak yönetimi API'leri, komut satırı araçları ile veya Portal'ın yeniden kullanılabilir, böylece bu JSON'dan şablon oluşturmaktır.

Pano JSON yapısı, bir şablon oluşturmak için tam olarak anlamak gerekli değildir. Çoğu durumda, yapısını ve yapılandırmasını her kutucuğun korumak ve ardından işaret Azure kaynaklarını kümesini parametreleştirmek istediğiniz. Dışarı aktarılan JSON panonuza arayın ve Azure kaynak kimliklerini tüm örneklerini bulun. Bizim örnek Pano sahip birden çok kutucuk tümü tek bir Azure sanal makinenin gelin. Bizim Pano yalnızca bu tek bir kaynakta görünen olmasıdır. (Belgenin sonunda dahil) için örnek json araması yaparsanız "/ abonelikleri", bu kimliğin birkaç yinelemesi Bul

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Herhangi bir sanal makine için bu panoyu yayımlama gelecekte bu dize JSON içinde her örneğini parametre haline getirmek ihtiyacınız. 

Azure'da kaynak oluşturmak API'ları iki türü vardır. [Kesinlik temelli API'leri](https://docs.microsoft.com/rest/api/resources/resources) bir kerede tek bir kaynak oluşturmak ve bir [şablona dayalı dağıtım](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) oluşturma düzenleyebilirsiniz sistem tek bir API çağrısı ile birden çok bağımlı kaynakları. Bizim örneğimizde kullanacağız ikinci yerel olarak Parametreleştirme ve şablon oluşturma destekler.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Program aracılığıyla şablonunuzdan bir şablon dağıtımı'nı kullanarak bir pano oluşturma

Azure, birden fazla kaynak dağıtımını düzenleme olanağı sunar. Onlar arasındaki ilişkileri yanı sıra dağıtmak için kaynak kümesini ifade bir dağıtım şablonu oluşturun.  Tek tek oluşturmakta gibi her kaynak JSON biçimi aynıdır. Fark [şablonu dil](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) değişkenleri, parametreleri, temel işlevleri ve daha fazla gibi birkaç kavram ekler. Bu söz dizimi genişletilmiş yalnızca şablon dağıtımı bağlamında desteklenir ve daha önce açıklanan kesinlik temelli API'leri ile kullanıldığında geçerli değildir.

Bu rota gideceğinizi sonra Parametreleştirme yapılması şablonun parametresi söz dizimini kullanarak.  Burada gösterildiği gibi daha önce bulduk kaynak kimliği tüm örneklerini değiştirin.

### <a name="example-json-property-with-hard-coded-resource-id"></a>Örnek JSON özelliği sabit kodlanmış kaynak kimliği ile
`id: "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"`

### <a name="example-json-property-converted-to-a-parameterized-version-based-on-template-parameters"></a>Şablon parametrelerine göre parametreli bir sürüm için örnek JSON özelliği dönüştürülür.

`id: "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"`

Bazı gerekli şablon meta verileri ve parametreler için json şablonunu şöyle üst kısmındaki bildirmeniz gerekir:

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachineName": {
            "type": "string"
        },
        "virtualMachineResourceGroup": {
            "type": "string"
        },
        "dashboardName": {
            "type": "string"
        }
    },
    "variables": {},

    ... rest of template omitted ...
```

__Bu belgenin sonunda tam çalışma şablonu görebilirsiniz.__

Şablonunuzu hazırlanmış sonra bunu kullanarak dağıtabilirsiniz [REST API'leri](https://docs.microsoft.com/rest/api/resources/deployments), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), [Azure CLI](https://docs.microsoft.com/cli/azure/group/deployment#az-group-deployment-create), veya [portalın şablon dağıtımı sayfası ](https://portal.azure.com/#create/Microsoft.Template).

Bizim örnek Pano JSON'ın iki sürümü aşağıda verilmiştir. İlk biz zaten bir kaynağa bağlı portalından dışarı sürümüdür. İkinci herhangi bir VM için programlı bir şekilde bağlanabilir ve Azure Resource Manager kullanılarak dağıtılan şablon sürümüdür.

## <a name="json-representation-of-our-example-dashboard-before-templating"></a>Bizim örnek Pano (şablon önce) JSON temsili

Zaten dağıtılmış bir Pano JSON temsili getirmek için önceki yönergeleri izlerseniz görmeyi bekleyebileceğiniz budur. Bu Pano belirli bir Azure sanal makine işaret ettiğini gösteren sabit kodlanmış kaynak tanımlayıcıları unutmayın.

```json

{
    "properties": {
        "lenses": {
            "0": {
                "order": 0,
                "parts": {
                    "0": {
                        "position": {
                            "x": 0,
                            "y": 0,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "## Azure Virtual Machines Overview\r\nNew team members should watch this video to get familiar with Azure Virtual Machines.",
                                        "title": "",
                                        "subtitle": ""
                                    }
                                }
                            }
                        }
                    },
                    "1": {
                        "position": {
                            "x": 3,
                            "y": 0,
                            "rowSpan": 4,
                            "colSpan": 8
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
                                        "title": "Test VM Dashboard",
                                        "subtitle": "Contoso"
                                    }
                                }
                            }
                        }
                    },
                    "2": {
                        "position": {
                            "x": 0,
                            "y": 2,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/VideoPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "title": "",
                                        "subtitle": "",
                                        "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
                                        "autoplay": false
                                    }
                                }
                            }
                        }
                    },
                    "3": {
                        "position": {
                            "x": 0,
                            "y": 4,
                            "rowSpan": 3,
                            "colSpan": 11
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Percentage CPU",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "4": {
                        "position": {
                            "x": 0,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "5": {
                        "position": {
                            "x": 3,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "6": {
                        "position": {
                            "x": 6,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Network In",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Network Out",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "7": {
                        "position": {
                            "x": 9,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 2
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "id",
                                    "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart",
                            "asset": {
                                "idInputName": "id",
                                "type": "VirtualMachine"
                            },
                            "defaultMenuItemId": "overview"
                        }
                    }
                }
            }
        },
        "metadata": { }
    },
    "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/dashboards/providers/Microsoft.Portal/dashboards/aa9786ae-e159-483f-b05f-1f7f767741a9",
    "name": "aa9786ae-e159-483f-b05f-1f7f767741a9",
    "type": "Microsoft.Portal/dashboards",
    "location": "eastasia",
    "tags": {
        "hidden-title": "Created via API"
    }
}

```

### <a name="template-representation-of-our-example-dashboard"></a>Bizim örnek Pano Şablonu temsili

Pano şablon sürümünü adlı üç parametrenin tanımladığı __Sourceserver__, __virtualMachineResourceGroup__, ve __dashboardName__.  Parametreleri dağıttığınız her seferinde farklı bir Azure sanal makine Bu panoyu noktası sağlar. Parametreli kimlikleri Bu panoyu programlı olarak yapılandırılabilir ve tüm Azure sanal makinesine işaret edecek şekilde dağıtılan olduğunu göstermek üzere vurgulanır. Aşağıdaki şablonu kopyalayın ve yapıştırın bu özelliği test etmek için en kolay yolu olan [Azure portal'ın şablon dağıtım sayfasına](https://portal.azure.com/#create/Microsoft.Template). 

Bu örnek bir pano, kendisi tarafından dağıtır, ancak şablon dili birden çok kaynak dağıtıp yanı sıra bir veya daha fazla panolar paket sağlar bunları. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachineName": {
            "type": "string"
        },
        "virtualMachineResourceGroup": {
            "type": "string"
        },
        "dashboardName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "properties": {
                "lenses": {
                    "0": {
                        "order": 0,
                        "parts": {
                            "0": {
                                "position": {
                                    "x": 0,
                                    "y": 0,
                                    "rowSpan": 2,
                                    "colSpan": 3
                                },
                                "metadata": {
                                    "inputs": [],
                                    "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                                    "settings": {
                                        "content": {
                                            "settings": {
                                                "content": "## Azure Virtual Machines Overview\r\nNew team members should watch this video to get familiar with Azure Virtual Machines.",
                                                "title": "",
                                                "subtitle": ""
                                            }
                                        }
                                    }
                                }
                            },
                            "1": {
                                "position": {
                                    "x": 3,
                                    "y": 0,
                                    "rowSpan": 4,
                                    "colSpan": 8
                                },
                                "metadata": {
                                    "inputs": [],
                                    "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                                    "settings": {
                                        "content": {
                                            "settings": {
                                                "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
                                                "title": "Test VM Dashboard",
                                                "subtitle": "Contoso"
                                            }
                                        }
                                    }
                                }
                            },
                            "2": {
                                "position": {
                                    "x": 0,
                                    "y": 2,
                                    "rowSpan": 2,
                                    "colSpan": 3
                                },
                                "metadata": {
                                    "inputs": [],
                                    "type": "Extension[azure]/HubsExtension/PartType/VideoPart",
                                    "settings": {
                                        "content": {
                                            "settings": {
                                                "title": "",
                                                "subtitle": "",
                                                "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
                                                "autoplay": false
                                            }
                                        }
                                    }
                                }
                            },
                            "3": {
                                "position": {
                                    "x": 0,
                                    "y": 4,
                                    "rowSpan": 3,
                                    "colSpan": 11
                                },
                                "metadata": {
                                    "inputs": [
                                        {
                                            "name": "queryInputs",
                                            "value": {
                                                "timespan": {
                                                    "duration": "PT1H",
                                                    "start": null,
                                                    "end": null
                                                },
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Percentage CPU",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    }
                                                ]
                                            }
                                        }
                                    ],
                                    "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                                    "settings": {}
                                }
                            },
                            "4": {
                                "position": {
                                    "x": 0,
                                    "y": 7,
                                    "rowSpan": 2,
                                    "colSpan": 3
                                },
                                "metadata": {
                                    "inputs": [
                                        {
                                            "name": "queryInputs",
                                            "value": {
                                                "timespan": {
                                                    "duration": "PT1H",
                                                    "start": null,
                                                    "end": null
                                                },
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Disk Read Operations/Sec",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Disk Write Operations/Sec",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    }
                                                ]
                                            }
                                        }
                                    ],
                                    "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                                    "settings": {}
                                }
                            },
                            "5": {
                                "position": {
                                    "x": 3,
                                    "y": 7,
                                    "rowSpan": 2,
                                    "colSpan": 3
                                },
                                "metadata": {
                                    "inputs": [
                                        {
                                            "name": "queryInputs",
                                            "value": {
                                                "timespan": {
                                                    "duration": "PT1H",
                                                    "start": null,
                                                    "end": null
                                                },
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Disk Read Bytes",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Disk Write Bytes",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    }
                                                ]
                                            }
                                        }
                                    ],
                                    "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                                    "settings": {}
                                }
                            },
                            "6": {
                                "position": {
                                    "x": 6,
                                    "y": 7,
                                    "rowSpan": 2,
                                    "colSpan": 3
                                },
                                "metadata": {
                                    "inputs": [
                                        {
                                            "name": "queryInputs",
                                            "value": {
                                                "timespan": {
                                                    "duration": "PT1H",
                                                    "start": null,
                                                    "end": null
                                                },
                                                "id": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]",
                                                "chartType": 0,
                                                "metrics": [
                                                    {
                                                        "name": "Network In",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    },
                                                    {
                                                        "name": "Network Out",
                                                        "resourceId": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                                    }
                                                ]
                                            }
                                        }
                                    ],
                                    "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                                    "settings": {}
                                }
                            },
                            "7": {
                                "position": {
                                    "x": 9,
                                    "y": 7,
                                    "rowSpan": 2,
                                    "colSpan": 2
                                },
                                "metadata": {
                                    "inputs": [
                                        {
                                            "name": "id",
                                            "value": "[resourceId(parameters('virtualMachineResourceGroup'), 'Microsoft.Compute/virtualMachines', parameters('virtualMachineName'))]"
                                        }
                                    ],
                                    "type": "Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart",
                                    "asset": {
                                        "idInputName": "id",
                                        "type": "VirtualMachine"
                                    },
                                    "defaultMenuItemId": "overview"
                                }
                            }
                        }
                    }
                }
            },
            "metadata": { },
            "apiVersion": "2015-08-01-preview",
            "type": "Microsoft.Portal/dashboards",
            "name": "[parameters('dashboardName')]",
            "location": "westus",
            "tags": {
                "hidden-title": "[parameters('dashboardName')]"
            }
        }
    ]
}


```
