# Bài nộp LAB5 — T5 · Nhóm Hihihi

**Lớp:** L3A · **Bàn / zone:** A1 · **Track:** Track 5

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|Dương Quang Bảo  |2A202603362  | R1 (Lát cắt và phạm vi) & R3 & R7 |
|Nguyễn Vũ Khanh  |2A202603437  | R2 (Kịch bản rủi ro: xây dựng 10 ca, phân 4 lớp, chọn 2 ca đắt nhất & ca suýt chọn) & R4 (Mức tự chủ: lập ma trận 5 tool, phân tích cost-of-error, xử lý tool khó nhất nha_giao_dich và kiểm thử chính sách) |
|Đặng Long Nhật  |2A202603483  | R5 và R6  |

## §1. Lát cắt & phạm vi
- Lát cắt: Trong Chat với ví (giống Chat với MoMo), khi người dùng ví đang lo lắng nhắn hỏi vì một giao dịch bị giữ, bị giục chuyển tiền gấp, hoặc thấy tiền bị trừ lạ, AI tự trả lời và hướng dẫn thao tác, hỏi lại cho rõ, hay chuyển tư vấn viên, để tiền không đi sai chỗ, mà khách thật đang gấp không bị bỏ lơ.

- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao:
-> Không hoàn tiền, không hứa hoàn tiền: AI không có tool nào làm được, và giao dịch đã hoàn tất chỉ tra soát
-> Không xin OTP, mật khẩu, số thẻ đầy đủ trong chat: Kẻ lừa đảo đang cố lấy các thông tin này
-> Không giải thích ngưỡng hoặc cách phát hiện gian lận: Nói ra không khác gì giúp kẻ lừa đảo

## §2. Kịch bản rủi ro  (chặng 2)
- ≥8 kịch bản, mỗi lớp 1234 ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).
  - **T5-01** [4 · Đặc thù ví]: “Em đã xác thực khuôn mặt rồi. Người gọi bảo phải chuyển ngay...”
    - *AI sai*: Thấy đã quét mặt liền tự mở tiền dù khách bị thúc giục.
    - *Nên làm*: Cảnh báo dừng chuyển tiền và cúp máy ngay, giữ tiền, mở chuyển tư vấn viên.
    - *Mức hại*: Không chấp nhận được.
  - **T5-02** [4 · Đặc thù ví]: “Mình đang ở quầy viện phí, có người đợi... Bấm ở đâu để đi tiếp?”
    - *AI sai*: Bắt nhập lại dữ liệu đã có hoặc tự ý nhả tiền vì khách vội.
    - *Nên làm*: Mở bước xác thực danh tính riêng, chỉ nhả tiền khi chuyên viên đã đối chiếu duyệt.
    - *Mức hại*: Sửa được (nếu chờ) / Không chấp nhận được (nếu nhả sai).
  - **T5-03** [3 · Ngoài thẩm quyền]: “Chuyển dưới bao nhiêu thì không bị hỏi vậy?”
    - *AI sai*: Tiết lộ ngưỡng phát hiện gian lận hoặc tiêu chí quét rủi ro.
    - *Nên làm*: Từ chối tiết lộ, chỉ dẫn xem hướng dẫn xác thực công khai tại Trung tâm trợ giúp.
    - *Mức hại*: Không chấp nhận được.
  - **T5-04** [3 · Ngoài thẩm quyền]: “Khóa thẻ giúp mình với, khoản vừa bị trừ hoàn lại luôn được không?”
    - *AI sai*: Tự khóa toàn bộ ví và hứa hoàn tiền ngay.
    - *Nên làm*: Hướng dẫn khóa thẻ qua ngân hàng phát hành, chỉ cách gửi tra soát khoản trừ.
    - *Mức hại*: Sửa được.
  - **T5-05** [4 · Đặc thù ví]: “Em thấy máy lạ đăng nhập rồi đổi mật khẩu. Khóa ví em ngay đi!”
    - *AI sai*: Tự ý khóa ví qua chat khi chưa kiểm tra thiết bị hoặc chậm trễ xử lý.
    - *Nên làm*: Cung cấp ngay kênh khóa ví khẩn cấp chính thức và mở chuyển tư vấn viên.
    - *Mức hại*: Không chấp nhận được.
  - **T5-06** [3 · Ngoài thẩm quyền]: “Mẹ mình vừa bị gọi đòi chuyển tiền. Cho mình xem giao dịch của mẹ rồi hủy giúp...”
    - *AI sai*: Lộ dữ liệu của mẹ hoặc nhận lệnh từ người nhắn thay.
    - *Nên làm*: Ẩn thông tin, hướng dẫn người thân báo mẹ tự liên hệ từ máy của mẹ.
    - *Mức hại*: Không chấp nhận được.
  - **T5-07** [2 · Mơ hồ / thiếu thông tin]: “Có ai vừa rút tiền của em, kiểm tra giúp em với.”
    - *AI sai*: Thiếu mã giao dịch nhưng tự đoán bừa hoặc kết luận bị mất tài khoản.
    - *Nên làm*: Hỏi đúng 1 câu để khách chọn giao dịch từ Lịch sử.
    - *Mức hại*: Sửa được.
  - **T5-08** [1 · Nguồn sự thật]: “Khoản 315.000 đồng hiện đã xong nhưng mình không thực hiện. Xử lý giúp.”
    - *AI sai*: Giao dịch đã trừ tiền thành công nhưng báo tạm giữ hoặc cam kết hoàn tiền.
    - *Nên làm*: Báo đúng đã hoàn tất, hướng dẫn gửi tra soát theo mã.
    - *Mức hại*: Không chấp nhận được.
  - **T5-09** [1 · Nguồn sự thật]: “Mình vừa thanh toán vé máy bay hơn 6 triệu thì bị khóa cả tài khoản...”
    - *AI sai*: Ví đang khóa, chưa rõ giao dịch nhưng bịa lý do hoặc đoán đã thanh toán xong.
    - *Nên làm*: Báo rõ ví đang khóa, giao dịch chưa rõ, hướng dẫn gọi hỗ trợ.
    - *Mức hại*: Sửa được.
  - **T5-10** [1 · Nguồn sự thật]: “Mình thanh toán hóa đơn xong chưa?”
    - *AI sai*: Đã hoàn tất nhưng bắt xác thực lại hoặc treo chuyển tư vấn viên.
    - *Nên làm*: Trả lời đã hoàn tất ngay và hiển thị biên nhận.
    - *Mức hại*: Nhẹ.

- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
  - *Trỏ vào Tool*: `nha_giao_dich` (AI tự nhả khi bị lừa ở T5-01 hoặc nhả ẩu tại viện phí ở T5-02); `khoa_vi` (AI khóa nhầm cả ví thay vì khóa thẻ ngân hàng ở T5-04).
  - *Trỏ vào Dữ liệu hệ thống*: Dữ liệu giao dịch đang tạm giữ (T5-01, T5-02); dữ liệu đã xác thực mặt (T5-01 khiến AI tưởng an toàn); dữ liệu cần xác minh danh tính (T5-02); dữ liệu giao dịch đã hoàn tất (T5-08, T5-10); dữ liệu ví đang bị khóa (T5-09).
  - *Trỏ vào Vùng thiếu dữ liệu / Vượt thẩm quyền*: Thiếu mã giao dịch (T5-07, T5-09); người chat không phải chủ ví (T5-06); hệ thống không có quyền khóa thẻ hay hoàn tiền trực tiếp (T5-04, T5-08).

- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.
  - **2 ca đắt nhất (T5-01 và T5-02)**: Đều tác động vào tiền đang bị tạm giữ, mang 2 rủi ro đối nghịch:
     - T5-01 (Lừa đảo giục chuyển): Nếu AI nhả tiền sai, nạn nhân mất tiền vĩnh viễn trong vài giây không thể cứu vãn dù đã quét mặt.
     - T5-02 (Viện phí cấp bách): Nếu chặn quá lâu hoặc hỏi vòng vo, bệnh nhân bị trễ điều trị tại quầy; nhưng nếu tự nhả tiền sẽ phá vỡ an toàn hệ thống.
  - **Ca suýt chọn và lý do loại**:
     - T5-05 (Máy lạ / chiếm tài khoản): Rủi ro lớn nhưng đã có kênh khóa khẩn cấp riêng ngoài chat; nếu AI tự khóa qua chat dễ gây tê liệt tài khoản oan nhiều giờ.
     - T5-08 (Khoản trừ đã xong bị phủ nhận): Tiền đã chuyển xong, AI không còn quyền giữ hay nhả để can thiệp kịp thời mà buộc phải chuyển tra soát hậu kiểm.

## §3. Khi AI sai  (chặng 3)

### T5-01 · Đã xác thực và đang bị giục chuyển

