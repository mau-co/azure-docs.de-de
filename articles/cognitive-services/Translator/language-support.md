---
title: 'Sprachunterstützung: Translator'
titleSuffix: Azure Cognitive Services
description: Von Cognitive Services Translator werden die folgenden Sprachen für die Übersetzung von Texten mithilfe der neuronalen maschinellen Übersetzung (Neural Machine Translation, NMT) unterstützt.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 06/10/2020
ms.author: lajanuar
ms.openlocfilehash: a195567117378a118b9bea8d15a7cb0a661d284a
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107303524"
---
# <a name="language-and-region-support-for-text-and-speech-translation"></a>Sprach- und Regionsunterstützung für Text- und Sprachübersetzung

Verwenden Sie den Übersetzer zum Übersetzen in jede und aus jeder der über 90 Textübersetzungssprachen und -dialekte. Die neuronale maschinelle Übersetzung (Neural Machine Translation, NMT) ist der neue Standard für qualitativ hochwertige, auf künstlicher Intelligenz basierende maschinelle Übersetzungen und steht standardmäßig in V3 von Translator zur Verfügung, wenn ein neuronales System verfügbar ist.

Sie können den Translator auch in Verbindung mit dem benutzerdefinierten Translator verwenden, um neuronale Übersetzungssysteme zu erstellen, die die in Ihrem Unternehmen und Ihrer Branche verwendete Terminologie verstehen, oder in Verbindung mit dem Spracherkennungsdienst von Microsoft, um Ihrer App die Sprachübersetzung hinzuzufügen.

