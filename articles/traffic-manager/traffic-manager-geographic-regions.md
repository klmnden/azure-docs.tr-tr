---
title: Azure Traffic Manager coğrafi yönlendirme türü tarafından kullanılan ülke/bölge hiyerarşisi | Microsoft Docs
description: Bu makalede, Azure Traffic Manager coğrafi yönlendirme türü tarafından kullanılan ülke/bölge hiyerarşisi listelenir.
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: allensu
ms.openlocfilehash: d16529e966fb2e16d1012f4aa0aafcff204a3093
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071177"
---
# <a name="countryregion-hierarchy-used-by-azure-traffic-manager-for-geographic-traffic-routing-method"></a>Coğrafi trafik yönlendirme yöntemi için Azure Traffic Manager tarafından kullanılan ülke/bölge hiyerarşisi

Bu makalede, ülke ve bölge tarafından kullanılan listeler **Geographic** trafiği yönlendirme yöntemini Azure Traffic Manager. Siz de bu bilgileri programlı olarak çağırarak elde edebileceğiniz [Azure Traffic Manager'ın REST API](https://docs.microsoft.com/rest/api/trafficmanager/). 

- WORLD(World)

    - GEO-EU(Europe)

        - AD(Andorra)

        - Al(Albania)

        - AT(Austria)

        - AX (Aland Adaları)

        - BA (Bosna-Hersek)

        - BE(Belgium)

        - BG(Bulgaria)

        - BY(Belarus)

        - CH(Switzerland)

        - CY(Cyprus)

        - CZ (Çek Cumhuriyeti)

        - DE(Germany)

        - DK(Denmark)

        - EE(Estonia)

        - ES(Spain)

        - FI(Finland)

        - FO (Faroe Adaları)

        - FR(France)

        - GB (Birleşik Krallık)

        - GG(Guernsey)

        - GI(Gibraltar)

        - Gr(Greece)

        - HR(Croatia)

        - HU(Hungary)

        - IE(Ireland)

        - Anlık ileti (Man Adası)

        - Is(Iceland)

        - IT(Italy)

        - JE(Jersey)

        - LI(Liechtenstein)

        - LT(Lithuania)

        - LU(Luxembourg)

        - LV(Latvia)

        - MC(Monaco)

        - MD(Moldova)

        - ME(Montenegro)

        - MK (Kuzey Makedonya)

        - MT(Malta)

        - NL(Netherlands)

        - No(Norway)

        - PL(Poland)

        - PT(Portugal)

        - Ro(Romania)

        - RS(Serbia)

        - RU(Russia)

        - SE(Sweden)

        - SI(Slovenia)

        - SJ(Svalbard)

        - SK(Slovakia)

        - SM (San Marino)

        - UA(Ukraine)
            - Crimea bölgesi

        - VA (Vatikan)

        - XJ (Jan Mayen)

        - XK(Kosovo)

    - Coğrafi ME(Middle East)

        - AE (Birleşik Arap Emirlikleri)

        - BH(Bahrain)

        - IL(Israel)

        - IQ(Iraq)

        - IR(Iran)

        - Jo(Jordan)

        - KW(Kuwait)

        - LB(Lebanon)

        - OM(Oman)

        - PS (Filistin)

        - QA(Qatar)

        - SY(Syria)

        - SA (Suudi Arabistan)

        - TR(Turkey)

        - YE(Yemen)

    - Coğrafi NA(North America / Central America / Caribbean)

        - AG (Antigua ve Barbuda)

        - AI(Anguilla)

        - AW(Aruba)

        - BB(Barbados)

        - BL (Saint Barthélemy)

        - BM(Bermuda)

        - BQ(Bonaire)

        - BS(Bahamas)

        - BZ(Belize)

        - CA(Canada)

            - CA-AB(Alberta)

            - CA BC(British Columbia)

            - CA-MB(Manitoba)

            - CA NB(New Brunswick)

            - CA NL(Newfoundland and Labrador)

            - CA-NS(Nova Scotia)

            - CA NT(Northwest Territories)

            - CA-nu(Nunavut)

            - CA-on(Ontario)

            - CA-PE(Prince Edward Island)

            - CA-QC(Québec)

            - CA-SK(Saskatchewan)

            - CA YT(Yukon Territory)

        - CR (Kosta Rika)

        - Cu(Cuba)

        - CW(CuraÃ§ao)

        - DM(Dominica)

        - (Dominik Cumhuriyeti)

        - GD(Grenada)

        - GL(Greenland)

        - GP(Guadeloupe)

        - GT(Guatemala)

        - HN(Honduras)

        - HT(Haiti)

        - JM(Jamaica)

        - KN (Saint Kitts ve Nevis)

        - KY (Cayman Adaları)

        - LC(Saint Lucia)

        - MF (Saint Martin)

        - MQ(Martinique)

        - MS(Montserrat)

        - MX(Mexico)

        - NI(Nicaragua)

        - PA(Panama)

        - PM (Saint Pierre ve Miquelon)

        - Çekme isteği (Porto Riko)

        - SV (El Salvador)

        - SX (Sint Maarten)

        - TC (Turks ve Caicos Adaları)

        - TT (Trinidad ve Tobago)

        - ÜMMÜL (ABD Harici Adaları)

        - US(United States)

            - US-ak(Alaska)

            - US-Al(Alabama)

            - US-ar(Arkansas)

            - US-az(Arizona)

            - US-CA(California)

            - US-co(Colorado)

            - US-CT(Connecticut)

            - ABD DC(District of Columbia)

            - US-de(Delaware)

            - US-fl(Florida)

            - US-GA(Georgia)

            - US-HI(Hawaii)

            - US-IA(Iowa)

            - US-ID(Idaho)

            - US-IL(Illinois)

            - US-ın(Indiana)

            - US-Ks(Kansas)

            - US-KY(Kentucky)

            - US-la(Louisiana)

            - US-ma(Massachusetts)

            - US-MD(Maryland)

            - US-Me(Maine)

            - US-mı(Michigan)

            - US-MN(Minnesota)

            - US-MO(Missouri)

            - US-MS(Mississippi)

            - US-MT(Montana)

            - ABD NC(North Carolina)

            - ABD ND(North Dakota)

            - US-NE(Nebraska)

            - ABD NH(New Hampshire)

            - ABD NJ(New Jersey)

            - ABD NM(New Mexico)

            - US-NV(Nevada)

            - ABD NY(New York)

            - US-OH(Ohio)

            - US-ok(Oklahoma)

            - US-OR(oregon)

            - US-Pa(Pennsylvania)

            - ABD RI(Rhode Island)

            - ABD SC(South Carolina)

            - ABD SD(South Dakota)

            - US-tn(Tennessee)

            - US-TX(Texas)

            - US-UT(Utah)

            - US-VA(Virginia)

            - US-VT(Vermont)

            - US-WA(Washington)

            - US-WI(Wisconsin)

            - ABD WV(West Virginia)

            - US-WY(Wyoming)

        - VC (Saint Vincent ve Grenadinler)

        - VG (İngiliz Virgin Adaları)

        - VI (ABD Virgin Adaları)

        - XE (Sint Eustatius)

        - XS(Saba)

    - GEO-AS(Asia)

        - AF(Afghanistan)

        - AM(Armenia)

        - AZ(Azerbaijan)

        - BD(Bangladesh)

        - Bn(Brunei)

        - BT(Bhutan)

        - CC (Cocos (Keeling) Adaları)

        - CN(China)

        - CX (Christmas Adası)

        - Ge(Georgia)

        - HK (Hong Kong ÖİB)

        - ID(Indonesia)

        - IN(India)

        - G/ç (İngiliz Hint Okyanusu Toprakları)

        - JP(Japan)

        - KG(Kyrgyzstan)

        - KH(Cambodia)

        - KP (Kuzey Kore)

        - KR(Korea)

        - KZ(Kazakhstan)

        - LA(Laos)

        - LK(Sri Lanka)

        - MM(Myanmar)

        - MN(Mongolia)

        - Ay (Makao ÖİB)

        - MV(Maldives)

        - My(Malaysia)

        - NP(Nepal)

        - PH(Philippines)

        - PK(Pakistan)

        - SG(Singapore)

        - TH(Thailand)

        - TJ(Tajikistan)

        - TL(Timor_Leste)

        - TM(Turkmenistan)

        - Tw(Taiwan)

        - UZ(Uzbekistan)

        - VN(Vietnam)

    - GEO-AF(Africa)

        - AO(Angola)

        - BF (Burkina Faso)

        - BI(Burundi)

        - BJ(Benin)

        - BV (Bouvet Adası)

        - BW(Botswana)

        - CD (Kongo Cumhuriyeti (KC))

        - CF (Orta Afrika Cumhuriyeti)

        - CI (Fildişi Sahili)

        - CM(Cameroon)

        - CV (Cabo Verde)

        - DJ(Djibouti)

        - Dz(Algeria)

        - EG(Egypt)

        - ER(Eritrea)

        - ET(Ethiopia)

        - GA(Gabon)

        - GH(Ghana)

        - GM(Gambia)

        - GN(Guinea)

        - GQ (Ekvator Ginesi)

        - GW(Guinea_Bissau)

        - KE(Kenya)

        - KM(Comoros)

        - LR(Liberia)

        - LS(Lesotho)

        - LY(Libya)

        - Ma(Morocco)

        - MG(Madagascar)

        - ML(Mali)

        - MR(Mauritania)

        - MU(Mauritius)

        - MW(Malawi)

        - MZ(Mozambique)

        - NA(Namibia)

        - NE(Niger)

        - NG(Nigeria)

        - RE(Réunion)

        - RW(Rwanda)

        - SC(Seychelles)

        - SD(Sudan)

        - SH (St. Helena, Ascension, Tristan da Cunha)

        - SL (Sierra Leone)

        - SN(Senegal)

        - SO(Somalia)

        - SS (Güney Sudan)

        - ST (Sao Tome ve Principe)

        - SZ(Swaziland)

        - TD(CHAD)

        - TF (Fransız Güney Toprakları)

        - TG(Togo)

        - TN(Tunisia)

        - TZ(Tanzania)

        - UG(Uganda)

        - YT(Mayotte)

        - ZA (Güney Afrika)

        - ZM(Zambia)

        - ZW(Zimbabwe)

    - GEO-an(Antarctica)

        - AQ(Antarctica)

    - Coğrafi SA(South America)

        - AR(Argentina)

        - BO(Bolivia)

        - BR(Brazil)

        - CL(Chile)

        - CO(Colombia)

        - EC(Ecuador)

        - FK (Falkland Adaları)

        - GF (Fransız Guyanası)

        - GS (Güney Georgia ve Güney Sandwich Adaları)

        - GY(Guyana)

        - PE(Peru)

        - PY(Paraguay)

        - SR(Suriname)

        - UY(Uruguay)

        - VE(Venezuela)

    - Coğrafi AP(Australia / Pacific)

        - AS(American Samoa)

        - AU(Australia)

            - AU ACT(Australian Capital Territory)

            - AU NSW(New South Wales)

            - AU NT(Northern Territory)

            - AU-QLD(Queensland)

            - AU SA(South Australia)

            - AU-TAS(Tasmania)

            - AU-VIC(Victoria)

            - AU WA(Western Australia)

        - Çekme (Cook Adaları)

        - FJ(Fiji)

        - FM(Micronesia)

        - GU(Guam)

        - HM (Heard Adası ve McDonald Adaları)

        - Kı(Kiribati)

        - MH (Marshall Adaları)

        - MP (Kuzey Mariana Adaları)

        - NC (Yeni Kaledonya)

        - NF (Norfolk Adası)

        - NR(Nauru)

        - NU(Niue)

        - NZ (Yeni Zelanda)

        - PF (Fransız Polinezyası)

        - PG (Papua Yeni Gine)

        - PN (Pitcairn Adaları)

        - PW(Palau)

        - SB (Solomon Adaları)

        - TK(Tokelau)

        - TO(Tonga)

        - TV(Tuvalu)

        - VU(Vanuatu)

        - WF (Wallis ve Futuna)

        - WS(Samoa)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [coğrafi trafik yönlendirme yöntemini Azure Traffic Manager](traffic-manager-routing-methods.md#geographic).
