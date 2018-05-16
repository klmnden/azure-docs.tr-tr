---
title: Program aracılığıyla Azure panolar oluşturun | Microsoft Docs
description: Bu makalede program aracılığıyla Azure panoları oluşturmayı açıklar.
services: azure-portal
documentationcenter: ''
author: adamab
manager: dougeby
editor: tysonn
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/01/2017
ms.author: adamab
ms.openlocfilehash: 8670d25e10b58c40b9d0807de1db88c3296b193d
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="programmatically-create-azure-dashboards"></a>Program aracılığıyla Azure panolar oluşturun

Bu belge, program aracılığıyla oluşturma ve Azure panolar yayımlama işleminde size kılavuzluk eder. Aşağıda gösterilen Pano belge boyunca başvuruluyor.

![Örnek Pano](./media/azure-portal-dashboards-create-programmatically/sample-dashboard.png)

## <a name="overview"></a>Genel Bakış

Azure olan panolarında paylaşılan [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) olduğu gibi sanal makineleri ve depolama hesapları.  Bu nedenle, bunlar program aracılığıyla aracılığıyla yönetilebilir [Azure Resource Manager REST API'leri](/rest/api/), [Azure CLI](https://docs.microsoft.com/cli/azure), [Azure PowerShell komutlarını](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-4.2.0)ve birçok [ Azure portal](https://portal.azure.com) özellikleri oluşturmak kaynak yönetimini kolaylaştırmak için bu API'leri üstünde.  

Her API ve araçları, listesi oluşturmak için yol almak, değiştirmek ve kaynakları silmek sunar.  Panolar kaynaklar olduğundan, sık kullanılan API'nizi çekme / aracı.

Kullandığınız hangi aracı kullanırsanız, tüm kaynak oluşturma API çağırmadan önce Pano nesnenizin bir JSON temsilini oluşturmak gerekir. Bu nesne (paketini bölümleri hakkında bilgi içerir Panoda bölmeleri). Boyutları, konumları, bağlı olan kaynaklar ve herhangi bir kullanıcı özelleştirmelerine içerir.

Bu JSON belgeyi oluşturmak için en kullanışlı yolu kullanmaktır [portal](https://portal.azure.com/) etkileşimli olarak ekleme ve kutucuklarınız getirin. Ardından JSON dışarı aktarın. Son olarak, sonuç komut dosyaları, programları ve dağıtım araçları daha sonra kullanmak için bir şablon oluşturun.

## <a name="create-a-dashboard"></a>Bir pano oluşturun

Yeni bir Pano oluşturmak için portal'ın ana ekranda yeni pano komutunu kullanın.

![Yeni Pano komutu](./media/azure-portal-dashboards-create-programmatically/new-dashboard-command.png)

Döşeme galeri ardından bulmak ve kutucukları eklemek için de kullanabilirsiniz. Döşeme sürükleyip bırakarak eklenir. Bazı kutucuklar destek kendi bağlam menüsünde görülebilir boyutları giderir diğerlerinin bir Sürükle tanıtıcı aracılığıyla yeniden boyutlandırma destekler.

### <a name="drag-handle"></a>tutamacını sürükleyin
![tutamacını sürükleyin](./media/azure-portal-dashboards-create-programmatically/drag-handle.png)

### <a name="fixed-sizes-via-context-menu"></a>Bağlam menüsü aracılığıyla sabit boyutları
![boyutları bağlam menüsü](./media/azure-portal-dashboards-create-programmatically/sizes-context-menu.png)

## <a name="share-the-dashboard"></a>Panoyu paylaşın

Sonraki adımlar (Share komutunu kullanarak) panoya yayımlamak ve JSON getirmek için kaynak Gezgini'ni kullanmak üzeresiniz, istediğiniz panonun yapılandırdıktan sonra.

![Paylaşım komutu](./media/azure-portal-dashboards-create-programmatically/share-command.png)

Paylaşımı komutunu tıklatarak yayımlamak için hangi aboneliğe ve kaynak grubu seçmenizi isteyen bir iletişim kutusunu gösterir. Aklınızda [yazma erişimi olmalıdır](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) seçtiğiniz abonelik ve kaynak grubuna.

![Paylaşım ve erişim](./media/azure-portal-dashboards-create-programmatically/sharing-and-access.png)

## <a name="fetch-the-json-representation-of-the-dashboard"></a>Pano JSON gösterimi getirme

Yayımlama, yalnızca birkaç saniye sürer.  Tamamlandığında, sonraki adıma gitmek için olan [kaynak Gezgini](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) JSON getirilemedi.

![Kaynak Gezgini Gözat](./media/azure-portal-dashboards-create-programmatically/browse-resource-explorer.png)

Kaynak Gezgini'nden seçtiğiniz abonelik ve kaynak grubuna gidin. Ardından, JSON ortaya çıkarmak için yeni yayımlanan Pano kaynak'ı tıklatın.

![Kaynak Gezgini json](./media/azure-portal-dashboards-create-programmatically/resource-explorer-json.png)

## <a name="create-a-template-from-the-json"></a>JSON öğesinden bir şablon oluşturma

Komut satırı araçlarını, uygun kaynak yönetim API'si ile ya da portal içinde program aracılığıyla kullanılabilme bu JSON öğesinden bir şablon oluşturmak için sonraki adımdır bakın.

Bir şablon oluşturmak için Pano JSON yapısındaki tam olarak anlamak gerekli değildir. Çoğu durumda, her bölme yapılandırmasını ve yapısı korumak ve fareyle Azure kaynaklarını kümesini parametreleştirme istiyor. Dışarı aktarılan JSON Panosu'nda arayın ve Azure kaynak kimlikleri tüm oluşumlarını bulun. Bizim örnek Pano birden çok kutucuklara sahiptir tümü tek bir Azure sanal makinenin gelin. Bizim Panosu yalnızca bu tek kaynak görüneceğinden olmasıdır. (Belgenin sonuna dahil) örnek json arıyorsanız "/ abonelikleri", bu kimliği birkaç kez Bul

`/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1`

Herhangi bir sanal makine için bu panoyu yayımlamak için gelecekte, bu dizenin JSON içindeki her örneğinin Parametreleştirme gerekir. 

İki türdeki kaynakları oluşturma API vardır. [Kesinlik temelli API'leri](https://docs.microsoft.com/rest/api/resources/resources) aynı anda bir kaynak oluşturmak ve bir [şablona dayalı dağıtım](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) oluşturma düzenleyebilirsiniz sistem tek bir API çağrısı ile birden çok bağımlı kaynakların. Bizim örneğimizde kullanırız ikinci yerel parametrelemeyi ve şablon destekler, böylece.

## <a name="programmatically-create-a-dashboard-from-your-template-using-a-template-deployment"></a>Program aracılığıyla bir şablon dağıtımı kullanarak şablonunuzu bir pano oluşturun

Azure birden fazla kaynak dağıtım düzenlemek olanağı sunar. Aralarındaki ilişkilerin yanı sıra dağıtmak için kaynak kümesini ifade bir dağıtım şablonu oluşturun.  Tek tek oluşturma gibi her bir kaynağın JSON biçimi aynıdır. Aralarındaki fark [şablonu dili](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) değişkenleri, parametreleri, temel işlevleri ve daha fazlasını gibi birkaç kavram ekler. Bu sözdizimi genişletilmiş bir şablon dağıtımı bağlamında yalnızca desteklenir ve daha önce bahsedilen kesinlik temelli API'leri ile kullanıldığında çalışmaz.

Bu rota oluşturacağız sonra parametrelemeyi yapılması şablonun parametresi sözdizimini kullanarak.  Aşağıda gösterildiği gibi daha önce bulduk kaynak kimliği tüm örneklerini değiştirin.

### <a name="example-json-property-with-hard-coded-resource-id"></a>Örnek JSON özelliğiyle sabit kodlanmış kaynak kimliği
`id: “/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1”`

### <a name="example-json-property-converted-to-a-parameterized-version-based-on-template-parameters"></a>Şablonun parametrelere göre parametreli bir sürüme dönüştürülen örnek JSON özelliği

`id: "[resourceId(parameters('virtualMachineResourceGroup'), ‘Microsoft.Compute/virtualMachines’, parameters('virtualMachineName'))]"`

Ayrıca bazı gerekli şablon meta verilerine ve bu gibi json şablonunu üstündeki parametreleri bildirmeniz gerekir:

```json

{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

__Bu belgenin sonundaki tam, çalışma şablonu görebilirsiniz.__

Şablonunuzu hazırlanmış sonra kullanarak dağıtabilirsiniz [REST API'leri](https://docs.microsoft.com/rest/api/resources/deployments), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), [Azure CLI](https://docs.microsoft.com/cli/azure/group/deployment#az_group_deployment_create), veya [portalın şablon dağıtımı sayfası ](https://portal.azure.com/#create/Microsoft.Template).

İşte bizim örnek Pano JSON iki sürümü. İlk biz zaten bir kaynağa bağlı Portalı'ndan dışarı sürümüdür. Program aracılığıyla tüm VM bağlanabilir ve Azure Resource Manager kullanılarak dağıtılan şablon sürümü saniyedir.

## <a name="json-representation-of-our-example-dashboard-before-templating"></a>JSON gösterimi bizim örnek Pano (önce şablonu oluşturma)

Zaten dağıtılmış bir Pano JSON gösterimi getirmek için önceki yönergeleri izleyin, görmeyi bekleyebilirsiniz budur. Bu Pano belirli bir Azure sanal makine işaret ettiğinden Göster sabit kodlanmış kaynak tanımlayıcılar unutmayın.

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

### <a name="template-representation-of-our-example-dashboard"></a>Bizim örnek Pano Şablonu gösterimi

Pano şablonu sürümü adında üç parametre tanımlanmış __virtualMachineName__, __virtualMachineResourceGroup__, ve __dashboardName__.  Parametreler, dağıttığınız her zaman bu Pano farklı bir Azure sanal makine noktası sağlar. Parametreli kimlikleri Bu panoyu programlı olarak yapılandırılabilir ve herhangi bir Azure sanal makineye işaret edecek şekilde dağıtılan olduğunu göstermek için vurgulanır. Bu özelliği test etmek için kolay aşağıdaki şablonu kopyalayın ve yapıştırın yoldur [Azure portal'ın şablon dağıtımı sayfası](https://portal.azure.com/#create/Microsoft.Template). 

Bu örnek bir Pano tek başına dağıtır, ancak şablon dili birden çok kaynak dağıtmak tarafı boyunca bir veya daha fazla panolar paketini imkan tanır ve bunları. 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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