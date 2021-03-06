﻿---
title: '允許使用者撥: Exchange Online Help'
TOCTitle: 允許使用者撥
ms:assetid: b6e696ce-c848-475b-a598-9035677497e2
ms:mtpsurl: https://technet.microsoft.com/zh-tw/library/Bb232172(v=EXCHG.150)
ms:contentKeyID: 51409207
ms.date: 05/23/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# 允許使用者撥

 

_**適用版本：**Exchange Online, Exchange Server 2013, Exchange Server 2016_

_**上次修改主題的時間：**2015-03-09_

*Outdialing*是程序的使用者撥打 Outlook 語音存取號碼相關法律以及使用 UM 撥號對應表或將電話轉接給內部或外部的電話號碼。整合的通訊使用許多 outdialing 設定撥號對應表之使用者的通話。若要設定撥出，您必須設定撥號規則撥號規則群組及撥號授權在整合通訊 (UM) 撥號對應表和授權 \[撥出在 UM 撥號對應表、 UM 信箱原則和自動語音應答。您也可以設定 UM 撥號對應表有撥號或存取碼，國際電話首碼，和國家/地區或國際號碼格式，可讓您在組織中撥出的控制項。本主題討論撥號規則、 撥號規則群組及撥號授權和授權的使用方式和控制撥出您的組織。

**目錄**

概觀

Types of users

Outdialing settings

Configuring outdialing

Applying configured dialing rule groups

Applying dialing rules

## 概觀

撥出會在下列情況發生：

  - 撥打外部電話號碼。

  - 來電轉接至自動語音應答。

  - 來電轉接至貴組織中的使用者。

  - 已啟用 UM 的使用者使用 \[在電話上播放\] 功能。

若要撥出能夠正確運作，必須正確地進行下列設定：

  - **撥號規則**   撥號規則會定義已啟用 UM 的使用者所撥打的號碼，以及專用交換機 (PBX) 或 IP PBX 將撥打的號碼。

  - **撥號規則群組**   撥號規則群組會決定撥號群組中的使用者所能撥打的電話類型。

  - **撥號授權**   撥號授權會決定要套用的限制，以防止為使用者帶來不必要的電話費，或防止使用者撥打長途電話。

若要為撥入到撥號對應表或撥入到自動語音應答的使用者啟用撥出功能，您必須：

  - 確定與撥號對應表連結的 UM IP 閘道所代表的 VoIP 閘道將允許撥出電話。

  - 在 UM 撥號對應表上建立撥號規則，以建立撥號規則群組。

  - 在與撥號對應表 (與 UM IP 閘道相同) 相關聯的 UM 撥號對應表、UM 信箱原則或自動語音應答上，為國內/地區和國際撥號規則群組新增撥號授權。

## 使用者類型

兩種類型的使用者可以使用整合通訊中的 outdialing 功能： 驗證且未經驗證的認證。撥入 UM 自動語音應答的所有使用者都皆未經過驗證。當使用者撥打 Outlook 語音存取號碼時，他們正在視為未驗證因為它們尚未提供其分機號碼和 PIN 和登入他們的信箱。使用者被驗證之後他們提供其分機號碼和 PIN 並已順利登入他們的信箱。

當使用者撥打 Outlook 語音存取號碼設定 UM 撥號對應表和嘗試撥打或傳輸呼叫不含登入他們的信箱，僅 UM 撥號對應表撥出的設定會套用至通話。匿名或未經過驗證的使用者撥入 UM 自動語音應答、 時的自動語音應答上設定的 outdialing 設定和 outdialing 撥號對應表相關聯的自動語音應答上設定的設定會套用至通話。

當使用者撥打 Outlook 語音存取號碼撥號對應表上設定及成功登入其信箱時\] 時即已驗證的使用者。當他們正在通過驗證時，outdialing 通話設定連結至這些使用者的 UM 信箱原則上會使用撥號規則與撥號授權設定。

回到頁首

## 撥出設定

