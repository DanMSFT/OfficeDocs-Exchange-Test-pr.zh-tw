﻿---
title: '轉換及容錯移轉: Exchange 2013 Help'
TOCTitle: 轉換及容錯移轉
ms:assetid: 75388645-cae1-402e-bf02-c4949d3e2c31
ms:mtpsurl: https://technet.microsoft.com/zh-tw/library/Dd298067(v=EXCHG.150)
ms:contentKeyID: 62523862
ms.date: 05/21/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# 轉換及容錯移轉

 

_**適用版本：**Exchange Server 2013 SP1_

_**上次修改主題的時間：**2015-12-02_

轉換和容錯移轉是 Microsoft Exchange Server 2013 中的兩種中斷形式：

  - *轉換*為資料庫或伺服器的明確起始指令程式或 Exchange 2013 中受管理的可用性系統在排定的中斷。轉換通常準備執行維護作業。轉換涉及作用中信箱資料庫複本中移至另一個伺服器的資料庫可用性群組 (DAG)。如果沒有正常目標找到在轉換期間，系統管理員會收到錯誤與信箱資料庫會保持向上、 或裝載。

  - *容錯移轉*是指未預期的事件所產生的服務、 資料或同時執行中。容錯移轉涉及啟用使其成為作用中信箱資料庫副本的被動信箱資料庫副本自動從故障復原系統。如果沒有狀況良好的目標找到容錯移轉期間，將會卸載信箱資料庫。

Exchange 2013 已特別設計來處理轉換和容錯移轉。

要尋找與高可用性和站台恢復相關的管理工作嗎？請參閱[管理高可用性和站台恢復](managing-high-availability-and-site-resilience-exchange-2013-help.md)。

## 轉換

Exchange 2013 中有三種轉換類型：

  - 資料庫轉換

  - 伺服器轉換

  - 資料中心轉換

## 資料庫轉換

「資料庫轉換」是由個別作用中資料庫轉換至另一個資料庫副本 (被動副本) 的程序，而該資料庫副本會建立新的作用中資料庫副本。資料庫轉換可能同時會在資料中心內部和之間發生。您可以使用 Exchange 系統管理中心 (EMC) 或命令介面來進行資料庫轉換。不論使用哪一種介面，轉換的程序如下：

1.  系統管理員初始化資料庫轉換，將目前作用中信箱資料庫副本移動到另一個伺服器。

2.  工作所使用的用戶端會對 DAG 成員上的 Microsoft Exchange 複寫服務進行 RPC 呼叫。

3.  如果 DAG 成員不具有 Primary Active Manager (PAM) 角色，則 DAG 成員會將工作轉交給具有 PAM 角色的伺服器。

4.  此工作會對具有 PAM 角色的伺服器上的 Microsoft Exchange 複寫服務進行 RPC 呼叫。

5.  PAM 會讀取並更新儲存在 DAG 的叢集資料庫中的資料庫位置資訊。

6.  PAM 會聯繫 DAG 成員上的 Microsoft Exchange 複寫服務，該成員的被動副本將啟用為新的作用中信箱資料庫副本。

7.  目標伺服器上的 Microsoft Exchange 複寫服務會在所有其他 DAG 成員上查詢 Microsoft Exchange 複寫服務，以判斷資料庫副本的最佳記錄檔來源。

8.  資料庫會從目前的伺服器卸載，而目標伺服器上的 Microsoft Exchange 複寫服務會將剩餘的記錄檔複製到目標伺服器。

9.  目標伺服器上的 Microsoft Exchange 複寫服務會要求裝載資料庫。

10. 目標伺服器上的 Microsoft Exchange 資訊儲存庫服務會重新顯示記錄檔，並裝載資料庫。

11. 所有錯誤碼都會傳回到目標伺服器上的 Microsoft Exchange 複寫服務。

12. PAM 會更新 DAG 的叢集資料庫中的資料庫副本狀態資訊。

13. 所有錯誤碼都會由目標伺服器上的 Microsoft Exchange 複寫服務傳回到 PAM 上的 Microsoft Exchange 複寫服務。

14. PAM 上的 Microsoft Exchange 複寫服務會將所有錯誤傳回到呼叫工作的系統管理介面。

15. 遠端 PowerShell 會將作業的結果傳回到呼叫的系統管理介面。

