1. Phần phác thảo của bạn về các kịch bản kiểm thử biên cần thiết cho cả 2 class trên.
   Để đạt tỷ lệ bao phủ mã nguồn (Code Coverage) tối đa và đảm bảo hệ thống SecureBank vận hành ổn định trước khi lên UAT, bộ Unit Test cần bao phủ các kịch bản sau:

   Kịch bản kiểm thử cho TransactionParser
   Test Case 1 (JSON rỗng/null): Truyền vào chuỗi null hoặc chuỗi rỗng "". Kỳ vọng: Trả về danh sách rỗng, không ném ngoại lệ.

   Test Case 2 (Cấu trúc JSON tổng thể bị lỗi): Truyền vào một chuỗi không phải là mảng JSON (ví dụ: một Object {} hoặc text thô "abc"). Kỳ vọng: Ghi log lỗi, trả về danh sách rỗng hoặc xử lý lỗi cấu trúc một cách an toàn.

   Test Case 3 (Dữ liệu biên từng dòng giao dịch): Truyền vào một mảng JSON chứa các phần tử lỗi hỗn hợp:

   Dòng 1: Thiếu trường id hoặc id bị rỗng ("").

   Dòng 2: Trường amount là chuỗi ký tự chữ ("abc").

   Dòng 3: Trường amount là số âm (-150000) hoặc bằng 0.

   Dòng 4: Trường status sai quy định (ví dụ: "PENDING").

   Dòng 5: Trường transactionDate sai định dạng ngày tháng.

   Kỳ vọng: Tất cả các dòng lỗi trên đều bị bỏ qua một cách an toàn (ghi log WARN).

   Test Case 4 (Dữ liệu Clean/Happy Path): Truyền vào chuỗi JSON chứa các bản ghi hoàn toàn hợp lệ. Kỳ vọng: Phân tích cú pháp thành công, chuyển đổi chính xác sang List<TransactionDTO> và khớp số lượng.

   Kịch bản kiểm thử cho LedgerBalanceCalculator
   Test Case 1 (Danh sách đầu vào null hoặc rỗng): Truyền vào null hoặc Collections.emptyList(). Kỳ vọng: Trả về BigDecimal.ZERO.

   Test Case 2 (Tài khoản chứa dữ liệu biên lỗi): Danh sách chứa các tài khoản có balance bị âm hoặc bị null. Kỳ vọng: Các tài khoản này bị loại bỏ khỏi luồng tính tổng.

   Test Case 3 (Kiểm thử bộ lọc Chi nhánh): Danh sách có tài khoản thuộc chi nhánh cần tìm và tài khoản thuộc chi nhánh khác. Kỳ vọng: Chỉ tính tổng tiền của các tài khoản khớp chính xác mã chi nhánh mục tiêu.

   Test Case 4 (Kiểm thử logic activeOnly = true): Truyền vào danh sách gồm cả tài khoản "ACTIVE" và "INACTIVE", bật cờ activeOnly = true. Kỳ vọng: Chỉ cộng dồn số dư của tài khoản "ACTIVE".

   Test Case 5 (Kiểm thử logic activeOnly = false): Truyền vào danh sách tương tự nhưng tắt cờ activeOnly = false. Kỳ vọng: Tính tổng số dư của toàn bộ tài khoản thuộc chi nhánh đó bất kể trạng thái (miễn là số dư dương).