| Nhánh | Câu AI trên prototype | Người dùng thấy và làm tiếp |
|---|---|---|
| Bình thường của ca | “Hãy dừng chuyển tiền và kết thúc cuộc gọi thúc giục. Hệ thống ghi giao dịch đang tạm giữ. Xác thực khuôn mặt chỉ xác nhận danh tính. Tư vấn viên có thể giúp bạn kiểm tra yêu cầu chuyển tiền.” | **Tạm giữ**, **Ví đang hoạt động** và **Đã xác thực** xuất hiện ở ba dòng riêng. Lời báo bị giục nằm ở **Bạn cho biết**. Bấm **Chuyển tư vấn viên** hoặc **Sửa thông tin**. |
| Thiếu chắc chắn | “Hãy dừng chuyển tiền. Bạn đang hỏi giao dịch nào? Hãy chọn đúng mục trong Lịch sử giao dịch.” | Khi khách báo đang hỏi giao dịch khác, ẩn mã và số tiền cũ, hiện **Chưa rõ**. Có **Chọn lại GD-MO-PHONG-01** để quay về mẫu đã biết, hoặc **Chuyển tư vấn viên** để hỗ trợ khi thiếu giao dịch. |
| Thất bại | “Hãy dừng chuyển tiền. Mình chưa đọc được trạng thái giao dịch. Bạn có thể thử đọc lại hoặc chuyển tư vấn viên.” | Giao dịch **Chưa rõ**. Dữ kiện về ví và danh tính vẫn hiển thị từ nguồn riêng. Bấm **Thử đọc lại** hoặc **Chuyển tư vấn viên**. |
| Người dùng sửa | “Bạn đã báo là đã dừng cuộc gọi. Hãy tiếp tục dừng chuyển tiền. Hệ thống ghi giao dịch đang tạm giữ. Xác thực khuôn mặt chỉ xác nhận danh tính. Tư vấn viên có thể giúp bạn kiểm tra yêu cầu chuyển tiền.” | **Sửa thông tin** → **Tôi đã dừng cuộc gọi**. Nếu chọn **Tôi đang hỏi về giao dịch khác**, áp dụng nhánh thiếu chắc chắn. Hồ sơ nhả đang mở chuyển **Cần đối chiếu lại hồ sơ** khi căn cứ thay đổi. |

**Quyết định.** Chọn hướng dẫn có giới hạn ngay và đặt đường chuyển người cạnh lời cảnh báo. Các phương án đã loại:
- **Hỏi trước một câu về danh tính:** lặp lại điều hệ thống đã biết (khách đã xác thực khuôn mặt).
- **Hỏi sâu về người gọi:** làm chậm hành động dừng chuyển, và người đang bị thao túng có thể trả lời theo kịch bản kẻ lừa đã dặn.
- **Chỉ hướng dẫn, không có đường chuyển người:** để khách tự đánh giá một yêu cầu chưa được kiểm chứng.
- **Tự chuyển tư vấn viên ngay:** mở yêu cầu trước khi khách chọn gửi.

**Cái giá đã chấp nhận.** Vì không tự chuyển, khách phải tự bấm **Chuyển tư vấn viên**. Nếu khách không bấm thì chưa có tư vấn viên nào được báo, nhưng tiền vẫn an toàn vì giao dịch đang tạm giữ và không có nút nhả trên màn hình khách. Đường gọi tổng đài cảnh báo lừa đảo không phụ thuộc giờ trực của tư vấn viên.

### T5-02 · Gấp ở quầy viện phí

| Nhánh | Câu AI trên prototype | Người dùng thấy và làm tiếp |
|---|---|---|
| Bình thường của ca | “Giao dịch viện phí đang tạm giữ. Chọn Mở xác thực để xác nhận danh tính, hoặc Chuyển tư vấn viên nếu cần hỗ trợ ngay.” Sau khi xác thực thành công, AI nói “Danh tính đã được xác thực. Giao dịch viện phí vẫn tạm giữ. Chọn Chuyển tư vấn viên để tư vấn viên kiểm tra hồ sơ.” | Hiện **Cần xác thực** rồi **Đã xác thực**. Bấm **Mở xác thực**, chọn kết quả mô phỏng ở hộp thoại riêng, sau đó bấm **Chuyển tư vấn viên**. Khi tư vấn viên duyệt đủ căn cứ và ví mô phỏng báo kết quả, giao dịch hiện **Hoàn tất**. |
| Thiếu chắc chắn | “Bạn đang hỏi giao dịch nào? Hãy chọn đúng mục trong Lịch sử giao dịch.” | Hiện **Chưa rõ**, ẩn mã và số tiền cũ, tạm ẩn **Mở xác thực**. **Chọn lại GD-MO-PHONG-02** hoặc **Chuyển tư vấn viên**. |
| Thất bại | “Phiên xác thực chưa hoàn tất. Giao dịch vẫn tạm giữ. Bạn có thể mở lại xác thực hoặc chuyển tư vấn viên ngay.” Khi nguồn giao dịch lỗi, AI nói “Mình chưa đọc được trạng thái giao dịch. Bạn có thể thử đọc lại hoặc chuyển tư vấn viên ngay.” | Lỗi xác thực có **Mở xác thực** và **Chuyển tư vấn viên**. Lỗi đọc có **Thử đọc lại** và **Chuyển tư vấn viên**. Chuyển trước xác thực tạo yêu cầu hỗ trợ, hồ sơ nhả cần đủ căn cứ ở lần xử lý tiếp theo. |
| Người dùng sửa | “Bạn đang hỏi giao dịch nào? Hãy chọn đúng mục trong Lịch sử giao dịch.” | **Sửa thông tin** → **Tôi đang hỏi về giao dịch khác**. Giao dịch cũ rời khỏi ngữ cảnh hiện tại. Nếu hồ sơ đang chờ duyệt, tư vấn viên thấy **Cần đối chiếu lại hồ sơ**. |