[Weitere Informationen zur Funktionsweise von maschineller Übersetzung](https://www.microsoft.com/translator/mt.aspx)

## <a name="text-translation"></a>Textübersetzung
Die Textübersetzung ist über den Vorgang „Translate“ (Übersetzen) in jede und aus jeder der im Translator verfügbaren Sprachen möglich. Die API bietet auch Spracherkennung mithilfe des Vorgangs „Detect“ (Erkennen), Transliteration mithilfe des Vorgangs „Transliterate“ (Transliterieren) und zweisprachige Wörterbücher mithilfe der Vorgänge „Dictionary Lookup“ (Nachschlagen im Wörterbuch) und „Dictionary Examples“ (Wörterbuchbeispiele). Die verfügbaren Sprachen für die einzelnen Vorgänge sind unten aufgeführt. 

### <a name="translate"></a>Translate

Der Translator unterstützt die folgenden Sprachen für die Übersetzung von Texten. 

[Referenzdokumentation zum Vorgang „Translate“ anzeigen](reference/v3-0-translate.md)

| Sprache | Sprachcode |
|:-|:-:|
| Afrikaans | `af` |
| Albanisch | `sq` |
| Amharisch | `am` |
| Arabisch | `ar` |
| Armenisch | `hy` |
| Assamesisch | `as` |
| Aserbaidschanisch | `az` |
| Bengalisch | `bn` |
| Bosnisch (Lateinisch) | `bs` |
| Bulgarisch | `bg` |
| Chinesisch (traditionell) | `yue` |
| Katalanisch | `ca` |
| Chinesisch (vereinfacht) | `zh-Hans` |
| Chinesisch (traditionell) | `zh-Hant` |
| Kroatisch | `hr` |
| Tschechisch | `cs` |
| Dänisch | `da` |
| Dari | `prs` |
| Niederländisch | `nl` |
| Englisch | `en` |
| Estnisch | `et` |
| Fidschi | `fj` |
| Filipino | `fil` |
| Finnisch | `fi` |
| Französisch | `fr` |
| Französisch (Kanada) | `fr-ca` |
| Deutsch | `de` |
| Griechisch | `el` |
| Gujarati | `gu` |
| Haitianisches Kreolisch | `ht` |
| Hebräisch | `he` |
| Hindi | `hi` |
| Hmong Daw | `mww` |
| Ungarisch | `hu` |
| Isländisch | `is` |
| Indonesisch | `id` |
| Inuktitut | `iu` |
| Irisch | `ga` |
| Italienisch | `it` |
| Japanisch | `ja` |
| Kannada | `kn` |
| Kasachisch | `kk` |
| Khmer | `km` |
| Klingonisch | `tlh-Latn` |
| Klingonisch (plqaD) | `tlh-Piqd` |
| Koreanisch | `ko` |
| Kurdisch (zentral) | `ku` |
| Kurdisch (nördlich) | `kmr` |
| Laotisch | `lo` |
| Lettisch | `lv` |
| Litauisch | `lt` |
| Madagassisch | `mg` |
| Malaiisch | `ms` |
| Malayalam | `ml` |
| Maltesisch | `mt` |
| Maori | `mi` |
| Marathi | `mr` |
| Myanmar | `my` |
| Nepalesisch | `ne` |
| Norwegisch | `nb` |
| Odia | `or` |
| Paschtu | `ps` |
| Persisch | `fa` |
| Polnisch | `pl` |
| Portugiesisch (Brasilien) | `pt` |
| Portugiesisch (Portugal) | `pt-pt` |
| Pandschabi | `pa` |
| Queretaro-Otomi | `otq` |
| Rumänisch | `ro` |
| Russisch | `ru` |
| Samoanisch | `sm` |
| Serbisch (Kyrillisch) | `sr-Cyrl` |
| Serbisch (Lateinisch) | `sr-Latn` |
| Slowakisch | `sk` |
| Slowenisch | `sl` |
| Spanisch | `es` |
| Suaheli | `sw` |
| Schwedisch | `sv` |
| Tahitisch | `ty` |
| Tamilisch | `ta` |
| Telugu | `te` |
| Thailändisch | `th` |
| Tigrinya | `ti` |
| Tongaisch | `to` |
| Türkisch | `tr` |
| Ukrainisch | `uk` |
| Urdu | `ur` |
| Vietnamesisch | `vi` |
| Walisisch | `cy` |
| Yukatekisches Maya | `yua` |

> [!NOTE]
> Der Sprachcode `pt` wird standardmäßig auf `pt-br`, Portugiesisch (Brasilien), festgelegt.

### <a name="detect"></a>Detect

Der Translator erkennt die folgenden Sprachen für die Übersetzung und Transliteration.

[Referenzdokumentation zum Vorgang „Detect“ anzeigen](reference/v3-0-detect.md)

| Sprache | Sprachcode |
|:-|:-:|
| Afrikaans | `af` |
| Albanisch | `sq` |
| Arabisch | `ar` |
| Bulgarisch | `bg` |
| Katalanisch | `ca` |
| Chinesisch (vereinfacht) | `zh-Hans` |
| Chinesisch (traditionell) | `zh-Hant` |
| Kroatisch | `hr` |
| Tschechisch | `cs` |
| Dänisch | `da` |
| Niederländisch | `nl` |
| Englisch | `en` |
| Estnisch | `et` |
| Finnisch | `fi` |
| Französisch | `fr` |
| Deutsch | `de` |
| Griechisch | `el` |
| Gujarati | `gu` |
| Haitianisches Kreolisch | `ht` |
| Hebräisch | `he` |
| Hindi | `hi` |
| Ungarisch | `hu` |
| Isländisch | `is` |
| Indonesisch | `id` |
| Inuktitut | `iu` |
| Irisch | `ga` |
| Italienisch | `it` |
| Japanisch | `ja` |
| Klingonisch | `tlh-Latn` |
| Koreanisch | `ko` |
| Kurdisch (zentral) | `ku-Arab` |
| Lettisch | `lv` |
| Litauisch | `lt` |
| Malaiisch | `ms` |
| Maltesisch | `mt` |
| Norwegisch | `nb` |
| Paschtu | `ps` |
| Persisch | `fa` |
| Polnisch | `pl` |
| Portugiesisch | `pt` |
| Rumänisch | `ro` |
| Russisch | `ru` |
| Serbisch (Kyrillisch) | `sr-Cyrl` |
| Serbisch (Lateinisch) | `sr-Latn` |
| Slowakisch | `sk` |
| Slowenisch | `sl` |
| Spanisch | `es` |
| Suaheli | `sw` |
| Schwedisch | `sv` |
| Tahitisch | `ty` |
| Thailändisch | `th` |
| Türkisch | `tr` |
| Ukrainisch | `uk` |
| Urdu | `ur` |
| Vietnamesisch | `vi` |
| Walisisch | `cy` |
| Yukatekisches Maya | `yua` |

### <a name="transliterate"></a>Transliterate

Die „Transliterate“-Methode unterstützt die folgenden Sprachen. In der Spalte „Von/In“ wird durch „<-->“ angezeigt, dass für die Sprache eine Transliteration beider Skripts in das jeweils andere erfolgen kann. Mit „-->“ wird angezeigt, dass für die Sprache nur eine Transliteration von einem Skript in das andere möglich ist.

[Referenzdokumentation zum Vorgang „Transliterate“ anzeigen](reference/v3-0-translate.md)


| Sprache    | Sprachcode | Skript | Von/In | Skript|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arabisch | `ar` | Arabisch `Arab` | <--> | Latein `Latn` |
| Assamesisch | `as` | Bangla `Beng` | <--> | Latein `Latn` |
| Bengalisch  | `bn` | Bangla `Beng` | <--> | Latein `Latn` |
|Belarussisch| `be` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
|Bulgarisch| `bg` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
| Chinesisch (vereinfacht) | `zh-Hans` | Chinesisch (vereinfacht) `Hans`| <--> | Latein `Latn` |
| Chinesisch (vereinfacht) | `zh-Hans` | Chinesisch (vereinfacht) `Hans`| <--> | Chinesisch (traditionell) `Hant`|
| Chinesisch (traditionell) | `zh-Hant` | Chinesisch (traditionell) `Hant`| <--> | Latein `Latn` |
| Chinesisch (traditionell) | `zh-Hant` | Chinesisch (traditionell) `Hant`| <--> | Chinesisch (vereinfacht) `Hans` |
|Griechisch| `el` | Griechisch `Grek`  | <--> | Latein `Latn` |
| Gujarati | `gu`  | Gujarati `Gujr` | <--> | Latein `Latn` |
| Hebräisch | `he` | Hebräisch `Hebr` | <--> | Latein `Latn` |
| Hindi | `hi` | Devanagari `Deva` | <--> | Latein `Latn` |
| Japanisch | `ja` | Japanisch `Jpan` | <--> | Latein `Latn` |
| Kannada | `kn` | Kannada `Knda` | <--> | Latein `Latn` |
|Kasachisch| `kk` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
|Koreanisch| `ko` | Koreanisch `Kore`  | <--> | Latein `Latn` |
|Kirgisisch| `ky` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
|Mazedonisch| `mk` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
| Malayalam | `ml` | Malayalam `Mlym` | <--> | Latein `Latn` |
| Marathi | `mr` | Devanagari `Deva` | <--> | Latein `Latn` |
|Mongolisch| `mn` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
| Odia | `or` | Oriya `Orya` | <--> | Latein `Latn` |
|Persisch| `fa` | Arabisch `Arab`  | <--> | Latein `Latn` |
| Pandschabi | `pa` | Gurmukhi `Guru`  | <--> | Latein `Latn`  |
|Russisch| `ru` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
| Serbisch (Kyrillisch) | `sr-Cyrl` | Kyrillisch `Cyrl`  | --> | Latein `Latn` |
| Serbisch (Lateinisch) | `sr-Latn` | Latein `Latn` | --> | Kyrillisch `Cyrl`|
|Sindhi| `sd` | Arabisch `Arab`  | <--> | Latein `Latn` |
|Singhalesisch| `si` | Singhalesisch `Sinh`  | <--> | Latein `Latn` |
|Tadschikisch| `tg` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
| Tamilisch | `ta` | Tamilisch `Taml` | <--> | Latein `Latn` |
|Tatarisch| `tt` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
| Telugu | `te` | Telugu `Telu` | <--> | Latein `Latn` |
| Thailändisch | `th` | Thailändisch `Thai` | --> | Latein `Latn` |
|Ukrainisch| `uk` | Kyrillisch `Cyrl`  | <--> | Latein `Latn` |
|Urdu| `ur` | Arabisch `Arab`  | <--> | Latein `Latn` |

### <a name="dictionary"></a>Wörterbuch

Das Wörterbuch unterstützt die Übertragung der folgenden Sprachen in das oder aus dem Englischen anhand der „Lookup“- und „Examples“-Methode.

Referenzdokumentation zu den Vorgängen [Dictionary Lookup](reference/v3-0-dictionary-lookup.md) und [Dictionary Examples](reference/v3-0-dictionary-examples.md) anzeigen

| Sprache    | Sprachcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalisch      | `bn`          |
| Bosnisch (Lateinisch)      | `bs`          |
| Bulgarisch      | `bg`          |
| Katalanisch      | `ca`          |
| Chinesisch (vereinfacht)      | `zh-Hans`          |
| Kroatisch      | `hr`          |
| Tschechisch      | `cs`          |
| Dänisch      | `da`          |
| Niederländisch      | `nl`          |
| Estnisch      | `et`          |
| Finnisch      | `fi`          |
| Französisch      | `fr`          |
| Deutsch      | `de`          |
| Griechisch      | `el`          |
| Haitianisches Kreolisch      | `ht`          |
| Hebräisch      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Ungarisch      | `hu`          |
| Isländisch    | `is`  |
| Indonesisch      | `id`          |
| Italienisch      | `it`          |
| Japanisch      | `ja`          |
| Klingonisch      | `tlh`          |
| Koreanisch      | `ko`          |
| Lettisch      | `lv`          |
| Litauisch      | `lt`          |
| Malaiisch      | `ms`          |
| Maltesisch      | `mt`          |
| Norwegisch      | `nb`          |
| Persisch      | `fa`          |
| Polnisch      | `pl`          |
| Portugiesisch (Brasilien)     | `pt`          |
| Rumänisch      | `ro`          |
| Russisch      | `ru`          |
| Serbisch (Lateinisch)      | `sr-Latn`          |
| Slowakisch     | `sk`          |
| Slowenisch      | `sl`          |
| Spanisch      | `es`          |
| Suaheli      | `sw`          |
| Schwedisch      | `sv`          |
| Tamilisch      | `ta`          |
| Thailändisch      | `th`          |
| Türkisch      | `tr`          |
| Ukrainisch      | `uk`          |
| Urdu      | `ur`          |
| Vietnamesisch      | `vi`          |
| Walisisch      | `cy`          |

### <a name="access-the-translator-language-list-programmatically"></a>Programmgesteuertes Zugreifen auf die Translator-Sprachliste

Mithilfe der Methode „Languages“ können Sie eine Liste der unterstützten Sprachen für den Translator abrufen. Sie können die Liste nach Funktion, Sprachcode sowie Sprachname in Englisch oder jeder anderen unterstützten Sprache anzeigen. Sobald neue Sprachen verfügbar gemacht werden, wird diese Liste automatisch vom Microsoft Translator-Dienst aktualisiert.

[Anzeigen der Referenzdokumentation zum Vorgang „Languages“](reference/v3-0-languages.md)

## <a name="customization"></a>Anpassung

Die folgenden Sprachen sind für die Anpassung an das Englische oder aus dem Englischen mit dem [benutzerdefinierten Translator](https://aka.ms/CustomTranslator) verfügbar.

| Sprache    | Sprachcode |
|:----------- |:-------------:|
|Afrikaans| `af`|
| Arabisch       | `ar`          |
| Bengalisch      | `bn`          |
| Bosnisch (Lateinisch)      | `bs`          |
| Bulgarisch      | `bg`          |
|Katalanisch|   `ca`    |
| Chinesisch (vereinfacht)      | `zh-Hans`          |
|Chinesisch (traditionell)|   `zh-Hant`   |
| Kroatisch      | `hr`          |
| Tschechisch      | `cs`          |
| Dänisch      | `da`          |
| Niederländisch      | `nl`          |
| Englisch    | `en`     |
| Estnisch      | `et`          |
|Fidschi|    `fj`    |
|Filipino|  `fil`   |
| Finnisch      | `fi`          |
| Französisch      | `fr`          |
| Deutsch      | `de`          |
| Griechisch      | `el`          |
| Gujarati| `gu`    |
| Hebräisch      | `he`          |
| Hindi      | `hi`          |
| Ungarisch      | `hu`          |
| Isländisch | `is` |
| Indonesisch|   `id`    |
| Inuktitut|    `iu`    |
| Irisch | `ga`  |
| Italienisch      | `it`          |
| Japanisch      | `ja`          |
|Kannada|`kn`|
| Koreanisch      | `ko`          |
| Lettisch      | `lv`          |
| Litauisch      | `lt`          |
| Madagassisch| `mg`    |
| Malaiisch|    `ms` |
|Maltesisch|   `mt`    |
| Maori| `mi`  |
| Marathi| `mr`  |
| Norwegisch      | `nb`          |
| Persisch      | `fa`          |
| Polnisch      | `pl`          |
| Portugiesisch (Brasilien) | `pt` |
| Pandschabi|`pa`|
| Rumänisch      | `ro`          |
| Russisch      | `ru`          |
| Samoanisch|   `sm`    |
| Serbisch (Lateinisch)      | `sr-Latn`          |
| Slowakisch     | `sk`          |
| Slowenisch      | `sl`          |
| Spanisch      | `es`          |
| Suaheli|  `sw`    |
| Schwedisch      | `sv`          |
|Tahitisch|  `ty`    |
| Thailändisch      | `th`          |
|Tongaisch|    `to`    |
| Türkisch      | `tr`          |
| Ukrainisch      | `uk`          |
| Urdu| `ur`    |
| Vietnamesisch      | `vi`          |
| Walisisch | `cy` |

## <a name="speech-translation"></a>Sprachübersetzung
Die Sprachübersetzung ist durch die Verwendung des Translator in Verbindung mit dem Spracherkennungsdienst von Cognitive Services verfügbar. In der [Dokumentation zum Spracherkennungsdienst](../speech-service/index.yml) erfahren Sie mehr über die Verwendung der Sprachübersetzung sowie über alle [verfügbaren Sprachoptionen](../speech-service/language-support.md).

### <a name="speech-to-text"></a>Spracherkennung
Wandeln Sie Sprache in Text um, damit dieser in eine beliebige Textsprache übersetzt werden kann. Die Spracherkennung wird für die Übersetzung von Sprache in Text oder für die Übersetzung von Sprache in Sprache verwendet, wenn sie zusammen mit der Sprachsynthese verwendet wird.

| Sprache    |
|:----------- |
|Arabisch|
|Chinesisch (traditionell)|
|Katalanisch|
|Chinesisch (vereinfacht)|
|Chinesisch (traditionell)|
|Dänisch|
|Niederländisch|
|Englisch|
|Finnisch|
|Französisch|
|Französisch (Kanada)|
|Deutsch|
|Gujarati|
|Hindi|
|Italienisch|
|Japanisch|
|Koreanisch|
|Marathi|
|Norwegisch|
|Polnisch|
|Portugiesisch (Brasilien)|
|Portugiesisch (Portugal)|
|Russisch|
|Spanisch|
|Schwedisch|
|Tamilisch|
|Telugu|
|Thailändisch|
|Türkisch|

### <a name="text-to-speech"></a>Text-zu-Sprache
Konvertieren von Text in Sprache. Text-zu-Sprache wird in Verbindung mit der Spracherkennung zum Hinzufügen einer hörbaren Ausgabe von Übersetzungsergebnissen oder für die Übersetzung von Sprache in Sprache verwendet. 

| Sprache |
|:-|
| Arabisch |
| Bulgarisch |
| Chinesisch (traditionell) |
| Katalanisch |
| Chinesisch (vereinfacht) |
| Chinesisch (traditionell) |
| Kroatisch |
| Tschechisch |
| Dänisch |
| Niederländisch |
| Englisch |
| Finnisch |
| Französisch |
| Französisch (Kanada) |
| Deutsch |
| Griechisch |
| Hebräisch |
| Hindi |
| Ungarisch |
| Indonesisch |
| Italienisch |
| Japanisch |
| Koreanisch |
| Malaiisch |
| Norwegisch |
| Polnisch |
| Portugiesisch (Brasilien) |
| Portugiesisch (Portugal) |
| Rumänisch |
| Russisch |
| Slowakisch |
| Slowenisch |
| Spanisch |
| Schwedisch |
| Tamilisch |
| Telugu |
| Thailändisch |
| Türkisch |
| Vietnamesisch |

## <a name="view-the-language-list-on-the-microsoft-translator-website"></a>Anzeigen der Liste der Sprachen auf der Microsoft Translator-Website

Für einen schnellen Überblick über die Sprachen werden auf der Microsoft Translator-Website alle Sprachen angezeigt, die vom Translator für die Textübersetzung und den Spracherkennungsdienst für die Sprachübersetzung unterstützt werden. Diese Liste enthält keine entwicklerspezifischen Informationen wie z.B. Sprachcodes.

[Die Liste der Sprachen](https://www.microsoft.com/translator/languages.aspx)
