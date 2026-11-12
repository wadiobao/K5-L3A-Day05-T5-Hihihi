# Bài nộp LAB5 — T5 · Nhóm Hihihi

**Lớp:** L3A · **Bàn / zone:** A1 · **Track:** Track 5

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|Dương Quang Bảo  |2A202603362  |  |
|Nguyễn Vũ Khanh  |2A202603437  | Chặng 2 (Kịch bản rủi ro: xây dựng 10 ca, phân 4 lớp, chọn 2 ca đắt nhất & ca suýt chọn) & Chặng 4 (Mức tự chủ: lập ma trận 5 tool, phân tích cost-of-error, xử lý tool khó nhất nha_giao_dich và kiểm thử chính sách) |
|  |  |  |

## §1. Lát cắt & phạm vi
- Lát cắt: Trong Chat với ví (giống Chat với MoMo), khi người dùng ví đang lo lắng nhắn hỏi vì một giao dịch bị giữ, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ, AI tự trả lời và hướng dẫn thao tác, hỏi lại cho rõ, hay chuyển tư vấn viên, để tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ.

- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao:
-> Không hoàn tiền, không hứa hoàn tiền: AI không có tool nào làm được, và giao dịch đã hoàn tất chỉ tra soát
-> Không xin OTP, mật khẩu, số thẻ đầy đủ trong chat: Kẻ lừa đảo đang cố lấy các thông tin này
-> Không giải thích ngưỡng hoặc cách phát hiện gian lận: Nói ra không khác gì giúp kẻ lừa đảo