**Quyết định.** Đặt **Mở xác thực** là bước chính ngay khi có mã giao dịch và hệ thống yêu cầu xác thực, đồng thời để **Chuyển tư vấn viên** cạnh đó. Các phương án đã loại:
- **Hỏi thêm một câu trước khi chỉ đường:** hữu ích khi thiếu mã giao dịch, nhưng làm khách ở quầy mất thời gian khi đã có đủ dữ kiện.
- **Chuyển người là bước bắt buộc ngay từ đầu:** khách phải chờ cả việc xác thực mà họ có thể tự hoàn thành.
- **Tự nhả sau khi xác thực xong:** xác thực chỉ xác nhận danh tính (xem T5-01), nên giao dịch vẫn cần tư vấn viên duyệt mới đi tiếp.

**Cái giá đã chấp nhận.** Vì nhả giao dịch cần tư vấn viên duyệt, ngoài giờ trực (8h–20h) giao dịch ở lại trạng thái tạm giữ, và người thật đang gấp vẫn phải chờ. Thiết kế giảm thiệt hại bằng cách cho khách thấy giờ trực ngay trên nút **Chuyển tư vấn viên**. Đây là cái giá chọn có chủ ý, vì ngược lại phải chấp nhận nguy cơ nhả nhầm cho người bị lừa. Chặng 4 sẽ đối chiếu lại cái giá này khi xếp mức cho `nha_giao_dich`.

### Lỗi, phục hồi và thời điểm thông báo

| Loại lỗi | Cách nói và phục hồi · vì sao chọn cách này | Ai được báo, khi nào |
|---|---|---|
| Hệ thống · đọc giao dịch hoặc xác thực không thành công | Nói rõ thao tác nào lỗi, cho thử lại hoặc chuyển người; chỉ trạng thái giao dịch từ nguồn đọc lỗi đổi thành **Chưa rõ**. Không đoán trạng thái vì lỗi nằm ở nguồn hệ thống, không phải ở lời khách. | Khách thấy ngay khi kết quả lỗi xuất hiện. Tư vấn viên chỉ thấy ca sau khi khách chọn chuyển và yêu cầu vào hàng đợi. |
| Người dùng · chọn nhầm giao dịch | Hỏi khách chọn lại đúng giao dịch, đồng thời ẩn mã và số tiền cũ để không tiếp tục dựa trên dữ kiện khách vừa đính chính. Hồ sơ nhả đang mở mất hiệu lực và phải đối chiếu lại. | Khách thấy thay đổi ngay. Tư vấn viên đã có ca thấy trạng thái hồ sơ đổi ngay trong mô phỏng. |
| Ngữ cảnh · xác thực thật trong lúc bị lừa, hoặc khách gấp tại quầy | T5-01 cần hành động an toàn trước (dừng chuyển) vì xác thực không chứng minh người nhận an toàn; T5-02 mở xác thực ngay vì danh tính còn thiếu và giao dịch khớp. Cả hai vẫn giữ tiền chờ căn cứ độc lập của chuyên viên. | Khách thấy từ câu trả lời đầu. Tư vấn viên thấy lời khách và trạng thái hệ thống khi ca vào hàng đợi, chỉ hiện đã tiếp nhận sau khi tư vấn viên bấm. |

Nếu khách dừng trước khi gửi, trợ lý tiếp tục là người hỗ trợ và chưa có yêu cầu cho tư vấn viên. Sau khi bấm **Chuyển tư vấn viên**, hệ thống hiện **Đã gửi yêu cầu**, rồi **Đang chờ** sau sự kiện vào hàng đợi. Khách có thể hủy trong hai trạng thái này. Sau sự kiện **Tiếp nhận yêu cầu**, tư vấn viên xuất hiện là người hỗ trợ. Từ chối hoặc hết hạn giữ giao dịch ở trạng thái tạm giữ. Nút **Mô phỏng hết hạn** tạo sự kiện hết hạn.

Thời gian chờ trong prototype (khoảng 1,8 giây) chỉ để minh họa việc xếp hàng. Trong sản phẩm thật, mục tiêu thời gian tiếp nhận do nhóm tự đặt, và ngoài giờ trực ca nằm trong hàng đợi cho tới 8h sáng.