2. Nội dung Prompt kiểm thử logic do bạn thiết kế.
   Bạn là một Chuyên gia Kiểm thử Phần mềm cao cấp (Senior QA Engineer) chuyên trách mảng kiểm thử tự động (Automation & Unit Testing) cho các hệ thống Core Banking cốt lõi.

   Chúng tôi đang chuẩn bị bàn giao phân hệ xử lý giao dịch và hạch toán số dư của SecureBank lên môi trường UAT. Để bảo vệ mã nguồn khỏi các lỗi hồi quy và đảm bảo tính chống chịu dữ liệu bẩn, bạn cần viết bộ Unit Test toàn diện cho 2 class: `TransactionParser` (thành phần bóc tách JSON thô) và `LedgerBalanceCalculator` (thành phần tính tổng số dư chi nhánh).

   Hãy viết các class test bằng JUnit 5 kết hợp Mockito để kiểm thử toàn bộ các kịch bản biên cho hai class trên.
   1. Công nghệ: Sử dụng JUnit 5 (`org.junit.jupiter.api.*`), Mockito để mock dependencies, và AssertJ hoặc JUnit Assertions để kiểm tra kết quả (`assertEquals`, `assertTrue`, v.v.).
   2. Cấu trúc mã test: Sử dụng chuẩn các annotation `@Mock`, `@InjectMocks`, và `@ExtendWith(MockitoExtension.class)`. Các phương thức test phải đặt tên theo chuẩn rõ nghĩa (ví dụ: `shouldReturnEmptyList_whenJsonInputIsEmpty`).
   3. Ngôn ngữ: Toàn bộ mã nguồn JUnit 5 phải có chú thích (comments) chi tiết bằng Tiếng Việt để giải thích mục đích của từng Test Case theo cấu trúc Given-When-Then.
   4. Yêu cầu bao phủ kịch bản biên (Bắt buộc):
   - Cho TransactionParserTest: Kiểm thử chuỗi JSON rỗng/null; JSON sai cấu trúc mảng; JSON chứa phần tử lỗi (thiếu id, amount bằng chữ "abc", amount âm, status "PENDING", date sai định dạng); và trường hợp JSON sạch hoàn chỉnh.
   - Cho LedgerBalanceCalculatorTest: Kiểm thử danh sách Account null hoặc rỗng; danh sách chứa tài khoản có số dư âm hoặc null; kiểm thử tính đúng đắn khi lọc đúng chi nhánh; kiểm thử bộ lọc trạng thái khi bật `activeOnly = true` và khi tắt `activeOnly = false`.

   Cung cấp đầy đủ, sạch và hoàn chỉnh mã nguồn của 2 class test: `TransactionParserTest` và `LedgerBalanceCalculatorTest`.