如需如何執行資料庫轉換的詳細步驟，請參閱[啟動信箱資料庫副本](activate-a-mailbox-database-copy-exchange-2013-help.md)。

## 伺服器轉換

伺服器轉換是在一個或多個其他 DAG 成員上啟動 DAG 成員上的所有作用中資料庫的程序。與資料庫轉換類似，伺服器轉換可能發生在一個資料中心內和多個資料中心之間，而且可以藉由使用 EAC 和命令介面進行初始化。不論使用哪一種介面，伺服器轉換的程序都如下所示：

1.  系統管理員會初始化伺服器轉換，將所有目前的作用中信箱資料庫副本移動到一個或多個其他伺服器。

2.  工作會為目前伺服器上的每個作用中資料庫的資料庫轉換，執行本主題先前所述的相同步驟 (步驟 2 到 4)。

3.  PAM 會讀取並更新儲存在 DAG 的叢集資料庫中的資料庫位置資訊。

4.  PAM 會聯繫將啟用被動副本的每個 DAG 成員上的 Microsoft Exchange 複寫服務。

5.  目標伺服器上的 Microsoft Exchange 複寫服務會查詢所有其他 DAG 成員上的 Microsoft Exchange 複寫服務，以判斷資料庫副本的最佳記錄檔來源。

6.  資料庫會從目前的伺服器卸載，而每個目標伺服器上的 Microsoft Exchange 複寫服務會複製剩餘的記錄檔。

7.  每個目標伺服器上的 Microsoft Exchange 複寫服務會要求裝載資料庫。

8.  每個目標伺服器上的 Microsoft Exchange 資訊儲存庫服務會重新顯示記錄檔，並裝載資料庫。

9.  所有錯誤碼都會傳回到目標伺服器上的 Microsoft Exchange 複寫服務。

10. PAM 會更新 DAG 的叢集資料庫中的資料庫副本狀態資訊。

11. 所有錯誤碼都會由目標伺服器上的 Microsoft Exchange 複寫服務傳回到 PAM 上的 Microsoft Exchange 複寫服務。

12. PAM 上的 Microsoft Exchange 複寫服務會將所有錯誤傳回到呼叫工作的系統管理介面。

13. 遠端 PowerShell 會將作業的結果傳回到呼叫的系統管理介面。

如需如何執行伺服器轉換的詳細步驟，請參閱[執行伺服器轉換](perform-a-server-switchover-exchange-2013-help.md)。

## 資料中心轉換

在站台恢復組態中，站台層級失敗之後的自動復原可能在 DAG 內進行，可讓郵件系統維持正常運作狀態。此組態至少需要三個位置，因為需要將 DAG 成員部署在兩個位置，而 DAG 的見證伺服器在第三個位置。

如果您沒有三個位置，或者，即使有三個位置，但想要控制資料中心層級復原動作，您可以設定 DAG 以便在站台層級失敗時手動復原。在此情況下，您需要執行所謂的*「資料中心轉換」*程序。和許多災難復原案例一樣，資料中心轉換的優先規劃和準備可以簡化復原程序，並縮短中斷的持續時間。

因為 Exchange 2013 中有許多架構性變更，包括合併伺服器角色，在 Exchange 2013 中執行資料中心轉換比在 Exchange 2010 中輕鬆許多。如需有關執行資料中心轉換的詳細步驟，請參閱＜[資料中心轉換](datacenter-switchovers-exchange-2013-help.md)＞。

## 容錯移轉

容錯移轉是一種可在資料庫、伺服器或資料中心層級上進行的自動啟動程序。進行容錯移轉是為了因應影響到個別資料庫 (例如，隔離的儲存庫遺失)、整個伺服器 (例如，主機板故障或斷電) 或整個網站 (例如，遺失網站中的所有 DAG) 的失敗狀況。

對於資料和可供存取資料的服務，DAG 和信箱資料庫副本提供完整備援和快速復原。下表列出各種失敗類型的預期復原動作。某些失敗需要系統管理員啟動復原，其他失敗則由系統自動處理。