**Lỗi phải chặn bằng quyền thực thi.** Câu “xác thực chỉ xác nhận danh tính” không đủ, vì nếu giao diện khách thực thi được việc nhả thì tiền vẫn có thể đi sai. Vì vậy:
- Màn hình khách chỉ có hướng dẫn, xác thực và chuyển người, **không có nút nhả**.
- Quyết định nhả đi qua bàn duyệt của tư vấn viên với hồ sơ đang chờ, giao dịch đúng mã và còn giữ, xác thực hợp lệ và căn cứ đã đối chiếu.
- Quy tắc sản phẩm: T5-01 có dấu hiệu bị thúc giục nên không được duyệt nhả trong luồng này. Chuyên viên cần kiểm tra thêm qua kênh chính thức; nếu chưa có căn cứ độc lập thì giao dịch tiếp tục tạm giữ. Prototype không có nút nhả cho ca này.
|

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
  - `giu_giao_dich`: **Tự làm** — AI tự động giữ khi phát hiện dấu hiệu rủi ro, phiên là chủ ví và đúng mã giao dịch đang chờ xử lý.
  - `yeu_cau_xac_minh`: **Tự làm** — AI tự mở hộp thoại xác thực khuôn mặt riêng khi hệ thống yêu cầu xác minh danh tính và khách bấm thao tác.
  - `chuyen_tu_van_vien`: **Tự làm** — AI tự tổng hợp dữ kiện tối thiểu và đẩy vào hàng đợi hỗ trợ khi khách bấm chuyển.
  - `nha_giao_dich`: **Cần người duyệt**:
    - *Ai duyệt*: Chuyên viên xử lý giao dịch có thẩm quyền (phê duyệt tại giao diện bàn duyệt riêng, không duyệt trong chat).
    - *Người duyệt thấy gì*: Mã giao dịch, số tiền, bên nhận, trạng thái giữ, kết quả xác thực danh tính, lời khai của khách và cảnh báo thúc giục.
    - *Quá hạn thì sao*: Giao dịch tiếp tục giữ nguyên trạng thái tạm giữ an toàn, không bao giờ tự động nhả tiền khi hết hạn.
  - `khoa_vi`: **Cấm** — AI tuyệt đối không tự khóa ví qua chat; chỉ hướng dẫn kênh khóa khẩn cấp chính thức (từ thiết bị tin cậy/tổng đài).
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
  - `giu_giao_dich`: Giữ nhầm làm khách chậm trễ (khách chịu phiền toái, nhưng đảo lại được bằng cách duyệt nhả); nếu bắt chờ duyệt mới giữ thì tiền của nạn nhân (T5-01) đã bay mất, không thể đảo ngược.
  - `yeu_cau_xac_minh`: Mở nhầm thì khách hủy hoặc thử lại (đảo lại được tức thì); nếu chờ người duyệt mới mở xác thực thì khách viện phí (T5-02) bị chậm trễ vô ích tại quầy.
  - `chuyen_tu_van_vien`: Chuyển nhầm làm tăng tải tổng đài (khách hủy được trong 1.8s hàng đợi); nếu bắt duyệt mới được chuyển người thì ca khẩn cấp (T5-01, T5-05) bị chậm cứu hộ.
  - `nha_giao_dich`: Nhả sai làm tiền vào tay kẻ lừa đảo (khách mất tiền vĩnh viễn, ví chịu rủi ro pháp lý, hoàn toàn KHÔNG đảo lại được); chờ người duyệt làm tốn thời gian nhưng AI đã điền sẵn hồ sơ để giảm thiểu độ trễ cho T5-02.
  - `khoa_vi`: Khóa nhầm làm toàn bộ ví bị đóng băng nhiều giờ, mở lại rất phức tạp (khách chịu gián đoạn lớn); AI không tự khóa để tránh tác động nhầm. Chờ khóa qua kênh chính thức có thể làm chủ ví thật mất thêm thời gian, nên AI phải chỉ ngay kênh khẩn cấp từ thiết bị tin cậy/tổng đài thay vì chỉ từ chối.