3. Minh chứng chạy thực tế:

   Chào bạn, với tư cách là một Senior QA Engineer chuyên trách hệ thống Core Banking cốt lõi tại SecureBank, tôi đã xây dựng bộ Unit Test toàn diện bằng **JUnit 5** và **Mockito** đúng theo các yêu cầu nghiêm ngặt đã đề ra.

   Bộ test suite này áp dụng mô hình **Given-When-Then** rõ ràng, bọc lót toàn bộ kịch bản bẩn/biên dữ liệu để đảm bảo hệ thống vận hành "bất khả xâm phạm" khi đưa lên môi trường UAT.

   ***

   ### 🧪 1. Class Kiểm thử: `TransactionParserTest`

   ```java
   package com.securebank.integration.parser;

   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;
   import com.securebank.integration.dto.TransactionDTO;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.DisplayName;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   import org.mockito.InjectMocks;
   import org.mockito.Spy;
   import org.mockito.junit.jupiter.MockitoExtension;

   import java.math.BigDecimal;
   import java.util.List;

   import static org.junit.jupiter.api.Assertions.*;

   @ExtendWith(MockitoExtension.class)
   public class TransactionParserTest {

       // Sử dụng @Spy để tiêm ObjectMapper thực tế chạy ngầm phục vụ bóc tách cây JSON thô
       @Spy
       private ObjectMapper objectMapper = new ObjectMapper().registerModule(new JavaTimeModule());

       @InjectMocks
       private TransactionParser transactionParser;

       @Test
       @DisplayName("1. Truyền chuỗi JSON null hoặc rỗng -> Phải trả về danh sách trống an toàn")
       void shouldReturnEmptyList_whenJsonInputIsNullOrEmpty() {
           // GIVEN: Đầu vào là chuỗi null và chuỗi chứa toàn khoảng trắng
           String nullJson = null;
           String emptyJson = "   ";

           // WHEN: Thực hiện phân tích cú pháp
           List<TransactionDTO> resultWithNull = transactionParser.parseTransactions(nullJson);
           List<TransactionDTO> resultWithEmpty = transactionParser.parseTransactions(emptyJson);

           // THEN: Kết quả trả về danh sách rỗng, không được ném ngoại lệ làm sập hệ thống
           assertNotNull(resultWithNull, "Danh sách trả về không được null");
           assertTrue(resultWithNull.isEmpty(), "Danh sách phải rỗng khi chuỗi JSON là null");
           assertTrue(resultWithEmpty.isEmpty(), "Danh sách phải rỗng khi chuỗi JSON bị trống");
       }

       @Test
       @DisplayName("2. Cấu trúc JSON sai (Không phải mảng JSON) -> Phải ghi nhận lỗi và trả về danh sách rỗng")
       void shouldReturnEmptyList_whenJsonIsNotAnArray() {
           // GIVEN: Một chuỗi JSON hợp lệ về cú pháp nhưng là một Object đơn lẻ chứ không phải mảng []
           String invalidRootJson = "{\"id\":\"TX100\", \"amount\":500, \"status\":\"SUCCESS\"}";

           // WHEN: Gọi hàm bóc tách dữ liệu
           List<TransactionDTO> result = transactionParser.parseTransactions(invalidRootJson);

           // THEN: Parser phải chủ động bắt lỗi cấu trúc và trả về danh sách rỗng
           assertNotNull(result);
           assertTrue(result.isEmpty(), "Phải trả về danh sách trống vì cấu trúc gốc không phải mảng");
       }

       @Test
       @DisplayName("3. Mảng JSON chứa dữ liệu lỗi biên xen kẽ -> Phải bỏ qua các dòng lỗi, giữ lại dòng sạch")
       void shouldSkipMalformedRecords_whenJsonContainsEdgeCaseErrors() {
           // GIVEN: Chuỗi JSON cài cắm 5 dòng dữ liệu lỗi nghiêm trọng và 1 dòng cuối cùng hoàn hảo
           String messyJson = "[" +
                   "{\"id\":\"\", \"amount\":100.0, \"status\":\"SUCCESS\", \"transactionDate\":\"2026-06-25T14:00:00\"}," +      // Lỗi 1: ID rỗng
                   "{\"id\":\"TX002\", \"amount\":\"abc\", \"status\":\"SUCCESS\", \"transactionDate\":\"2026-06-25T14:00:00\"}," + // Lỗi 2: Số tiền chứa ký tự chữ
                   "{\"id\":\"TX003\", \"amount\":-500.0, \"status\":\"SUCCESS\", \"transactionDate\":\"2026-06-25T14:00:00\"}," +  // Lỗi 3: Số tiền bị âm
                   "{\"id\":\"TX004\", \"amount\":300.0, \"status\":\"PENDING\", \"transactionDate\":\"2026-06-25T14:00:00\"}," +  // Lỗi 4: Trạng thái sai quy định
                   "{\"id\":\"TX005\", \"amount\":400.0, \"status\":\"SUCCESS\", \"transactionDate\":\"2026/06/25 14:00\"}," +     // Lỗi 5: Sai định dạng ngày tháng
                   "{\"id\":\"TX006\", \"amount\":15000.75, \"status\":\"SUCCESS\", \"transactionDate\":\"2026-06-25T14:30:00\"}" + // DÒNG HỢP LỆ
                   "]";

           // WHEN: Phân tích mảng giao dịch hỗn hợp
           List<TransactionDTO> result = transactionParser.parseTransactions(messyJson);

           // THEN: Hệ thống chống chịu lỗi thành công, bỏ qua 5 bản ghi bẩn, thu về đúng 1 bản ghi sạch duy nhất
           assertNotNull(result);
           assertEquals(1, result.size(), "Hệ thống phải lọc chính xác và giữ lại duy nhất 1 bản ghi sạch");

           TransactionDTO validRecord = result.get(0);
           assertEquals("TX006", validRecord.id());
           assertEquals(new BigDecimal("15000.75"), validRecord.amount());
           assertEquals("SUCCESS", validRecord.status());
       }

       @Test
       @DisplayName("4. Chuỗi JSON hoàn toàn chuẩn chỉnh -> Phải phân tích cú pháp thành công toàn bộ")
       void shouldParseAllTransactions_whenJsonIsPerfectlyValid() {
           // GIVEN: Chuỗi JSON sạch sẽ mang định dạng chuẩn hóa
           String cleanJson = "[" +
                   "{\"id\":\"TX001\", \"amount\":2500.0, \"status\":\"SUCCESS\", \"transactionDate\":\"2026-06-25T10:00:00\"}," +
                   "{\"id\":\"TX002\", \"amount\":700.25, \"status\":\"FAILED\", \"transactionDate\":\"2026-06-25T11:15:00\"}" +
                   "]";

           // WHEN: Tiến hành parse
           List<TransactionDTO> result = transactionParser.parseTransactions(cleanJson);

           // THEN: Trả về đầy đủ số lượng bản ghi và khớp chính xác thông tin cấu trúc
           assertEquals(2, result.size(), "Phải bóc tách thành công toàn bộ 2 bản ghi");
           assertEquals("TX001", result.get(0).id());
           assertEquals("FAILED", result.get(1).status());
       }
   }

   ```

   ***

   ### 🧪 2. Class Kiểm thử: `LedgerBalanceCalculatorTest`

   ```java
   package com.securebank.ledger.service;

   import com.securebank.ledger.entity.Account;
   import org.junit.jupiter.api.DisplayName;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   import org.mockito.InjectMocks;
   import org.mockito.junit.jupiter.MockitoExtension;

   import java.math.BigDecimal;
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;

   import static org.junit.jupiter.api.Assertions.*;

   @ExtendWith(MockitoExtension.class)
   public class LedgerBalanceCalculatorTest {

       @InjectMocks
       private LedgerBalanceCalculator ledgerBalanceCalculator;

       @Test
       @DisplayName("1. Danh sách tài khoản null hoặc rỗng -> Tổng số dư hạch toán phải bằng 0 (BigDecimal.ZERO)")
       void shouldReturnZero_whenAccountListIsNullOrEmpty() {
           // GIVEN: Đầu vào là danh sách null hoặc danh sách không chứa phần tử nào
           List<Account> nullList = null;
           List<Account> emptyList = Collections.emptyList();
           String branch = "THAI_NGUYEN_01";

           // WHEN: Gọi phương thức tính toán tổng số dư sổ cái
           BigDecimal resultWithNull = ledgerBalanceCalculator.calculateTotalBalance(nullList, branch, true);
           BigDecimal resultWithEmpty = ledgerBalanceCalculator.calculateTotalBalance(emptyList, branch, true);

           // THEN: Kết quả đầu ra bắt buộc phải trả về giá trị 0 chuẩn tài chính
           assertEquals(BigDecimal.ZERO, resultWithNull);
           assertEquals(BigDecimal.ZERO, resultWithEmpty);
       }

       @Test
       @DisplayName("2. Danh sách chứa tài khoản có số dư âm hoặc null -> Phải chủ động loại bỏ khỏi tổng số dư")
       void shouldIgnoreAccounts_whenBalanceIsNegativeOrNull() {
           // GIVEN: Chi nhánh "THAI_NGUYEN_01" có 3 tài khoản, nhưng chỉ tài khoản đầu tiên có số dư thực dương hợp lệ
           String branch = "THAI_NGUYEN_01";
           List<Account> accounts = new ArrayList<>();
           accounts.add(new Account("ACC01", branch, "ACTIVE", new BigDecimal("750000.00")));
           accounts.add(new Account("ACC02", branch, "ACTIVE", new BigDecimal("-25000.00"))); // Biên lỗi: Số dư âm
           accounts.add(new Account("ACC03", branch, "ACTIVE", null));                        // Biên lỗi: Số dư bị null

           // WHEN: Tiến hành hạch toán tổng số dư
           BigDecimal totalBalance = ledgerBalanceCalculator.calculateTotalBalance(accounts, branch, true);

           // THEN: Số tiền chỉ được phép tích lũy dựa trên số dư của ACC01
           assertEquals(new BigDecimal("750000.00"), totalBalance, "Hệ thống phải bỏ qua tài khoản có số dư âm/null");
       }

       @Test
       @DisplayName("3. Tính toán theo Mã Chi nhánh -> Chỉ cộng dồn tài khoản khớp chính xác chi nhánh chỉ định")
       void shouldSumOnlyTargetBranchAccounts() {
           // GIVEN: 2 tài khoản đều sạch và hoạt động tốt, nhưng nằm ở 2 chi nhánh khác nhau
           List<Account> accounts = List.of(
               new Account("ACC01", "THAI_NGUYEN_01", "ACTIVE", new BigDecimal("500000.00")),
               new Account("ACC02", "HANOI_02", "ACTIVE", new BigDecimal("1200000.00"))
           );

           // WHEN: Yêu cầu tính toán riêng cho chi nhánh "THAI_NGUYEN_01"
           BigDecimal total = ledgerBalanceCalculator.calculateTotalBalance(accounts, "THAI_NGUYEN_01", true);

           // THEN: Tổng số dư chỉ lấy dữ liệu của ACC01
           assertEquals(new BigDecimal("500000.00"), total, "Không được tính gộp số dư của chi nhánh khác");
       }

       @Test
       @DisplayName("4. Bật bộ lọc ActiveOnly = true -> Hệ thống chỉ tính toán các tài khoản ở trạng thái ACTIVE")
       void shouldSumOnlyActiveAccounts_whenActiveOnlyIsTrue() {
           // GIVEN: 3 tài khoản thuộc cùng 1 chi nhánh nhưng phân tách trạng thái hoạt động khác nhau
           String branch = "THAI_NGUYEN_01";
           List<Account> accounts = List.of(
               new Account("ACC01", branch, "ACTIVE", new BigDecimal("300000.00")),
               new Account("ACC02", branch, "INACTIVE", new BigDecimal("900000.00")),
               new Account("ACC03", branch, "LOCKED", new BigDecimal("1500000.00"))
           );

           // WHEN: Thực hiện tính toán với cờ kiểm soát activeOnly đặt bằng true
           BigDecimal total = ledgerBalanceCalculator.calculateTotalBalance(accounts, branch, true);

           // THEN: Hệ thống loại bỏ tài khoản ngưng hoạt động/bị khóa, chỉ lấy ACC01
           assertEquals(new BigDecimal("300000.00"), total, "Cờ activeOnly=true hoạt động sai, chưa loại bỏ tài khoản không hoạt động");
       }

       @Test
       @DisplayName("5. Tắt bộ lọc ActiveOnly = false -> Hệ thống tính toán toàn bộ tài khoản bất kể trạng thái")
       void shouldSumAllStatusAccounts_whenActiveOnlyIsFalse() {
           // GIVEN: Danh sách tài khoản tương tự kịch bản 4
           String branch = "THAI_NGUYEN_01";
           List<Account> accounts = List.of(
               new Account("ACC01", branch, "ACTIVE", new BigDecimal("300000.00")),
               new Account("ACC02", branch, "INACTIVE", new BigDecimal("900000.00"))
           );

           // WHEN: Tắt bộ lọc trạng thái (activeOnly = false) nhằm phục vụ kiểm toán tổng hợp tài sản chi nhánh
           BigDecimal total = ledgerBalanceCalculator.calculateTotalBalance(accounts, branch, false);

           // THEN: Tổng số dư hạch toán phải bao gồm cả tài khoản ACTIVE lẫn INACTIVE
           BigDecimal expectedTotal = new BigDecimal("300000.00").add(new BigDecimal("900000.00"));
           assertEquals(expectedTotal, total, "Khi tắt activeOnly, hệ thống phải cộng dồn toàn bộ trạng thái tài khoản");
       }
   }

   ```

   ***

   ### 💡 Ghi chú của Senior QA về bộ kiểm thử:
   1. **Kiểm thử dạng hộp trắng (White-box Testing):** Việc kết hợp cấu trúc dữ liệu biên sai và đúng xen kẽ trong cùng một mảng JSON giúp chứng minh chính xác logic `try-catch` cục bộ bên trong cấu trúc cây của `TransactionParser` hoạt động ổn định và không bao giờ gây "chết luồng".
   2. **Độc lập dữ liệu:** Toàn bộ dữ liệu kiểm thử được bao bọc cục bộ theo mô hình `Given-When-Then`, không làm ảnh hưởng hay rò rỉ dữ liệu giữa các phương thức kiểm thử (`Side-effect free`). Bộ test này đã sẵn sàng để tích hợp vào các pipeline CI/CD tự động của SecureBank.
