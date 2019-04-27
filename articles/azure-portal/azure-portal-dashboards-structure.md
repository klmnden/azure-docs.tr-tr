---
title: Azure panolarının yapısı | Microsoft Docs
description: Bu makalede bir Azure panosunu JSON yapısını açıklar.
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
ms.openlocfilehash: a7e9acbe78ffdca2e615873cc4c33f86b250a429
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60551523"
---
# <a name="the-structure-of-azure-dashboards"></a>Azure panolarının yapısı
Bu belgede örnek olarak aşağıdaki Panoyu kullanarak bir Azure panosunu yapısını anlatılmaktadır:

![Örnek Pano](./media/azure-portal-dashboards-structure/sample-dashboard.png)

Paylaşılan beri [Azure panoları kaynaklar olduğundan](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), bu panoyu JSON olarak gösterilebilir.  Aşağıdaki JSON yukarıda görselleştirilmiş Pano temsil eder.

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
        "metadata": {
            "model": {
                "timeRange": {
                    "value": {
                        "relative": {
                            "duration": 24,
                            "timeUnit": 1
                        }
                    },
                    "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
                }
            }
        }
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

## <a name="common-resource-properties"></a>Genel kaynak özellikleri

Şimdi JSON ilgili bölümlerine bölün.  Üst düzey özellikler __kimliği__, __adı__, __türü__, __konumu__, ve __etiketleri__ özellikleri Tüm Azure kaynak türleri arasında paylaşılan. Diğer bir deyişle, bunlar çok Panodaki içerikle yapmanız gerekmez.

### <a name="the-id-property"></a>ID özelliği