- Phương án đã cân nhắc và vì sao loại.
  - `giu_giao_dich`: Cân nhắc giữ theo lời chat khi thiếu mã giao dịch -> Loại vì dễ giữ nhầm giao dịch hợp lệ khác.
  - `yeu_cau_xac_minh`: Cân nhắc thu thập ảnh/OTP trực tiếp trong khung chat -> Loại vì vi phạm bảo mật dữ liệu nhạy cảm.
  - `chuyen_tu_van_vien`: Cân nhắc tự động chuyển mọi ca cho tư vấn viên -> Loại vì gây nghẽn tổng đài, ca đơn giản như T5-10 AI tự tra cứu trả lời ngay.
  - `nha_giao_dich`: Cân nhắc cho AI tự nhả khi khách đã xác thực khuôn mặt -> Loại vì ở T5-01 kẻ lừa đảo ép nạn nhân tự quét mặt; bắt buộc cần con người đối chiếu.
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
  - **T5-01 (Bị giục lừa đảo)**: Cái sai bị chặn. Dù khách đã xác thực mặt, ca có nhãn dấu hiệu thúc giục không thể duyệt nhả; từ chối hoặc hết hạn vẫn giữ tiền. Nếu giao dịch chưa rõ, phải chọn lại giao dịch trước khi có hồ sơ; không được tiếp tục dựa trên mã cũ.
  - **T5-02 (Viện phí gấp)**: Không hỏi lại mã giao dịch khi mã đã khớp. AI mở hộp thoại xác thực; xác thực xong khách vẫn chủ động bấm **Chuyển tư vấn viên**. Chỉ sau khi khách gửi yêu cầu và chuyên viên tiếp nhận, hồ sơ mới vào bàn duyệt; nếu khách chọn giao dịch khác thì phải hỏi/chọn lại, không tiếp tục hồ sơ cũ.
  - **T5-10 (Tra cứu hóa đơn - bình thường)**: Người dùng bình thường không phải chờ vô ích. AI đọc nguồn sự thật thấy giao dịch đã hoàn tất, lập tức trả lời và gửi biên nhận; không tự giữ giao dịch, không bắt xác thực lại và không đẩy sang tư vấn viên.
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
  - **Tách bước**: Tách riêng việc xếp hàng hỗ trợ (`chuyen_tu_van_vien`) khỏi quyết định tiền (`nha_giao_dich`); tách việc xác thực danh tính ra modal bảo mật riêng biệt.
  - **Luật thêm**:
    - *Khóa nút duyệt*: Ca được gắn nhãn có dấu hiệu thúc giục không thể duyệt nhả trong luồng thường (chặn đứng T5-01). Trong prototype, nhãn này là dữ liệu cố định của ca mô phỏng, không phải bộ nhận diện từ khóa hay bộ phân loại đã kiểm chứng.
    - *Hủy hiệu lực hồ sơ*: Nếu khách bấm "Sửa thông tin" hoặc đổi mã giao dịch trong khi chờ duyệt, hồ sơ lập tức chuyển sang trạng thái `Cần đối chiếu lại hồ sơ`.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.
  - *Lời hứa cũ (thiếu an toàn)*: “Giao dịch đang được xử lý, hệ thống sẽ tự động mở lại tiền ngay khi bạn hoàn tất xác thực khuôn mặt.”
  - *Lời hứa mới (chuẩn mức tự chủ)*: “Giao dịch đang tạm giữ để bảo vệ bạn. Bạn có thể mở xác thực danh tính và yêu cầu hỗ trợ ngay tại đây. Chuyên viên xử lý giao dịch sẽ quyết định nhả tiền sau khi đối chiếu đầy đủ căn cứ an toàn.”

## §5. Prototype  (chặng 5)
Mở [prototype/index.html](prototype/index.html). HTML, CSS và JavaScript nằm trong một tệp. Prototype có hai màn hình **Khách hàng** và **Bàn duyệt**, cùng hộp thoại xác thực riêng. Cả 10 ca dùng tin mẫu và dữ liệu cục bộ; câu trả lời được chọn theo kịch bản, không gọi mô hình AI hay dịch vụ ví thật. Xác thực, hàng đợi hỗ trợ và kết quả giao dịch đều là mô phỏng. Vai **Bàn duyệt** minh họa quy trình và quyền hạn, không phải công cụ vận hành thực tế.

**Ca demo mặc định T5-01.** Gửi tin → xem trạng thái giao dịch, ví và danh tính → gửi yêu cầu chuyển tư vấn viên → theo dõi hàng đợi → hủy rồi gửi lại → đổi sang Bàn duyệt → tiếp nhận → từ chối hoặc mô phỏng hết hạn → trở về màn hình khách xem giao dịch vẫn tạm giữ. T5-02 minh họa luồng xác thực riêng; khách vẫn chủ động gửi yêu cầu hỗ trợ, và chỉ tư vấn viên mới có thể duyệt nhả sau khi đối chiếu.

| Vị trí trên UI | Nội dung · HAX/PAIR · mức tự chủ | Ảnh |
|---|---|---|
| **1 · Từ hệ thống** | Trạng thái giao dịch, ví và danh tính được tách riêng để người dùng đối chiếu. **HAX G11** · Tool tra trạng thái tự làm; không tự nhả giao dịch. | [Ảnh 01](prototype/01-t5-01-khach-hang.jpeg) |
| **2 · Chuyển/Hủy yêu cầu** | Khách chủ động gửi yêu cầu; có thể hủy khi còn trong hàng đợi. **HAX G10** · Chuyển hỗ trợ tự thực hiện sau xác nhận của khách; không đồng nghĩa với duyệt nhả. | [Ảnh 01](prototype/01-t5-01-khach-hang.jpeg) |
| **3 · Sửa thông tin** | Khách có thể sửa lời khai hoặc chọn giao dịch khác; hồ sơ chờ duyệt cần đối chiếu lại. **HAX G9** · Khách kiểm soát việc sửa; quyết định nhả vẫn cần người duyệt. | [Ảnh 01](prototype/01-t5-01-khach-hang.jpeg) |
| **4 · Yêu cầu hỗ trợ** | Chuỗi trạng thái cho biết yêu cầu đã gửi, đang chờ hay đã được tiếp nhận. **HAX G11** · Hệ thống tự cập nhật trạng thái sau sự kiện; việc nhả tiền không tự động theo trạng thái hàng đợi. | [Ảnh 01](prototype/01-t5-01-khach-hang.jpeg) và [Ảnh 02](prototype/02-t5-01-ban-duyet.jpeg) |
| **5 · Bàn duyệt** | Chuyên viên thấy giao dịch, danh tính, lời khách và lý do khóa nút nhả ở T5-01. **HAX G11** · Nhả giao dịch cần người duyệt; từ chối/hết hạn thì tiền vẫn tạm giữ. | [Ảnh 02](prototype/02-t5-01-ban-duyet.jpeg) |

