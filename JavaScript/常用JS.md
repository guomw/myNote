# 获取请求参数

        /*
        * @brief 获得页面参数
        * @param 参数名
        * */
        function=getQuery(name) {
            var strHref = window.document.location.href;
            var intPos = strHref.indexOf("?");
            var strRight = strHref.substr(intPos + 1);
            var arrTmp = strRight.split("&");
            for (var i = 0; i < arrTmp.length; i++) {
                var arrTemp = arrTmp[i].split("=");
                if (arrTemp[0].toUpperCase() == name.toUpperCase()) return arrTemp[1];
            }
            if (arguments.length == 1)
                return "";
            if (arguments.length == 2)
                return arguments[1];
        }



# ajax 封装

        /*
        * @brief ajaxPost请求
        * @param data 请求数据
        * @param callback 回调函数(ret,err)
        * @param header 自定义请求头
        * */
        function ajaxCall (url, data, callback, header) {
            var op = {
                url: url,
                data: data,
                type: "post",
                dataType: "json",
                success: function (ret) {
                    if (typeof callback == "function") {
                        if (typeof ret == "undefined")
                            ret = null;
                        callback(ret, null);
                    }
                },
                error: function (err) {
                    if (typeof callback == "function") {
                        if (typeof err == "undefined")
                            err = null;
                        callback(null, err);
                    }
                },
                beforeSend: function (request) {
                    var timestamp = (Date.parse(new Date()) / 1000).toString();
                    request.setRequestHeader("_timestamp", timestamp);
                    request.setRequestHeader("_md5", $.md5(timestamp));
                    request.setRequestHeader("_hottag", "0");
                    if (header != null) {
                        $.each(header, function (i, item) {
                            request.setRequestHeader("_" + i, item);                        
                        });
                    }                
                    request.setRequestHeader("_sign", "no");
                }
            };
            $.ajax(op);
        }


# 判断字符是否为空、null、undefined

        /*
        * @brief 判断字符是否为null或空或undefined
        */
        function isNullOrEmpty(str) {
            if (typeof str != 'undefined' && str != null && str.length > 0)
                return false;
            else
                return true;
        }

# 隐藏部分手机号码

        //手机或身份证号部分隐藏
        function subStringText(value) {
            if (value != null) {
                var result = "";
                if (value.length == 11) {
                    for (var i = 0; i < value.length; i++) {
                        result += (i < 3 || i > 8) ? value.substr(i, 1) : "*";
                    }
                }
                else if (value.length == 18) {
                    for (var i = 0; i < value.length; i++) {
                        result += (i < 3 || i > 15) ? value.substr(i, 1) : "*";
                    }
                }
                else if (value.length == 15) {
                    for (var i = 0; i < value.length; i++) {
                        result += (i < 3 || i > 12) ? value.substr(i, 1) : "*";
                    }
                }
                return result;
            }
            return "";
        }



# 发送短信验证码倒计时

        function LastTime(el, disCls) {
            if (el.length < 1)
                return;
            var oldText = el.html();
            var functionName = el.attr("onclick");
            el.removeAttr("onclick", "");
            if (el.hasClass(disCls)) {
                el.addClass("disabled");
                el.removeClass(disCls);
                var time = 60;
                var elTime = el.find("span");
                if (elTime.length == 0) {
                    el.html("<span>" + time + "</span>秒后重发");
                    elTime = el.find("span");
                }
                var timer = setInterval(function () {
                    if (time > 1) {
                        var str = time - 1;
                        time = time - 1;
                        elTime.html(str);
                    } else {
                        str = "";
                        el.html(oldText);
                        el.addClass(disCls);
                        el.removeClass("disabled");
                        if (functionName) { el.attr("onclick", functionName); }
                        clearInterval(timer);
                    }

                }, 1000);
            } else {
                el.addClass(disCls);
                var time = 60;
                var elTime = el.find("span");
                if (elTime.length == 0) {
                    el.html("<span>" + time + "</span>秒后重发");
                    elTime = el.find("span");
                }
                var timer = setInterval(function () {
                    if (time > 1) {
                        var str = time - 1;
                        time = time - 1;
                        elTime.html(str);
                    } else {
                        str = "";
                        el.html(oldText);
                        if (functionName) { el.attr("onclick", functionName); }
                        el.removeClass(disCls);
                        clearInterval(timer);
                    }

                }, 1000)
            }
        }

# 常用类型判断
        function isInt(str) { return /^-?\d+$/.test(str); }
        function isNumber(obj) { return typeof obj == 'number'; }
        function isFloat(str) { return /^(-?\d+)(\.\d+)?$/.test(str); }
        function isEmail(str) { return /^[A-Z_a-z0-9-\.]+@([A-Z_a-z0-9-]+\.)+[a-z0-9A-Z]{2,4}$/.test(str); }
        function isMobile(str) { return /^(1)\d{10}$/.test(str); }
        function encode(str) { return encodeURIComponent(str); }
        function decode(str) { return decodeURIComponent(str); }