<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th>描述</th>
<th>自動啟動</th>
<th>自動修復動作</th>
<th>修復期間狀態：主動</th>
<th>修復期間的狀態：被動</th>
<th>修復動作</th>
<th>註解</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>可延伸儲存引擎 (ESE) 軟體資料庫失敗：儲存資料庫的磁碟機在進行某些讀取時傳回錯誤 (例如，-1018 錯誤)。</p></td>
<td><p>可能會出現短暫中斷的情形。</p>
<p>可能會自動進行容錯移轉。</p></td>
<td><p>自動修補錯誤頁面。</p></td>
<td><p>手動轉換、自動容錯移轉或線上修復。</p></td>
<td><p>失敗</p></td>
<td><p>RAID 重建、資料庫和資料庫副本修復、還原和執行復原後修補頁面，或從副本修補頁面。</p></td>
<td><p>可能會包含其他軟體資料庫失敗碼。</p>
<p>不包括 NTFS 檔案系統區塊失敗。</p>
<p>如果執行了容錯移轉或轉換，就會更新主機伺服器。</p></td>
</tr>
<tr class="even">
<td><p>ESE「半軟體」資料庫失敗：儲存資料庫的磁碟機在寫入某些資料時傳回錯誤。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>在可能更換磁碟機之後的自動重建磁碟區/磁碟。</p></td>
<td><p>如果無法復原則予以卸載。</p></td>
<td><p>失敗</p></td>
<td><p>RAID 重建可能會解決問題。</p>
<p>複製及修復、還原及執行復原，或是在可能的更換作業之後重建磁碟區/磁碟。</p></td>
<td><p>ESE 半軟體寫入錯誤表示某些寫入已成功。</p>
<p>不包括 NTFS 區塊失敗。</p></td>
</tr>
<tr class="odd">
<td><p>ESE「半軟體」記錄檔失敗：儲存記錄檔資料的磁碟機在進行某些讀取或寫入時傳回非復原的錯誤。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>在可能更換磁碟機之後的自動重建磁碟區/磁碟。</p></td>
<td><p>如果無法復原則予以卸載。</p></td>
<td><p>失敗</p></td>
<td><p>RAID 重建可能會解決問題。</p>
<p>複製及修復、還原及執行復原，或是在可能的更換作業之後重建磁碟區/磁碟。</p></td>
<td><p>ESE 半軟體讀取/寫入錯誤表示一些讀取/寫入已成功。</p>
<p>如果資料庫失敗，則會在記錄檔資料復原程序開始之前進行自動復原。</p></td>
</tr>
<tr class="even">
<td><p>ESE 軟體錯誤或資源耗盡：ESE 終止執行個體 (例如，事件識別碼 1022，檢查點深度太深) 的錯誤。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>如果無法復原則予以卸載。</p></td>
<td><p>失敗</p></td>
<td><p>修正基礎的資源問題。</p></td>
<td><p>這項失敗可能是其他情況下出現的錯誤。</p></td>
</tr>
<tr class="odd">
<td><p>NTFS 區塊失敗：儲存資料庫或記錄檔的磁碟機發生對 NTFS 控制結構讀取錯誤或寫入錯誤。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>在可能的磁碟機更換後對磁碟區進行完整的重建。</p></td>
<td><p>如果無法復原則予以卸載。</p></td>
<td><p>失敗</p></td>
<td><p>RAID 重建可能會解決問題。NTFS 公用程式可以解決 NTFS 問題。可能需要進行 Exchange 復原。</p></td>
<td><p>這更有可能會在 RAID 未使用時發生。如果這會影響作用中的記錄檔磁碟區，則會遺失某些最近的記錄檔。</p>
<p>不包含由 NTFS 或其基礎軟體或硬體堆疊自動修正的錯誤。</p></td>
</tr>
<tr class="even">
<td><p>資料庫或記錄檔磁碟機失敗：儲存資料庫或記錄檔的磁碟機已經完全失敗且無法存取。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>重新格式化或更換磁碟機，然後再進行完整的磁碟區重建。</p></td>
<td><p>如果無法復原則予以卸載。</p></td>
<td><p>失敗</p></td>
<td><p>在更換磁碟機之後，可能會重建 RAID。</p>
<p>在更換磁碟機之後，完整重建磁碟區。</p>
<p>完整重建磁碟區。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>資料庫或記錄檔磁碟區失敗：由於 NTFS 或較低層級的磁碟區問題造成磁碟區失敗。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>重新格式化或更換磁碟機。</p></td>
<td><p>如果無法復原則予以卸載。</p></td>
<td><p>失敗</p></td>
<td><p>在更換磁碟機之後，可能會重建 RAID。</p>
<p>在更換磁碟機之後，完整重建磁碟區。</p>
<p>完整重建磁碟區。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>資料庫或記錄檔磁碟區空間不足：含有資料庫或記錄檔的 NTFS 檔案系統空間不足。</p></td>
<td><p>如果其他副本未處於類似的狀態則自動進行容錯移轉。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>失敗</p></td>
<td><p>執行完整或增量備份、手動刪除記錄檔、讓時間經過、繼續資料庫副本，或修復失敗的資料庫副本。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>系統管理員卸載錯誤的資料庫。</p></td>
<td><p>如果自動容錯移轉未被系統管理員封鎖，則會出現短暫的中斷。</p>
<p>如果禁止執行自動容錯移轉，則在裝載資料庫之前都會中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>不適用</p></td>
<td><p>系統管理員修正錯誤。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>系統管理員擱置錯誤的資料庫副本。</p></td>
<td><p>依據組態和受影響的副本，可能無法自動復原。</p></td>
<td><p>無。</p></td>
<td><p>不適用。</p></td>
<td><p>已擱置</p></td>
<td><p>系統管理員修正錯誤。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>系統管理員卸載用於儲存區、NTFS 或磁碟區維護的資料庫。</p></td>
<td><p>如果自動容錯移轉未被系統管理員封鎖，則會出現短暫的中斷。</p>
<p>如果已封鎖自動容錯移轉，則在系統管理員完成工作前將會中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>不適用</p></td>
<td><p>系統管理員完成工作。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>系統管理員擱置用於儲存區、NTFS 或磁碟區維護的資料庫副本。</p></td>
<td><p>依據組態和受影響的副本，可能無法自動復原。</p></td>
<td><p>無。</p></td>
<td><p>不適用。</p></td>
<td><p>已擱置</p></td>
<td><p>系統管理員完成動作。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>系統管理員卸載資料庫，進行離線資料庫維護。</p></td>
<td><p>中斷直到修復為止。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>已擱置</p></td>
<td><p>系統管理員完成動作。</p></td>
<td><p>主動和被動資料庫副本出現分歧的情況。</p>
<p>系統管理員必須擱置副本。</p></td>
</tr>
<tr class="even">
<td><p>儲存區域網路 (SAN)、磁碟或儲存控制站失敗。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>修復硬體。</p></td>
<td><p>被動資料庫副本將處於系統失敗時存在的狀態。</p></td>
</tr>
<tr class="odd">
<td><p>伺服器硬體維護。</p></td>
<td><p>自動容錯移轉時短暫中斷 (除非遭到系統管理員封鎖)。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>完成動作。</p></td>
<td><p>被動資料庫副本的狀態，將是系統關閉時的狀態。</p></td>
</tr>
<tr class="even">
<td><p>伺服器軟體維護。</p></td>
<td><p>自動容錯移轉時短暫中斷 (除非遭到系統管理員封鎖)。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>完成動作。</p></td>
<td><p>被動資料庫副本的狀態，將是系統關閉時的狀態。</p></td>
</tr>
<tr class="odd">
<td><p>Microsoft Exchange 資訊儲存庫服務已由系統管理員停止或暫停。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>重新啟動 Microsoft Exchange 資訊儲存庫服務。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>Microsoft Exchange 資訊儲存庫服務失敗；作業系統仍在執行中。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>服務控制管理員重新啟動 Microsoft Exchange 資訊儲存庫服務。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>手動或自動重新啟動 Microsoft Exchange 資訊儲存庫服務。</p></td>
<td><p>被動資料庫副本將處於 Microsoft Exchange 資訊儲存庫服務失敗時存在的狀態。</p></td>
</tr>
<tr class="odd">
<td><p>部分 Microsoft Exchange 資訊儲存庫服務失敗；Exchange 儲存區的某些部分停止工作，但未將其識別為完全失敗。</p></td>
<td><p>自動容錯移轉期間的可能短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已裝載和部分作用。</p></td>
<td><p>任何，不過可能只是部分作用</p></td>
<td><p>重新啟動伺服器、作業系統或 Microsoft Exchange 資訊儲存庫服務。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>伺服器失敗：由於下列其中一個原因而導致伺服器失敗：</p>
<ul>
<li><p>電力完全中斷</p></li>
<li><p>無法復原故障的處理器晶片、主機板或背板</p></li>
<li><p>作業系統停止錯誤</p></li>
<li><p>作業系統停止回應</p></li>
<li><p>通訊完全失敗</p></li>
</ul></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>重新啟動電腦。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>還原電源、變更作業系統設定、變更硬體設定、更換硬體、重新啟動作業系統、服務作業系統、服務硬體，或修復通訊問題。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>DAG 遭遇仲裁失敗。</p></td>
<td><p>中斷直到修復為止。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>修復失敗的仲裁、指派新仲裁，或還原導致仲裁失敗的網路。</p></td>
<td><p>被動資料庫副本將處於系統失敗時存在的狀態。</p></td>
</tr>
<tr class="even">
<td><p>MAPI 網路通訊失敗：無法再使用 MAPI 網路上的伺服器。</p></td>
<td><p>自動容錯移轉時短暫中斷；不會有所損失。</p></td>
<td><p>無。將會繼續嘗試通訊。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>更正硬體或軟體問題，以修正通訊問題。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>複寫網路通訊失敗：伺服器無法透過失敗的複寫網路接收活動訊號、記錄檔副本或植入。</p></td>
<td><p>在工作量切換到其他網路時，可能會出現短暫的複製或植入中斷情形。</p></td>
<td><p>無。將會繼續嘗試通訊。</p></td>
<td><p>無。</p></td>
<td><p>任何</p></td>
<td><p>更正硬體或軟體問題，以修正通訊問題。</p></td>
<td><p>因失敗而影響恢復功能。</p></td>
</tr>
<tr class="even">
<td><p>多個網路通訊失敗：伺服器無法透過多個網路接收活動訊號、記錄檔副本或植入。</p></td>
<td><p>自動容錯移轉時短暫中斷；不會有所損失。</p></td>
<td><p>無。將會繼續嘗試通訊。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>更正硬體或軟體問題，以修正通訊問題。</p></td>
<td><p>至少有一個網路仍正常運作。</p></td>
</tr>
<tr class="odd">
<td><p>一或多個網路的部分失敗：網路出現高錯誤率的情形。</p></td>
<td><p>未偵測到失敗，未執行任何動作。</p></td>
<td><p>無。</p></td>
<td><p>已裝載，但可能是效能問題。</p></td>
<td><p>任何</p></td>
<td><p>更正硬體或軟體問題，以修正通訊問題。</p></td>
<td><p>網路的錯誤率比一般情形更高。</p></td>
</tr>
<tr class="even">
<td><p>無法偵測的作業系統當機：作業系統停止回應，但它不是由監視或叢集來偵測。</p></td>
<td><p>無。</p></td>
<td><p>無。</p></td>
<td><p>任何。</p></td>
<td><p>任何</p></td>
<td><p>重新啟動或結束沒有回應的資源。</p></td>
<td><p>未偵測到當機情形，因此不會採取任何動作。</p>
<p>某些功能可能會運作。</p></td>
</tr>
<tr class="odd">
<td><p>作業系統磁碟機發生失敗。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>更換磁碟機和重建伺服器，或藉由使用 RAID 來重建磁碟區。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>作業系統磁碟機空間不足。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>手動釋放磁碟區上的空間。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>Exchange 二進位碼檔案所在的磁碟機發生磁碟區或磁碟機失敗。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>更換磁碟機和重新安裝應用程式，或藉由使用 RAID 來重建磁碟區。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="even">
<td><p>Exchange 二進位碼檔案所在磁碟機的空間不足。</p></td>
<td><p>自動容錯移轉時的短暫中斷。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>任何</p></td>
<td><p>手動釋放磁碟區上的空間。</p></td>
<td><p>不適用。</p></td>
</tr>
<tr class="odd">
<td><p>偵測到無效的新記錄檔：記錄檔順序已被現有的檔案中斷。</p></td>
<td><p>自動容錯移轉期間的短暫中斷；假設其他副本沒有相同問題。</p></td>
<td><p>無。</p></td>
<td><p>已卸載。</p></td>
<td><p>失敗</p></td>
<td><p>在判定來源之後移除中斷的記錄檔。</p></td>
<td><p>不應該複寫中斷的記錄檔。</p></td>
</tr>
<tr class="even">
<td><p>連續複寫偵測到無效的記錄檔：在複製或重新顯示期間重新偵測不適當的記錄檔。</p></td>
<td><p>不適用。</p></td>
<td><p>捨棄記錄檔。</p></td>
<td><p>不適用。</p></td>
<td><p>失敗</p></td>
<td><p>捨棄無效的記錄檔；移動影響的記錄檔資料流。</p></td>
<td><p>不適用。</p></td>
</tr>
</tbody>
</table>


