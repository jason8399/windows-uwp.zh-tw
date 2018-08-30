---
author: drewbatgit
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: 本文列出適用於 UWP 應用程式支援的 HTTP 即時資料流 (HLS) 通訊協定標記。
title: HTTP 即時資料流 (HLS) 標記支援
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d47a5f755d9ba18f8e95362d254239499d29b9e5
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843681"
---
# <a name="http-live-streaming-hls-tag-support"></a>HTTP 即時資料流 (HLS) 標記支援
下表列出適用於 UWP 應用程式支援的 HLS 標記。

> [!NOTE] 
> 以 "X-" 開頭的自訂標記可做為定時中繼資料來存取，如[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)文章中所述。

|標記 |HLS 通訊協定版本所引進|HLS 通訊協定文件草稿版本|用戶端上的必要項|7 月發行的 Windows 10|Windows 10，版本 1511|Windows 10，版本 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  基本標記                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|必要|支援|支援|支援|
| 4.3.1.2.  EXT-X-VERSION |2|3|必要|支援|支援|支援
|4.3.2.  媒體區段標記                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|必要|支援|支援|支援
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|選用|支援|支援|支援|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|選用|支援|支援|支援|
| 4.3.2.4.  EXT-X-KEY |1|0|選用|支援|支援|支援|
|&nbsp;&nbsp;&nbsp; METHOD|1|0|屬性|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp; IV|2|3|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|屬性|不支援|不支援|不支援|
| 4.3.2.5.  EXT-X-MAP |5|9|選用|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp; URI|5|9|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|屬性|不支援|不支援|不支援|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|選用|不支援|不支援|不支援|
|4.3.3.  媒體播放清單標記                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|必要|支援|支援|支援|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|選用|支援|支援|支援|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|選用|不支援|不支援|不支援|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|選用|支援|支援|支援|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|選用|支援|支援|支援|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|選用|不支援|不支援|不支援|
|4.3.4.  主要播放清單標記                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|選用|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  TYPE|4|7|屬性|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  LANGUAGE|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|屬性|不支援|不支援|支援|
|&nbsp;&nbsp;&nbsp;  DEFAULT|4|7|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  AUTOSELECT|4|7|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  FORCED|5|9|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  CHARACTERISTICS|5|9|屬性|不支援|不支援|不支援|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|必要|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  BANDWIDTH|1|0|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|屬性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AVERAGE-BANDWIDTH|7|14|屬性|不支援|不支援|不支援|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  RESOLUTION|2|3|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  FRAME-RATE|7|15|屬性|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  VIDEO|4|7|屬性|支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  SUBTITLES|5|9|屬性|不支援|不支援|支援|
|&nbsp;&nbsp;&nbsp;  CLOSED-CAPTIONS|6|12|屬性|不支援|不支援|不支援|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|選用|不支援|不支援|不支援|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|選用|不支援|不支援|不支援|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|選用|不支援|不支援|不支援|
|4.3.5.  媒體或主要播放清單標記                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|選用|不支援|支援|支援|
| 4.3.5.2.  EXT-X-START  |6|12|選用|不支援|部分支援|部分支援|
|&nbsp;&nbsp;&nbsp;  TIME-OFFSET|6|12|屬性|不支援|支援|支援|
|&nbsp;&nbsp;&nbsp;  PRECISE|6|12|屬性|不支援|支援預設 "NO"|支援預設 "NO"|



## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [彈性資料流](adaptive-streaming.md)
 

 