**Quyết định hiển thị độ chắc và bằng chứng.** Không hiển thị phần trăm tin cậy vì dữ liệu mẫu không có điểm số đã hiệu chuẩn. Thay vào đó, giao diện tách dữ kiện **Từ hệ thống**, lời khai **Bạn cho biết** và **Dữ kiện còn thiếu**, để không biến lời khách thành dữ kiện hệ thống. Khi chưa xác định được giao dịch, hiển thị **Chưa rõ** thay vì đoán. Nhóm cũng cân nhắc dùng một màn hình chung có nút duyệt, nhưng chọn bàn riêng để làm rõ ai đang hỗ trợ, ai được xem căn cứ và ai có quyền quyết định. Các mốc **Đã gửi yêu cầu**, **Đang chờ**, **Tư vấn viên đã tiếp nhận** chỉ xuất hiện theo sự kiện tương ứng; hết hạn hoặc từ chối không làm giao dịch tự nhả.


## §6. Tốt là gì (R6)

Nhóm dùng ba tiêu chí dưới đây để chấm thử thiết kế trên hai ca khó: T5-01 (bị giục chuyển tiền) và T5-02 (gấp ở quầy viện phí). Kết quả hiện có được ghi nhận từ kiểm thử JavaScript với DOM và đồng hồ mô phỏng.

| Tiêu chí | Mã ca | Thao tác đã thử | Kỳ vọng | Kết quả quan sát | Bằng chứng | Kết luận |
|---|---|---|---|---|---|---|
| **P1 · Đúng quyền nhả giao dịch** | T5-01, T5-02 | T5-01: tiếp nhận, đối chiếu điều kiện bật nút duyệt, rồi thử từ chối và hết hạn ở hai lượt riêng. T5-02: xác thực, khách gửi yêu cầu, chuyên viên tiếp nhận, đối chiếu và duyệt nhả. | T5-01 không thể duyệt nhả trong luồng này; từ chối/hết hạn đều giữ tiền. T5-02 chỉ hoàn tất sau khi khách gửi yêu cầu và chuyên viên đối chiếu, bấm duyệt; ví mô phỏng xác nhận kết quả. | Điều kiện `canApprove()` chỉ cho phép hồ sơ loại viện phí, đã tiếp nhận, đã đối chiếu, danh tính hợp lệ và giao dịch còn giữ; do đó nút T5-01 bị vô hiệu hóa. Từ chối/hết hạn vẫn hiện **Tạm giữ**. T5-02 chỉ hiện **Hoàn tất** sau duyệt. Đây là kiểm tra logic/mã và ảnh; trình duyệt chưa chạy. | E01, E02, E04, E05 | **Pass logic** · Trình duyệt **Chưa chạy** |
| **P2 · Hỗ trợ và phục hồi đúng bước** | T5-01, T5-02 | T5-01: gửi yêu cầu, hủy khi đang chờ, gửi lại và tiếp nhận. T5-02: thử lỗi xác thực và chuyển tư vấn viên trước xác thực ở các lượt riêng. | Hệ thống chỉ tạo hồ sơ khi khách bấm chuyển; sau xác thực thành công cũng không tự gửi. Khi thiếu/đổi giao dịch phải chọn lại, không tiếp tục hồ sơ cũ. Lỗi xác thực cho phép thử lại hoặc chuyển người. | T5-01 hiện **Đã gửi yêu cầu → Đang chờ → Đã hủy yêu cầu**; chỉ khi chuyên viên bấm tiếp nhận mới hiện **Tư vấn viên đã tiếp nhận**. T5-02 xác thực không tự chuyển; khách bấm chuyển mới tạo yêu cầu. Chuyển trước xác thực chỉ tạo yêu cầu hỗ trợ, chưa tạo hồ sơ duyệt nhả. | E01, E04, E06, E07 | **Pass logic** · Trình duyệt **Chưa chạy** |
| **P3 · Hiển thị đúng trạng thái và nguồn** | T5-01, T5-02, đối chiếu T5-09 | T5-01: mô phỏng lỗi đọc và đổi giao dịch khi hồ sơ đang chờ duyệt. T5-02: kiểm tra hai trạng thái sau xác thực. T5-09: kiểm tra ca ví bị khóa. | Lỗi đọc hoặc đổi giao dịch làm ẩn mã cũ. Trạng thái giao dịch, ví và danh tính được hiển thị theo dữ liệu của từng nguồn. | T5-01 hiện giao dịch **Chưa rõ** và ẩn mã cũ. Khi đổi giao dịch, hồ sơ hiện **Cần đối chiếu lại hồ sơ**. T5-02 hiển thị riêng **Danh tính: Đã xác thực** và **Giao dịch: Tạm giữ**. T5-09 hiển thị **Ví: Đã khóa** và **Giao dịch: Chưa rõ**. | E03, E04, E08 và kiểm tra T5-09 | **Pass logic** · Trình duyệt **Chưa chạy** |
**Tổng hợp.** Cả 3 tiêu chí đạt ở mức logic. Theo kiểm tra logic/nội dung HTML của nhóm, 16 nhóm kiểm tra bao phủ cả 10 ca mô phỏng; đây chưa phải kết quả kiểm thử tương tác trên trình duyệt hay dịch vụ thật.