## 資料庫容錯移轉

當作用中的資料庫副本無法再保持作用中狀態時，就會發生資料庫容錯移轉。下列情況會在資料庫容錯移轉時發生：

1.  Microsoft Exchange 資訊儲存庫服務已偵測到資料庫失敗。

2.  Microsoft Exchange 資訊儲存庫服務寫入失敗事件到 crimson 頻道事件記錄檔。

3.  包含失敗資料庫之伺服器上的 Active Manager 偵測到失敗事件。

4.  Active Manager 從保留資料庫副本的其他伺服器要求資料庫副本狀態。

5.  其他伺服器對要求的 Active Manager 傳回所要求的資料庫副本狀態。

6.  PAM 使用最佳副本選取演算法，開始將主動資料庫移至 DAG 中的另一個伺服器。

7.  PAM 在叢集資料庫中更新資料庫裝載位置，以參照所選的伺服器。

8.  PAM 在所選伺服器上將要求傳送到 Active Manager，以成為資料庫主機。

9.  所選伺服器上的 Active Manager 要求 Microsoft Exchange 複寫服務嘗試從先前伺服器複製最後的記錄檔，並且為資料庫設定可裝載旗標。

10. Microsoft Exchange 複寫服務會從先前持有資料庫作用中副本的伺服器複製記錄檔。