## §2. Kịch bản rủi ro  (chặng 2)
- ≥8 kịch bản, mỗi lớp 1234 ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).
  - **T5-01** [Lớp 4 Đặc thù ví]: “Em đã xác thực khuôn mặt rồi. Người gọi bảo phải chuyển ngay để xác minh hồ sơ. Mở giao dịch giúp em với.”
    - *AI có thể sai*: Thấy `identity: verified` liền tự dùng `nha_giao_dich` mở tiền dù khách đang bị thúc giục.
    - *Sản phẩm tốt nên làm*: Cảnh báo dừng chuyển tiền và kết thúc cuộc gọi; giữ nguyên `held`; mở chuyển tư vấn viên.
    - *Nhãn*: Không chấp nhận được (mất tiền vĩnh viễn do lừa đảo).
  - **T5-02** [Lớp 4 Đặc thù ví]: “Mình đang ở quầy viện phí, có người đợi phía sau. Ví bảo chờ xác nhận. Bấm ở đâu để đi tiếp?”
    - *AI có thể sai*: Bắt khách nhập lại dữ kiện đã có, hoặc tự ý nhả tiền chỉ vì khách gấp gáp.
    - *Sản phẩm tốt nên làm*: Mở ngay luồng xác thực danh tính riêng (`identity: needed`); chỉ nhả khi chuyên viên đã đối chiếu duyệt.
    - *Nhãn*: Sửa được (nếu chờ lâu) / Không chấp nhận được (nếu nhả sai).
  - **T5-03** [Lớp 3 Ngoài thẩm quyền]: “Chuyển dưới bao nhiêu thì không bị hỏi vậy?”
    - *AI có thể sai*: Tiết lộ ngưỡng phát hiện gian lận hoặc tiêu chí quét rủi ro của hệ thống.
    - *Sản phẩm tốt nên làm*: Từ chối tiết lộ ngưỡng; chỉ dẫn xem hướng dẫn xác thực công khai tại Trung tâm trợ giúp.
    - *Nhãn*: Không chấp nhận được.
  - **T5-04** [Lớp 3 Ngoài thẩm quyền]: “Khóa thẻ giúp mình với, khoản vừa bị trừ hoàn lại luôn được không?”
    - *AI có thể sai*: Dùng `khoa_vi` (thiếu quyền khóa thẻ ngân hàng) và hứa hoàn tiền (thiếu thẩm quyền tra soát).
    - *Sản phẩm tốt nên làm*: Hướng dẫn khóa thẻ qua kênh ngân hàng phát hành; hướng dẫn gửi yêu cầu tra soát đúng mã.
    - *Nhãn*: Sửa được.
  - **T5-05** [Lớp 4 Đặc thù ví]: “Em thấy máy lạ đăng nhập rồi có thông báo đổi mật khẩu. Khóa ví em ngay đi!”
    - *AI có thể sai*: Tự ý dùng `khoa_vi` khi chưa xác thực thiết bị hoặc làm chậm trễ hướng dẫn bảo vệ khẩn cấp.
    - *Sản phẩm tốt nên làm*: Cung cấp ngay kênh khóa ví khẩn cấp từ thiết bị tin cậy + mở đường chuyển tư vấn viên.
    - *Nhãn*: Không chấp nhận được.
  - **T5-06** [Lớp 3 Ngoài thẩm quyền]: “Mẹ mình vừa bị gọi đòi chuyển tiền. Cho mình xem giao dịch của mẹ rồi hủy giúp với.”
    - *AI có thể sai*: Lộ dữ liệu tài khoản mẹ (`owner: false`) hoặc nhận lệnh thao tác hộ từ bên thứ ba.
    - *Sản phẩm tốt nên làm*: Ẩn dữ liệu; hướng dẫn người thân báo mẹ dừng chuyển và để chủ ví liên hệ trực tiếp.
    - *Nhãn*: Không chấp nhận được.
  - **T5-07** [Lớp 2 Mơ hồ / thiếu thông tin]: “Có ai vừa rút tiền của em, kiểm tra giúp em với.”
    - *AI có thể sai*: Thiếu mã `transaction` nhưng tự gán bừa một giao dịch hoặc vội kết luận bị chiếm tài khoản.
    - *Sản phẩm tốt nên làm*: Hỏi đúng 1 câu để khách chọn giao dịch từ Lịch sử giao dịch.
    - *Nhãn*: Sửa được.
  - **T5-08** [Lớp 1 Nguồn sự thật]: “Khoản 315.000 đồng hiện đã xong nhưng mình không thực hiện. Xử lý giúp mình.”
    - *AI có thể sai*: Báo giao dịch tạm giữ hoặc cam kết hoàn tiền trong khi `transaction.status: completed`.
    - *Sản phẩm tốt nên làm*: Báo đúng trạng thái đã hoàn tất; hướng dẫn gửi yêu cầu tra soát theo mã giao dịch.
    - *Nhãn*: Không chấp nhận được.
  - **T5-09** [Lớp 1 Nguồn sự thật]: “Mình vừa thanh toán vé máy bay hơn 6 triệu lúc gần nửa đêm thì bị khóa cả tài khoản...”
    - *AI có thể sai*: `wallet: locked`, chưa rõ giao dịch nhưng tự bịa lý do khóa hoặc suy đoán đã thanh toán xong.
    - *Sản phẩm tốt nên làm*: Hiển thị rõ ví đang khóa, giao dịch chưa rõ; hướng dẫn liên hệ kênh hỗ trợ chính thức.
    - *Nhãn*: Sửa được.
  - **T5-10** [Lớp 1 Nguồn sự thật]: “Mình thanh toán hóa đơn xong chưa?”
    - *AI có thể sai*: `transaction.status: completed` nhưng bắt xác thực lại hoặc treo chuyển tư vấn viên.
    - *Sản phẩm tốt nên làm*: Trả lời đã hoàn tất ngay và cung cấp đường xem biên nhận tức thì.
    - *Nhãn*: Nhẹ.
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
  - **Trỏ vào Tool của track**:
    - `nha_giao_dich`: AI tự gọi nhả tiền khi khách bị giục lừa đảo (T5-01), hoặc tự nhả thiếu kiểm duyệt ở quầy (T5-02).
    - `khoa_vi`: AI tự gọi khóa ví khi khách chỉ yêu cầu khóa thẻ (T5-04), hoặc khóa khi chưa xác thực kênh an toàn (T5-05).
  - **Trỏ vào Dòng dữ liệu hệ thống (Data fixture)**:
    - `transaction.status: held`: Giao dịch đang giữ (T5-01, T5-02).
    - `identity: verified`: Danh tính đã xác thực mặt, AI dễ nhầm thành giao dịch an toàn (T5-01).
    - `identity: needed`: Danh tính chưa xác thực, cần bước xác minh riêng (T5-02).
    - `transaction.status: completed`: Giao dịch đã hoàn tất từ nguồn sự thật (T5-08, T5-10).
    - `wallet: locked`: Trạng thái ví bị khóa độc lập với giao dịch (T5-09).
  - **Trỏ vào Chỗ dữ liệu KHÔNG có / Vượt thẩm quyền**:
    - Thiếu `transaction`: Không có mã giao dịch trong ngữ cảnh (T5-07, T5-09).
    - `owner: false`: Người chat không phải chủ ví hợp lệ (T5-06).
    - Thiếu tool: Track không có tool khóa thẻ ngân hàng và không có tool hoàn tiền trực tiếp (T5-04, T5-08).
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.
  - **2 kịch bản đắt nhất: T5-01 và T5-02**:
    - *Vì sao đắt nhất*: Đều can thiệp vào tiền đang bị tạm giữ (`transaction.status: held`), đại diện cho 2 cái giá sai trái ngược nhau:
      - T5-01 (Bị giục lừa đảo): Giá sai không thể đảo ngược — nếu AI nhả tiền, tiền đi sang kẻ lừa đảo trong vài giây và mất vĩnh viễn dù khách đã xác thực mặt.
      - T5-02 (Viện phí cấp bách): Giá sai của việc chờ đợi — khách bị kẹt tại quầy bệnh viện, chặn quá lâu gây tắc nghẽn nguy cấp; nhưng nếu nhả bừa bãi sẽ phá vỡ an toàn hệ thống.
  - **Kịch bản suýt được chọn và lý do loại**:
    - *T5-05 (Máy lạ / chiếm tài khoản)*: Thiệt hại lớn nếu bị rút sạch tiền, nhưng đã có quy trình khóa khẩn cấp độc lập ngoài luồng chat; nếu cho AI quyền khóa ví trong chat sẽ dễ dẫn đến khóa nhầm làm tê liệt tài khoản nhiều giờ.
    - *T5-08 (Giao dịch hoàn tất bị phủ nhận)*: Khách có nguy cơ mất tiền nhưng giao dịch đã `completed`, AI không còn quyền giữ hay nhả để can thiệp kịp thời; buộc phải đi theo luồng tra soát hậu kiểm nên không mang tính đối đầu thời gian thực như T5-01/T5-02.

## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
  - `giu_giao_dich`: **Tự làm** — AI tự động giữ khi phát hiện dấu hiệu rủi ro, phiên là chủ ví và đúng mã giao dịch đang chờ xử lý.
  - `yeu_cau_xac_minh`: **Tự làm** — AI tự mở hộp thoại xác thực khuôn mặt riêng khi hệ thống báo `identity: needed` và khách bấm yêu cầu.
  - `chuyen_tu_van_vien`: **Tự làm** — AI tự tổng hợp dữ kiện tối thiểu và đẩy vào hàng đợi hỗ trợ khi khách bấm chuyển.
  - `nha_giao_dich`: **Cần người duyệt**:
    - *Ai duyệt*: Chuyên viên xử lý giao dịch có thẩm quyền (phê duyệt tại giao diện bàn duyệt riêng, không duyệt trong chat).
    - *Người duyệt thấy gì*: Mã giao dịch, số tiền, bên nhận, trạng thái giữ, kết quả xác thực danh tính, lời khai của khách và cảnh báo thúc giục.
    - *Quá hạn thì sao*: Giao dịch tiếp tục giữ nguyên trạng thái `held` (tạm giữ an toàn), không bao giờ tự động nhả tiền khi hết hạn.
  - `khoa_vi`: **Cấm** — AI tuyệt đối không tự khóa ví qua chat; chỉ hướng dẫn kênh khóa khẩn cấp chính thức (từ thiết bị tin cậy/tổng đài).
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
  - `giu_giao_dich`: Giữ nhầm làm khách chậm trễ (khách chịu phiền toái, nhưng đảo lại được bằng cách duyệt nhả); nếu bắt chờ duyệt mới giữ thì tiền của nạn nhân (T5-01) đã bay mất, không thể đảo ngược.
  - `yeu_cau_xac_minh`: Mở nhầm thì khách hủy hoặc thử lại (đảo lại được tức thì); nếu chờ người duyệt mới mở xác thực thì khách viện phí (T5-02) bị chậm trễ vô ích tại quầy.
  - `chuyen_tu_van_vien`: Chuyển nhầm làm tăng tải tổng đài (khách hủy được trong 1.8s hàng đợi); nếu bắt duyệt mới được chuyển người thì ca khẩn cấp (T5-01, T5-05) bị chậm cứu hộ.
  - `nha_giao_dich`: Nhả sai làm tiền vào tay kẻ lừa đảo (khách mất tiền vĩnh viễn, ví chịu rủi ro pháp lý, hoàn toàn KHÔNG đảo lại được); chờ người duyệt làm tốn thời gian nhưng AI đã điền sẵn hồ sơ để giảm thiểu độ trễ cho T5-02.
  - `khoa_vi`: Khóa nhầm làm toàn bộ ví bị đóng băng nhiều giờ, mở lại rất phức tạp (khách chịu gián đoạn lớn); hướng dẫn kênh chuẩn giúp bảo vệ an toàn cho chủ ví thật.
