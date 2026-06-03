## 1. Sản phẩm dùng: Vietnam Airlines's NEO
## 2. Promise vs Reality
- Product hứa sẽ hổ trợ về việc hành lý, thay đổi vé/hoàn tiền và thủ tục cho người dùng
- Mình kỳ vọng AI có thể xử lý việc đặt vé
- Khi dùng, AI không xử lý việc đặt vé mà chuyển hướng cho nhân viên
- AI trả lời không đúng về việc hỏi giá
- Khi user sửa lại câu trả lời thì AI vẫn giữ nguyên câu trả lời ban đầu.

<div style="display: flex; gap: 10px; flex-wrap: wrap;">
  <img src="01-invidual-workshop/images/promises.png" width="250" alt="Promises" />
  <img src="01-invidual-workshop/images/Image1.png" width="250" alt="Evidence1" />
  <img src="01-invidual-workshop/images/image2.png" width="250" alt="Evidence2" />
  <img src="01-invidual-workshop/images/image3.png" width="250" alt="Evidence3" />
</div>

## 3. 4 Paths

```mermaid
graph TD
    %% 1. Start point
    Start([User Input / Trigger]) --> Process(AI Evaluates Request)
    
    %% 2. Evaluation / Split
    Process --> Conf{Confidence & Accuracy?}
    
    %% 3. The 4 Paths
    Conf -->|High Confidence & Correct| Happy[Path 1: Happy Path - Accurate direct answer]
    
    Conf -->|Low Confidence / Missing Info| LowConf[Path 2: Low-Confidence Path - Ask clarifying questions / Show options]
    LowConf --> UserClarifies[User clarifies / selects category]
    UserClarifies --> Process
    
    Conf -->|Incorrect / Hallucination| Failure[Path 3: Failure Path - AI answers wrongly / fails task]
    Failure --> UXFallback[System UX Recovery - Show disclaimers / Transfer to human agent]
    
    UXFallback --> Correction[Path 4: Correction Path - User overrides or corrects data]
    Correction --> LogFeedback(System updates context & logs correction for learning)
    LogFeedback --> Process
    
    %% Styling the paths (Color-coded with black text)
    style Start fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style Process fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style Conf fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style UserClarifies fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style UXFallback fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style LogFeedback fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    
    style Happy fill:#d4edda,stroke:#28a745,stroke-width:2px,color:#000
    style LowConf fill:#fff3cd,stroke:#ffc107,stroke-width:2px,color:#000
    style Failure fill:#f8d7da,stroke:#dc3545,stroke-width:2px,color:#000
    style Correction fill:#d1ecf1,stroke:#17a2b8,stroke-width:2px,color:#000
```
### 4. Finding
```text
Khi user hỏi "Is flight from A to B expensive?",
AI hiểu như user muốn tìm vé máy bay đi từ A đến B thay vì tìm giá vé,
hậu quả là user không biết giá vé để đưa ra quyết định.
Lỗi thuộc Intent.
Nên sửa bằng Failure path: chuyển đến nhân viên tư vấn.
```

### As-is Flow (Current Broken Experience - VNA NEO Chatbot)

```mermaid
graph TD
    A[User asks: 'Is flight from A to B expensive?'] --> B(AI processes input)
    B --> C{AI Intent Classification}
    C -->|Incorrect Intent| D[AI maps to 'Search/Book flight' instead of 'General pricing inquiry']
    D --> E[AI shows flight options or instructions to search/book on website/app]
    E --> F[User does not get price information to make decision]
    E --> G[User corrects AI: 'No, I want to know if it is expensive']
    G --> H[AI maintains original wrong answer / repeats same instructions]
    
    style A fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style B fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style C fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style E fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style G fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    
    style D fill:#ffcccc,stroke:#dc3545,stroke-width:2px,color:#000
    style F fill:#ffcccc,stroke:#dc3545,stroke-width:2px,color:#000
    style H fill:#ffcccc,stroke:#dc3545,stroke-width:2px,color:#000
```



### To-be Flow (Proposed Solution - In-chat Booking & UX Recovery)


```mermaid
graph TD
    A[User: Is flight from A to B expensive?] --> B(AI processes input)
    B --> C{AI Intent Classification}
    C -->|Correct Intent| D[NEO asks: There are X, Y, Z flights going from A to B, would you like to purchase a ticket?]
    D --> E[User: Yes]
    E --> F(NEO Request user to provide flight date/time and other information)
    F --> G[User complete payment]
    G --> H[NEO Update booking status & send receipt]
    style A fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style B fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style C fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style E fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    style G fill:#f8f9fa,stroke:#343a40,stroke-width:2px,color:#000
    
    style D fill:#5bb450,stroke:#5bb450,stroke-width:2px,color:#000
    style F fill:#5bb450,stroke:#5bb450,stroke-width:2px,color:#000
    style H fill:#5bb450,stroke:#5bb450,stroke-width:2px,color:#000
```