11. Active Manager 會從叢集資料庫讀取最大的記錄檔產生號碼。

12. Microsoft Exchange 資訊儲存庫服務會裝載新的作用中資料庫副本。

## 伺服器容錯移轉

伺服器容錯移轉會在 DAG 成員無法再對 MAPI 網路提供服務，或當 DAG 成員上的叢集服務無法再聯繫其餘的 DAG 成員時發生。下列情況會在伺服器容錯移轉時發生：

1.  PAM 上的叢集服務會因為以下兩個條件之一而傳送通知給 PAM：
    
    1.  **節點關閉**   可以與伺服器連線，但無法參與 DAG 作業。
    
    2.  **MAPI 網路關閉**   無法透過 MAPI 網路聯繫伺服器，因此無法參與 DAG 作業。

2.  如果可以與伺服器連線，則 PAM 會聯繫受影響伺服器上的 Active Manager，並要求立即卸載所有資料庫。

3.  每個受影響的資料庫副本：
    
    1.  PAM 要求 DAG 中所有伺服器的資料庫副本狀態。
    
    2.  PAM 從所有可連線和作用中 DAG 成員收到回應。
    
    3.  藉由從每個回應程式查詢最近的記錄檔產生號碼，PAM 會嘗試在所有回應的伺服器之間判斷最佳記錄檔來源。
    
    4.  每一部伺服器會以記錄檔產生號碼回應。

