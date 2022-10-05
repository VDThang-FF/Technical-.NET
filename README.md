# Technical-.NET
<b><i>Menu</i></b>
<ol>
    <li><a href="#id1">Appsettings</a></li>
    <li><a href="#id2">CORS</a></li>
    <li><a href="#id3">Middleware</a></li>
    <li><a href="#id4">JWT(JSON Web Token)</a></li>
    <li><a href="#id5">Dependency Injection</a></li>
    <li><a href="#id6">ORM Framework</a></li>
    <li><a href="#id7">SQL Server, MySQL, NoSQL</a></li>
    <li><a href="#id8">Elasticsearch, Logging(Sight)</a></li>
    <li><a href="#id9">Mô hình MicroService, Microfrontend</a></li>
</ol>
<ul>
    <li id="id1">
        <b>Appsettings</b>
        <ul>
            <li>nôm na thì là nơi để cấu hình môi trường chạy tránh hard code khi đổi môi trường chạy mất công sửa code vừa lâu vừa dài đã thế còn k may là ăn bug
                <ul>
                    <li><b>Development</b>: chạy trên mỗi máy thằng code thôi</li>
                    <li><b>Local</b>: chạy trong cùng dải mạng (vd: dải mảng của FPT)</li>
                    <li><b>Staging</b>: test online --> customer test</li>
                    <li><b>Production</b>: online --> end user --> ai cũng dùng được</li>
                </ul>
            </li>
            <li>Trong dotnet core còn có thằng Resource - resx (gồm các file string, imagine,....) tương đương với thằng Appsettings (cũng dùng để cấu hình môi trường chạy), ngoài ra thì bên FE cũng có thằng .evn cũng có công dụng tương đương</li>
        </ul>
    </li>
    <li id="id2">
        <b>CORS</b>
        <ul>
            <li>sinh ra vì cơ chế same-origin-policy để chỉ định thằng nào mới dùng được API (security), nếu muốn cho phép thằng bên kia request chung với cookie thì phải set Access-Control-Allow-Credentials:true</li>
            <li>Thường thì khi chạy dứ án thì tại lắm người truy cập API quá nên toàn để public thằng nào cũng truy cập được nhưng bên phía khách hàng sẽ set ip mạng cho những ai mới được dùng (k hiểu vụ set ip mạng này lắm)</li>
        </ul>
    </li>
    <li id="id3">
        <b>Middleware</b>: hiểu nôm na là request đi vào kestrel đầu tiên rồi qua pipeline xong quay trở lại khi xử lý xong để trả về client. Các thành phần cấu tạo nên pipeline sẽ gọi là middleware<br/>
        <img src="https://images.viblo.asia/d4de0491-220f-4a55-833e-41cfda531038.png" alt="chu trinh" style="max-width:50%"/>
        <figure>
        <img src="https://images.viblo.asia/a459e9c2-f455-40ec-a725-d2b20b5d3369.png" alt="pipeline" style="max-width:50%"/>
        <figcaption>Vì thằng request nào cùng qua middleware nên nếu không tối ưu tốt middleware sẽ dẫn tới câu chuyện hiệu năng</figcaption>
        </figure>
    </li>
    <li id="id4">
        <b>JWT(JSON Web Token)</b><br/>
        <img src="https://topdev.vn/blog/wp-content/uploads/2017/12/jwt-la-gi.jpeg" alt="" style="max-width: 50%"/>
        <ul>
            <li>
                Lí do tại sao cần dùng token
                <ul>
                    <li>API Key --> k rõ thời hạn hết hạn, không định danh được</li>
                    <li>Muốn dùng hệ thống của tôi thì phải đăng nhập(định danh)</li>
                </ul>
            </li>
            <li>là một chuẩn mở (RFC 7519) có thể giúp bạn tạo ra một cái chuỗi mã hóa chứa các dữ liệu để bạn trao đổi thông tin giữa các hệ thống khác nhau một cách an toàn và đáng tin cậy. Các chuỗi thông tin thì sẽ được mã hóa một cách ngẫu nhiên, tùy hứng và không theo một trật tự sắp xếp nào.</li>
            <li>
                Những thành phần chính của JWT
                <ul>
                    <li>
                        <b>Header</b>: chứa kiểu dữ liệu và các thuật toán được sử dụng nhanh chóng để mã hóa ra chuỗi JWT một cách hoàn hảo. Ngoài ra, Header sẽ bao gồm 2 phần chính, đó là:
                        <ul>
                            <li><b>Typ(Type)</b>: Là loại token và được mặc định là một JWT</li>
                            <li><b>ALG (Algorithm)</b>: Được xem là thuật toán mới, sử dụng để mã hóa nhanh chóng (thuật toán chữ ký được sử dụng phổ biến như HMAC, SHA256, RSA)</li>
                        </ul>
                    </li>
                    <li>
                        <b>Payload</b>: nó đóng một vai trò rất quan trọng trong JWT, đây là nơi chứa các nội dung của thông tin (claim) mà người sử dụng muốn truyền đi ở bên trong chuỗi.Các thông tin này góp phần mô tả thực thể một cách đơn giản và nhanh chóng hoặc cũng có thể là các thông tin bổ sung thêm cho phần Header. Claims là một biểu thức về một thực thể chẳng hạn như người dùng (user) và một số metadata phụ trợ khác. Nhìn chung thì Claim được chia làm 3 loại là: reserved, public và private.
                        <ul>
                            <li><b>Reserved</b>: đây là những thông tin đã được quy định trong IANA JSON Web Token Claims registry. Tuy nhiên, những thông tin này không mang tính bắt buộc. Bạn có thể tùy vào từng ứng dụng khác nhau mà bạn có thể để đặt ra những điều kiện ràng buộc đối với những thông tin cần thiết nhất. Ví dụ như:
                                <ul>
                                    <li>iss (issuer): tổ chức phát hành của Token</li>
                                    <li>sub (subject): chủ đề Token</li>
                                    <li>aud (audience): đối tượng sử dụng Token</li>
                                    <li>exp (expired time): thời điểm token sẽ hết hạn</li>
                                    <li>nbf (not before time): token chưa hợp lệ trước thời điểm này</li>
                                    <li>iat (issued at): thời điểm token sẽ được phát hành, tính theo UNIX time</li>
                                    <li>jti: ID của JWT</li>
                                </ul>
                            </li>
                            <li><b>Public</b>: Được định nghĩa tùy theo ý muốn của người sử dụng JWT. Tuy nhiên để tránh tình trạng trùng lặp xảy ra thì nên được quy định ở trong <i>"IANA JSON Web Token Registry"</i> hoặc là 1 URL có chứa không gian tên không bị trùng lặp</li>
                            <li><b>Private</b>: Đây là phần thông tin thêm được dùng để truyền tải qua lại giữa các máy khách với nhau</li>
                        </ul>
                    </li>
                    <li>
                        <b>Signature</b>: là phần chữ ký bí mật, được tạo ra bởi mã hóa phần <b>Header</b> cùng với phần <b>Payload</b> kèm theo đó là một chuỗi secret (khóa bí mật). Khi ta kết hợp 3 phần đó lại với nhau, ta sẽ có một chuỗi JWT hoàn chỉnh nhất
                    </li>
                    <li>
                        <b>Bảo mật</b>
                        <ul>
                            <li>Giả sự bị hacker tấn công lấy mã token --> để thời hạn token ngắn để giảm thiểu rủi ro</li>
                            <li>Ngân hàng thường sẽ để token chỉ có hiệu lực trong 30s và không có cơ chế Refresh token</li>
                            <li>Về câu chuyện Refresh token thì sẽ phải để thời hạn của Refesh token > Access token (Access chưa hết và thời gian tự động gia hạn đã tèo trước rồi) nói rõ hơn là nếu trong khoảng thời gian Refresh token vẫn còn hiệu lực thì kể cả khi Access token có hết hạn thì khi refresh thì server sẽ trả lại 1 cặp access token + refresh token mới (thời gian refresh token cũng sẽ làm mới lại thời gian hết hạn luôn)</li>
                            <li>Câu truyện hình dung Access token và Refresh token: có 2 cái chìa khóa, chìa khóa A là chìa khóa nhà của bạn, và một cái chìa khóa R là chìa khóa nhà ông nội của bạn. Bạn có 1 chìa khóa A' cất ở nhà ông nội bạn, phòng khi mất chìa khóa A bạn sẽ đến đó lấy A' về.
                                <ul>
                                    <li>Chìa khóa A bạn thường xuyên đeo trên người, và đi lui đi tới ngoài đường rủi ro bạn bị thằng nào đó chặn lại cướp là rất cao.</li>
                                    </liChìa Khóa R bạn giấu ở nơi nào đó, nằm yên 1 chỗ, khả năng bị cướp là rất thấp, có cướp được cũng phải thêm 1 bước qua nhà ông nội để xin lại chìa A'.</li>
                                    <li>A chính là access token.</li>
                                    <li>R chính là refresh token.</li>
                                    <li>A' là token mới sau khi refresh bằng token R</li>
                                </ul>
                            </li>
                        </ul>
                        <img src="https://i.ibb.co/k9Tpvps/62f1fa8f2519af3b084e265e-JWT-authentication.jpg" alt="" style="width: 400px; height: 260px"/>
                    </li>
                    <li>
                        Bàn về câu truyện cơ chế
                        <ol>
                            <li>Khi user đăng nhập, BE check username/password => check đúng thì tạo token (access token + refresh token). Cả 2 Token này đều gửi lại cho Client theo restful api</li>
                            <li>Khi có 2 token này rồi thì Client lưu lại trong local storage (với web thì lưu trong cookie/local storage, với mobile thì lưu ở Async Storage).</li>
                            <li>Khi đã có 2 token, mỗi lần vào app (hoặc call api) app sẽ check trong storage có token hay không?