您需要將數個設定為適用於撥出貴組織的規則。除了設定 UM 撥號對應表、 UM 自動語音應答和 UM 信箱原則您已建立具有正確的撥號規則與撥號授權，您必須設定存取代碼、 電話首碼及數字格式在 \[UM 撥號對應表。撥號對應表、 自動語音應答和 UM 信箱原則上設定下列 outdialing 設定：

  - 外線、國家/地區及國際電話撥接碼

  - 國際電話首碼

  - 國內/地區及國際號碼格式

  - 設定的國內/地區及國際撥號規則群組

  - 允許的國內/地區及國際撥號規則群組

  - 撥號規則項目

  - 撥號授權

您已成功設定撥出您的組織，您需要了解如何使用每個元件與撥出及必須設定元件的方式。下表介紹每個需要設定 UM 撥號對應表、 UM 自動語音應答和 UM 信箱原則之前撥出正常運作的元件。

### 撥出元件

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>元件</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>撥號代碼、電話首碼及電話號碼格式</p></td>
<td><p>UM 用途撥號代碼、 電話首碼及呼叫來決定正確的號碼來撥號對應表時進行外寄的數字格式。您可以設定撥號代碼、 電話首碼及限制撥出電話撥入 UM 自動語音應答與 UM 撥號對應表相關聯的使用者或使用者的撥號對應表的撥號對應表上設定 Outlook 語音存取號碼的數字格式。</p></td>
</tr>
<tr class="even">
<td><p>撥號規則群組</p></td>
<td><p>若要啟用它們針對撥出電話傳送至 PBX 之前要修改的電話號碼建立撥號規則群組。撥號規則群組中移除的數字或新增至 UM 所呼叫的電話號碼的數字。例如，您可以建立自動將字首為 9 新增 7 位數電話號碼來提供外線存取撥號規則群組。在這個範例中，放置撥出電話的使用者不需要再連絡某人組織外部的電話號碼 9 的撥號對應表。</p>
<p>每個撥號規則群組包含決定的國家/地區及國際撥號規則群組內的使用者可以進行的通話類型的撥號規則。撥號規則群組適用於與 UM 撥號對應表或 UM 自動語音應答並與 UM 撥號對應表關聯的 UM 信箱原則相關聯的使用者。每個撥號規則群組必須包含至少一個撥號規則。</p></td>
</tr>
<tr class="odd">
<td><p>撥號規則項目</p></td>
<td><p>撥號規則用於判斷的撥號規則群組內的使用者可以進行的通話類型。當您建立撥號規則群組時，您可以設定一或多個撥號規則。</p>
<p>當您設定每個撥號規則時，您必須輸入撥號規則名稱、 轉換 （<em>數字遮罩</em>） 的號碼模式及撥號號碼。您也可以輸入註解。註解可用來描述如何將會使用撥號規則或來說明撥號規則會套用收件者的使用者群組。當您將號碼遮罩和撥打的號碼新增至撥號規則時，您可以改用的數字的電話號碼，例如 91425xxxxxxx 字母 x。您也可以使用星號 （*） 符號做為萬用字元，例如 91425 *。</p></td>
</tr>
<tr class="even">
<td><p>撥號授權</p></td>
<td><p>撥號授權使用撥號規則群組將套用撥號限制是與特定的 UM 信箱原則、 撥號對應表或自動語音應答產生關聯的使用者。他們也可用時您想要讓使用者撥打電話至國家/地區或國際電話號碼。</p>
<p>在 UM 撥號對應表建立撥號規則之後，您將撥號規則群組新增至 UM 信箱原則、 撥號對應表規劃、 或自動語音應答。撥號規則群組新增至 UM 信箱原則之後，所有設定或定義規則會都套用至連結與 UM 信箱原則已啟用 UM 的使用者。</p></td>
</tr>
</tbody>
</table>


回到頁首

## 設定撥出

撥號規則群組會在 UM 撥號對應表上設定的一或多個撥號規則的集合。可設定 UM 撥號對應表使用兩種類型的撥號規則群組 ︰ 國家/地區及國際。國家/地區的撥號規則群組套用至相同的國家或地區內所撥打的電話號碼。國際撥號規則群組套用至從一個國家或地區撥打到另一個國家或地區的國際電話號碼。

每個 UM 撥號對應表可以包含一或多個撥號規則群組。若要將套用於撥號規則群組一組使用者，建立撥號規則群組之後，您必須將它加入清單的允許撥號規則群組在 UM 撥號對應表和 UM 自動語音應答和與 UM 撥號對應表關聯的 UM 信箱原則。

撥號規則群組可讓您指定您想要套用至一群屬於特定類別已啟用 UM 之使用者的撥號規則。例如，您可以使用撥號規則群組指定哪些使用者群組可以撥打國際電話和哪一個群組可以進行只有在狀態或本機的通話。您可以建立撥號規則群組在 Exchange 管理命令介面中使用 Exchange 系統管理中心 (EAC) 或**Set-UMDialPlan**指令程式。當您建立撥號規則群組時，您必須定義至少一個撥號規則群組。

當使用者撥打的電話號碼時、 UM 採用數，並尋找在撥號規則相符。如果找到相符項目、 UM 會使用撥號規則來決定要撥號對應表來查看電話號碼或撥號規則的**撥打的號碼**\] 區段中所列的數字的數字。將撥打**撥打的號碼**\] 方塊中的撥號規則中所列的號碼。

