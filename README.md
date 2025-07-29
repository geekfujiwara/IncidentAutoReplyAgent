# IncidentAutoReplyAgent
顧客からのメールでの不具合報告に対して保証判定やメール返信などの支援を行うエージェント



## 📊 シーケンス図

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Trigger as 不具合報告トリガー
    participant Parent as PC不具合対応エージェント
    participant Child1 as PC不具合対策検討エージェント
    participant Child2 as 保証判定エージェント
    participant ToolOK as 不具合対策案のメール返信
    participant ToolNG as 保証期間外メール
    participant ToolLog as 対応履歴記録ツール

    User->>Trigger: 不具合報告メール送信
    Trigger->>Parent: 起動通知
    Parent->>Child1: 対策案検討依頼
    Child1-->>Parent: 対策案
    Parent->>Child2: 保証判定依頼
    Child2-->>Parent: 判定結果
    alt 保証期間内
        Parent->>ToolOK: 対策案メール送信
    else 保証期間外
        Parent->>ToolNG: 対応不可メール送信
    end
    Parent->>ToolLog: 対応履歴を記録