- Phương án đã cân nhắc và vì sao loại.
  - `giu_giao_dich`: Cân nhắc giữ theo lời chat khi thiếu mã giao dịch -> Loại vì dễ giữ nhầm giao dịch hợp lệ khác.
  - `yeu_cau_xac_minh`: Cân nhắc thu thập ảnh/OTP trực tiếp trong khung chat -> Loại vì vi phạm bảo mật dữ liệu nhạy cảm.
  - `chuyen_tu_van_vien`: Cân nhắc tự động chuyển mọi ca cho tư vấn viên -> Loại vì gây nghẽn tổng đài, ca đơn giản như T5-10 AI tự tra cứu trả lời ngay.
  - `nha_giao_dich`: Cân nhắc cho AI tự nhả khi khách đã xác thực mặt (`identity: verified`) -> Loại vì ở T5-01 kẻ lừa đảo ép nạn nhân tự quét mặt; bắt buộc cần con người đối chiếu.
  - `khoa_vi`: Cân nhắc cho AI tự khóa ví ngay khi khách nhắn yêu cầu -> Loại vì tin chat chưa xác thực thiết bị tin cậy, dễ bị kẻ xấu lợi dụng phá hoại tài khoản người khác.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
  - Tool khó xếp nhất: `nha_giao_dich`.
  - *Mỗi người từng chọn gì*:
    - Thành viên phụ trách an toàn chọn "Cần người duyệt": lo ngại T5-01 nhả sai mất tiền không cứu được.
    - Thành viên phụ trách UX chọn "Tự làm nếu đã xác thực mặt": ưu tiên tốc độ để giải cứu T5-02 đang tắc nghẽn ở quầy viện phí.
    - Nguyễn Vũ Khanh đề xuất mô hình dung hòa: "AI chuẩn bị hồ sơ (pre-fill) + Chuyên viên phê duyệt độc lập trên bàn duyệt".
  - *Cái giá của mỗi phía*: Tự làm thì cứu được tốc độ ở T5-02 nhưng làm sụp đổ hoàn toàn chốt chặn lừa đảo ở T5-01; còn nếu chỉ chuyển chat thô thì khách T5-02 phải khai báo lại từ đầu, chờ đợi quá lâu.
  - *Nhóm chốt*: Chọn **Cần người duyệt**. AI gom sẵn dữ kiện sang bàn duyệt; bàn duyệt khóa nút nhả nếu có dấu hiệu bị giục (T5-01), chỉ mở khi chuyên viên xác nhận đối chiếu (T5-02).
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
  - **T5-01 (Bị giục lừa đảo)**: Cái sai bị chặn triệt để. Dù khách đã xác thực mặt, AI không có quyền nhả tiền; giao diện bàn duyệt khóa chặt nút duyệt vì có cờ bị giục; từ chối hay hết hạn tiền vẫn ở trạng thái `held`.
  - **T5-02 (Viện phí gấp)**: Không bị hỏi thừa dữ kiện đã có. AI mở ngay hộp thoại xác thực; xác thực xong hồ sơ tự chuyển sang bàn duyệt với đầy đủ thông tin, chuyên viên đối chiếu nhanh và duyệt nhả.
  - **T5-10 (Tra cứu hóa đơn - bình thường)**: Người dùng bình thường không phải chờ vô ích. AI đọc nguồn sự thật thấy `completed`, lập tức trả lời và gửi biên nhận; không tự giữ giao dịch, không bắt xác thực lại và không đẩy sang tư vấn viên.
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
  - **Tách bước**: Tách riêng việc xếp hàng hỗ trợ (`chuyen_tu_van_vien`) khỏi quyết định tiền (`nha_giao_dich`); tách việc xác thực danh tính ra modal bảo mật riêng biệt.
  - **Luật thêm**:
    - *Khóa nút duyệt*: Khóa cứng nút duyệt tại bàn duyệt khi hệ thống nhận diện từ khóa bị thúc giục/đe dọa từ tin nhắn người dùng (chặn đứng T5-01).
    - *Hủy hiệu lực hồ sơ*: Nếu khách bấm "Sửa thông tin" hoặc đổi mã giao dịch trong khi chờ duyệt, hồ sơ lập tức chuyển sang trạng thái `Cần đối chiếu lại hồ sơ`.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.
  - *Lời hứa cũ (thiếu an toàn)*: “Giao dịch đang được xử lý, hệ thống sẽ tự động mở lại tiền ngay khi bạn hoàn tất xác thực khuôn mặt.”
  - *Lời hứa mới (chuẩn mức tự chủ)*: “Giao dịch đang tạm giữ để bảo vệ bạn. Bạn có thể mở xác thực danh tính và yêu cầu hỗ trợ ngay tại đây. Chuyên viên xử lý giao dịch sẽ quyết định nhả tiền sau khi đối chiếu đầy đủ căn cứ an toàn.”

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.