下表顯示範例撥號規則群組及撥號規則。在這個範例中，只有本地通話和低率是已建立撥號規則群組。撥號規則群組只有本地通話有兩種撥號規則： 91425 \* 和 91206 \*，和撥號規則群組低率也有兩種撥號規則： 91509 \* 和 91360 \*。

### 撥號規則群組及撥號規則

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>名稱</th>
<th>NumberMask</th>
<th>DialedNumber</th>
<th>註解</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Local-Calls-Only</p></td>
<td><p>91425*</p></td>
<td><p>91*</p></td>
<td><p>市內電話</p></td>
</tr>
<tr class="even">
<td><p>Local-Calls-Only</p></td>
<td><p>91206*</p></td>
<td><p>91*</p></td>
<td><p>市內電話</p></td>
</tr>
<tr class="odd">
<td><p>Low-Rate</p></td>
<td><p>91509*</p></td>
<td><p>9*</p></td>
<td><p>省/市電話</p></td>
</tr>
<tr class="even">
<td><p>Low-Rate</p></td>
<td><p>91360*</p></td>
<td><p>9*</p></td>
<td><p>省/市電話</p></td>
</tr>
</tbody>
</table>


例如，當使用者撥打 9-1-425-555-1234年、 UM 撥打 4255551234。UM 會移除任何非數值的字元 （在本例中為連字號），並從撥號規則套用的號碼遮罩。在本例中為 UM 套用的數字遮罩 91 \*。這會告知 UM 不適用於撥 9 或 1，但撥號對應表中的所有其他號碼的電話號碼右邊的數字 1 顯示的。這包括由星號 （\*） 表示的所有號碼。

您可以使用 EAC 或命令介面來建立和設定單一或多個國家/地區內及國際撥號規則群組及撥號規則。不過，如果您正在建立兩個或多個複雜撥號規則群組及撥號規則，您可以使用命令介面中的逗號分隔值 (.csv) 檔案。您可以匯入或匯出撥號規則群組及撥號規則的清單。

若要匯入已定義在 .csv 檔案中的撥號規則群組與撥號規則清單，請執行 **Set-UMDialPlan** 指令程式，如下所示。

    Set-UMDialPlan "MyUMDialPlan" -ConfiguredInCountryOrRegionGroups $(IMPORT-CSV c:\dialrules\InCountryRegion.csv)

若要擷取已設定在 UM 撥號對應表上的撥號規則群組清單，請執行 **Get-UMDialPlan** 指令程式 (如下所示)。

    (Get-UMDialPlan -id "MyUMDialPlan").ConfiguredInCountryOrRegionGroups | EXPORT-CSV C:\incountryorregion.csv

必須建立並儲存在正確的格式之.csv 檔案。在.csv 檔案中的每行代表一個撥號規則。不過，每個撥號規則被設定在相同的撥號規則群組。在檔案中的每一個規則會有四個以逗號分隔的區段。以下各節會名稱、 數字遮罩、 撥打的號碼和註解。每一節需要，以及您必須在 \[註解\] 區段中除了每一節中輸入正確的資訊。那里應該是文字項目和逗點之間不含空白的 \[下一步\] 區段中，也不應該會有任何空白處規則之間或結尾。以下是可用來建立國家/地區的撥號規則群組與撥號規則.csv 檔案的範例。

**Name,NumberMask,DialedNumber,Comment**

**Low-rate,91425xxxxxxx,9xxxxxxx,市內電話**

**Low-rate,9425xxxxxxx,9xxxxxxx,市內電話**

**Low-rate,9xxxxxxx,9xxxxxxx,市內電話**

**Any,91\*,91\*,開放存取國內/地區號碼**

**Long-distance,91408\*,91408\*,長途電話**

