# ajax 封装

    /*
    * @brief ajaxPost请求
    * @param data 请求数据
    * @param callback 回调函数(ret,err)
    * @param header 自定义请求头
    * */
    ajaxCall: function (url, data, callback, header) {
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
    },