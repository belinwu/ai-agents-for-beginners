# 主動式 RAG

本課程為你提供一個全面的介紹，了解主動式檢索增強生成（Agentic RAG），這是一種新興的人工智能模式，讓大型語言模型（LLMs）能自主規劃下一步行動，同時從外部數據源獲取資訊。與傳統的靜態檢索-閱讀模式不同，主動式 RAG 涉及到多次迭代地調用 LLM，穿插使用工具或函數調用，並生成結構化的輸出。系統會評估結果，改進查詢，必要時調用額外工具，並重複這個過程，直到找到滿意的解決方案為止。

## 課程介紹

本課程將涵蓋以下內容：

- **理解主動式 RAG：** 學習這種新興的 AI 模式，讓大型語言模型（LLMs）能自主規劃下一步行動，同時從外部數據源中提取資訊。
- **掌握迭代式 Maker-Checker 模式：** 理解這種迭代調用 LLM 的循環過程，穿插工具或函數調用和結構化輸出，旨在提升正確性並處理不良查詢。
- **探索實際應用：** 確定主動式 RAG 的適用場景，例如以正確性為優先的環境、複雜的數據庫交互以及長期工作流程。

## 學習目標

完成本課程後，你將了解以下內容：

- **理解主動式 RAG：** 學習這種新興的 AI 模式，讓大型語言模型（LLMs）能自主規劃下一步行動，同時從外部數據源中提取資訊。
- **迭代式 Maker-Checker 模式：** 掌握這種迭代調用 LLM 的概念，穿插工具或函數調用和結構化輸出，旨在提升正確性並處理不良查詢。
- **掌控推理過程：** 理解系統如何自主掌控推理過程，決定如何處理問題，而不是依賴預定的路徑。
- **工作流程：** 理解主動模型如何自主決定檢索市場趨勢報告、識別競爭對手數據、關聯內部銷售數據、綜合結論並評估策略。
- **迭代循環、工具整合與記憶：** 學習系統如何依賴迴圈交互模式，跨步驟維持狀態和記憶，避免重複循環並做出更明智的決策。
- **處理失敗模式與自我糾正：** 探索系統的自我糾正機制，包括迭代與重新查詢、使用診斷工具以及在必要時依賴人工監督。
- **代理邊界：** 理解主動式 RAG 的限制，聚焦於特定領域的自主性、基礎設施依賴性以及遵守安全規範。
- **實際使用案例與價值：** 確定主動式 RAG 的適用場景，例如以正確性為優先的環境、複雜的數據庫交互以及長期工作流程。
- **治理、透明性與信任：** 學習治理與透明性的必要性，包括可解釋的推理、偏見控制以及人工監督。

## 什麼是主動式 RAG？

主動式檢索增強生成（Agentic RAG）是一種新興的 AI 模式，讓大型語言模型（LLMs）能自主規劃下一步行動，同時從外部數據源中提取資訊。與靜態的檢索-閱讀模式不同，主動式 RAG 涉及到多次迭代地調用 LLM，穿插工具或函數調用和結構化輸出。系統會評估結果，改進查詢，必要時調用額外工具，並重複這個過程，直到找到滿意的解決方案為止。

這種迭代的 “Maker-Checker” 操作方式提升了正確性，能處理結構化數據庫（例如 NL2SQL）中的不良查詢，並確保高質量的結果。系統主動掌控其推理過程，能重寫失敗的查詢，選擇不同的檢索方法，並整合多種工具，例如 Azure AI Search 的向量檢索、SQL 數據庫或自定義 API，在最終給出答案之前。主動式系統的顯著特點是它能掌控推理過程。傳統的 RAG 實現依賴於預定的路徑，而主動式系統能自主根據所獲得的資訊質量來決定步驟順序。

## 定義主動式檢索增強生成（Agentic RAG）

主動式檢索增強生成（Agentic RAG）是一種新興的 AI 開發模式，讓 LLMs 不僅能從外部數據源中提取資訊，還能自主規劃下一步行動。與靜態的檢索-閱讀模式或精心設計的提示序列不同，主動式 RAG 涉及到一個迭代循環，調用 LLM，穿插工具或函數調用和結構化輸出。每一步，系統都會評估已獲得的結果，決定是否需要改進查詢，必要時調用額外工具，並持續這個循環，直到找到滿意的解決方案。

這種迭代的 “Maker-Checker” 操作方式旨在提升正確性，處理結構化數據庫（例如 NL2SQL）中的不良查詢，並確保平衡且高質量的結果。系統不僅依賴於精心設計的提示鏈，還能主動掌控推理過程。它能重寫失敗的查詢，選擇不同的檢索方法，並整合多種工具，例如 Azure AI Search 的向量檢索、SQL 數據庫或自定義 API，在最終給出答案之前。這消除了對過於複雜的編排框架的需求。相反，一個相對簡單的循環 “LLM 調用 → 工具使用 → LLM 調用 → …” 就能產生精細且有根據的輸出。

