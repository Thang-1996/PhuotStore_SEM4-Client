<template>
  <div>
    <!-- Map Start -->
    <div class="sigma_map">
      <iframe
        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2624.9914406081493!2d2.292292615201654!3d48.85837360866272!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x47e66e2964e34e2d%3A0x8ddca9ee380ef7e0!2sEiffel%20Tower!5e0!3m2!1sen!2sin!4v1571115084828!5m2!1sen!2sin"
        allowfullscreen=""></iframe>
    </div>
    <!-- Map End -->

    <!-- Contact form Start -->
    <div class="section mt-negative">
      <div class="container">

        <form class="sigma_box box-lg mf_form_validate ajax_submit m-0" action="https://slidesigma.com/themes/html/campa/sendmail.php" method="post" enctype="multipart/form-data">
          <div class="row">
            <div class="col-lg-4">
              <div class="form-group">
                <i class="far fa-user"></i>
                <input type="text" placeholder="Full Name" class="form-control dark" name="name">
              </div>
            </div>
            <div class="col-lg-4">
              <div class="form-group">
                <i class="far fa-envelope"></i>
                <input type="email" placeholder="Email Address" class="form-control dark" name="email">
              </div>
            </div>
            <div class="col-lg-4">
              <div class="form-group">
                <i class="far fa-pencil"></i>
                <input type="text" placeholder="Subject" class="form-control dark" name="subject">
              </div>
            </div>
          </div>
          <div class="form-group">
            <textarea name="message" placeholder="Enter Message" cols="45" rows="5" class="form-control dark"></textarea>
          </div>
          <div class="text-center">
            <button type="submit" class="sigma_btn-custom secondary" name="button">Submit Now <i class="far fa-paper-plane"></i> </button>
            <div class="server_response w-100">
            </div>
          </div>
        </form>

      </div>
    </div>
    <!-- Contact form End -->

    <!-- Icons Start -->
    <div class="section section-padding pt-0">
      <div class="container">
        <div class="row">

          <div class="col-lg-4">
            <div class="sigma_icon-block icon-block-2">
              <div class="icon-wrapper-outer">
                <div class="icon-wrapper">
                  <i class="fal fa-envelope"></i>
                </div>
                <div>
                  <h5> Email Address </h5>
                  <p>Send mail asap anytime</p>
                </div>
              </div>
              <div class="sigma_icon-block-content">
                <div>
                  <p>example@example.com</p>
                  <p>info@example.com</p>
                </div>
                <i class="fal fa-arrow-right"></i>
              </div>
            </div>
          </div>

          <div class="col-lg-4">
            <div class="sigma_icon-block icon-block-2">
              <div class="icon-wrapper-outer">
                <div class="icon-wrapper">
                  <i class="fal fa-map-marker"></i>
                </div>
                <div>
                  <h5> Address </h5>
                  <p>Find us asap anytime</p>
                </div>
              </div>
              <div class="sigma_icon-block-content">
                <div>
                  <p>16/A Daddy Yankee Tower</p>
                  <p>16/A Daddy Yankee Tower</p>
                </div>
                <i class="fal fa-arrow-right"></i>
              </div>
            </div>
          </div>

          <div class="col-lg-4">
            <div class="sigma_icon-block icon-block-2">
              <div class="icon-wrapper-outer">
                <div class="icon-wrapper">
                  <i class="fal fa-phone"></i>
                </div>
                <div>
                  <h5> Call Us </h5>
                  <p>Call us asap anytime</p>
                </div>
              </div>
              <div class="sigma_icon-block-content">
                <div>
                  <p>+123 456 789 29</p>
                  <p>+123 478 373 38</p>
                </div>
                <i class="fal fa-arrow-right"></i>
              </div>
            </div>
          </div>

        </div>
      </div>
    </div>
    <!-- Icons End -->
  </div>
</template>

<script>
export default {
  name: 'index.vue',
}
</script>