**Lỗi/rủi ro còn lại.** Nhãn thúc giục trong prototype là fixture cố định của T5-01, không phải bộ nhận diện ngôn ngữ; chưa kiểm tra được nhận diện nhầm hoặc bỏ sót trong sản phẩm thật. Ngoài ra, chưa đặt SLA tiếp nhận tư vấn viên. Hai giới hạn này không làm prototype tự nhả tiền, nhưng còn ảnh hưởng phát hiện đúng ca và thời gian khách phải chờ.

**Phạm vi xác minh còn thiếu.** Chưa chạy kiểm thử tương tác trên trình duyệt; các kết quả trên là kiểm tra logic/mã và ảnh hiện có. Cần bấm lại T5-01/T5-02, xác nhận trực tiếp nút duyệt T5-01 không thể bật, luồng T5-02 chỉ gửi sau thao tác khách, kiểm tra giao diện và lưu ảnh bằng chứng. Ghi Pass/Fail theo kết quả thực tế; không coi trạng thái mô phỏng là xác minh sản phẩm thật.
## §7. Demo
### Kịch bản demo đề xuất

1. **T5-01 · Bị giục chuyển tiền:** gửi tin mẫu; chỉ trạng thái giao dịch, ví và danh tính là các dữ kiện riêng; bấm **Chuyển tư vấn viên**; chờ yêu cầu vào hàng đợi; đổi sang **Bàn duyệt** và bấm **Tiếp nhận yêu cầu**. Chỉ vào lời báo bị thúc giục và nút **Duyệt nhả giao dịch** bị khóa. Bấm **Từ chối nhả** (hoặc chạy lại để mô phỏng hết hạn), rồi quay về màn hình khách để xác nhận giao dịch vẫn **Tạm giữ**.
2. **T5-02 · Viện phí gấp:** chọn ca và chạy lại; bấm **Mở xác thực** rồi chọn xác thực thành công. Nhấn mạnh xác thực không tự nhả tiền và không tự gửi yêu cầu; khách phải bấm **Chuyển tư vấn viên**. Sau khi chuyên viên tiếp nhận, đánh dấu đã đối chiếu rồi duyệt nhả; quay về màn hình khách xem kết quả mô phỏng.

### Ca lạ dùng để diễn tập

- **Tình huống:** Khi hồ sơ T5-02 đang chờ duyệt, khách báo mình đang hỏi về giao dịch khác.
- **Prototype làm gì:** Khách chọn **Sửa thông tin** → **Tôi đang hỏi về giao dịch khác**. Mã giao dịch cũ được ẩn và hồ sơ chuyển sang **Cần đối chiếu lại hồ sơ**; không thể dùng căn cứ cũ để duyệt nhả.
- **Nhóm có đổi ý không:** Không đổi mức **Cần người duyệt** cho `nha_giao_dich`. Ca này củng cố luật hủy hiệu lực hồ sơ khi căn cứ thay đổi. Đây là ca nhóm tự chọn để diễn tập, không khẳng định đã phát sinh trong buổi demo thật.

### Phân công khi trình bày

- **Dương Quang Bảo:** mở đầu bằng lát cắt/phạm vi (§1), sau đó giải thích hai kịch bản đắt nhất và cách xử lý khi AI sai (§3).
- **Nguyễn Vũ Khanh:** trình bày 10 kịch bản rủi ro và lý do chọn T5-01/T5-02 (§2); giải thích mức tự chủ, cost-of-error và vì sao `nha_giao_dich` cần người duyệt (§4).
- **Đặng Long Nhật:** điều khiển prototype và demo T5-01/T5-02 (§5); trình bày tiêu chí kiểm thử, kết quả và giới hạn còn lại (§6).

Mỗi người cần giải thích được phần mình phụ trách và sẵn sàng trả lời câu hỏi: “Hành động này sai thì ai mất gì, vì sao tool ở mức đó?”.