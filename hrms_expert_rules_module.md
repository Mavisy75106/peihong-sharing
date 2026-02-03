# HRMS 專家級擴充方案：行政管控與自動化規則模組

> **設計者語：** 
> 作為一名資深人事系統開發專家，我認為一個卓越的 HRMS 不僅在於「紀錄」，更在於「規則的彈性」與「數據的互通」。本方案將針對管理者需求，導入高階的請假規則引擎與行事曆整合機制。

---

## 📅 1. 全方位行事曆同步 (Unified Calendar Integration)
為了消除資訊孤島，系統應具備自動化同步能力：
*   **iCal/ICS 支持**：允許管理者上傳或訂閱外部 ICS 連結（Google/Outlook），將公司公告之國定假日、補班日一鍵匯入。
*   **衝突偵測**：當員工請假時間與行事曆上的「關鍵專案里程碑」或「強制會議」衝突時，系統主動標註預警。
*   **API 雙向同步**：預留 OAuth 接口，未來可支持將核准後的假單自動寫入員工的個人工作行事曆。

## 📋 2. 請假規則引擎 (Advanced Leave Rule Engine)
管理者可透過 `admin_config.json` 或管理後台設定精細規則：

### A. 假別定義 (Custom Leave Types)
*   **特休 (Annual Leave)**：支持按資歷自動比例累計（例：滿半年 3 天，滿一年 7 天）。
*   **病假 (Sick Leave)**：設定「半薪/無薪」與「年度上限天數」。
*   **事假 (Personal Leave)**：可設定最小請假單位（例：1 小時、0.5 天）。
*   **補休 (Compensatory Leave)**：與出缺勤模組聯動，加班時數自動轉換為補休額度。

### B. 審核邏輯設定 (Approval Workflows)
*   **自動核准機制**：特定天數內（例：0.5 天以下）且假別為「病假」時，系統可設定為「自動核准並通知主管」。
*   **代理人制度**：強制要求請假時需填寫「職務代理人」，並自動發送確認訊息給代理人。

## 🔐 3. 數據模型設計 (Expert Data Schema)

### `data/leave_rules.json`
```json
{
  "leave_types": {
    "annual": {
      "accrual_policy": "seniority_based",
      "carry_over": true,
      "max_carry_over_days": 5
    },
    "sick": {
      "paid_ratio": 0.5,
      "unlimited": false,
      "max_days": 30
    }
  },
  "company_calendar": "urls_or_path_to_ics"
}
```

---

## 🛠️ 開發實作優先級 (Expert Recommendations)
1.  **Priority High**：建立 `rules_engine.py` 核心類別，負責計算員工剩餘天數與校核申請資格。
2.  **Priority Medium**：開發 `.ics` 檔解析器，支持管理者匯入年度假日。
3.  **Priority Low**：視覺化管理介面，讓非技術行政人員也能操作。

---
**專家結語：**
這套設計保留了極高的擴展性。透過將「規則」與「代碼」分離，管理者未來可以在不更動系統底層的前提下，因應勞基法修法或公司政策變動，靈活調整請假參數。