下列是可以用來建立國際撥號規則群組及撥號規則項目的 .csv 檔案範例。

**Name,NumberMask,DialedNumber,Comment**

**International, 901144\*, 901144\*, 國際電話**

**International, 901133\*, 901133\*, 國際電話**

回到頁首

## 套用設定的撥號規則群組

撥號規則群組會建立 UM 撥號對應表上。您可以建立國家/地區或國際撥號規則群組在命令介面中使用 EAC 或**Set-UMDialPlan**指令程式。在建立適當的撥號規則群組之後 UM 撥號對應表和定義撥號規則，您可以套用至 UM 撥號對應表計劃、 建立 UM 自動語音應答，或使用者相關聯的 UM 信箱原則和授權撥出如何根據使用者存取語音信箱系統的撥號規則群組。

您可以將在 UM 撥號對應表上建立的撥號規則群組套用到下列項目：

  - **同一個撥號對應表**  設定會套用至所有使用者撥打 Outlook 語音存取號碼，但未登入他們的信箱。若要套用一個名為`MyAllowedDialRuleGroup`至相同的撥號對應表的國家/地區的撥號規則群組，使用命令介面**Set-UMDialPlan**指令程式，如下所示。
    
        Set-UMDialPlan -Identity MyUMDialPlan -AllowedInCountryOrRegionGroups MyAllowedDialRuleGroup

  - **單一或多個 UM 信箱原則**UM 信箱原則所設定的設定會套用至連結與該 UM 信箱原則的所有使用者。在 UM 信箱原則上設定這些設定套用到使用者撥打 Outlook 語音存取號碼並登入他們的信箱。若要套用一個名為`MyAllowedDialRuleGroup`到單一的 UM 信箱原則的國家/地區的撥號規則群組，請在 EAC 中的 UM 信箱原則上使用 \[**撥號授權**\] 頁面上或使用**Set-UMMailboxPolicy**指令程式在命令介面中，如下所示。
    
        Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowedInCountryOrRegionGroups MyAllowedDialRuleGroup

  - **撥號對應表 UM 相關聯的單一或多個自動語音應答**這將會套用至所有撥入 UM 自動語音應答的使用者。套用名為`MyAllowedDialRuleGroup`到單一的 UM 自動語音應答的國家/地區的撥號規則群組，使用**撥號授權**\] 頁面上的 EAC 中的自動語音應答或**Set-UMAutoAttendant**指令程式在命令介面中，如下所示。
    
        Set-UMAutoAttendant -Identity MyUMAutoAttendant -AllowedInCountryOrRegionGroups MyAllowedDialRuleGroup

下表摘要說明在整合通訊中套用撥號規則群組的方式。

### 套用撥出規則

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>來電者類型</th>
<th>範圍</th>
<th>套用的撥出設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 語音存取號碼</p></td>
<td><p>使用者撥打撥號對應表 Outlook 語音存取號碼，並登入信箱</p></td>
<td><p>UM 信箱原則</p></td>
</tr>
<tr class="even">
<td><p>匿名來電者</p></td>
<td><p>使用者撥打撥號對應表 Outlook 語音存取號碼</p></td>
<td><p>UM 撥號對應表</p></td>
</tr>
<tr class="odd">
<td><p>匿名來電者</p></td>
<td><p>使用者撥打自動語音應答整合通訊總機號碼或分機號碼</p></td>
<td><p>UM 自動語音應答</p></td>
</tr>
<tr class="even">
<td><p>組織外部的來電者</p></td>
<td><p>使用者呼叫「在電話上播放」號碼</p></td>
<td><p>UM 信箱原則</p></td>
</tr>
</tbody>
</table>


回到頁首

## 套用撥號規則

下列情況會進行撥出處理程序：

  - 整合通訊幫撥號者撥打外部電話號碼。

  - 整合通訊將來電轉接給自動語音應答。

  - 整合通訊將來電轉接給貴組織中的使用者。

  - 已啟用 UM 的使用者使用 \[在電話上播放\] 功能。

在每個 outdialing 的案例中，UM 將套用撥號規則已設定，然後通電話使用者。不過，根據此案例中，且使用者如何啟動通話、 UM 可能適用於只將部分撥號規則所撥打的電話號碼。在其他 outdialing 的情況下，UM 可能適用於設定為所撥打的電話號碼的所有 outdialing 規則。

回到頁首