Nếu có access token thì cho phép call tới api để xác minh login => nếu access token hết hạn => check refresh token => có refresh token thì gửi lên be xin access token về xài tiếp, trường hợp refresh token hết hạn thì tự động log out (cũng như call tới be sẽ trả về lỗi chưa authenticate).</li>
                        </ol>
                    </li>
                </ul>
            </li>
        </ul>
    </li>
    <li id="id5">
        Dependency Injection
        <ul>
            <li>
                Các kiểu Dependency Injection
                <ul>
                    <li>Constructor injection: Các dependency (biến phụ thuộc) được cung cấp thông qua constructor (hàm tạo lớp).</li>
                    <li>Getter injection: Các dependency sẽ được truyền vào 1 class thông qua các setter method (hàm setter).</li>
                    <li>Interface injection: Dependency sẽ cung cấp một Interface, trong đó có chứa hàm có tên là Inject. Các client phải triển khai một Interface mà có một setter method dành cho việc nhận dependency và truyền nó vào class thông qua việc gọi hàm Inject của Interface đó.</li>
                </ul>
            </li>
        </ul>
    </li>
    <li id="id6">
        ORM Framework (Dapper, Entity Framework, ADO.NET)
        <ul>
            <li>
                So sánh
                <ul>
                    <li>
                        ADO.NET
                        <ul>
                            <li>
                                Ưu điểm:
                                <ul>
                                    <li>Thao tác bằng SQL Queries nên dễ dàng tunning để đạt performance cao, dễ dàng sửa đổi</li>
                                </ul>
                            </li>
                            <li>
                                Nhược điểm:
                                <ul>
                                    <li>Thao tác bằng SQL Queries nên dễ dàng tunning để đạt performance cao, dễ dàng sửa đổi</li>
                                </ul>
                            </li>
                        </ul>
                    </li>
                </ul>
            </li>
            <li>Phân tích các trường hợp sử dụng</li>
        </ul>
    </li>
    <li id="id7">
        SQL Server, MySQL, NoSQL
        <ul>
            <li>Index, Lock, </li>
        </ul>
    </li>
    <li id="id8">
        Elasticsearch, Logging(Sight)
    </li>
    <li id="id9">
        Mô hình MicroService, Microfrontend
    </li>
</ul>