![Agentic RAG 核心循環](../../../translated_images/agentic-rag-core-loop.2224925a913fb3439f518bda61a40096ddf6aa432a11c9b5bba8d0d625e47b79.hk.png)

## 掌控推理過程

讓系統成為 “主動式” 的關鍵特質在於它能掌控推理過程。傳統的 RAG 實現通常依賴於人類預先定義的模型路徑：一條明確的思路，指導模型應該檢索什麼以及何時檢索。但當系統真正實現主動性時，它能內部決定如何處理問題。這不僅僅是執行一個腳本，而是自主根據所找到的資訊質量決定步驟順序。

例如，如果被要求制定一個產品上市策略，主動式模型不會僅僅依賴於一個明確指出整個研究和決策流程的提示。相反，主動模型會自主決定：

1. 使用 Bing Web Grounding 檢索當前市場趨勢報告。
2. 使用 Azure AI Search 識別相關的競爭對手數據。
3. 使用 Azure SQL Database 關聯歷史內部銷售數據。
4. 通過 Azure OpenAI Service 將發現綜合成一個連貫的策略。
5. 評估該策略是否存在差距或不一致之處，必要時啟動另一輪檢索。

所有這些步驟——改進查詢、選擇來源、迭代直到對答案“滿意”——都是由模型決定的，而不是由人類預先編寫的腳本。

## 迭代循環、工具整合與記憶

![工具整合架構](../../../translated_images/tool-integration.7b05a923e3278bf1fd2972faa228fb2ac725f166ed084362b031a24bffd26287.hk.png)

主動式系統依賴於一個迴圈交互模式：

- **初始調用：** 用戶的目標（即用戶提示）被呈現給 LLM。
- **工具調用：** 如果模型發現資訊不足或指令不明確，它會選擇一個工具或檢索方法，例如向量數據庫查詢（如 Azure AI Search 的混合檢索私有數據）或結構化 SQL 調用，以獲取更多上下文。
- **評估與改進：** 在審查返回的數據後，模型會決定該資訊是否足夠。如果不夠，它會改進查詢，嘗試不同的工具或調整方法。
- **重複直到滿意：** 這個循環會持續進行，直到模型認為它有足夠的清晰度和證據來提供最終的、經過深思熟慮的回應。
- **記憶與狀態：** 系統會在各步驟間維持狀態和記憶，因此能回憶起先前的嘗試及其結果，避免重複循環並在過程中做出更明智的決策。

隨著時間的推移，這種方式會創造一種不斷演進的理解，讓模型能在沒有人工持續干預或重塑提示的情況下，完成複雜的多步驟任務。

## 處理失敗模式與自我糾正

主動式 RAG 的自主性還包含了強大的自我糾正機制。當系統遇到瓶頸（例如檢索到不相關的文件或遇到不良查詢）時，它可以：

- **迭代與重新查詢：** 與其返回低價值的回應，模型會嘗試新的搜索策略，重寫數據庫查詢或尋找替代數據集。
- **使用診斷工具：** 系統可能調用額外的功能來幫助其調試推理步驟或確認檢索數據的正確性。像 Azure AI Tracing 這樣的工具對於實現強大的可觀測性和監控至關重要。
- **依賴人工監督：** 對於高風險或持續失敗的場景，模型可能會標記不確定性並請求人工指導。一旦人類提供了糾正意見，模型可以將其納入以後的學習。

這種迭代和動態的方法讓模型能不斷改進，確保它不僅僅是一個一次性的系統，而是一個在特定會話期間從錯誤中學習的系統。

![自我糾正機制](../../../translated_images/self-correction.3d42c31baf4a476bb89313cec58efb196b0e97959c04d7439cc23d27ef1242ac.hk.png)

## 主動性邊界

儘管在任務內部具備自主性，主動式 RAG 並不等同於通用人工智能。它的“主動性”能力局限於人類開發者提供的工具、數據源和政策。它無法創建自己的工具，也無法超出設定的領域範圍。相反，它在動態協調現有資源方面表現出色。

主要與更高級 AI 形式的區別包括：

1. **領域特定的自主性：** 主動式 RAG 系統專注於在已知領域內實現用戶定義的目標，通過查詢重寫或工具選擇等策略來改善結果。
2. **依賴基礎設施：** 系統的能力取決於開發者整合的工具和數據。沒有人工干預，它無法超越這些邊界。
3. **遵守安全規範：** 道德指導方針、合規規則和商業政策仍然非常重要。代理的自由度始終受到安全措施和監督機制的約束（希望如此）。

