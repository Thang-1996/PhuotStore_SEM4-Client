<template>
  <div>
    <!-- About start -->
    <section class="section">
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-4 d-none d-lg-block">
            <img src="assets/img/about.jpg" alt="service" />
          </div>
          <div class="col-lg-8">
            <div class="mr-lg-30">
              <div class="section-title text-left">
                <h4 class="title">Giúp Trải nghiệm cắm trại của bạn thú vị</h4>
              </div>
              <div class="row">
                <div class="col-lg-6 mb-lg-30">
                  <p>
                    Thay vì một chuyến nghỉ dưỡng xa hoa, và cao cấp bạn có thể
                    trại nghiệm tự nhiên trong lành với chi phí vừa phải giúp
                    bạn vừa có một chuyến đi chơi bằng cách mua những đồ dùng hỗ
                    trợ của chúng tôi
                  </p>
                  <p>Còn chần chờ gì mà không khám phá ngay</p>
                </div>
                <div class="col-lg-6 mb-lg-30" style="display: flex">
                  <nuxt-link
                    :to="{ path: `/shop` }"
                    style="align-self: center"
                    class="sigma_btn-custom"
                    >Shop Now</nuxt-link
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- About end -->
    <!-- Counter & Video End -->
    <!-- Blog Start -->
    <div class="col-lg-12">
      <!-- Product Count & Orderby Start -->
      <div class="section-title text-center">
        <h4 class="title">Sản phẩm nổi bật</h4>
      </div>
      <!-- Product Count & Orderby End -->

      <div class="row masonry">
        <div
          v-for="(product, index) in products"
          :key="index"
          class="col-md-4 col-sm-6 masonry-item"
        >
          <div class="sigma_product">
            <div class="sigma_product-thumb">
              <nuxt-link :to="{ path: `/shop/${product.productID}` }"
                ><img
                  style="width: 100%; height: 300px"
                  :src="JSON.parse(product.images)[0]"
                  alt="product"
              /></nuxt-link>
              <div class="sigma_product-controls">
                <a href="#" data-toggle="tooltip" title="Wishlist">
                  <i class="far fa-heart"></i>
                </a>
                <a
                  data-toggle="tooltip"
                  title="Add To Cart"
                  @click="addToCard(product)"
                >
                  <i class="far fa-shopping-basket"></i>
                </a>
                <a href="#" data-toggle="tooltip" title="Quick View">
                  <i
                    data-toggle="modal"
                    data-target="#quickViewModal"
                    class="far fa-eye"
                  ></i>
                </a>
              </div>
            </div>
            <div class="sigma_product-body">
              <h5 class="sigma_product-title">
                <nuxt-link :to="{ path: `/shop/${product.productID}` }">{{
                  product.productName
                }}</nuxt-link>
              </h5>
              <div class="sigma_product-price">
                <span>{{ formatPrice(product.price) }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="section section-padding pt-0" style="margin-top: 100px">
      <div class="container">
        <div class="section-title text-center">
          <h4 class="title">Blog</h4>
        </div>
        <div class="row">
          <!-- Article Start -->
          <div v-for="blog in blogs" :key="blog.id" class="col-lg-4 col-md-6">
            <article class="sigma_post">
              <div class="sigma_post-thumb">
                <nuxt-link :to="{ path: `/blog/${blog.id}` }">
                  <img :src="blog.thumbnail" style="height: 300px" alt="post" />
                </nuxt-link>
              </div>
              <div class="sigma_post-body" style="height: 500px">
                <div style="height: 370px">
                  <h5>
                    <nuxt-link :to="{ path: `/blog/${blog.id}` }">{{
                      blog.title
                    }}</nuxt-link>
                  </h5>
                  <p>
                    {{ blog.description }}
                  </p>
                </div>

                <div class="sigma_post-footer">
                  <nuxt-link :to="{ path: `/blog/${blog.id}` }"
                    >Admin - {{ blog.createAt }}</nuxt-link
                  >
                  <nuxt-link
                    :to="{ path: `/blog/${blog.id}` }"
                    class="btn-link"
                  >
                    Read More <i class="far fa-arrow-right"></i>
                  </nuxt-link>
                </div>
              </div>
            </article>
          </div>
        </div>
      </div>
    </div>

    <!-- Blog End -->
  </div>
</template>

<script>
const blogs = [
  {
    id: 1,
    title: 'REVIEW 3 ĐỊA ĐIỂM CẮM TRẠI GẦN HÀ NỘI ĐẸP VÀ RỘNG THOÁNG NHẤT',
    thumbnail:
      'https://file.hstatic.net/1000169629/file/cam-trai-gan-ha-noi_00b822796ee04a5e8d06aaca8dec911d.jpg',
    images: [
      'https://file.hstatic.net/1000169629/file/khu-cam-trai-o-ban-rom_a00955f4e45b4703b12876aa45baafb9.jpg',
      'https://file.hstatic.net/1000169629/file/di-cam-trai-ban-rom_9f1f1a447a4b444894e15e4469a04985.jpg',
      'https://file.hstatic.net/1000169629/file/s-nvh06856_a1e89c70810442b188adea05bdc9c964.jpg',
    ],
    createAt: 'September 1, 2021',
    content: [
      'Khu du lịch sinh thái Bản Rõm Sóc Sơn đúng nghĩa là một khu cắm trại rất phát triển hình thức du lịch với các dịch vụ cho thuê lều trại, bếp nướng hay những món đồ dùng cần thiết để các bạn có một ngày vui chơi trọn vẹn và đúng với không gian cắm trại nhất tại đây.\n' +
        '\n' +
        'Đặc biệt bạn có thể dành trọn một ngày để vui chơi và nghỉ dưỡng hay tham gia những hoạt động khám phá thú vị và chẳng có phút giây nào khiến bạn nhàm chán cả.',
      "Camping Sport Đồng Mô là địa điểm cắm trại cách Hà Nội chỉ 40' đi xe với cung đường đẹp, nằm trong quần thể có rừng, núi, sông hồ bao quanh tạo không gian xanh mát và không khí rất trong lành. Thật tuyệt vời nếu mỗi cuối tuần bạn và gia đình hay công ty có thể tổ chức một chuyến đi chơi ở đây và tham gia các hoạt động BBQ hay Team buiding phải không nào? Ngày 19/12/2020 tới đây, tại Camping Sport Đồng Mô sẽ diễn ra một chương trình đại nhạc hội có tên Camping Acoustic Night. \n" +
        '\n' +
        'Thời gian: Từ 17h00 - 22h00 ngày 19/12/2020\n' +
        'Địa điểm: Camping Sport Đồng Mô - Cắm Trại - Dã Ngoại - Team building ',
      'Từ trung tâm thành phố (hồ Hoàn Kiếm) bạn di chuyển theo hướng Lê Đại Hành - Đại Cồ Việt sau đó rẽ sang đường Giải Phóng. Đi hết đường Giải Phóng thì rẽ trái đến Hoàng Liệt. Tiếp tục đi thêm khoảng 200m là tới công viên Yên Sở. Công viên Yên Sở có gì? \n' +
        '\n' +
        'Nếu như bạn không có nhiều thời gian và muốn tìm cho mình một khu cắm trại ở Hà Nội thì đừng bỏ qua công viên Yên Sở nhé. Vì chỉ cách trung tâm khoảng chừng 10km nên bạn có thể dễ dàng đi chơi gần Hà Nội bằng xe máy một cách thuận tiện. Tới đây chắc chắn bạn sẽ phải choáng ngợp trước một không gian xanh ngập tràn cây cối. Công viên Yên Sở có rất nhiều nơi để bạn có thể dựng trại hay đơn giản là dã ngoại, ăn uống cùng với bạn bè. Tại đây cũng có đầy đủ các dịch vụ cho thuê lều cắm trại, bếp nướng, than củi... ',
    ],
    description:
      'Cắm trại cũng được xem là một trong những hình thức thư giãn và du lịch một cách độc đáo giúp bạn F5 bản thân hiệu quả. Nếu còn e ngại không có nhiều thời gian để di chuyển và dạo chơi cho những điểm đến xa thì hãy cùng chúng mình tham khảo những địa điểm cắm trại gần Hà Nội với không gian đep, rộng, thoáng mát và đầy những điều bất ngờ sau đây nhé!',
  },
  {
    id: 2,
    title: '17 đồ dùng cần thiết khi đi cắm trại “nhớ check list nhé” !',
    thumbnail:
      'https://armyhaus.com/wp-content/uploads/2019/01/Complete-camping-essentials-810x402.jpg',
    images: [
      'https://armyhaus.com/wp-content/uploads/2019/01/camping.jpg',
      'https://armyhaus.com/wp-content/uploads/2019/01/223201893319PM%D9%84%DA%A9104.jpg',
      'https://armyhaus.com/wp-content/uploads/2019/01/carry-on-2_0.jpg',
      'https://armyhaus.com/wp-content/uploads/2019/01/2-2.jpg',
    ],
    createAt: 'September 2, 2021',
    content: [
      'Những người đã nhiều lần chuẩn bị những chuyến đi cắm trại, dã ngoại thường có lối tư duy tối giản đồ dùng cần thiết khi đi cắm trại. Với những kế hoặc và phụ kiện nâng cao, bạn có thể bắt đầu sắp xếp từ những thứ đơn giản như lều, túi ngủ.\n' +
        '\n' +
        'Hãy nhớ rằng, chuẩn bị đồ dùng đi cắm trại là điều không hề khó khăn nhưng cũng không hề đơn giản. Và nó sẽ dễ dàng hơn khi bạn lên kế hoạch cụ thể. Vì vậy, trước mỗi chuyến đi, lập kế hoạch đi cắm trại chi tiết nhất và những món đồ không thể thiếu được chúng tôi đề cập trong bài viết sẽ giúp bạn có cái nhìn toàn diện hơn về những phụ kiện cắm trại, dã ngoại không thể thiếu.',
      'Chuẩn bị đồ dùng cần thiết khi đi cắm trại\n' +
        'Bạn muốn tìm hiểu làm thế nào để sắp xếp ba lô cho cắm trại? Cho dù bạn là người mới bắt đầu hoặc chỉ muốn một danh sách kiểm tra ba lô để giúp bạn đi trên đường mòn, bạn sẽ muốn xem xét danh sách thiết bị này cho cuộc phiêu lưu lớn hơncủa bạn. Danh sách kiểm tra này được dự định là nhiều hơn đối với một số người, bạn sẽ không cần mọi thứ. Trên thực tế, tốt nhất bạn nên đóng gói một lượng tối thiểu thiết bị, phụ kiện khi đeo balo, nhất là khi bạn phải di chuyển bằng hình thức đi bộ nhiều.\n' +
        '\n' +
        'Hãy chắc chắn để nghiên cứu thời tiết và khí hậu của điểm đến của bạn và điều chỉnh khối luọngcủa bạn cho phù hợp. Nếu bạn sẽ đi bộ đường dài ở vùng lạnh hoặc mưa, đặc biệt chú ý đến thiết bị chống nước. Nếu trời sẽ lạnh, hãy lên kế hoạch mang thêm các lớp quần áo. Nếu bạn đủ may mắn để đi bộ và cắm trại trong thời tiết ấm áp, bạn có thể không cần nhiều đồ dùng cần thiết khi đi cắm trại như vậy.\n' +
        '\n' +
        'Balo du lịch\n' +
        'Cho dù bạn đang mong đợi thời tiết ấm áp, lạnh, nắng hay mưa, có những món đồ mà chiếc ba lô cơ bản bạn sẽ cần phải mang theo. Lều cắm trại, túi ngủ, thiết bị nấu ăn, và quần áo tối thiểu được yêu cầu cho bất kỳ chuyến đi qua đêm. Đây là những gì bạn nên chuẩn bị đầu tiên\n' +
        '\n' +
        'Ba lô cần có: Túi ngủ, đệm ngủ, Bếp dã ngoại và bật lửa, Món ăn, Bình nước , Đèn lều, Bảo vệ cơ thể bạn khỏi ánh nắng mặt trời bằng các loại kem dưỡng, Thêm quần áo nếu thời tiết lạnh.',
      'Quần áo cho du lịch ba lô\n' +
        'Quần áo cắm trại cũng giống như đi bộ đường dài ngoại trừ khi bạn sẽ qua đêm. Xem xét cẩn thận thời tiết và chuẩn bị.\n' +
        '\n' +
        'Lựa chọn chất liệu vải quần áo thấm hút mồ hôi tốt và khô thoáng nhanh chóng. Nếu có thể, chọn mua quần áo chống nước và chống nắng. Những chiếc áo khoác gió cũng rất cần thiết và là đồ dùng cần thiết khi đi cắm trại khi bạn sử dụng khi thời tiết có gió lạnh bất chợt hoặc bạn đêm khi nhiệt độ hạ thấp.\n' +
        '\n' +
        'Mẹ thiên nhiên có thể rất khó tính bất cứ lúc nào trong năm, đặc biệt là ở vùng cao. Và hạ thân nhiệt là nguyên nhân gây tử vong số một cho những người bị mất ở nơi hoang dã, vì nhiều người quên đi việc nhiệt độ giảm nghiêm trọng, một khi mặt trời lặn. Nếu mưa, tuyết hoặc thời tiết lạnh tấn công khi bạn đi xa, quần áo thêm sẽ giữ cho bạn ấm áp, khô ráo và khỏe mạnh. Chọn một chất liệu thấm mồ hôi và khô nhanh chóng và không bao giờ quên áo khoác đóng gói của bạn và những đôi tất.\n' +
        '\n' +
        'Quần tháo ống Columbia được sử dụng rất nhiều hiện nay bởi tính tiện lợi khi mang theo di chuyển. Khi đi trên đường, chúng có thể thành chiếc quần dài tiện lợi. Nhưng khi tới được địa điểm, bạn có thể tháo ống và giúp chúng trở thành quần sooc mát mẻ, thoải mái.',
      'Giày đi bộ\n' +
        'Những đôi giày nhẹ tới từ thương hiệu nổi tiếng The norh Face là lựa chọn tốt khi bạn chuẩn bị đồ dùng cần thiết khi đi cắm trại. Những đôi giày nhẹ nhàng và phù hợp sẽ giúp bạn được thoải mái và hoạt động cả một ngày dài. Nếu chọn những đôi giày quá nặng và không vừa, hoặc chật. Mụn nước và những vết sưng nóng có thể phát hiện khiến chân của bạn tổn thương.\n' +
        '\n' +
        'Găng tay đa năng (khăn buff) là phụ kiện đa năng giá rẻ (khoảng 50.000-80.000 đồng) mà rất nhiều người không biết khi chuẩn bị đồ dùng cần thiết khi đi cắm trại cho mình. Với hơn 36 cách sử dụng khăn đa năng đi phượt, các bạn có thể dùng nó và thay thế rất nhiều các nhu cầu trong hoạt động sống hàng ngày.',
    ],
    description:
      'Cắm trại cũng được xem là một trong những hình thức thư giãn và du lịch một cách độc đáo giúp bạn F5 bản thân hiệu quả. Nếu còn e ngại không có nhiều thời gian để di chuyển và dạo chơi cho những điểm đến xa thì hãy cùng chúng mình tham khảo những địa điểm cắm trại gần Hà Nội với không gian đep, rộng, thoáng mát và đầy những điều bất ngờ sau đây nhé!',
  },
  {
    id: 3,
    title: 'KINH NGHIỆM DU LỊCH, CẮM TRẠI NÚI HÀM LỢN SÓC SƠN 2020 TỪ A-Z',
    thumbnail:
      'https://file.hstatic.net/1000169629/file/nui-ham-lon-soc-son1_96f2994fed3f41808318daed082ecad1.jpg',
    images: [
      'https://file.hstatic.net/1000169629/file/khu-du-lich-ham-lon_12bb3ade37fc49a9a4e1a5bff55a4960.jpg',
      'https://file.hstatic.net/1000169629/file/duong-di1_ac86766185d0451592efdbe3afe4db5c.jpg',
      'https://file.hstatic.net/1000169629/file/leu-trai1_578b013afe9840e788f41d3309f9b0cf.jpg',
      'https://file.hstatic.net/1000169629/file/rung-thong-o-ham-lon_c9e93654f4e4492ca3d8e102b5e54a57.jpg',
    ],
    createAt: 'August 20, 2021',
    content: [
      'Nếu như bạn đang nghĩ rằng du lịch Sóc Sơn không biết có những địa chỉ nào thú vị vào dịp cuối tuần thì hãy đến ngay núi Hàm Lợn ở Sóc Sơn để trải nghiệm nhé.Nằm cách xa trung tâm thành phố nhộn nhịp, tấp nập đến đây bạn có thể được trải nghiệm không gian núi non hùng vĩ, nét hoang sơ vốn có của núi rừng Sóc Sơn. Chính vì những nét hoang sơ, đường núi gồ ghề của nơi đây lại là nét thu hút rất nhiều các bạn trẻ ghé tới cắm trại. \n' +
        '\n' +
        'NÚI HÀM LỢN Ở ĐÂU?\n' +
        'Đây được xem như là "nóc nhà thủ đô" khi là ngọn núi cao nhất trên dãy núi Độc Tôn của Hà Nội, với khung cảnh mang nét hoang sơ, độc đáo của nơi này. Núi Hàm Lợn cách trung tâm thành phố Hà Nội khoảng 40km theo hướng cao tốc Bắc Thăng Long - Nội Bài nên bạn có thể dễ dàng tìm đến. Chính vì vậy địa điểm cắm trại gần Hà Nội này được rất nhiều người tìm đến mỗi dịp cuối tuần.',
      'ĐƯỜNG ĐI NÚI HÀM LỢN NHƯ THẾ NÀO?\n' +
        'Chắc hẳn rất nhiều bạn muốn du lịch núi Hàm Lợn hay muốn đi cắm trại nhưng lại không biết đường đi hay không biết nên di chuyển bằng phương tiện nào cho phù hợp nhất. Đừng lo, hãy cùng mình chia sẽ kinh nghiệm cắm cắm trại núi Hàm Lợn Sóc Sơn Hà Nội ngay nhé.\n' +
        '\n' +
        'Vì quãng đường di chuyển không mấy khó khăn nên bạn có thể dễ dàng đi bằng xe máy. Bạn có thể mất khoảng 1 tiếng để di chuyển từ trung tâm thành phố Hà Nội đến núi Hàm Lợn: Đầu tiên, từ trung tâm bạn đi theo đường Đinh Tiên Hoàng, Trần Nguyên Hãn và Hàng Tre đến Trần Nhật Duật tại Lý Thái Tổ. Sau đó bạn di chuyển từ Âu Cơ, Cầu Nhật Tân/Võ Chí Công và Võ Nguyên Giáp đến Mai Đình, rẽ trái tại Km 5+250 TL131. Và đi từ ĐT35 đến Nam Sơn rồi tiếp tục di chuyển thêm khoảng 2km nữa thì sẽ đến với núi Hàm Lợn.\n' +
        '\n' +
        'Ngoài ra, với những bạn không có phương tiện di chuyển hoặc là muốn được nghỉ ngơi trước khi đến với khu du lịch Hàm Lợn này thì có thể di chuyển bằng xe bus. Bạn có thể chọn xe 64 từ KCN Bắc Thăng Long đến TTTM Bình An, xe bus này sẽ dừng ngay ở lối mòn vào núi nên rất thuận tiện cho các bạn đi vào khu cắm trại Hàm Lợn.',
      'Đi cắm trại Hàm Lợn Sóc Sơn thì nên chuẩn bị những gì? Nhiều bạn chưa đi cắm trại bao giờ thì chắc chẳn sẽ rất thắc mắc đúng không nào, vậy thì hãy tham khảo ngay những gợi ý đưới đây:\n' +
        '\n' +
        'Những đồ cần mang:\n' +
        '\n' +
        'LỀU CẮM TRẠI \n' +
        'THẢM TRẢI PICNIC \n' +
        'TÚI NGỦ (nếu bạn đi cắm trại qua đêm)\n' +
        'Tấm cách nhiệt (thảm lót)\n' +
        'Bếp nướng thịt\n' +
        'Than hoa, củi khô\n' +
        'Bật lửa, diêm\n' +
        'Bát đĩa đựng đồ ăn\n' +
        'Dụng cụ nấu ăn\n' +
        'BÀN GHẾ DÃ NGOẠI\n' +
        'XIÊN QUE NƯỚNG\n' +
        'Thực phẩm, đồ ăn, nước uống\n' +
        'Túi đựng rác\n' +
        'Đèn pin chiếu sángNhững đồ có thể chuẩn bị thêm:\n' +
        '\n' +
        'Áo khoác gió chống nước\n' +
        'Thùng nước sạch\n' +
        'Thùng giữ nhiệt (bảo quản đồ ăn và nước uống)\n' +
        'Ngoài những món đồ cắm trại ở bên trên bạn cũng cần chuẩn bị đồ ăn đi picnic cắm trại. Nếu còn đang không không biết đi picnic ăn gì thì hãy lựa chọn những món ăn đơn giản dễ làm. Ngoài ra, các món nướng thường cũng được rất nhiều người yêu thích đó!',
      'LỰA CHỌN ĐIỂM CẮM TRẠI Ở HÀM LỢN\n' +
        'Trải nghiệm cắm trại ở núi Hàm Lợn chắc chắn sẽ mang đến cho bạn nhiều kỉ niệm thú vị. Vậy cắm trại Hàm Lợn nên chọn địa điểm ở đâu đẹp và lý tưởng nhất? Theo như kinh nghiệm đi núi Hàm Lợn Sóc Sơn Hà Nội của nhiều người thì tại đây thường có 2 nơi chính thường được lựa chọn để cắm trại: \n' +
        '\n' +
        '+ Cắm trại hồ Hàm Lợn: \n' +
        '\n' +
        'Ở Hàm Lợn có một điểm cắm trại cho bạn ghé tới chính là hồ Hàm Lợn. Nơi đây còn được biết đến với tên gọi khác là hồ Núi Bàu. Với khu bãi đất rộng rãi, bằng phảng nên bạn có thể cùng bạn bè và gia đình cắm trại ở đây. Cùng nhau nhìn ngắm nguồn nước trong vắt, núi rừng bao quanh hùng vĩ và cùng nhau vui hát cùng nhau nướng đồ ăn cho chuyến đi thì quả là một trải nghiệm không thể bỏ lỡ.Nhiều người thường lựa chọn cắm trại ở khu vực hồ bởi không gian tại đây thường khá mát mẻ, xung quanh có nhiều nơi đất bằng phẳng rất lý tưởng để bạn cắm trại và tổ chức ăn uống. Tuy nhiên, nếu bạn đi cùng với trẻ nhỏ thì hãy cẩn thận một chút nhé!\n' +
        '\n' +
        '+ Cắm trại trên đồi hoa sim: \n' +
        '\n' +
        'Ngoài hồ Hàm Lợn thì cắm trại trên đồi cũng sẽ mang đến cho bạn nhiều trải nghiệm thú vị. Hơn nữa, thời tiết vào mùa hè còn rất mát mẻ. Đặc biệt, nếu như bạn đi vào mùa hoa sim thì còn được ngắm một khung cảnh cực đẹp và lãng mạn nữa chứ! + Ở khu cắm trại Hàm Lợn không có dịch vụ cung cấp đồ ăn nên bạn hãy chuẩn bị trước từ ở nhà nhé! Đừng quên bảo quản đồ ăn và nước uống cẩn thận để đảm bảo vệ sinh.\n' +
        '\n' +
        '+ Khi cắm trại qua đêm vào mùa đông bạn nên mang theo áo khoác hoặc một tấm lót cách nhiệt để đảm bảo cơ thể luôn được giữ ấm. \n' +
        '\n' +
        'Như vậy phía trên mình vừa chia sẻ cho các bạn kinh nghiệm cắm trại ở núi Hàm Lợn Sóc Sơn Hà Nội. Hy vọng với những chia sẻ này sẽ giúp cho các bạn có thêm nhiều hiểu biết về cắm trại hay du lịch Sóc Sơn nhé.',
    ],
    description:
      'Cắm trại cũng được xem là một trong những hình thức thư giãn và du lịch một cách độc đáo giúp bạn F5 bản thân hiệu quả. Nếu còn e ngại không có nhiều thời gian để di chuyển và dạo chơi cho những điểm đến xa thì hãy cùng chúng mình tham khảo những địa điểm cắm trại gần Hà Nội với không gian đep, rộng, thoáng mát và đầy những điều bất ngờ sau đây nhé!',
  },
]
export default {
  data() {
    return {
      blogs,
      products: [],
    }
  },
  async created() {
    await this.loadPage()
  },
  methods: {
    async loadPage() {
      const products = await this.$api.productList({
        headers: {
          Authorization: this.$auth.$storage.getUniversal('token').token,
        },
      })
      this.products = products.content.slice(0, 6)
    },
    addToCard(product) {
      if (process.browser) {
        let cart = localStorage.getItem('cart')
        if (cart === null) cart = []
        else cart = JSON.parse(cart)
        let count = 0
        for (let i = 0; i < cart.length; i++) {
          if (cart[i].product.productID === product.productID) {
            if (cart[i].quantity < product.qty) {
              cart[i].quantity++
            }
            count++
          }
        }
        if (count === 0) {
          cart.push({ product, quantity: 1 })
        }
        this.$message.success(`Add To Cart Successfully!`)
        localStorage.setItem('cart', JSON.stringify(cart))
      }
    },
    formatPrice(money) {
      return new Intl.NumberFormat('vi-VN', {
        style: 'currency',
        currency: 'VND',
      }).format(money)
    },
  },
}
</script>