4.  PAM 從叢集資料庫擷取目前搜尋索引類別目錄狀態。

5.  PAM 會根據每個資料庫副本的記錄檔產生號碼和類別目錄健康情況，來選取要啟動的最佳副本。

6.  PAM 更新叢集資料庫中資料庫的裝載位置。

7.  PAM 藉由在一或多部其他伺服器上與 Active Manager 通訊，來啟動資料庫容錯移轉。

8.  所選伺服器上的 Active Manager 要求 Microsoft Exchange 複寫服務嘗試從先前伺服器複製最後的記錄檔，並且設定可裝載旗標。

9.  當資料庫處於可裝載狀態時，伺服器上的 Active Manager 會裝載資料庫。

如需 Active Manager 之最佳副本選擇程序的詳細資訊，請參閱[Active Manager](active-manager-exchange-2013-help.md)。

## 資料中心容錯移轉

Exchange 2013 中有重大變更，可解決 Exchange 2010 站台恢復組態所面臨的挑戰。透過命名空間簡化、伺服器角色合併、區隔 Client Access Server 與 DAG 復原 (在 Exchange 2013 中，命名空間不需要隨 DAG 移動)，以及負載平衡相關的變更，Exchange 2013 提供新的站台恢復選項，例如能夠使用單一全域命名空間。此外，如果您有兩個以上的位置要部署郵件服務元件，則 Exchange 2013 也可讓您設定郵件服務，以便在失敗時自動容錯移轉，而這在 Exchange 2010 中需要手動介入。