## 實際使用案例與價值

主動式 RAG 在需要迭代改進和精準度的場景中表現出色：

1. **正確性優先的環境：** 在合規檢查、法規分析或法律研究中，主動模型可以反覆驗證事實，查閱多個來源，並重寫查詢，直到產生經過徹底驗證的答案。
2. **複雜的數據庫交互：** 在處理結構化數據時，查詢可能經常失敗或需要調整，系統可以自主使用 Azure SQL 或 Microsoft Fabric OneLake 改進查詢，確保最終檢索符合用戶意圖。
3. **長期工作流程：** 當新的資訊不斷出現時，長期會話可能會發生變化。主動式 RAG 可以持續整合新數據，隨著對問題空間的理解加深而調整策略。

## 治理、透明性與信任

隨著這些系統在推理方面變得越來越自主，治理和透明性至關重要：

- **可解釋的推理：** 模型可以提供查詢記錄、所查閱的來源以及其得出結論的推理步驟。像 Azure AI Content Safety 和 Azure AI Tracing / GenAIOps 這樣的工具可以幫助保持透明性並降低風險。
- **偏見控制與平衡檢索：** 開發者可以調整檢索策略，確保考慮到平衡且具有代表性的數據來源，並定期審核輸出以檢測偏見或不均衡模式，使用 Azure Machine Learning 為進階數據科學組織提供自定義模型。
- **人工監督與合規性：** 對於敏感任務，人工審查仍然是必要的。主動式 RAG 並不會取代高風險決策中的人類判斷，而是通過提供經過更徹底驗證的選項來輔助人類。

擁有能提供清晰行動記錄的工具至關重要。沒有這些工具，調試多步驟過程可能會非常困難。以下是 Literal AI（Chainlit 背後的公司）提供的 Agent 運行範例：

![AgentRunExample](../../../translated_images/AgentRunExample.27e2df23ad898772d1b3e7a3e3cd4615378e10dfda87ae8f06b4748bf8eea97d.hk.png)

![AgentRunExample2](../../../translated_images/AgentRunExample2.c0e8c78b1f2540a641515e60035abcc6a9c5e3688bae143eb6c559dd37cdee9f.hk.png)

## 結論

主動式 RAG 代表了 AI 系統在處理複雜且數據密集型任務方面的一種自然進化。通過採用迴圈交互模式，自主選擇工具，並改進查詢直到達成高質量結果，系統超越了靜態的提示執行，成為更具適應性和上下文感知的決策者。儘管仍受限於人類定義的基礎設施和道德指導方針，但這些主動性能力使企業和最終用戶能夠享受到更豐富、更動態、也更有用的 AI 互動。

## 其他資源

- 使用 Azure OpenAI Service 實現檢索增強生成（RAG）：學習如何使用自己的數據與 Azure OpenAI Service。[這個 Microsoft Learn 模組提供了實現 RAG 的全面指南](https://learn.microsoft.com/training/modules/use-own-data-azure-openai)
- 使用 Azure AI Foundry 評估生成式 AI 應用：本文涵蓋了基於公開數據集的模型評估與比較，包括[主動式 AI 應用和 RAG 架構](https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai)
- [什麼是主動式 RAG | Weaviate](https://weaviate.io/blog/what-is-agentic-rag)
- [主動式 RAG：一份關於基於代理的檢索增強生成的完整指南 – generation RAG 的新聞](https://ragaboutit.com/agentic-rag-a-complete-guide-to-agent-based-retrieval-augmented-generation/)
- [主動式 RAG：使用查詢重構和自查詢為你的 RAG 提速！ Hugging Face 開源 AI 手冊](https://huggingface.co/learn/cookbook/agent_rag)
- [為 RAG 添加主動層](https://youtu.be/aQ4yQXeB1Ss?si=2HUqBzHoeB5tR04U)
- [知識助理的未來：Jerry Liu](https://www.youtube.com/watch?v=zeAyuLc_f3Q&t=244s)
- [如何構建主動式 RAG 系統](https://www.youtube.com/watch?v=AOSjiXP1jmQ)
- [使用 Azure AI Foundry Agent Service 擴展你的 AI 代理](https://ignite.microsoft.com/sessions/BRK102?source=sessions)

### 學術論文

- [2303.17651 Self-Refine: Iterative Refinement with Self-Feedback](https://arxiv.org/abs/2303.17651)
- [2303.11366 Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/

**免責聲明**:  
此文件使用機器人工智能翻譯服務進行翻譯。雖然我們致力於提供準確的翻譯，但請注意，自動翻譯可能包含錯誤或不準確之處。應以原文作為權威來源。對於關鍵信息，建議尋求專業人工翻譯。我們對因使用此翻譯而引起的任何誤解或錯誤解釋概不負責。