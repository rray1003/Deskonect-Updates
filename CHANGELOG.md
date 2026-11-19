# Deskonect 版本更新日誌

此檔自 **0.2.0-alpha.82** 起維護每次實際成功發布的內容。依 SemVer 分系列，**同一 minor 系列最新 Build 永遠置頂**；新的 minor／major 另開系列，舊系列不得刪除，移到下方或封存到 `docs/changelog/`（私人來源庫）並保留連結。每個版本固定按「新增／優化／除錯」分類，未做的項目明列「無」。只有兩端安裝檔皆完成發布並核對，才能列為「已發布」；未驗收的功能不得寫成已通過。

## 0.2.x 系列

### 0.2.0-alpha.87 — 2026-11-19

**狀態：已發布（Windows）；沿用本輪 Windows 優先發布授權，Android 保持原版。**

**新增**
- App 內使用 Google 憑證 HTTPS 回應時間建立登入驗證基準，不修改 Windows 時鐘。

**優化**
- 登入有效期及續期改用單調時鐘；Google 金鑰輪替時重新抓取並重驗一次。

**除錯**
- 修正本機時間不準導致新 Google／Firebase 憑證被拒絕；仍以 Google 時間嚴格檢查有效期並驗證簽章、issuer、audience。

**驗證與限制**
- 175 Python tests、Ruff、四個執行檔 smoke 通過；模擬本機 ±30 天，新 token 通過、過期及竄改 token 仍拒絕。真實 Firebase 續期於程序內模擬本機 ±1 天均通過。
- 上傳三個資產 SHA-256 相符，公開更新器可由 Build 86 讀到 87。新 Build 在第三台錯時鐘環境下的實機驗收仍待確認；TLS 無法建立可信連線時不會繞過安全檢查。
- 來源：`24c97d981b88fc48f15ad37ee748cffea7a5166e`。

[Windows 登入時間校正修正版](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.87)



### 0.2.0-alpha.86 — 2026-11-19

**狀態：已發布（依本次使用者指示僅 Windows）；Android 保留 Build 84。**

**新增**
- 無。

**優化**
- Windows 更新期間暫停背景代理 watchdog，等待代理生命週期 mutex 釋放後才執行安裝；逾時取消並恢復原程式。

**除錯**
- 修正舊版 headless 攝影機代理無法被安裝器正常關閉，造成靜默安裝中止、重新啟動後重複要求更新的問題；安裝器僅針對 Deskonect 執行檔關閉占用，成功後恢復代理。
- 包含 main 已合併的 Google 憑證傳輸備援與錯誤分類修正，避免把無效憑證一律誤報為網路問題；詳見 docs/GOOGLE_LOGIN_NETWORK_REPAIR.md。

**驗證與限制**
- 合併最新 main 後本機 170 項 Python 測試、Ruff、GUI／CLI／Agent／獨立更新助手 smoke、兩種 Windows Setup 建置通過；三個公開資產 SHA-256 均與本機相符。
- 公開 latest 已核對為 Build 86；舊更新器以 Build 83／85 檢查可取得 86，以 Build 86 檢查無更新。此為真實更新 API 驗證，不代表已完成安裝。
- Windows 實際安裝目前仍為 Build 83；修正後端到端升級、另一台電腦登入、真實手機與跨網相機未驗收。未聲稱已修復所有設備問題。
- Android v2 發行金鑰密碼目前不可用，依使用者明確選擇只發布 Windows；沒有換金鑰或把舊 APK 標為新 Build。
- 安裝檔來源：`e768e44d1236163fd10e0d190c345efa54a092cb`。

[Windows 更新修正版](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.86)

### 0.2.0-alpha.85 — 2026-11-19

**狀態：已發布（本次使用者明確同意僅 Windows）；Android 保留 Build 84。**

**新增**
- Google／Firebase 公開驗證憑證下載的系統 HTTPS 備援與有限重試。

**優化**
- 尊重伺服器有效期快取公開憑證，不使用過期快取。

**除錯**
- 修正無效簽章被誤報為 G-NETWORK；回傳網頁等待 Windows Google 身分驗證結果，失敗不回報成功。

**驗證與限制**
- 本機 167 Python tests、Ruff、GUI／CLI／Agent／更新助手 smoke 通過；三個上傳資產 SHA-256 相符，公開更新器從 Build 84 可讀到 Build 85。
- 第三台 Windows 真實 Google 登入、完整更新安裝與受管理網路尚待驗收；斷網不會繞過驗證。Windows Setup 尚無 Authenticode 簽章。
- 原始碼：`ca472bc9c8f72eee8106c80615a6d659b63cbae0`。

[Windows 修正版](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.85)

### 0.2.0-alpha.82 — 2026-11-19

**狀態：已發布（Alpha）；功能實機驗收未全數完成。**

**新增**
- Windows x64 與 Android v2 的 Build 82 安裝檔整合於同一個公開 Release，並保留舊 Android 更新器使用的相容入口（沿用 Build 81 起的發布設計）。

**優化**
- Windows 提供輕量網路安裝程式、完整的離線 App 內更新包與 SHA-256 驗證；公開舊版資產已先完成中繼資料歸檔後下架。
- Android v2 模擬器完成 Build 81 → 82 的 App 內下載、系統安裝及重新啟動驗證。

**除錯**
- 修復 Windows 線上安裝時只解壓最外層、導致舊版 `_internal/assets/build-info.json` 仍留在安裝目錄的問題；Build 82 本機安裝已核對新版本戳記。

**驗證與限制**
- Python 123 tests、Flutter 21 tests、Ruff、Flutter analyze 通過；Windows 本機安裝與 Android 36 模擬器測試通過。
- 真實手機、另一台 Windows、完整跨網控制與相機、Google 登入、Windows Build 82 → 後續 Build 的端到端自動更新仍未在本輪全部驗收。
- Android 舊 v1 package 的簽章與 v2 不同，不能原地覆蓋更新；Windows 安裝程式尚未有 Authenticode 簽章。

[共同下載與驗證報告](https://github.com/rray1003/Deskonect-Updates/releases/tag/v0.2.0-preview.82) · [Build 82 驗證資料（私人來源庫）](https://github.com/rray1003/Deskonect/blob/main/docs/ALPHA82_RELEASE_REPORT.md)

---

## 歷史發布（0.2.0-alpha.82 之前）

舊版本的 Release 已按要求從公開下載頁撤下，tag 仍保留。2026-11-19 清理前的 15 筆 Release 文字、資產名稱、雜湊、日期已記錄於私人來源庫的 [舊版歸檔](https://github.com/rray1003/Deskonect/blob/main/docs/RELEASE_ARCHIVE_PRE_ALPHA82.json)。這些歷史資料不是逐版人工整理的「新增／優化／除錯」日誌，因此不冒充已完整補記。

## 後續版本撰寫規則

每次成功發布後，在目前 minor 系列**最上方**加入 `### x.y.z-channel.Build — YYYY-MM-DD`，依序撰寫「新增／優化／除錯／驗證與限制」，附 Release 連結與來源提交。新的 minor／major 開新系列並保留過去系列與每筆 Build；不得因公開 Release 檔案下架而抹除文字紀錄。若發布失敗，保留為草稿而非「已發布」。
