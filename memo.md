# Memo Teardown — CURSOR (Anysphere)

**Nhóm:** cá nhân · **Thành viên:** Nguyễn Minh Quang (2A202601730) · **Ngày:** 14/08/2026

**Vì sao chọn sản phẩm này:** Cursor là ca hiếm cho phép quan sát trọn một vòng đời "wrapper AI" chỉ trong ba năm — từ 4 người fork VS Code (2023), đến tự huấn luyện model nền của riêng mình (2025–2026), rồi bị chính lớp hạ tầng mua lại với giá 60 tỷ USD (2026). Mọi nguyên lý học trong buổi đều có một mốc thật để đối chiếu, thay vì phải suy đoán.

---

## §1. Timeline các cập nhật lớn

| Thời điểm | Cập nhật | Context lúc đó | Nguyên lý |
|---|---|---|---|
| **3/2023** | Ra mắt Cursor: **fork toàn bộ VS Code** thay vì viết một extension. Chat và sửa đa file được nhúng thẳng vào lõi editor. ([Wikipedia](https://en.wikipedia.org/wiki/Cursor_(code_editor)), [Contrary Research](https://research.contrary.com/company/cursor)) | GPT-4 vừa ra (3/2023). GitHub Copilot đang thống trị nhưng chỉ là extension autocomplete từng dòng, bị giới hạn bởi API của VS Code. Anysphere (thành lập 2022) vừa pivot khỏi mảng AI cho cơ khí. | **x10, không phải x1.2.** Muốn hơn Copilot 10 lần thì không thể ngồi trong khung extension do Microsoft định nghĩa — phải chiếm lớp trải nghiệm. Fork editor là cái giá phải trả để gỡ trần. |
| **11/2024** | **Mua đứt Supermaven** (startup autocomplete) và gọi vốn ở định giá 2,5 tỷ USD. Năng lực này trở thành tính năng "Tab" — dự đoán cả khối chỉnh sửa, không chỉ dòng kế tiếp. ([Wikipedia](https://en.wikipedia.org/wiki/Cursor_(code_editor))) | Copilot vẫn dẫn về thị phần; khác biệt kỹ thuật duy nhất Cursor có lúc đó là tốc độ/độ chính xác của gợi ý. Toàn bộ phần "thông minh" còn lại vẫn là API của OpenAI/Anthropic. | **Wrapper → moat.** Bước đầu tiên thoát kiếp wrapper: sở hữu ít nhất một model của riêng mình ở chỗ nào chạm user nhiều nhất. Tab tạo ra dữ liệu chỉnh sửa độc quyền — thứ không mua được bằng API. |
| **4/6/2025** | **Cursor 1.0**: Bugbot (tự review PR), Background Agent (agent chạy nền trên máy cloud) mở cho mọi user, cài MCP một cú click, Memories. ([Changelog 1.0](https://cursor.com/changelog/1-0)) | ARR nhảy từ 100 triệu USD (1/2025) lên 500 triệu USD (5/2025). Series C 900 triệu USD ở định giá 9,9 tỷ đóng ngày 5/6/2025. Claude Code và Devin đang định nghĩa lại kỳ vọng về "agent". | **Đổi định nghĩa "tốt".** "Tốt" thôi không còn là *gợi ý đúng dòng tiếp theo* mà là *hoàn thành xong việc khi tôi không ngồi trước máy*. Định nghĩa mới này kéo theo toàn bộ kiến trúc sản phẩm phía sau. |
| **6–7/2025** | **Đổi pricing**: bỏ hạn mức 500 request/tháng của gói Pro, chuyển sang pool tín dụng 20 USD tính theo compute thực dùng. Cộng đồng phản ứng dữ dội → CEO xin lỗi công khai và hoàn tiền (4/7/2025). ([FinTech Weekly](https://www.fintechweekly.com/magazine/articles/cursor-pricing-change-user-backlash-refund), [Finout](https://www.finout.io/blog/what-happened-to-cursor-pricing-2026-guide-5-cost-cutting-tips)) | Background Agent vừa mở đại trà một tháng trước. Một user chạy agent nền cả ngày tốn compute gấp hàng chục lần user chỉ dùng chat. | **Kinh tế đơn vị của Vertical AI.** Khi giá trị chuyển từ "tính năng" sang "compute tiêu thụ", COGS thành biến phí — mô hình SaaS phẳng vỡ. Đây là hệ quả tất yếu của quyết định ở mốc trên, không phải tai nạn rời rạc. |
| **29/10/2025** | **Cursor 2.0 + Composer**: model coding tự huấn luyện đầu tiên (nhanh ~4x model cùng tầm trí tuệ), chạy tối đa 8 agent song song trên các git worktree tách biệt, browser tool để agent tự kiểm chứng kết quả. ([Cursor blog 2.0](https://cursor.com/blog/2-0), [Changelog 2.0](https://cursor.com/changelog/2-0)) | Cursor vẫn phụ thuộc hoàn toàn model của OpenAI/Anthropic/Google — vừa là chi phí lớn nhất, vừa là rủi ro chiến lược khi chính các nhà cung cấp đó ra sản phẩm coding riêng. | **Vòng lặp học.** Composer được huấn luyện bằng RL trên chính hành vi và codebase thật trong Cursor. Moat thật không nằm ở UI (fork được trong một tuần) mà ở vòng lặp *dùng → dữ liệu → model tốt hơn → dùng nhiều hơn*. |
| **19/12/2025** | **Mua Graphite** — nền tảng code review (hơn 500 công ty: Shopify, Snowflake, Figma), giá "cao hơn nhiều" định giá 290 triệu USD trước đó. ([Cursor blog](https://cursor.com/blog/graphite), [Fortune](https://fortune.com/2025/12/19/cursor-ai-coding-startup-graphite-competition-heats-up/)) | AI đã làm khâu *viết* code nhanh gấp bội, nhưng khâu *review* vẫn y nguyên — nút thắt của tổ chức kỹ thuật dịch sang đó. | **x10 dịch nút thắt → Vertical AI.** Khi bạn tăng tốc một công đoạn 10 lần, nghẽn chỉ chuyển chỗ chứ không biến mất. Muốn giữ được giá trị đã tạo ra thì phải nuốt nốt công đoạn kế tiếp trong workflow. |
| **19/3/2026** | **Composer 2**: model frontier tự phát triển, cửa sổ 200K token, kiến trúc MoE, giá 0,50 USD/1M token input. Cursor công bố điểm trên **CursorBench** — bộ đo của riêng họ. ([SiliconANGLE](https://siliconangle.com/2026/03/19/vibe-coding-startup-cursor-launches-programming-optimized-composer-2-model/), [VentureBeat](https://venturebeat.com/technology/cursors-new-coding-model-composer-2-is-here-it-beats-claude-opus-4-6-but)) | Claude Opus 4.6 và GPT-5.4 đang là chuẩn frontier. Chạy đua benchmark chung thì Cursor không thắng nổi các phòng lab nghìn tỷ. | **Tự định nghĩa "tốt".** Không đua trên thước đo của người khác, mà công bố thước đo dựng từ task thật của user mình. Ai định nghĩa được "tốt" thì kiểm soát được sân chơi — và kiểm soát luôn biên lợi nhuận. |
| **16/6/2026 → 8/2026** | **SpaceX thực thi quyền mua Anysphere giá 60 tỷ USD** (all-stock, dự kiến đóng Q3/2026 — *chưa hoàn tất*). Hệ quả sản phẩm ngay sau đó: **Cursor Router** (22/7, tự định tuyến model theo 3 chế độ Cost/Balance/Intelligence), **Cursor Start** ₹649/tháng riêng cho Ấn Độ chạy **Grok 4.5** + Composer (28/7), plugin Google Workspace (3/8). ([CNBC](https://www.cnbc.com/2026/06/16/spacex-spcx-cursor-acquisition-ipo.html), [Forbes](https://www.forbes.com/sites/sandycarter/2026/06/16/spacex-buys-cursor-in-largest-startup-acquisition-ever-at-60-billion/), [Changelog](https://cursor.com/changelog), [Cursor Start](https://cursor.com/blog/cursor-start-india)) | SpaceX đã sáp nhập xAI và vừa IPO. Cursor ~2,6–4 tỷ USD doanh thu annualized, enterprise chiếm ~60%. Ấn Độ là thị trường lớn thứ ba với hơn 3 triệu dev. | **Wrapper thắng cuộc thì bị hạ tầng mua lại.** Lớp ứng dụng tiêu thụ compute nhiều nhất là mục tiêu thâu tóm của lớp sở hữu compute. Grok 4.5 xuất hiện trong Cursor chỉ 6 tuần sau đó cho thấy moat cuối cùng của ngành này là **compute + phân phối**, không phải tính năng. |

### Vì sao chọn những mốc này (và loại mốc nào)

Tiêu chí duy nhất: **mốc đó có làm đổi câu trả lời cho "ai trả tiền, trả cho cái gì, và tốt nghĩa là gì" không.** Vì vậy tôi loại:

- **Các vòng gọi vốn đứng một mình** (seed 8 triệu USD do OpenAI Startup Fund dẫn 10/2023; Series D 2,3 tỷ USD ở định giá 29,3 tỷ 13/11/2025). Tiền là *điều kiện* để ra quyết định, không phải quyết định. Tôi chỉ giữ vốn khi nó đi kèm nước đi sản phẩm (11/2024 gắn với Supermaven, 6/2025 gắn với Cursor 1.0).
- **Sự cố chatbot hỗ trợ "Sam" bịa ra chính sách đăng nhập (4/2025)** — sự cố vận hành gây mất user, nhưng không ai ở Anysphere *chọn* nó. Không phải quyết định sản phẩm.
- **Hỗ trợ iPad (29/7/2026) và Cloud Agents Builds boot nhanh 3x (13/8/2026)** — đúng nghĩa "bản vá lời": mở rộng bề mặt và cải thiện hiệu năng, nhưng không đổi tệp trả tiền, không đổi JTBD, không đổi định nghĩa "tốt".
- **Acqui-hire nhân sự từ Koala (7/2025)** — tuyển người, không đổi sản phẩm.

---

## §2. Tệp user & JTBD

| | **Early adopters (2023 – giữa 2024)** | **Tệp hiện tại (2025 – 8/2026)** |
|---|---|---|
| **Đặc điểm** | Kỹ sư full-stack ở startup 5–30 người (đậm đặc trong hệ YC/SF), đã sống trong VS Code hằng ngày, **đã từng trả 10 USD/tháng cho Copilot nên không cần thuyết phục rằng AI-code đáng tiền**, theo dõi AI Twitter/X và Hacker News, sẵn sàng đổi editor trong một buổi tối. Tự quyết chi tiêu bằng thẻ cá nhân, không qua ai duyệt. Tìm họ ở: Show HN, r/cursor, X, Discord của Cursor. | **Hai lớp.** (1) *Người mua doanh nghiệp*: VP Engineering / Head of Platform ở tổ chức 500–5.000 kỹ sư — hơn 50.000 team, ~64% Fortune 500, enterprise chiếm ~60% doanh thu; người **quyết định mua không phải người gõ code**, và họ cần SSO, audit log, trần chi tiêu, báo cáo ROI. (2) *Lớp giá thấp mới (từ 7/2026)*: dev Ấn Độ trả ₹649/tháng (~7,4 USD) — thị trường lớn thứ ba, hơn 3 triệu dev, thanh toán qua UPI, chạy Grok 4.5 + Composer thay vì model frontier. |
| **JTBD chính** | *"Khi tôi phải ship một feature trước sprint review mà nó đụng 6 file rải rác trong repo tôi chưa thuộc, tôi muốn mô tả thay đổi bằng tiếng Anh và nhìn thấy diff ngay trong editor — để tôi không mất hai tiếng đọc code rồi copy-paste qua lại tab ChatGPT."* | *(Doanh nghiệp)* **"Khi throughput kỹ thuật phải tăng mà headcount bị đóng băng, tôi muốn một nền tảng agent chuẩn hoá toàn tổ chức có kiểm soát chi tiêu và dấu vết kiểm toán — để tôi bảo vệ được ngân sách trước CFO và bảo vệ được source code trước phòng bảo mật."**<br>*(Lớp giá thấp)* "Khi tôi muốn giao việc cho agent cả ngày mà 20 USD/tháng bằng cả ngày công, tôi muốn một gói trả bằng nội tệ đủ để chạy agent thật." |
| **Trước đó họ làm bằng cách nào** | VS Code + Copilot lo autocomplete, còn việc "nghĩ" thì alt-tab sang ChatGPT web: dán từng file vào, chép câu trả lời ra, tự ghép lại. Refactor đa file làm tay bằng find-and-replace. | *(Doanh nghiệp)* Mua Copilot Business theo seat, ký kèm hợp đồng GitHub/Microsoft có sẵn nên không cần qua vòng procurement mới. Song song đó là **shadow IT**: kỹ sư tự cài Cursor và trả bằng thẻ cá nhân — chính luồng này tạo ra sóng inbound khiến Anysphere phải lập đội enterprise sales đầu 2025.<br>*(Lớp giá thấp)* Dùng gói Free đến hết hạn mức, hoặc dùng chung tài khoản, hoặc quay về Copilot/Gemini free tier. |

### Dịch chuyển tệp: mốc nào ở §1 gây ra?

Chủ yếu là **mốc 4/6/2025 (Cursor 1.0 — Background Agent + Bugbot)**, được khoá chặt bởi **29/10/2025 (Cursor 2.0, 8 agent song song)** và **19/12/2025 (mua Graphite)**.

Cơ chế: chừng nào AI còn *gõ hộ* thì đơn vị mua tự nhiên là **một cá nhân** — người dùng chính là người trả tiền. Từ lúc AI **chạy nền trên máy cloud và tự mở PR**, ba thứ mới xuất hiện cùng lúc: hoá đơn compute biến động (cần ngân sách phòng ban), code rời khỏi máy cá nhân (cần phê duyệt bảo mật), và output phải qua review của người khác (cần quy trình tổ chức). Cả ba đều là bài toán của tổ chức, nên **đơn vị mua buộc phải chuyển từ cá nhân sang team**. Graphite là mảnh cuối: review vốn dĩ là quy trình tập thể, không phải hành vi cá nhân. Mốc pricing 6–7/2025 vừa là hệ quả vừa là chất xúc tác — giá theo usage chỉ chấp nhận được với người có ngân sách phòng ban, và chính nó đẩy nhanh việc rời bỏ của một phần tệp cá nhân.

Còn mốc **28/7/2026 (Cursor Start)** thì mở ngược trở lại xuống dưới: sau khi lên hết enterprise, trục phân tầng tiếp theo không phải nghề nghiệp mà là **khả năng chi trả theo địa lý**.

### Switching cost — map 4 forces

| Lực | Nội dung (tệp hiện tại, 2026) |
|---|---|
| **Push** (đẩy khỏi hiện trạng) | Áp lực chứng minh ROI của AI trước ban lãnh đạo; đối thủ ship nhanh hơn nhờ agent; nguy cơ mất kỹ sư giỏi nếu bắt họ dùng công cụ đời cũ; Copilot bị coi là "chỉ autocomplete". |
| **Pull** (hút sang Cursor) | Chất lượng Tab + Composer 2; chạy 8 agent song song; end-to-end viết → review (Bugbot/Graphite); social proof 64% Fortune 500; Router hạ chi phí 30–50%. |
| **Anxiety** (lo ngại khi đổi) | Rò rỉ IP/source code; **bất định hậu thâu tóm — SpaceX chưa đóng deal (dự kiến Q3/2026) và chưa có track record phần mềm doanh nghiệp**; chi phí biến động theo usage sau ký ức 7/2025; lệ thuộc một vendor cho cả viết lẫn review. |
| **Habit** (giữ ở hiện trạng cũ) | Hợp đồng Copilot Enterprise đã ký và bundle sẵn trong GitHub/M365 — không tốn vòng procurement mới; quy trình review đã gắn chặt GitHub; ngại retrain cả đội. |

**Lực nào đang giữ user mạnh nhất — và nếu nó biến mất thì sao?**

Lực mạnh nhất là **Pull thuần kỹ thuật ("hiện chưa có gì tốt hơn")**, chứ *không* phải Habit hay data lock-in. Đây là chỗ dễ ngộ nhận nhất khi phân tích Cursor: Cursor là fork VS Code nên settings, extension và keybinding port sang chỗ khác gần như miễn phí; code thì nằm trong git repo của khách, không nằm trong Cursor. Nói cách khác **chi phí rời đi về mặt dữ liệu gần bằng 0**.

Mà Pull kỹ thuật là lực mong manh nhất trong bốn lực, vì nó **reset mỗi lần có model mới ra** — mỗi 3–6 tháng. Nếu Anthropic hay OpenAI ship một agent rõ ràng tốt hơn Composer trong hai quý, Pull bốc hơi, và lúc đó Habit (hợp đồng Copilot có sẵn) sẽ kéo khách quay lại chứ không giữ họ ở Cursor.

Chính vì hiểu điều này mà các mốc 5, 6, 7, 8 ở §1 đều là **nỗ lực chuyển moat từ Pull sang Habit**: tự train model để không phụ thuộc chu kỳ của người khác (Composer), nuốt khâu review để bám vào quy trình tổ chức (Graphite), và từ 8/2026 là lớp cấu hình tổ chức — rules, skills, MCP, subagent, team marketplace, audit log. Đó mới là những thứ đau đớn khi rời đi. Cuộc đua thật của Cursor là **kịp dựng Habit trước khi Pull hết hạn**.

---

## §3. Ba dự đoán hướng đi (6–12 tháng tới)

**Dự đoán 1** *(loại: mở rộng segment)*
- **Dự đoán:** Cursor sẽ nhân bản mô hình "Cursor Start" sang 3–5 thị trường mới nổi khác (Brazil, Indonesia, Nigeria, Việt Nam) với giá nội tệ + thanh toán nội địa, mặc định chạy Grok/Composer thay vì model frontier — trong vòng 6–12 tháng.
- **Lập luận:** Playbook đã chạy thật một lần ở mốc 28/7/2026 (₹649, UPI, Ấn Độ là thị trường thứ ba với 3 triệu dev), và mốc 19/3/2026 (Composer 2 giá 0,50 USD/1M token) cho đúng biên lợi nhuận để bán ở mức giá đó mà không lỗ — thứ Cursor không có trước khi tự chủ model. §2 cho thấy trục phân tầng tệp hiện tại đã là **khả năng chi trả theo địa lý**, không còn là nghề nghiệp.

**Dự đoán 2** *(loại: thay đổi mô hình kiếm tiền)*
- **Dự đoán:** Gói enterprise sẽ dịch từ per-seat sang **định giá theo agent** (agent-hour hoặc pool compute cấp theo team), kèm trần chi tiêu và dashboard dự báo bắt buộc — Router trở thành cơ chế bảo vệ biên lợi nhuận thay vì chỉ là tính năng tiết kiệm cho khách.
- **Lập luận:** Mốc 6–7/2025 đã chứng minh bằng máu rằng seat cố định không khớp COGS biến động, và Cursor học được rằng vấn đề nằm ở *tính bất ngờ* chứ không ở mức giá — nên lần này sẽ đi kèm trần và dự báo. Mốc 22/7/2026 (Router, giảm 30–50% chi phí) là hạ tầng đo-đếm-định-tuyến từng request, tức điều kiện kỹ thuật cho việc tính tiền theo agent. §2: nhóm chiếm ~60% doanh thu là người cần **dự đoán được** chi phí hơn là cần rẻ.

**Dự đoán 3** *(loại: đe dọa từ Big Tech)*
- **Dự đoán:** Microsoft/GitHub sẽ ép từ phía hợp đồng (Copilot bundle sẵn trong M365/GitHub) còn Anthropic ép từ phía terminal/CI — và Cursor phản ứng bằng cách **rời khỏi IDE**: đẩy trọng tâm sang cloud agent, PR review và plugin ở nơi làm việc (Workspace, Slack/Jira, mobile), tự định vị là *lớp điều phối agent* chứ không phải editor.
- **Lập luận:** Cursor không thắng được lực **Habit** ở §2 bằng một editor tốt hơn, vì hợp đồng Copilot đã nằm sẵn trong ngân sách khách hàng. Hướng đi này đã lộ rõ qua chuỗi nước đi 12/2025 → 8/2026: mua Graphite để đánh thẳng vào sân nhà GitHub (review), rồi iPad (29/7), plugin Google Workspace (3/8), Cloud Agents Builds (13/8) — toàn bộ đều là bề mặt **ngoài editor**. Compute và model từ SpaceX/xAI (mốc 8) là thứ khiến hướng này khả thi về chi phí.

**Dự đoán nào tự tin nhất — và giả định nào làm nó gãy?** Tự tin nhất là **Dự đoán 1**, vì nó là playbook đã ship thật chứ không phải suy diễn, và chỉ cần lặp lại. Giả định làm nó gãy: rằng ban lãnh đạo sau thâu tóm giữ nguyên ưu tiên tăng trưởng người dùng. Nếu deal đóng trong Q3/2026 và SpaceX ưu tiên biên lợi nhuận cùng khách hàng doanh nghiệp/chính phủ Mỹ — hoặc vướng ràng buộc kiểm soát xuất khẩu công nghệ — thì việc mở rộng giá rẻ ở thị trường mới nổi sẽ bị hoãn vô thời hạn dù mọi điều kiện sản phẩm đều đã sẵn sàng.

---

## §4. AI Log

| Việc | AI làm hay nhóm làm? | Nhóm kiểm chứng/phán đoán lại thế nào? |
|---|---|---|
| Thu thập mốc thời gian, số liệu funding/ARR/định giá | **AI** (Claude Opus 5, web search + đọc trực tiếp nguồn) | Đối chiếu chéo Wikipedia với changelog gốc của Cursor. **Phát hiện và sửa 2 lỗi trong bản nháp trước:** (a) thương vụ SpaceX bị ghi là "hoàn tất 16/6/2026" — thực tế mới ký/thực thi quyền mua, **dự kiến đóng Q3/2026**; (b) Anysphere thành lập 2022 chứ không phải 2023 (Cursor mới ra mắt 2023). |
| Xác minh ngày tháng của các bản release | **Nhóm yêu cầu → AI thực hiện** | Bắt buộc lấy từ changelog chính chủ thay vì bài blog thứ cấp: Cursor 1.0 = 4/6/2025, Cursor 2.0 = 29/10/2025, Composer 2 = 19/3/2026, Router = 22/7/2026, Cursor Start = 28/7/2026. |
| Số liệu ARR giữa 2026 | **AI tổng hợp** | **Các nguồn mâu thuẫn nhau**: báo cáo thương vụ ghi ~2,6 tỷ USD annualized, Wikipedia ghi "3 tỷ USD annual sales rate" (5/2026), DevGraphiq ghi ~4 tỷ USD. Không chọn con số đẹp nhất mà **ghi cả khoảng 2,6–4 tỷ USD và nêu rõ là tuỳ nguồn**. |
| Chọn 8/13 mốc đưa vào timeline | **Nhóm** | Đặt một tiêu chí duy nhất rồi lọc: mốc có đổi câu trả lời "ai trả tiền / trả cho cái gì / tốt là gì" không. Loại 5 mốc: 2 vòng gọi vốn đứng một mình, sự cố chatbot "Sam", iPad, Cloud Agents Builds, acqui-hire Koala. |
| Map mỗi mốc về một nguyên lý có tên | **Nhóm** | AI đề xuất diễn giải chung chung kiểu "tích hợp dọc"; nhóm buộc phải map về đúng bộ khái niệm đã học (x10 · wrapper/moat · Vertical AI · vòng lặp học · định nghĩa "tốt") và viết lại cho mỗi mốc chỉ mang **một** nguyên lý chính. |
| Phân tích 4 forces | **Nhóm phán đoán lại** | Bản AI ban đầu xếp Habit/data lock-in là lực giữ mạnh nhất. Nhóm bác bỏ: Cursor là fork VS Code (settings port đi dễ) và code nằm trong git của khách → chi phí rời đi gần bằng 0. **Kết luận lại: lực mạnh nhất là Pull kỹ thuật — cũng là lực mong manh nhất.** Đây là thay đổi lớn nhất so với bản nháp. |
| Ba dự đoán | **Nhóm** (AI viết lại cho gọn) | Bản nháp cũ có một dự đoán kiểu "doanh thu vẫn tăng nhưng chậm lại" — loại vì không kiểm chứng được đúng/sai. Thay bằng 3 dự đoán có **sự kiện quan sát được** để đối chứng, mỗi cái buộc phải trỏ ngược về ít nhất một mốc ở §1. |

---

## Nguồn tham khảo

- [Wikipedia — Cursor (code editor)](https://en.wikipedia.org/wiki/Cursor_(code_editor))
- [Contrary Research — Cursor Business Breakdown](https://research.contrary.com/company/cursor)
- [Cursor Changelog 1.0 (4/6/2025)](https://cursor.com/changelog/1-0) · [Cursor 2.0 (29/10/2025)](https://cursor.com/blog/2-0) · [Changelog tổng](https://cursor.com/changelog)
- [Cursor — Graphite joins Cursor (19/12/2025)](https://cursor.com/blog/graphite) · [Fortune](https://fortune.com/2025/12/19/cursor-ai-coding-startup-graphite-competition-heats-up/)
- [SiliconANGLE — Composer 2 (19/3/2026)](https://siliconangle.com/2026/03/19/vibe-coding-startup-cursor-launches-programming-optimized-composer-2-model/) · [VentureBeat](https://venturebeat.com/technology/cursors-new-coding-model-composer-2-is-here-it-beats-claude-opus-4-6-but)
- [CNBC — SpaceX mua Cursor 60 tỷ USD (16/6/2026)](https://www.cnbc.com/2026/06/16/spacex-spcx-cursor-acquisition-ipo.html) · [Forbes](https://www.forbes.com/sites/sandycarter/2026/06/16/spacex-buys-cursor-in-largest-startup-acquisition-ever-at-60-billion/)
- [Cursor — Introducing Cursor Start (28/7/2026)](https://cursor.com/blog/cursor-start-india) · [TestingCatalog](https://www.testingcatalog.com/cursor-launches-start-plan-in-india-for-649-per-month/)
- [FinTech Weekly — Cursor pricing backlash & refund](https://www.fintechweekly.com/magazine/articles/cursor-pricing-change-user-backlash-refund) · [Finout — What happened to Cursor pricing](https://www.finout.io/blog/what-happened-to-cursor-pricing-2026-guide-5-cost-cutting-tips)
- [DevGraphiq — Cursor Statistics 2026](https://devgraphiq.com/cursor-statistics/)
