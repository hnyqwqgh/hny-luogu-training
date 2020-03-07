/**
 * Created by Administrator on 2015/10/29.
 */


var initTop = 0;
var scrollplace = 0;
var refer;
var rep; //record auto reload
var online_coding = 0;

var loadstr = {
    0: "<img src='/images/loading1.png'>" +
        "<div class='lg-loading-desc'><h2>少女祈祷中……</h2>" +
        "<p>&nbsp;</p><p>稍微等一下，马上就好了。</p></div>",
    1: "<img src='/images/loading2.png'>" +
        "<div class='lg-loading-desc'><h2>舰船建造中……</h2>" +
        "<p>&nbsp;</p><p>稍微等一下，马上就好了。</p></div>",
    2: "<img src='/images/loading3.png'>" +
        "<div class='lg-loading-desc'><h2>live进行中……</h2>" +
        "<p>&nbsp;</p><p>稍微等一下，马上就好了。</p></div>"
};
var verify = "<img src=\"/download/captcha\" onclick=\"this.src='/download/captcha'\">";

var backarticle = function () {
    $(".lg-article-sub").hide();
    $(".lg-article").show();
    $(document).scrollTop(scrollplace);
};

var getunreadtime = 10;

var navtmp = '',
    navflag = 0;

//benben and list
var benbenpage = 0;
var autoload = 0;

function get_notice(url) {
    benbenpage++;
    $("#benbennextrow").html('<img src="/images/loading.gif" width="20">加载中。。。等一下 马上就好啦');
    $.get(url + "&page=" + benbenpage,
        function (data) {
            var arr = eval('(' + data + ')');
            var html = arr['more']['html'];

            if (html == "") {
                $("#benbennextrow").html('真的没有啦！');
            } else {
                $('#notice_list').append(html);
                $(".am-comment-avatar").error(function () {
                    $(this).attr("src", "/images/icon.png");
                });
                $("#benbennextrow").html('<a class="lg-benbennext">点击查看更多。。。</a>');
            }
        });
}

function hide_artile() {
    scrollplace = $(document).scrollTop();
    $(".lg-article-sub").html(loadstr[mt_rand(0, 2)]);
    $(".lg-article-sub").show();
    $(".lg-article").hide();
    $(document).scrollTop(0);
}

function mt_rand(min, max) {
    var range = max - min,
        rand = Math.random();
    return min + Math.round(rand * range);
}

function downloadFile(fileName, content) {
    var blob = new Blob([content], {
        type: "text/plain"
    });
    if (window.navigator.msSaveBlob) // IE hack
        window.navigator.msSaveBlob(blob, fileName);
    else {
        var aLink = document.createElement('a');
        aLink.setAttribute('download', fileName);
        aLink.href = window.URL.createObjectURL(blob);
        document.body.appendChild(aLink);
        aLink.click();
        document.body.removeChild(aLink);
    }
}

function captchaFallback() {
    $('input[name^="verify"]').show();
    $("#verify_img").show();
    $("#captcha-fallback").hide();
    $("#g-recaptcha").hide();
    grecaptcha = undefined;
}

function renderCaptcha() {
    if (typeof grecaptcha !== "undefined" && $('input[name^="verify"]').length !== 0) {
        $('input[name^="verify"]').hide();
        $("#verify_img").hide();
        $("#captcha-fallback").show();
        if(!document.getElementById("g-recaptcha"))
            return;

        try {
            grecaptcha.reset();
        } catch (e) {}
        try {
            id = grecaptcha.render("g-recaptcha", {
                'sitekey': '6Ld0ExUUAAAAAGvarSgV0-P2SfvIOHB_tbN0R5ps',
                'callback': function () {
                    $('input[name^="verify"]').val(grecaptcha.getResponse(id));
                    var captchaInstance = $("#g-recaptcha");
                    if(captchaInstance.attr('data-callback'))
                        window[captchaInstance.attr('data-callback')]();
                }
            });
        } catch(e) {
        }
    }
}

$.ajaxSetup({
    beforeSend: function (xhr, settings) {
        var csrf_token = $('meta[name=csrf-token]').attr('content');
        if (csrf_token)
            xhr.setRequestHeader('X-CSRF-Token', csrf_token);
    }
});