

<style type="text/css" media="screen">
    /* Example 2 (login form) */
    .login-block{
      position: fixed;
      background: #fff;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
    }

    .login_form{
      width: 450px;
      margin: 0 auto;
    }

    .login_form .input{
      padding: 5px;
      border: 1px black solid;
    }

    .login_form.modal {
      border-radius: 0;
      line-height: 18px;
      padding: 0;
      font-family: "Lucida Grande", Verdana, sans-serif;
    }

    .login_form h3 {
      margin: 0;
      padding: 10px;
      color: #fff;
      font-size: 14px;
      background: -moz-linear-gradient(top, #2e5764, #1e3d47);
      background: -webkit-gradient(linear,left bottom,left top,color-stop(0, #1e3d47),color-stop(1, #2e5764));
    }

    .login_form.modal p { padding: 20px 30px; border-bottom: 1px solid #ddd; margin: 0;
      background: -webkit-gradient(linear,left bottom,left top,color-stop(0, #eee),color-stop(1, #fff));
      overflow: hidden;
    }
    .login_form.modal p:last-child { border: none; }
    .login_form.modal p label { float: left; font-weight: bold; color: #333; font-size: 13px; width: 110px; line-height: 22px; }
    .login_form.modal p input[type="text"],
    .login_form.modal p input[type="password"] {
      font: normal 12px/18px "Lucida Grande", Verdana;
      padding: 3px;
      border: 1px solid #ddd;
      width: 200px;
    }

    .lds-spinner {
      color: official;
      display: block;
      position: relative;
      width: 80px;
      height: 80px;
      margin: 0 auto;
    }
    .lds-spinner div {
      transform-origin: 40px 40px;
      animation: lds-spinner 1.2s linear infinite;
    }
    .lds-spinner div:after {
      content: " ";
      display: block;
      position: absolute;
      top: 3px;
      left: 37px;
      width: 6px;
      height: 18px;
      border-radius: 20%;
      background: #BAE1FF;
    }
    .lds-spinner div:nth-child(1) {
      transform: rotate(0deg);
      animation-delay: -1.1s;
    }
    .lds-spinner div:nth-child(2) {
      transform: rotate(30deg);
      animation-delay: -1s;
    }
    .lds-spinner div:nth-child(3) {
      transform: rotate(60deg);
      animation-delay: -0.9s;
    }
    .lds-spinner div:nth-child(4) {
      transform: rotate(90deg);
      animation-delay: -0.8s;
    }
    .lds-spinner div:nth-child(5) {
      transform: rotate(120deg);
      animation-delay: -0.7s;
    }
    .lds-spinner div:nth-child(6) {
      transform: rotate(150deg);
      animation-delay: -0.6s;
    }
    .lds-spinner div:nth-child(7) {
      transform: rotate(180deg);
      animation-delay: -0.5s;
    }
    .lds-spinner div:nth-child(8) {
      transform: rotate(210deg);
      animation-delay: -0.4s;
    }
    .lds-spinner div:nth-child(9) {
      transform: rotate(240deg);
      animation-delay: -0.3s;
    }
    .lds-spinner div:nth-child(10) {
      transform: rotate(270deg);
      animation-delay: -0.2s;
    }
    .lds-spinner div:nth-child(11) {
      transform: rotate(300deg);
      animation-delay: -0.1s;
    }
    .lds-spinner div:nth-child(12) {
      transform: rotate(330deg);
      animation-delay: 0s;
    }
    @keyframes lds-spinner {
      0% {
        opacity: 1;
      }
      100% {
        opacity: 0;
      }
    }

    #iderrorlogin{
      color: #FF9AA2;
    }

  </style>
<!-- Remember to include jQuery :) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.0.0/jquery.min.js"></script>

<div class="login-block" id="loginblock">
<!-- Modal HTML embedded directly into document -->
<div  id="ldsspinne" class="lds-spinner"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>

<div class="login_form" style="display:none">
  <h3>Please login to continue</h3>
  <div class="input">
    <p><label>Username:</label><input id="idusernamelogin" type="text" /></p>
    <p><label>Password:</label><input id="idpasswordlogin" type="password" /></p>
    <p><input id="idbuttonlogin" type="button" value="Login" /></p>
    <p id="iderrorlogin"></p>
  </div>
</div>
</div>
<script type="text/javascript">
  $(document).ready(function(){
    $("body").append($("#loginblock"));
    $.ajax({
        type: 'GET',
        url: '/.netlify/functions/server/api/profile',
        beforeSend: function(xhr) {
          if (localStorage.token) {
            xhr.setRequestHeader('Authorization', 'Bearer ' + localStorage.token);
          }
        },
        success: function(data) {
            $("#loginblock").hide();
            $("#navigation ul.nav").append("<li id='idlogout'><a href='javascript:void(0)'>Logout</a></li>");
             $("#idlogout").click(function() {
               var confirmmsg = confirm("Are you sure to Logout?");
               if(confirmmsg){
                localStorage.clear();
                window.location.href="/";
              }
            });
        },
        error: function() {
          $("#ldsspinne").hide();
          $(".login_form").removeAttr('style');
          //$("#iderrorlogin").text("Sorry, you are not logged in.");
        }
      });
    $("#idbuttonlogin").click(function(){
      $("#iderrorlogin").text("");
      // console.log("username : ", $("#idusernamelogin").val());
      // console.log("password : ", $("#idpasswordlogin").val());
      $("#ldsspinne").show();
      $(".login_form").hide();
      $.ajax({
        type: "POST",
        url: "/.netlify/functions/server/login",
        data: {
          username: $("#idusernamelogin").val(),
          password: $("#idpasswordlogin").val()
        },
        success: function(data) {
          localStorage.token = data.token;
          //alert('Got a token from the server! Token: ' + data.token);
          $("#loginblock").hide();
          $("#navigation ul.nav").append("<li id='idlogout'><a href='javascript:void(0)'>Logout</a></li>");
             $("#idlogout").click(function() {
               var confirmmsg = confirm("Are you sure to Logout?");
               if(confirmmsg){
                localStorage.clear();
                window.location.href="/";
              }
            });
        },
        error: function() {
          $("#ldsspinne").hide();
          $(".login_form").show();
          $("#iderrorlogin").text("Login Failed");
        }
      });
      return false;
    });
  });
</script>