Konu için Azure kaynak kimliğine [adlandırma kuralları Azure kaynaklarınızın](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Portal bir Pano oluşturur, genellikle bir kimliği bir GUID biçiminde seçer, ancak programlı olarak oluşturduğunuzda, geçerli bir ad kullanmak ücretsizdir. 

### <a name="the-name-property"></a>Name özelliği
Kaynak abonelik, kaynak türü ya da kaynak grubu bilgileri içerip içermediğini kimliğini segmentini addır. Esas olarak, kaynak kimliğini son segmenti olduğu.

### <a name="the-type-property"></a>Type özelliği
Tüm panoları türlerinin __Microsoft.Portal/dashboards__.

### <a name="the-location-property"></a>Konum özelliği
Diğer kaynaklarının aksine, bir çalışma zamanı bileşeni panonuz yok.  Panolar için Panodaki JSON gösterimi depolayan birincil coğrafi konumu konumu gösterir. Değer kullanarak en iyi duruma getirilebilir konum kodlarından birini olmalıdır [abonelikler kaynak konumları API](https://docs.microsoft.com/rest/api/resources/subscriptions).

### <a name="the-tags-property"></a>Etiketler özelliği
Etiketler, kaynağınızı rastgele ad değer çiftleri olarak düzenlemenize olanak tanıyan Azure kaynaklarınızın genel bir özelliğidir. Panolar için bir tane olduğunu özel etiket __gizli başlık__. Doldurulmuş bu özellik panonuz varsa panonuzu portalında görünen adı olarak kullanılır. Azure kaynak kimliklerini yeniden adlandırılamaz, ancak etiketleri kullanabilirsiniz. Bu etiket, panonuzu renamable görünen adı için bir yol sunar.

`"tags": { "hidden-title": "Created via API" }`

### <a name="the-properties-object"></a>Özellikleri nesnesi
İki özellik, özellikler nesneyi içeren __merceklerden__ ve __meta verileri__. __Merceklerden__ özelliği içeren kutucukları hakkında bilgi (yani) Panoda bölümleri).  __Meta verileri__ özelliği için olası gelecek özellikleri oradadır.

### <a name="the-lenses-property"></a>Merceklerden özelliği
__Merceklerden__ Pano özelliğini içerir. Not merceklerden Bu örnekte, nesne, "0" adlı tek bir özellik içerir. Merceklerden panoları şu anda uygulanmamaktadır bir gruplandırma kavramdır. Şimdilik, tüm panolarınıza "0" olarak adlandırılan tek tuto vlastnost nelze upravovat yeniden lens nesnesi vardır.

### <a name="the-lens-object"></a>Odağı nesnesi
İki özellik, "0" altında nesneyi içeren __sipariş__ ve __bölümleri__.  Panolar,'ın geçerli sürümünde __sipariş__ olduğundan her zaman 0. __Bölümleri__ özelliği, tek tek bölümleri (diğer adıyla) tanımlayan bir nesne içerir Panoda kutucuğu).

__Bölümleri__ nesne birkaç olduğu özelliğin adı, her parça için bir özellik içerir. Bu sayı, önemli değildir. 

### <a name="the-part-object"></a>Bölümü nesnesi
Her ayrı bölümü nesnesinin bir __konumu__, ve __meta verileri__.

### <a name="the-position-object"></a>Konum nesnesi
__Konumu__ özelliği bir parçası olarak ifade edilen için boyut ve konum bilgileri içeren __x__, __y__, __rowSpan__ve __colSpan__. Kılavuz birimleri cinsinden değerlerdir. Bu kılavuz birimleri, Pano, burada gösterildiği gibi Özelleştir modunda olduğunda görünür. İki Kılavuz birim genişliğini sağlamak için bir kutucuk istiyorsanız konum nesnesi şöyle sonra bir yükseklikte bir kılavuz biriminin yanı sıra, bir konum üst köşe panosunun sol:

`location: { x: 0, y: 0, rowSpan: 2, colSpan: 1 }`

![Grid-birimleri](./media/azure-portal-dashboards-structure/grid-units.png)

### <a name="the-metadata-object"></a>Meta veri nesnesi
Her parça bir metadata özelliğine sahip, bir nesne olarak adlandırılan tek gerekli özellik sahip __türü__. Bu dize portalı söyler kutucuktan gösterilecek. Bizim örnek Pano kutucukları bu tür kullanır:


1. `Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart` – Göstermek için kullanılan ölçümlerini izleme
1. `Extension[azure]/HubsExtension/PartType/MarkdownPart` – Metin veya bağlantılar, listeler için temel biçimlendirmeye sahip görüntü ile göstermek için kullanılan vb.
1. `Extension[azure]/HubsExtension/PartType/VideoPart` – Başka türde bir video HTML etiketinde çalışır video YouTube'da ve Channel9 videoları göstermek için kullanılır.
1. `Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart` – Bir Azure sanal makinesinin durumu ve adını göstermek için kullanılır.

Her bölüm kendi yapılandırma türündedir. Olası yapılandırma özelliklerini adlı __girişleri__, __ayarları__, ve __varlık__. 

### <a name="the-inputs-object"></a>Giriş nesnesi
Giriş nesnesi, genellikle bir kutucuk bir kaynak örneğine bağlanan bilgileri içerir.  Bizim örnek Panosu sanal makine bölümünde bağlama ifade etmek için Azure kaynak kimliğini kullanan tek bir giriş içerir.  Bu kaynak kimliği biçimi, tüm Azure kaynakları arasında tutarlı olur.

```json
"inputs":
[
    {
        "name": "id",
        "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
    }
]

```
Ölçüm grafiği bölümü, görüntülenmekte metric(s) hakkında bilgilerinin yanı sıra bağlamak için kaynak ifade tek bir giriş vardır. Ağ giriş ve çıkış ağ ölçümleri gösteren bir kutucuk için giriş aşağıda verilmiştir.

```json
“inputs”:
[
    {
        "name": "queryInputs",
        "value": 
        {
            "timespan": 
            {
                "duration": "PT1H",
                "start": null,
                "end": null
           },
            "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
            "chartType": 0,
            "metrics": 
            [
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
]

```

### <a name="the-settings-object"></a>Ayarlar nesnesi
Ayarlar nesnesini bölümünün yapılandırılabilir öğeleri içerir.  Bizim örnek Panoda Markdown bölümü özel markdown yapılandırılabilir başlığı ve alt konu başlığını yanı sıra içerik depolamak için ayarları kullanır.

```json
"settings": 
{
    "content": 
    {
        "settings": 
        {
            "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
            "title": "Test VM Dashboard",
            "subtitle": "Contoso"
        }
    }
}

```

Benzer şekilde, bir işaretçi oynatmak için video içeren kendi ayarlarını, bir otomatik ayarlama ve isteğe bağlı başlık bilgilerini video kutucuğunu sahiptir.

```json
"settings": 
{
   "content": 
    {
        "settings": 
        {
            "title": "",
            "subtitle": "",
            "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
            "autoplay": false
        }
    }
}

```

### <a name="the-asset-object"></a>Varlık nesnesi
(Varlıklar olarak adlandırılır) yönetilebilir birinci sınıf portal nesnelere bağlı kutucuklar, varlık nesnesi ile ifade edilen bu ilişkisine sahiptir.  Bizim örnek Panoda sanal makine kutucuğu bu varlık açıklamasını içerir.  __İdInputName__ özelliği içeren varlık, bu durumda kaynak kimliği için benzersiz tanımlayıcı kimliği giriş portalı söyler. Çoğu Azure kaynak türlerini Portalı'nda tanımlanan varlıkları vardır.

`"asset": {    "idInputName": "id",    "type": "VirtualMachine"    }`