Exchange 2013 中已簡化站台恢復的運作。Exchange 透過多重 IP 位址、負載平衡，運用命名空間內建的容錯能力 (如果需要的話，還能夠使伺服器開始運作和停止服務)。Exchange 2013 中最重大的變更之一是發揮用戶端的能力，以快取 DNS 伺服器為了回應名稱解析要求而傳回的多個 IP 位址。假設用戶端能夠快取多個 IP 位址 (幾乎所有 HTTP 用戶端都有此能力，而由於 Exchange 2013 中幾乎所有用戶端存取通訊協定都是以 HTTP 為基礎 (Outlook、Outlook Anywhere、EAS、EWS、OWA、EAC、RPS 等)，因此，所有支援的 HTTP 用戶端都能夠使用多個 IP 位址)，因此可在用戶端提供容錯移轉能力。您可以設定 DNS 以在名稱解析期間處理至用戶端的多重 IP 位址。例如，用戶端要求 mail.contoso.com 並送回兩個 IP 位址或四個 IP 位址。然而，用戶端取回的許多 IP 位址會由用戶端確實使用。這讓用戶端更有彈性，因為如果其中一個 IP 位址失敗，用戶端可以嘗試連接一或多個其他的 IP 位址。如果用戶端嘗試一個 IP 位址但失敗，則會等候大約 20 秒，然後嘗試清單中的下一個 IP 位址。因此，如果您與主要 CAS 陣列中斷連線，但有第二個 CAS 陣列已發佈的第二個 IP 位址，則會自動復原用戶端 (大約 21 秒)。

現代 HTTP 用戶端 (10 年以內的作業系統和網頁瀏覽器) 自動可使用此備援。HTTP 堆疊可接受 FQDN 的多個 IP 位址，如果嘗試的第一個 IP 嚴重失敗 (例如，無法連線)，則會嘗試清單中的下一個 IP。在輕微失敗的情況下 (工作階段建立之後中斷連線，或許是因為服務發生間歇性故障，例如，裝置丟棄封包而必須停止服務)，使用者可能需要重新整理其瀏覽器。

使用適當的組態，用戶端層級上可進行容錯移轉，而且用戶端會自動重新導向至具有運作中 Client Access Server 的第二個資料中心，這些運作中 Client Access Server 會將通訊移回到不受中斷影響的使用者信箱伺服器 (因為您沒有執行轉換)。不必設法復原服務，服務本身會自行復原，而您可以專注於修正核心問題 (例如，更換故障的負載平衡器)。

因為可以在資料中心之間容錯移轉命名空間，實現資料中心容錯移轉所需的只是 Mailbox role 跨資料中心的容錯移轉機制。若要為 DAG 自動容錯移轉，您只需要架構解決方案，讓 DAG 在兩個資料中心之間均分，然後在第三個位置放置見證伺服器，以便任何一個資料中心內的 DAG 成員均可仲裁，而不論含有 DAG 成員的資料中心之間的網路狀態為何。關鍵在於第三個位置與影響到 DAG 成員所在位置的網路失敗隔開。

如果您僅有兩個資料中心，但想要能夠設定自動容錯移轉，您可以使用 Microsoft Azure 為您的第三個位置。您必須建立 Azure 虛擬網路，並將其連線到您使用多點 VPN 的兩個資料中心。您再能夠見證伺服器放在 Microsoft Azure 虛擬機器上。如需詳細資訊，請參閱[使用 Microsoft Azure 虛擬機器為 DAG 見證伺服器](using-a-microsoft-azure-vm-as-a-dag-witness-server-exchange-2013-help.md)。

